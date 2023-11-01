# 在 Verrazzano 上部署 Tomcat 應用程式

## 簡介

此實驗室會逐步引導您完成部署 Tomcat 範例應用程式碼頭影像的程序。

預估時間：10 分鐘

### Verrazzano 和應用程式部署

Verrazzano 支援使用[開放應用程式模型 (OAM)](https://oam.dev/) 定義應用程式。Verrazzano 應用程式由元件和應用程式配置組成。

當您使用 Verrazzano 部署應用程式時，平台會建立連線和網路原則、服務網狀組織中的傳入，並佈線監控堆疊以擷取指標、日誌和追蹤。Verrazzano 使用 OAM 元件定義系統的功能單位，然後透過定義關聯的應用程式組態來組合和配置。

### Verrazzano 元件

Verrazzano OAM 元件是描述應用程式一般組合與環境需求的 [Kubernetes 自訂資源](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)。

下列程式碼顯示用於此實驗室之 Tomcat 範例應用程式的簡單 Tomcat 應用程式元件。此資源描述由包含公開單一端點之 Tomcat 應用程式的單一 Docker 映像檔實行的元件。

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: tomcat-component
    spec:
      workload:
        apiVersion: core.oam.dev/v1alpha2
        kind: ContainerizedWorkload
        metadata:
          name: tomcat-workload
          labels:
            app: tomcat
            version: v2
        spec:
          containers:
          - name: tomcat-container
            image: "ENDPOINT_OF_REGION/TENANCY_NAMESPACE/tomcat-example-your_firstname:v1"
            ports:
              - containerPort: 8080
                name: ecnj
    

元件每個欄位的簡短描述：

*   **apiVersion** - 元件自訂資源定義的版本
*   **kind** - 元件自訂資源定義的標準名稱
*   **metadata.name** - 用來建立元件自訂資源的名稱
*   **spec.workload.kind** - ContainerizedWorkload 定義了 Kubernetes 的無狀態工作負載
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - 容器公開的連接埠

### Verrazzano 應用程式組態

Verrazzano 應用程式組態為提供環境特定自訂項目的 Kubernetes 自訂資源。下列程式碼顯示此實驗室中使用之 Tomcat 應用程式範例的應用程式組態。此資源指定應用程式部署至 Tomcat-ns 命名空間。

其他的程式實際執行功能則是使用特性指定，或擴增工作負載的程式實際執行重疊。例如，輸入特徵會指定輸入主機和路徑，而測量結果特徵則提供用來取得應用程式相關測量結果的 Prometheus 刮痕。

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: tomcat-appconf
      annotations:
        version: v1.0.0
        description: "Verrazzano demo application"
    spec:
      components:
        - componentName: tomcat-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                  scraper: verrazzano-system/vmi-system-prometheus-0
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: tomcat-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
            - trait:
                apiVersion: core.oam.dev/v1alpha2
                kind: ManualScalerTrait
                spec:
                  replicaCount: 2
    

應用程式組態中每個欄位的簡短描述：

*   **apiVersion** - ApplicationConfiguration 自訂資源定義的版本
*   **kind** - 應用程式組態自訂資源定義的標準名稱
*   **metadata.name** - 用來建立此應用程式組態資源的名稱
*   **spec.components** - 參照用於指定執行時期配置的應用程式元件
*   **spec.components\[\].traits** - 為應用程式元件指定的特性

為了探索特徵，我們可以檢查傳入特徵的領域：

*   **apiVersion** - OAM 特徵自訂資源定義的版本
*   **kind** - IngressTrait 是 OAM 應用程式輸入特性自訂資源定義的名稱
*   **spec.rules.paths** - 存取應用程式的相關資訊環境路徑

### 目標

在此實驗室中，您將：

*   部署 Tomcat 範例應用程式。
*   驗證 Tomcat 範例應用程式的部署。

### 先決條件

若要執行此實驗室，您必須具備：

*   在 Oracle Cloud Infrastructure 上執行的 Kubernetes (OKE) 叢集。
*   Verrazzano 安裝已開始於 Kubernetes (OKE) 叢集上。
*   容器登錄中提供封裝的 _`tomcat-example-your_first_name`_ 應用程式。

## 作業 1：部署 Tomcat 範例應用程式

1.  為 Tomcat 範例應用程式建立 `tomcat-ns` 命名空間。我們會將所有 Kubernetes 工藝品保存在不同的命名空間中。
    
        <copy>kubectl create namespace tomcat-ns</copy>
        
    
    > 命名空間是一種將叢集組織成虛擬子叢集的方式。我們可以在叢集內擁有任何數目的命名空間，每個命名空間會以邏輯方式區隔，但能夠相互通訊。
    
2.  我們需要讓 Verrazzano 知道我們在該命名空間中儲存 Verrazzano 使用者自建物件。因此，我們需要新增一個可識別由 Verrazzano 管理之 `tomcat-ns` 命名空間的標籤。標籤用於指定有意義且與使用者相關之物件的識別屬性。
    
    對於 `tomcat-ns` 命名空間，我們附加了標籤，這會將這個命名空間標示為由 Verrazzano 管理。 _istio-injection=enabled_ 啟用 Istio "sidecar"，因此可協助建立 Istio 代理主機。藉由 Istio 代理主機，我們可以存取其他 Istio 服務，例如 Istio 閘道。若要使用先前提及的屬性將標籤新增至 `tomcat-ns` 命名空間，請複製下列命令並在 Cloud Shell 中執行：
    
        <copy>kubectl label namespace tomcat-ns verrazzano-managed=true istio-injection=enabled</copy>
        
3.  在 _~/tomcat-examples/_ 中，我們有 Tomcatsample 應用程式的組態檔。在實驗室 2 中，我們為應用程式元件建置新的 Docker 映像檔。此外，我們會將該 Docker 映像檔推送至 Oracle Cloud Container Registry 儲存區域。現在，在此實驗室中，我們將修改 _tomcat-comp.yaml_ 檔案，以便從 Oracle Cloud Container Registry 儲存區域使用新的 Docker 映像檔。若要修改 _tomcat-comp.yaml_ 檔案，請複製下列命令並將其貼到 _Cloud Shell_ 中。
    
        <copy>vi ~/tomcat-examples/tomcat-comp.yaml</copy>
        
4.  在實驗室 3 中，您已儲存 Docker 映像檔全名。您需要複製下列行並將它貼到文字編輯器中。接著，您必須將 `docker image full name` 取代為您的 Docker 映像檔名稱。然後複製修改過的行，然後按 _i_ 將文字插入 `*tomcat-comp.yaml*` 檔案中。在行號 **19** 處貼上輸出 (確定保持縮排)，然後按 _Esc_ ，然後鍵入 _：wq_ 以儲存檔案。
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![插入明細行](images/insert-line.png " ")
    
5.  現在，我們希望在 _cluster1_ 上部署 Tomcat 容器化應用程式。因此，我們需要 Kubernetes 部署組態。此部署會指示 Kubernetes 建立並更新 Tomcat 應用程式的執行處理。在這裡，我們有指示 Kubernetes 的 `tomcat-comp.yaml` 檔案。
    
    若要部署 Tomcat 範例應用程式，請複製並貼上下列兩個指令 (如圖所示)。`tomcat-comp.yaml` 檔案包含各種 OAM 元件的定義，其中 OAM 元件是描述應用程式一般組合與環境需求的 Kubernetes 自訂資源。
    
        <copy>kubectl apply -f ~/tomcat-examples/tomcat-comp.yaml -n tomcat-ns</copy>
        
    
    `tomcat-app.yaml` 檔案是 Verrazzano 應用程式組態檔，提供環境特定的自訂項目。
    
        <copy>kubectl apply -f ~/tomcat-examples/tomcat-app.yaml -n tomcat-ns</copy>
        
6.  等待 Pod 處於_執行中_狀態。使用此 _kubectl_ 指令，等待 _tomcat-ns_ 命名空間中所有 Pod 處於_執行中_ 狀態。約需 1-2 分鐘。
    
        <copy>kubectl wait --for=condition=Ready pods --all -n tomcat-ns --timeout=600s</copy>
        
    
    Pod 準備就緒時，您可以看到類似的回應：
    
        $ kubectl wait --for=condition=Ready pods --all -n tomcat-ns --timeout=600s
        pod/tomcat-workload-57d75d66d9-598lq condition met
        pod/tomcat-workload-57d75d66d9-b2m4l condition met
        
    
    您也可以直接列出 Pod 以檢查其狀態：
    
        <copy>kubectl  get po -n tomcat-ns</copy>
        
    
        $ kubectl  get po -n tomcat-ns
        NAME                               READY   STATUS    RESTARTS   AGE
        tomcat-workload-57d75d66d9-598lq   2/2     Running   0          57s
        tomcat-workload-57d75d66d9-b2m4l   2/2     Running   0          54s
        $
        

## 作業 2：確認 Tomcat 應用程式的成功部署

1.  檢查 `/sample-webapp/` 端點。若要判斷從外部 / 負載平衡器 IP 和應用程式組態所建構的 URL，請執行下列命令：
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io tomcat-ns-tomcat-appconf-gw -n tomcat-ns -o jsonpath={.spec.servers[0].hosts[0]})/sample-webapp/</copy>
        
    
    這會將正確的 URL 列印到您的 REST 端點，例如：
    
        https://tomcat-appconf.tomcat-ns.xx.xx.xx.xx.nip.io/sample-webapp/
        
2.  在瀏覽器中貼上 URL，然後按一下_按一下以呼叫 Servlet_ 。 ![應用程式 URL](images/application-url.png)
    
3.  您會看到_主機名稱 (Host Name)_ 與 _IP Address_ 。在另一個_無痕式視窗_中開啟相同的 URL，您將注意到此要求將由另一個伺服器執行處理提供。![主機名稱](images/host-name.png) ![顯示負載平衡](images/show-loadbalancing.png)
    

> 它顯示兩個複本之間的負載平衡。

您現在可以**進入下一個實驗室**。

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月