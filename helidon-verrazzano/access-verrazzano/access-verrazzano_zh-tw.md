# 使用 Verrazzano 存取及監控應用程式

## 簡介

您已部署 Helidon _quickstart-mp_ 應用程式。在此實驗室中，我們將存取應用系統，並確認使用 Verrazzano 提供的多種管理工具。

預估時間：15 分鐘

**Grafana**

Grafana 是一個開放原始碼的解決方案，可用來執行資料分析、提取有大量資料感的指標，以及透過好用的可自訂儀表板來監控我們的應用程式。此工具可協助研究、分析及監控一段時間的資料，技術上稱為時間序列分析。可用來追蹤使用者行為、應用程式行為、生產中彈出錯誤頻率或生產前環境、彈出錯誤類型，以及提供相對資料以情境情境情境情境情境。

[https://grafana.com/grafana/](https://grafana.com/grafana/)

**OpenSearch 資料面板**

OpenSearch 儀表板是 OpenSearch 叢集上索引之內容的視覺化儀表板。Verrazzano 建立 OpenSearch 儀表板部署，提供使用者介面以查詢和視覺化 OpenSearch 中收集的日誌資料。

若要存取 OpenSearch 儀表板主控台，請閱讀[存取 Verrazzano](https://verrazzano.io/latest/docs/access/) 。

若要透過 OpenSearch 儀表板查看 OpenSearch 索引的記錄，請建立索引樣式來篩選想要之索引下的記錄。

在任務 3 中，我們將探索 OpenSearch 儀表板。

[https://opensearch.org/docs/latest/dashboards/index/](https://opensearch.org/docs/latest/dashboards/index/)

**Prometheus**

Prometheus 是監控和警示工具組。Prometheus 會收集並儲存其評量標準作為時間序列資料，亦即指標資訊會與所記錄的時間戳記一併儲存，以及選擇性的索引鍵值組 (稱為標籤)。

[https://prometheus.io/](https://prometheus.io/)

**Rancher**

Rancher 是一個平台，可讓 Verrazzano 在生產環境中執行多個 Kubernetes 叢集上的容器。此服務實現混合式，這意謂著單一窗口管理企業內部部署叢集並在雲端服務上代管。

[https://rancher.com/](https://rancher.com/)

**卡利**

Kiali 是 Istio 的使用者介面，而且使用起來很簡單 。您可以從 Verrazzano 主控台直接存取 Kiali。您可以從該處查看流量以及任何熱點或問題區域。然後您可以向下探鑽以查看每個項目的詳細資料。Kiali 使用 Prometheus 中儲存的 Envoy 的度量資料來建立圖形模型

### 目標

在此實驗室中，您將：

*   探索 Rancher 主控台。
*   探索 Grafana 主控台。
*   探索 OpenSearch 儀表板。
*   探索 Prometheus 主控台。
*   探索 Rancher 主控台。
*   探索 Kiali 主控台 。

### 先決條件

*   在 Oracle Cloud Infrastructure 上執行的 Kubernetes (OKE) 叢集。
*   安裝在 Kubernetes (OKE) 叢集上的 Verrazzano。
*   已部署 Helidon _quickstart-mp_ 應用程式。

## 工作 1：探索 Rancher 主控台

Verrazzano 安裝數個主控台。安裝的端點會儲存在已安裝 Verrazzano 自訂資源的 `Status` 欄位中。

1.  您可以使用下列命令來取得這些主控台的端點：
    
        <copy>kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .</copy>
        
    
    執行結果應該和下列類似：
    
        $ kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .
        {
        "consoleUrl": "https://verrazzano.default.xx.xx.xx.xx.nip.io",
        "grafanaUrl": "https://grafana.vmi.system.default.xx.xx.xx.xx.nip.io",
        "keyCloakUrl": "https://keycloak.default.xx.xx.xx.xx.nip.io",
        "kialiUrl": "https://kiali.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchDashboardsUrl": "https://osd.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchUrl": "https://opensearch.vmi.system.default.xx.xx.xx.xx.nip.io",
        "prometheusUrl": "https://prometheus.vmi.system.default.xx.xx.xx.xx.nip.io",
        "rancherUrl": "https://rancher.default.xx.xx.xx.xx.nip.io"
        }
        $
        
2.  使用 `https://rancher.default.YOUR_UNIQUE_IP.nip.io` 開啟 Rancher 主控台。
    
3.  Verrazzano _dev_ 設定檔使用自行簽署的憑證，因此您需要按一下**進階**來接受風險並略過警告。
    
    ![進階](images/rancher-advanced.png)
    
4.  按一下**繼續執行 Rancher 預設 XX.XX.XX.XX.nip.io (不安全)** 。如果沒有這個選項可以進行的話，只要在瀏覽器視窗的任何地方輸入 _thisisunsafe_ 即可。您在 chrome 瀏覽器視窗中輸入後便看不到該視窗，但只要您輸入 _thisisunsafe_ 後，就可以立即看到下一頁。如需詳細資訊，請參閱[這個網頁](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)。
    
    ![繼續](images/rancher-proceed.png)
    
5.  按一下_使用 Keycloak 登入_。 ![Rancher 登入](images/keycloak-login.png)
    
6.  由於會重新導向至 Keycloak 主控台 URL 進行認證，請按一下**進階**。
    
    ![Keycloak 認證](images/keycloak-advanced.png)
    
7.  按一下**繼續進行 Keycloak 預設 XX.XX.XX.XX.nip.io (不安全)** 。如果沒有這個選項可以進行的話，只要在瀏覽器視窗的任何地方輸入 _thisisunsafe_ 即可。您在 chrome 瀏覽器視窗中輸入後便看不到該視窗，但只要您輸入 _thisisunsafe_ 後，就可以立即看到下一頁。如需詳細資訊，請參閱[這個網頁](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)。
    
    ![繼續](images/keycloak-proceed.png)
    
8.  現在，我們需要 Verrazzano 主控台的使用者名稱和密碼。_使用者名稱_是 _verrazzano_ ，若要找出密碼，請返回 _Cloud Shell_ ，然後貼上下列命令來尋找 _Rancher Console_ 的密碼。
    
        <copy>kubectl get secret --namespace verrazzano-system verrazzano -o jsonpath={.data.password} | base64 --decode; echo</copy>
        
9.  複製密碼並返回開啟 _Rancher Console_ 的瀏覽器。在_密碼_欄位中貼上密碼，輸入 _verrazzano_ 作為_使用者名稱_，然後按一下**登入**。
    
    ![SignIn](images/verrazzano-sign-in.png)
    
10.  您可以從 rancher 主控台的首頁檢視 Verrazzano 連結。您可以從 rancher console.Click _漢堡功能表_ -> _Verrazzano_ 存取這些主控台。
    
    ![Rancher Home](images/rancher-home.png)
    
11.  按一下_應用程式 (Applications)_ 。本節顯示所有應用程式及其命名空間，並由 Verrazzano 管理。按一下 _hello-helidon_ 名稱空間內的 _hello-helidon-appconf_ 應用程式。 ![Helidon 應用程式](images/helidon-application.png)
    
12.  您可以檢視與應用程式關聯的 Pod。Pod 名稱包含自動產生的唯一字串，可識別該特定複本。若要檢視 Pod 的日誌，請按一下 \[ _三個點_ \] -> \[ _檢視日誌_ \]。 ![Helidon 元件](images/view-pod-logs.png)
    
13.  您可以檢視應用程式的應用程式日誌。如果看不到應用程式日誌，請按一下**設定值** (有齒輪圖示的藍色按鈕) 並變更時間篩選，以顯示容器啟動的所有日誌項目。若要檢視與應用程式關聯的「元件」，請按一下_元件_。 ![應用程式日誌](images/view-pod-log.png)
    
14.  此應用程式只有一個 component.To 檢視相關的資源，按一下_相關資源_。![Helidon 資源](images/helidon-resource.png) ![Helidon 資源](images/helidon-resources.png)
    
15.  按一下 \[ _漢堡功能表_ \] -> \[ _本機_ \]，開啟 \[ _叢集總管_ \]。_叢集總管_可讓您從 Rancher UI 檢視和操控 Kubernetes 叢集中的所有自訂資源和 CRD。 ![Verrazzano 叢集](images/verrazzano-cluster.png)
    
16.  此儀表板提供叢集和已部署應用程式的總覽。資源數目屬於幾乎所有資源 (包括系統) 的_使用者命名空間_。您可以依儀表板頂端的命名空間進行篩選，但目前不需要這樣做。按一下左側功能表中的**節點**項目，以取得節點目前載入的概要。
    
    ![叢集總管](images/cluster-dashboard.png)
    
17.  整個部署對 OKE 叢集沒有任何影響。現在按一下左側功能表中的**部署**項目，以檢查已部署的應用程式。
    
    ![節點](images/cluster-nodes.png)
    
18.  您可以查看數個部署。
    
    ![建置](images/deployment.png)
    

## 作業 2：探索 Grafana 主控台

1.  按一下_漢堡功能表_ -> _首頁_。 ![住家](images/ranchar-menu.png)
    
2.  在首頁上，您將看到開啟 _Grafana 主控台_的連結。按一下 **Grafana 主控台** 的連結，如下所示：
    
    ![Grafana 本位目錄](images/grafana-link.png)
    
3.  按一下**進階**。
    
    ![進階](images/grafana-advanced.png)
    
4.  按一下**繼續進行 grafana.vmi.system.default.XX.XX.XX.XX.XX.nip.io (不安全)** 。如果沒有這個選項可以進行的話，只要在瀏覽器視窗的任何地方輸入 _thisisunsafe_ 即可。您在 chrome 瀏覽器視窗中輸入後便看不到該視窗，但只要您輸入 _thisisunsafe_ 後，就可以立即看到下一頁。如需詳細資訊，請參閱[這個網頁](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)。
    
    ![繼續](images/grafana-proceed.png)
    
5.  Grafana 首頁隨即開啟。按一下左上角的**首頁**。
    
    ![住家](images/grafana-home.png)
    
6.  輸入 `Helidon`，您將會在_一般_底下看到 **Helidon 監控儀表板**。按一下 **Helidon 監控儀表板**。
    
    ![Helidon 儀表板](images/search-helidon.png)
    
7.  注意 Helidon _quickstart-mp_ 應用程式的 JVM 詳細資訊，例如狀態、堆集使用狀況、執行時間、JVM 堆集、繫線計數、HTTP 要求等等。這是專為 Helidon 工作負載而預先建立的儀表板。當然，您可以根據需求自訂此儀表板，並新增自訂診斷資訊。
    
    ![儀表板](images/helidon-dashboard.png)
    

## 任務 3：探索 OpenSearch 儀表板

1.  回到 Verrazzano 首頁，然後按一下 **OpenSearch 儀表板** 主控台。
    
    ![Kibana 連結](images/opensearch-link.png)
    
2.  如有必要，請按一下_繼續進行 ... 預設 XX.XX.XX.XX.nip.io (不安全)_ 。第一次 _OpenSearch 儀表板_顯示歡迎頁面。它提供嘗試 OpenSearch 的內建範例資料，但您可以選取**自行瀏覽**選項，因為 Verrazzano 已完成必要組態，且應用程式資料已可供使用。
    
    ![Kibana 歡迎頁面](images/opensearch-proceed.png)
    
3.  在 OpenSearch 首頁中，按一下**首頁** -> **探索**。
    
    ![Kibana 儀表板點擊](images/discover-1.png)
    
4.  如上所示選取 _`verrazzano-application*`_ 命名空間，然後在篩選文字方塊中輸入您在 Helidon 應用程式 `help` 中建立的自訂日誌項目值。按 **Enter** 或按一下**重新整理**。您至少應該得到一個結果。
    
    > 如果您尚未命中應用程式端點，或是長期發生，只要在 Cloud Shell 中再次對端點呼叫下列 HTTP 要求即可。您可以多次執行要求。
    
        <copy>curl -k https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings; echo</copy>
        
    
    ![日誌結果](images/log-result.png)
    

## 作業 4：探索 Prometheus 主控台

1.  回到 Verrazzano 首頁，然後按一下 **Prometheus** 主控台。
    
    ![Prometheus 連結](images/prometheus-link.png)
    
2.  如果出現提示，請按一下**繼續 ... 預設 XX.XX.XX.XX.nip.io (不安全)** 。
    
3.  在 Prometheus 儀表板頁面上，在搜尋欄位中輸入 _help_ ，然後按一下您的自訂評量標準 _application \_me \_user \_mp \_quickstart \_GreetHelpResource \_helpCalled \_total_ 。
    
    ![Prometheus 執行](images/prometheus-query.png)
    
4.  按一下**執行 (Execute)** 並檢查下方結果。您應該會看到度量的目前值，這表示端點已經完成多少要求。您也可以切換至_圖表_檢視，而非_主控台_模式。
    
    ![Prometheus 值](images/execute-query.png) ![圖表檢視](images/graph-view.png)
    
    > 您也可以將其他分析指標新增至儀表板。尋找清單中可用的預設度量。
    

## 工作 5：探索 Kiali 主控台

1.  回到 Verrazzano 首頁，然後按一下 **Kiali** 主控台。
    
    ![Rancher 連結](images/kiali-link.png)
    
2.  如果出現提示，請按一下**繼續 ... 預設 XX.XX.XX.XX.nip.io (不安全)** 。
    
3.  在左側，按一下圖表。
    
    ![Kiali 儀表板](images/kiali-dashboard.png " ")
    
4.  在「命名空間 (Namespace)」下拉式清單中，勾選 _verrazzano-system_ 的方塊，然後將游標移到下拉式清單中。 ![Bobs 命名空間](images/verrazzano-namespace.png " ")
    
5.  您可以檢視 _verrazzano-system_ 名稱空間的圖形檢視。按一下_圖例_以檢視「圖例」檢視。
    
    ![圖形化檢視](images/graphical-view.png " ")
    
6.  您可以在此處檢視每個資源配置代表的，例如圓形代表_工作負載_。
    
    ![圖例檢視](images/legend-view.png " ")
    
7.  在左側，按一下_應用程式_以檢視已部署的應用程式。
    
    ![應用程式](images/applications.png " ")
    

## 作業 6：探索 Keycloak 主控台

1.  回到 Verrazzano 首頁，然後按一下 **Keycloak** 主控台。
    
    ![Keycloak 連結](images/keycloak-link.png)
    
2.  如果出現提示，請按一下**繼續 ... 預設 XX.XX.XX.XX.nip.io (不安全)** 。
    
3.  在「歡迎使用金鑰鎖定 (Welcome to Keycloak)」頁面上，按一下_管理主控台 (Administration Console)_ 。 ![Keycloak 家](images/keycloak-home.png)
    
4.  現在我們需要 Keycloak 主控台的使用者名稱與密碼。_使用者名稱_是 _keycloakadmin_ ，若要找出密碼，請返回 _Cloud Shell_ ，然後貼上下列命令來尋找 _Keycloak 主控台_的密碼。
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  複製密碼並返回開啟 _Keycloak 主控台_的瀏覽器。在_密碼_欄位中貼上密碼，輸入 _keycloakadmin_ 作為_使用者名稱_，然後按一下**登入**。
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  您可以在這裡檢視 Verrazzano 完成的預設配置。 ![Keycloak 首頁](images/keycloak-realms.png)
    

恭喜您已在 Verrazzano 實驗室上完成 Helidon 應用程式部署。

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月