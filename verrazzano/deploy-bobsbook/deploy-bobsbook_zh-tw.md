# 建置 Bobby 的 Books 範例應用程式

## 簡介

### 關於 Bobby 的書籍應用程式

![Bobby 的書籍應用程式](images/bobbyoverview.png " ")

[Bobby 的書籍](https://verrazzano.io/docs/samples/bobs-books/)由三個主要部分組成：

*   後端_「訂單處理」_應用程式，這是 REST 服務的 Java EE 應用程式，也是非常簡單的 JSP UI，可將資料儲存在 MySQL 資料庫中。此應用程式會在 WebLogic 伺服器上執行。
*   前端網路商店 _"Robert's Books"_ 是一般的書籍賣家。這是以 Helidon 微服務 (從 Coherence 取得工作簿資料) 的方式實行，此微服務使用 Coherence 快取存放區來保存訂單管理程式的資料，並具有 React Web UI。
*   前端網路商店 _"Bobby's Books"_ ，是兒童書店的專門店。這是導入為 Helidon 微服務，它會從 (不同) Coherence 快取存放區取得工作簿資料、直接與訂單管理程式連接，並在 WebLogic 伺服器上執行 JSF Web UI。

如需此應用程式的詳細資訊和原始碼，請參閱 [Verrazzano Examples](https://github.com/verrazzano/examples) 。

### Verrazzano 和應用程式部署

Verrazzano 支援使用[開放應用程式模型 (OAM)](https://oam.dev/) 定義應用程式。Verrrazzano 應用程式由元件和應用程式配置組成。

當您使用 Verrazzano 部署應用程式時，平台會在服務網狀組織中建立連線、網路原則和輸入，並佈線監控堆疊以擷取測量結果、日誌以及追蹤。Verrazzano 使用 OAM 元件定義系統的功能單位，然後透過定義關聯的應用程式組態來組合和配置。

### Verrazzano 元件

Verrazzano OAM 元件是描述應用程式一般組合與環境需求的 [Kubernetes 自訂資源](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)。

下列程式碼顯示此實驗室中使用的 Bobby's Books 範例應用程式元件。此資源描述由包含顯示單一端點之 Helidon 應用程式的單一 Docker 映像檔實行的元件。

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: bobby-helidon
      namespace: bobs-books
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: bobbys-helidon-stock-application
          labels:
            app: bobbys-helidon-stock-application
        spec:
          deploymentTemplate:
            metadata:
              name: bobbys-helidon-stock-application
            podSpec:
              containers:
                - name: bobbys-helidon-stock-application
                  image: container-registry.oracle.com/verrazzano/example-bobbys-helidon-stock-application:0.1.12-1-20210624160519-017d358
                  imagePullPolicy: IfNotPresent
                  ports:
                    - containerPort: 8080
                      name: http
                  env:
                    - name: BACKEND_PORT
                      value: "8001"
                    - name: BACKEND_HOSTNAME
                      value: bobs-bookstore-cluster-cluster-1.bobs-books.svc.cluster.local
                    - name: COH_CLUSTER
                      value: bobbys-coherence
                    - name: COH_CACHE_CONFIG
                      value: coherence-cache-config.xml
                    - name: COH_POF_CONFIG
                      value: pof-config.xml
              imagePullSecrets:
                - name: bobs-books-repo-credentials
    

元件每個欄位的簡短描述：

*   **apiVersion** - 元件自訂資源定義的版本
*   **kind** - 元件自訂資源定義的標準名稱
*   **metadata.name** - 用來建立元件自訂資源的名稱
*   **metadata.namespace** - 用來建立此元件之自訂資源的命名空間
*   **spec.workload.kind** - VerrazzanoHelidonWorkload 定義了 Kubernetes 的無狀態工作負載
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name** - 用來建立 Kubernetes 無狀態工作負載的名稱
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - 實作容器
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - 容器公開的連接埠

您可以在 [bobs-books-comp.yaml](https://github.com/verrazzano/verrazzano/blob/master/examples/bobs-books/bobs-books-comp.yaml) 檔案中找到 Bobbys 之書應用程式的完整元件描述。

### Verrazzano 應用程式組態

Verrazzano 應用程式組態為提供環境特定自訂項目的 Kubernetes 自訂資源。下列程式碼顯示此實驗室中使用之 Bob's Books 範例的應用程式組態。此資源指定將應用程式建置到 Bobs-books 名稱空間。

其他的程式實際執行功能則是使用特性指定，或擴增工作負載的程式實際執行重疊。例如，輸入特徵會指定輸入主機和路徑，而測量結果特徵則提供用來取得應用程式相關測量結果的 Prometheus 刮痕。

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: bobs-books
      namespace: bobs-books
      annotations:
        version: v1.0.0
        description: "Bob's Books"
    spec:
      components:
        - componentName: robert-helidon
          traits:
            - trait:
                apiVersion: core.oam.dev/v1alpha2
                kind: ManualScalerTrait
                spec:
                  replicaCount: 2
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
        - componentName: robert-coh
        - componentName: bobby-coh
        - componentName: bobby-helidon
        - componentName: bobby-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobbys-front-end"
                          pathType: Prefix
        - componentName: bobs-orders-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobs-bookstore-order-manager/orders"
                          pathType: Prefix
        - componentName: bobs-orders-configmap
        - componentName: bobs-mysql-deployment
        - componentName: bobs-mysql-service
        - componentName: bobs-mysql-configmap
    

應用程式組態中每個欄位的簡短描述：

*   **apiVersion** - ApplicationConfiguration 自訂資源定義的版本
*   **kind** - 應用程式組態自訂資源定義的標準名稱
*   **metadata.name** - 用來建立此應用程式組態資源的名稱
*   **metadata.namespace** - 此應用程式組態自訂資源所使用的命名空間
*   **spec.components** - 參照用於指定執行時期配置的應用程式元件
*   **spec.components\[\].traits** - 為應用程式元件指定的特性

為了探索特徵，我們可以檢查傳入特徵的領域：

*   **apiVersion** - OAM 特徵自訂資源定義的版本
*   **kind** - IngressTrait 是 OAM 應用程式輸入特性自訂資源定義的名稱
*   **spec.rules.paths** - 存取應用程式的相關資訊環境路徑

此實驗室會逐步引導您完成建置 Bobby 之 Books 範例應用程式的程序。

預估時間：10 分鐘

### 目標

在此實驗室中，您將：

*   接受從 Oracle Container Registry 中的儲存區域下載映像檔的授權。
*   建置 Bobby 的 Books 範例應用程式。
*   驗證成功部署 Bobby 的 Books 範例應用程式。

### 先決條件

若要執行「實驗室 3」，您必須具備：

*   執行實驗室 1，在 Oracle Cloud Infrastructure 上建立 OKE 叢集。

\* 執行實驗室 1，將 kubectl 設定為存取 Oracle Cloud Infrastructure 上的 OKE 叢集。

*   執行實驗室 2，在 Kubernetes 叢集上安裝 Verrazzano。
*   文字編輯器，您可以在其中根據您的環境貼上指令和 URL 並加以修改。接著您可以複製並貼上修改的指令，以便在 _Cloud Shell_ 中執行這些指令。

## 作業 1：接受授權合約以從 Oracle Container Registry 中的儲存區域下載映像檔

若要部署 _Bobby 的 Books_ 範例應用程式，我們將使用範例影像。由於這些映像檔包含 Oracle 產品，因此您必須先接受授權合約，才能在 bobs-books 檔案 `bobs-books-comp.yaml` 中使用這些映像檔。

1.  按一下 Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) 的連結並登入。您需要有 Oracle 帳戶。
    
    ![登入](images/registrysignin.png " ")
    
2.  在「使用者名稱」和「密碼」欄位中輸入您的 _Oracle 帳戶證明資料_，然後按一下_登入_。我們之後將使用這些證明資料在 Kubernetes 中建立加密密碼。
    
    ![Oracle SSO ](images/oraclesso.png " ")
    
3.  在首頁上，選擇 _Verrazzano_ 。
    
    ![登錄首頁](images/registryhomepage.png " ")
    
4.  例如 -bobbys-coherence、example-bobbys-front-end、example-bobs-books-order-manager 以及 example-roberts-coherence 儲存區域，選取 _English_ 作為語言，然後按一下_繼續_。
    
    ![繼續](images/continue.png " ")
    
5.  按一下_接受_接受授權合約。
    
    ![接受協議](images/acceptagreement.png " ")
    
6.  請確認您已接受與 Verrazzano 相關的儲存庫授權合約，如下列影像所示。
    
    ![驗證授權協議](images/verifyaggrement.png " ")
    
7.  在 Oracle Container Registry 的首頁中，搜尋 _weblogic_
    
    ![搜尋 WebLogic](images/searchweblogic.png " ")
    
8.  如上所示，按一下 _weblogic_ ，並接受您為 Verrazzano imagaes 所做的授權。![點擊網路邏輯](images/clickweblogic.png) ![接受 weblogic](images/acceptagreement.png " ")
    

## 作業 2：部署 Bobby 的報表簿應用程式

我們需要下載原始碼，其中包含組態檔：`bobs-books-app.yaml` 和 `bobs-books-comp.yaml`。

1.  下載 Bobby 之 Book 範例的 Verrazzano OAM 元件 yaml 檔案與 Verrazzano 應用程式組態檔。按一下_複製_，然後在 Cloud Shell 中貼上命令，如下所示：
    
        <copy>
        curl -LSs  https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-app.yaml >~/bobs-books-app.yaml
        curl -LSs https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-comp.yaml >~/bobs-books-comp.yaml
        cd ~
        </copy>
        
2.  我們會將所有 Kubernetes 使用者自建物件保留在不同的命名空間中。為 Bob 的 Books 範例應用程式建立命名空間。命名空間是一種將叢集組織成虛擬子叢集的方式。我們可以在叢集內擁有任何數目的命名空間，每個命名空間會以邏輯方式區隔，但能夠相互通訊。此外，我們還需要讓 Verrazzano 知道我們在該命名空間中儲存 Verrazzano 使用者自建物件。因此，我們需要新增一個識別由 Verrazzano 管理之 Bobs-books 名稱空間的標籤。標籤用於指定有意義且與使用者相關之物件的識別屬性。在這裡，針對 Bobs-book 命名空間，我們會附加標籤，將此命名空間標記為由 Verrazzano 管理 。 _istio-injection=enabled_ 啟用 Istio "sidecar"，因此可協助建立 Istio 代理主機。藉由 Istio 代理主機，我們可以存取其他 Istio 服務，例如 Istio 閘道等等。若要使用先前提及的屬性將標籤新增至 bobs-books 命名空間，請複製下列命令，然後在 _Cloud Shell_ 中執行該標籤
    
        <copy>
        kubectl create namespace bobs-books
        kubectl label namespace bobs-books verrazzano-managed=true istio-injection=enabled
        </copy>
        
    
    ![建立命名空間](images/createnamespace.png " ")
    
3.  複製下列命令以下載命令檔。此命令檔會認證 Oracle Container Registry 的使用者。如果認證成功，將會建立 Docker 登錄加密密碼。Docker 登錄可儲存及版本映像檔的方式，例如 GitHub 代表一般程式碼，但若為容器 (Kubernetes 可以提取)。在此，我們將建立一個 docker-registry 密碼，以從 Oracle Container Registry 擷取 Bobby 的 Books 範例影像。按一下下列命令上的_複製_，然後將它貼到您選擇的任何文字編輯器中，並分別以您在作業 1 中使用的電子郵件 ID 和密碼取代使用者名稱和密碼，以接受從 Oracle Container Registry 下載映像檔的授權合約。然後，在 Cloud Shell 中貼上修改的命令，如下所示：
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/deploy-bobsbook/create_secret.sh >~/create_secret.sh
        chmod 777 create_secret.sh
        ./create_secret.sh username 'password'    
        </copy>
        
    
    ![建立加密密碼](images/createsecret.png " ")
    
    > 請以單引號輸入密碼。
    
4.  我們需要使用證明資料建立數個 Kubernetes 加密密碼。在 Bobby 的 Books 應用程式中，有兩個 WebLogic 網域 _bobby-front-end_ 和 _bobs-bookstore_ 。WebLogic 網域的證明資料保留在 Kubernetes 加密密碼中，加密密碼的名稱是使用 WebLogic 網域資源中的 _webLogicCredentialsSecret_ 指定。此外，必須在執行網域的命名空間中建立網域證明資料加密密碼。我們需要建立稱為 _bobbys-front-end-weblogic-credentials_ 的加密密碼，以及 WebLogic 伺服器網域使用的 _bobs-bookstore-weblogic-credentials_ 和使用者名稱值 `weblogic`，以及在 Bobs-books 名稱空間中隨機產生的密碼。Bobby 的 Books 應用程式使用 _mysql_ 資料庫。因此，我們會建立一個名為 _mysql-credentials_ 的新加密密碼，使用者名稱值為 `weblogic`、隨機產生的密碼，以及 bobs-books 名稱空間中的 JDBC URL _jdbc:mysql：//mysql.bobs-books.svc.cluster.local:3306/books_ 。我們會在 WebLogic DataSource 物件的 JDBC 連線字串中使用這個值。請將命令區塊複製並貼到 _Cloud Shell_ 中。
    
        <copy>
        export WLS_USERNAME=weblogic
        export WLS_PASSWORD=$((< /dev/urandom tr -dc 'A-Za-z0-9"'\''/<=>?\^_`|~' | head -c10);(date +%S))
        echo $WLS_PASSWORD
        kubectl create secret generic bobbys-front-end-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic bobs-bookstore-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic mysql-credentials \
            --from-literal=username=$WLS_USERNAME \
            --from-literal=password=$WLS_PASSWORD \
            --from-literal=url=jdbc:mysql://mysql.bobs-books.svc.cluster.local:3306/books \
            -n bobs-books
        cd ~
        </copy>
        
    
    ![建立資源](images/createresource.png " ")
    
5.  我們有一個 Kuberneter 叢集 _cluster1_ _verrazzano_，其中包含三個節點。現在，我們希望在 _cluster1_ _verrazzano_ 上部署 Bobby 的 Books 容器化應用程式。因此，我們需要 Kubernetes 部署組態。此部署指示 Kubernetes 建立和更新 Bobby's Books 應用程式的執行處理。這裡有 `bobs-books-comp.yaml` 檔案，指示 Kubernetes 部署 Bobby 的 Books 應用程式。複製並貼上下列兩個指令，如圖所示。`bobs-books-comp.yaml` 檔案包含各種 OAM 元件的定義，其中 OAM 元件是描述應用程式一般組合與環境需求的 Kubernetes 自訂資源。若要深入瞭解 `bobs-books-comp.yaml` 檔案，請檢閱此實驗室 3 簡介區段中的 Verrazzano 元件。
    
        <copy>kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books</copy>
        
    
    執行結果應該和下列類似：
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh created
        component.core.oam.dev/robert-helidon created
        component.core.oam.dev/bobby-coh created
        component.core.oam.dev/bobby-helidon created
        component.core.oam.dev/bobby-wls created
        component.core.oam.dev/bobs-mysql-configmap created
        component.core.oam.dev/bobs-mysql-service created
        component.core.oam.dev/bobs-mysql-deployment created
        component.core.oam.dev/bobs-orders-configmap created
        component.core.oam.dev/bobs-orders-wls created
        $
        
6.  `bobs-books-app.yaml` 檔案是 Verrazzano 應用程式組態檔，提供環境特定的自訂項目。若要深入瞭解 `bobs-books-app.yaml` 檔案，請檢閱此實驗室 3「簡介」一節中的「Verrazzano 應用程式組態」。
    
        <copy>kubectl apply -f ~/bobs-books-app.yaml -n bobs-books</copy>
        
    
    執行結果應該和下列類似：
    
        $ kubectl apply -f ~/bobs-books-app.yaml -n bobs-books
        applicationconfiguration.core.oam.dev/bobs-books created
        $
        
7.  等待 Bobby 之 Books 範例應用程式中的所有 Pod 處於_執行中_狀態。您可能需要重複此指令幾次，才能順利執行。WebLogic 伺服器和 Coherence Pod 可能需要一些時間來建立和就緒。此 _kubectl_ 指令將等待 bobs-books 名稱空間內的所有 Pod 處於_執行中_ 狀態。約需 4-5 分鐘。如果您等待超過 5 分鐘，可以再次重新執行命令。
    
        <copy>kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s</copy>
        
    
    執行結果應該和下列類似：
    
          $ kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s
          pod/bobbys-coherence-0 condition met
          pod/bobbys-front-end-adminserver condition met
          pod/bobbys-front-end-managed-server1 condition met
          pod/bobbys-helidon-stock-application-5f74cbcc8b-cw4x4 condition met
          pod/bobs-bookstore-adminserver condition met
          pod/bobs-bookstore-managed-server1 condition met
          pod/mysql-6bc8f9f785-n4qjh condition met
          pod/robert-helidon-65b8874988-7x5vj condition met
          pod/robert-helidon-65b8874988-vnntp condition met
          pod/roberts-coherence-0 condition met
          pod/roberts-coherence-1 condition met
          $
        

## 作業 3：驗證成功部署 Bobby 的 Book 應用程式

確定應用程式組態、網域、Coherence 資源以及傳入特性都存在。

1.  若要確認已順利將 _Bobby 的報表簿_應用程式部署在工作簿命名空間中。
    
        <copy>kubectl get ApplicationConfiguration -n bobs-books</copy>
        
    
    執行結果應該和下列類似：
    
        $ kubectl get ApplicationConfiguration -n bobs-books
          NAME         AGE
          bobs-books   72m
        $
        
2.  若要確認兩個 WebLogic 網域均已順利建立在 Bobs-books 命名空間內。
    
        <copy>kubectl get Domain -n bobs-books</copy>
        
    
    執行結果應該和下列類似：
    
        $ kubectl get Domain -n bobs-books
          NAME               AGE
          bobbys-front-end   73m
          bobs-orders-wls    73m
        $
        
3.  若要確認兩個 Coherence 叢集都已順利建立在工作簿命名空間內。
    
        <copy>kubectl get Coherence -n bobs-books</copy>
        
    
    執行結果應該和下列類似：
    
        $ kubectl get Coherence -n bobs-books
        NAME              CLUSTER           ROLE           REPLICAS  READY PHASE
        bobbys-coherence  bobbys-coherence  bobbys-coherence  1       1    Ready
        roberts-coherence roberts-coherence roberts-coherence 2       2    Ready
        $
        
4.  若要取得 Bobby 之 Book 應用程式的 IngressTrait，請在 _Cloud Shell_ 中執行下列指令。
    
        <copy>kubectl get IngressTrait -n bobs-books</copy>
        
    
    執行結果應該和下列類似：
    
        $ kubectl get IngressTrait -n bobs-books
          NAME                               AGE
          bobby-wls-trait-79b67d9d88         76m
          bobs-orders-wls-trait-57b4d8cb4b   76m
          robert-helidon-trait-54d7bcd54b    76m
        $
        
5.  確認已順利建立服務 Pod 並轉換成_執行中_狀態。請注意，這可能需要幾分鐘的時間，您可能會看到部分服務已終止並重新啟動。最後，您將會看到與工作手冊命名空間關聯的所有 Pod 都處於_執行中_狀態。請複製 _bobbys-helidon-stock-application_ 的 Pod 詳細資訊。
    
        <copy>kubectl get pods -n bobs-books</copy>
        
    
        $ kubectl get pods -n bobs-books
        NAME                                          READY STATUS    RESTARTS   AGE
        bobbys-coherence-0                             2/2  Running   0          7m51s
        bobbys-front-end-adminserver                   4/4  Running   0          5m28s
        bobbys-front-end-managed-server1               4/4  Running   0          4m30s
        bobbys-helidon-stock-application-5f74cbc-cw4x4 2/2  Running   0          7m54s
        bobs-bookstore-adminserver                     4/4  Running   0          4m31s
        bobs-bookstore-managed-server1                 4/4  Running   0          3m41s
        mysql-6bc8f9f785-n4qjh                         2/2  Running   0          5m52s
        robert-helidon-65b8874988-7x5vj                2/2  Running   0          7m53s
        robert-helidon-65b8874988-vnntp                2/2  Running   0          7m54s
        roberts-coherence-0                            2/2  Running   0          7m52s
        roberts-coherence-1                            2/2  Running   0          7m51s
        $
        
    
    > 請注意 **bobbys-helidon-stock-application** 的 Pod 名稱。重新部署此元件時，您會發現這個 Pod 將會進入_終止_ 狀態，而新的 Pod 將會啟動並處於實驗室 7 的_執行中_ 狀態。
    
    保持開啟 _Cloud Shell_ ，我們將在下個實驗室使用。
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月