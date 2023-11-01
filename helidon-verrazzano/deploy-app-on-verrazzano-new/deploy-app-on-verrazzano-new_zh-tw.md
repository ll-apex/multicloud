# 在 Verrazzano 上部署 Helidon 應用程式

## 簡介

本實驗室會逐步引導您完成部署 Hello Helidon 應用程式的程序。

預估時間：10 分鐘

### Verrazzano 和應用程式部署

Verrazzano 支援使用[開放應用程式模型 (OAM)](https://oam.dev/) 定義應用程式。Verrazzano 應用程式由元件和應用程式配置組成。

當您使用 Verrazzano 部署應用程式時，Verrazzano 會執行智慧型工作負載管理。此平台會設定連線和網路原則、服務網狀組織中的輸入，然後佈線監控堆疊以擷取度量、日誌及追蹤。Verrazzano 使用 OAM 元件定義系統的功能單位，然後透過定義關聯的應用程式組態來組合和配置。

![Verrazzano，韋拉茲諾](images/Verrazzano_IntelligentWorkload.png)

### 目標

在此實驗室中，您將：

*   驗證成功安裝 Verrazzano 環境。
*   部署 Hello Helidon 應用程式。
*   驗證 Hello Helidon 應用程式的部署。

### 先決條件

若要執行此實驗室，您必須具備：

*   在 Oracle Cloud Infrastructure 上執行的 Kubernetes (OKE) 叢集。
*   Verrazzano 安裝已開始於 Kubernetes (OKE) 叢集上。

## 工作 1：驗證成功的 Verrazzano 安裝

Verrazzano 在多個命名空間中安裝多個物件。Verrazzano 元件已安裝在命名空間 _verrazzano-system_ 中。

1.  請確認與多個物件關聯的所有 Pod 都處於_執行中_狀態。
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    執行結果應該和下列類似：
    
        $   kubectl get pods -n verrazzano-system
        NAME                                         READY   STATUS RESTARTS AGE
        coherence-operator-b5dc669c6-zm4dw           1/1     Running   1     153m
        fluentd-5s7hh                                2/2     Running   1     144m
        fluentd-bt4t4                                2/2     Running   1     144m
        fluentd-ghmkx                                2/2     Running   1     144m
        oam-kubernetes-runtime-5b48f944b-9bv2v       1/1     Running   0     154m
        verrazzano-application-operator-f976cf96f    1/1     Running   0     152m
        verrazzano-application-operator-webhook      1/1     Running   0     152m
        verrazzano-authproxy-58f8975c5d-jhx72        3/3     Running   0     151m
        verrazzano-cluster-operator-544d494f49       1/1     Running   0     144m
        verrazzano-cluster-operator-webhook-79       1/1     Running   0     144m
        verrazzano-console-6f59f7485d-xrdt8          2/2     Running   0     147m
        verrazzano-monitoring-operator-d88cdd66c     2/2     Running   0     151m
        vmi-system-es-master-0                       2/2     Running   0     151m
        vmi-system-grafana-78cd975dd9-vfjv5          3/3     Running   0     151m
        vmi-system-kiali-57d859dc4b-bnvjh            2/2     Running   0     151m
        vmi-system-osd-7687d6fccf-cjwqr              2/2     Running   0     147m
        weblogic-operator-54979449f4-t9q26           2/2     Running   0     152m
        weblogic-operator-webhook-f7ff8c8cf-nc4r     1/1     Running   0     152m
        

## 任務 2：部署 Hello Helidon 應用程式

1.  為 Helidon quickstart-mp 應用程式建立 `hello-helidon` 命名空間。我們會將所有 Kubernetes 工藝品保存在不同的命名空間中。
    
        <copy>kubectl create namespace hello-helidon</copy>
        
    
    > 命名空間是一種將叢集組織成虛擬子叢集的方式。我們可以在叢集內擁有任何數目的命名空間，每個命名空間會以邏輯方式區隔，但能夠相互通訊。
    
2.  我們需要讓 Verrazzano 知道我們在該命名空間中儲存 Verrazzano 使用者自建物件。因此，我們需要新增一個可識別由 Verrazzano 管理之 `hello-helidon` 命名空間的標籤。標籤用於指定有意義且與使用者相關之物件的識別屬性。
    
    對於 `hello-helidon` 命名空間，我們附加了標籤，這會將這個命名空間標示為由 Verrazzano 管理。 _istio-injection=enabled_ 啟用 Istio "sidecar"，因此可協助建立 Istio 代理主機。藉由 Istio 代理主機，我們可以存取其他 Istio 服務，例如 Istio 閘道。若要使用先前提及的屬性將標籤新增至 `hello-helidon` 命名空間，請複製下列命令並在 Cloud Shell 中執行：
    
        <copy>kubectl label namespace hello-helidon verrazzano-managed=true istio-injection=enabled</copy>
        
3.  現在，我們想要在 _cluster1_ 上部署 Hello Helidon 容器化應用程式。因此，我們需要 Kubernetes 部署組態。此部署會指示 Kubernetes 建立和更新 Hello Helidon 應用程式的執行處理。在這裡，我們有指示 Kubernetes 的 `hello-helidon-comp.yaml` 檔案。
    
    若要部署 Hello Helidon 應用程式，請複製並貼上下列兩個命令 (如圖所示)。`hello-helidon-comp.yaml` 檔案包含各種 OAM 元件的定義，其中 OAM 元件是描述應用程式一般組合與環境需求的 Kubernetes 自訂資源。
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/verrazzano/verrazzano/v1.5.2/examples/hello-helidon/hello-helidon-comp.yaml -n hello-helidon</copy>
        
    
    `hello-helidon-app.yaml` 檔案是 Verrazzano 應用程式組態檔，提供環境特定的自訂項目。
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/verrazzano/verrazzano/v1.5.2/examples/hello-helidon/hello-helidon-app.yaml -n hello-helidon</copy>
        
    
    > 使用 verrazzano 部署應用程式的動作會執行下列智慧型工作負載管理工作。
    
    *   視需要建立服務帳戶和授權原則
    *   建置應用程式
    *   視需要設定閘道與虛擬服務。
    *   依據 Istio 的要求設定憑證、加密密碼、網路原則。
    *   自動化可觀測性管理啟動
        *   報廢日誌至 OpenSearch
        *   將所有監控資訊提取至 Prometheus 和 Grafana
4.  等待 Pod 處於_執行中_狀態。使用此 _kubectl_ 指令，等待所有 Pod 處於 hello-helidon 名稱空間內的_執行中_ 狀態。約需 1-2 分鐘。
    
        <copy>kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s</copy>
        
    
    Pod 準備就緒時，您可以看到類似的回應：
    
        $ kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s
        pod/hello-helidon-deployment-58fdd5cd4-94wjf condition met
        
    
    您也可以直接列出 Pod 以檢查其狀態：
    
        $ kubectl  get po -n hello-helidon
        NAME                                       READY   STATUS    RESTARTS   AGE
        hello-helidon-deployment-58fdd5cd4-94wjf   2/2     Running   0          34m
        

## 作業 3：驗證 Hello Helidon 應用程式的成功部署

1.  檢查 `/greet` 端點。若要判斷從外部 / 負載平衡器 IP 和應用程式組態所建構的 URL，請執行下列命令：
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io hello-helidon-hello-helidon-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/greet</copy>
        
    
    這會將正確的 URL 列印到您的 REST 端點，例如：
    
        https://hello-helidon.hello-helidon.xx.xx.xx.xx.nip.io/greet
        
2.  使用此連結從您的瀏覽器進行測試。不過，由於自行簽署的憑證，您必須接受風險並允許瀏覽器繼續處理要求。
    
    因為回應只是字串，您可能會發現使用 `curl` 更容易：
    
        <copy>curl -k https://$(kubectl get gateways.networking.istio.io hello-helidon-hello-helidon-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/greet; echo</copy>
        
    
    您應該會看到在開發期間所收到的相同結果：
    
        {"message":"Hello World!"}
        
3.  只要開啟 _Cloud Shell_ ，我們就會將其用於下一個實驗室。
    

## 瞭解有關開啟應用程式模型的詳細資訊

**Verrazzano 元件**

Verrazzano OAM 元件是描述應用程式一般組合與環境需求的 [Kubernetes 自訂資源](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)。

下列程式碼顯示此實驗室中使用的 Hello Helidon 應用程式簡單 Helidon 應用程式元件。此資源描述由包含顯示單一端點之 Helidon 應用程式的單一 Docker 映像檔實行的元件。

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: hello-helidon-component
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: hello-helidon-workload
          labels:
            app: hello-helidon
            version: v1
        spec:
          deploymentTemplate:
            metadata:
              name: hello-helidon-deployment
            podSpec:
              containers:
                - name: hello-helidon-container
                  image: "ghcr.io/verrazzano/example-helidon-greet-app-v1:1.0.0-1-20230126194830-31cd41f"
                  ports:
                    - containerPort: 8080
                      name: http
    

元件每個欄位的簡短描述：

*   **apiVersion** - 元件自訂資源定義的版本
*   **kind** - 元件自訂資源定義的標準名稱
*   **metadata.name** - 用來建立元件自訂資源的名稱
*   **metadata.namespace** - 用來建立此元件之自訂資源的命名空間
*   **spec.workload.kind** - VerrazzanoHelidonWorkload 定義了 Kubernetes 的無狀態工作負載
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name** - 用來建立 Kubernetes 無狀態工作負載的名稱
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - 實作容器
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - 容器公開的連接埠

**Verrazzano 應用程式組態**

Verrazzano 應用程式組態為提供環境特定自訂項目的 Kubernetes 自訂資源。下列程式碼顯示此實驗室中所使用 Helidon _quickstart-mp_ 範例的應用程式組態。此資源指定將應用程式部署到 hello-helidon 名稱空間。

其他的程式實際執行功能則是使用特性指定，或擴增工作負載的程式實際執行重疊。例如，輸入特徵會指定輸入主機和路徑，而測量結果特徵則提供用來取得應用程式相關測量結果的 Prometheus 刮痕。

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: hello-helidon
      annotations:
        version: v1.0.0
        description: "Hello Helidon application"
    spec:
      components:
        - componentName: hello-helidon-component
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
                  name: hello-helidon-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/greet"
                          pathType: Prefix
    

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

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月