# 存取 Bobby 的書籍範例應用程式，探索 Verrazzano、Grafana 和 Kiali Console

## 簡介

在實驗室 3 中，我們部署了 Bobby 的 Book 應用程式。在此實驗室中，我們將存取應用程式並確認應用程式是否有效。之後，我們將探索 Verrazzano 和 Grafana 主控台。

預估時間：10 分鐘

### 目標

在此實驗室中，您將：

*   存取 Bobby 的 Book 應用程式。
*   探索 Verrazzano 主控台。
*   探索 Grafana 主控台。

### 先決條件

\* 執行實驗室 1，在 Oracle Cloud Infrastructure 上建立 OKE 叢集。 \* 執行實驗室 1，可設定 kubectl 存取 Oracle Cloud Infrastructure 上的 OKE 叢集。

*   執行實驗室 2，在 Kubernetes 叢集上安裝 Verrazzano。
*   執行實驗室 3，其部署 Bobby 的 Book 應用程式。
*   您應該有一個文字編輯器，您可以在其中根據您的環境貼上指令和 URL 並加以修改。接著您可以複製並貼上修改的指令，以便在 _Cloud Shell_ 中執行這些指令。
*   我們建議您使用 Firefox 開啟 Bobby 之書應用程式 Verrazano、Grafana 及 Kiali Console 的 URL。不過，如果您要使用 Chrome，則必須使用未記載的 'thisisunsafe' 解決方法以允許 chrome 接受憑證。

## 工作 1：存取 Bobby 的 Book 應用程式

1.  我們需要 `EXTERNAL_IP` 位址，才能存取 Bobby 的 Book 應用程式。若要取得 istio-ingressgateway 服務的 `EXTERNAL_IP` 位址，請複製下列命令並將其貼到 _Cloud Shell_ 中。
    
        <copy> kubectl get service \
        -n "istio-system" "istio-ingressgateway" \
        -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo</copy>
        
    
    執行結果應該和下列類似：
    
           $ kubectl get service \
           > -n "istio-system" "istio-ingressgateway" \
           > -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo
           XX.XX.XX.XX
        
2.  若要開啟 Robert 的 Book Store 首頁，請複製下列 URL 並將 _XX.XX.XX.XX_ 取代為我們在最後一個步驟中所得到的 _EXTERNAL\_IP_ 位址，如下列影像所示。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/</copy>
        
    
    ![Roberts 書本頁面](images/robertbooks.png " ")
    
3.  按一下_進階_，如下所示：
    
    ![Robert Advanced](images/robertadvanced.png " ")
    
4.  選取_繼續執行 bobs-books.bobs-books。EXTERNAL\_IP .nip.io (不安全)_ 可存取應用程式。如果沒有這個選項可以繼續，只要在瀏覽器視窗的任何地方輸入 _thisisunsafe_ 即可。您在 chrome 瀏覽器視窗中輸入後便看不到該視窗，但只要您輸入 _thisisunsafe_ 後，就可以立即看到應用程式頁面。如需詳細資訊，請參閱[這個網頁](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)。
    
    ![Robert Unsafe](images/robertunsafe.png " ") ![Robert Page](images/robertpage.png " ")
    
5.  若要開啟 Bobby 的 \[Book Store Home\] (書本商店首頁)，請開啟新頁籤並複製下列 URL，然後將 _XX.XX.XX.XX_ 取代為您的 `EXTERNAL_IP` 位址，如下列影像所示。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![Bobbys URL](images/bobbysurl.png " ")
    
    ![博比書店](images/bobbysbookstore.png " ")
    
    > 因為我們將在實驗室 8 中使用這個頁面，所以請將它保持開啟。
    
6.  若要開啟 Bobby 的 Book Order Manager UI，請開啟新頁標並複製下列 URL，然後以下列影像所示的 _EXTERNAL\_IP_ 地址來取代 _XX.XX.XX.XX_ 。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobs-bookstore-order-manager/orders</copy>
        
    
    ![訂單經理](images/ordermanager.png " ")
    
7.  返回 _Bobby 的書籍_頁面，然後購買一本書。按一下_工作簿_，如下圖所示。
    
    ![取出訂單](images/checkoutorder.png " ")
    
8.  選取 _Twilight_ 工作簿的影像，如下列影像所示。
    
    ![採購帳本](images/purchasebook.png " ")
    
9.  首先，按一下_新增至購物車_，然後按一下_結帳_，如下圖所示。
    
    ![按一下「新增購物車」](images/clickaddcart.png " ")
    
10.  輸入採購工作簿的詳細資料。針對_您的州別_，輸入兩位數的州別代碼，然後按一下_提交訂單_。
    

![提交訂單](images/submitorder.png " ") 11. 返回_訂單管理程式_頁面，然後選取_重新整理_按鈕，檢查訂單是否已順利記錄在訂單管理程式中。

![驗證訂單](images/verify-order.png " ")

## 工作 2：探索 Rancher 主控台

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
    
10.  您可以從 rancher 主控台的首頁檢視 Verrazzano 連結。按一下 \[ _漢堡功能表_ \] -> \[_Verrazzano_ \]。
    
    ![Rancher Home](images/rancher-home.png)
    
11.  按一下_應用程式 (Applications)_ 。本節顯示所有應用程式及其命名空間，並由 Verrazzano 管理。按一下 _bobs-books_ 命名空間中的 _bobs-books_ 應用程式。 ![Bobs 應用程式](images/bobs-application.png)
    
12.  您可以檢視與應用程式關聯的 Pod。Pod 名稱包含自動產生的唯一字串，可識別該特定複本。若要檢視 _bobbys-helidon-stok-application_ Pod 的日誌，請按一下_三個點_ -> _檢視日誌_。 ![Bobbys 日誌](images/bobs-logs.png)
    
13.  您可以檢視應用程式的應用程式日誌。如果看不到應用程式日誌，請按一下**設定值** (有齒輪圖示的藍色按鈕) 並變更時間篩選，以顯示容器啟動的所有日誌項目。若要檢視與應用程式關聯的「元件」，請按一下_元件_。 ![Bobbys 日誌](images/bobs-log.png)
    
14.  您可以在此處檢視 _bobs-books_ 應用程式的所有元件。若要檢視相關的資源，請按一下_相關資源_。![Bobs 資源](images/bobs-resource.png) ![Bobs 資源](images/bobs-resources.png)
    
15.  按一下 \[ _漢堡功能表_ \] -> \[ _本機_ \]，開啟 \[ _叢集總管_ \]。_叢集總管_可讓您從 Rancher UI 檢視和操控 Kubernetes 叢集中的所有自訂資源和 CRD。 ![Verrazzano 叢集](images/verrazzano-cluster.png)
    
16.  此儀表板提供叢集和已部署應用程式的總覽。資源數目屬於幾乎所有資源 (包括系統) 的_使用者命名空間_。您可以依儀表板頂端的命名空間進行篩選，但目前不需要這樣做。按一下左側功能表中的**節點**項目，以取得節點目前載入的概要。
    
    ![叢集總管](images/cluster-dashboard.png)
    
17.  整個部署對 OKE 叢集沒有任何影響。現在按一下左側功能表中的**部署**項目，以檢查已部署的應用程式。
    
    ![叢集節點](images/cluster-nodes.png)
    
18.  您可以查看數個部署。
    
    ![建置](images/cluster-deployments.png)
    

## 作業 3：探索 Grafana 主控台

1.  按一下 \[ _漢堡功能表_ \] -> \[ _首頁_ \] 以開啟 Rancher 首頁。 ![住家](images/ranchar-menu.png)
    
2.  在首頁上，您將看到開啟 _Grafana 主控台_的連結。按一下 **Grafana 主控台** 的連結，如下所示：
    
    ![Grafana 本位目錄](images/grafana-link.png)
    
3.  按一下_進階_。
    
    ![Grafana 進階版](images/grafanaadvanced.png " ")
    
4.  選取 _grafana.vmi.system.default.XX.XX.XX.XX.nip.io (unsafe)_ 。如果未能使用此選項繼續，請直接鍵入不含任何空間的 _thisisunsafe_ 。
    
    ![Grafana 繼續](images/grafanaproceed.png " ")
    
5.  Grafana 首頁便會開啟。選取左上方的_首頁_。
    
    ![Grafana 首頁](images/grafana-homepage.png " ")
    
6.  鍵入 _WebLogic_ ，您將會在 _General_ 下看到 _WebLogic 伺服器儀表板_。選取 _WebLogic 伺服器儀表板_。
    
    ！\[ 搜尋 WebLogic\] (images/search-weblogic.png" ")
    
    您可以在此處觀察_網域_和執行中伺服器、部署的應用程式、伺服器名稱及其狀態、堆記憶體使用狀況、執行時間、JVM 堆集下的兩個網域。如果您的應用程式有 JDBC 和 JMS 等資源，您也可以在此取得其相關詳細資訊。
    
    ![WebLogic 儀表板](images/weblogic-dashboard.png " ")
    
7.  現在，選取 WebLogic 伺服器儀表板並輸入 _Helidon_ ，您將看到 _Helidon 監控儀表板_。選取 _Helidon 監控儀表板_。
    
    ![Helidon](images/search-helidon.png " ")
    
    您可以在此處查看各種詳細資訊，例如應用程式的_狀態_及其_正常運作時間_、資源回收器，以及標記清理總計及其時間、繫線計數。
    
    ![Helidon 儀表板](images/helidon-dashboard.png " ")
    
8.  現在，選取「Helidon 監督儀表板」並輸入 _Coherence_ ，您將看到 _Coherence 儀表板主要_。選取 _Coherence 儀表板主要_。
    
    ![Coherence](images/search-coherence.png " ")
    
9.  您可以在此處查看 _Coherence 叢集_的詳細資訊。對於 Bobby 的 Books 應用程式，有兩個 Coherence 叢集，一個是 Bobby 的 Books，另一個是 Robert 的 Books。您必須選取_叢集名稱_的下拉式功能表，才能檢視這兩個叢集。
    
    ![Bobby Cluster](images/bobby-cluster.png " ")
    
    ![Robert Cluster](images/robert-cluster.png " ")
    

## 作業 4：探索 Kiali 主控台

1.  回到 Verrazzano 首頁，然後按一下 **Kiali** 主控台。
    
    ![Rancher 連結](images/kiali-link.png)
    
2.  在左側，按一下圖表。
    
    ![Kiali 儀表板](images/kiali-dashboard.png " ")
    
3.  在「命名空間」下拉式清單中，勾選_工作簿_的方塊，然後將游標移到下拉式清單中。 ![Bobs 命名空間](images/bobs-namespace.png " ")
    
4.  您可以檢視 _bobs-books_ 應用程式的圖形檢視。按一下_圖例_以檢視「圖例」檢視。
    
    ![圖形化檢視](images/graphical-view.png " ")
    
5.  您可以在此處檢視每個資源配置代表的，例如圓形代表_工作負載_。
    
    ![圖例檢視](images/legend-view.png " ")
    
6.  在左側，按一下_應用程式_以檢視所有已部署的應用程式。
    
    ![應用程式](images/kiali-applications.png " ")
    

## 作業 5：探索 OpenSearch 儀表板

1.  回到 Verrazzano 首頁，然後按一下 **OpenSearch 儀表板** 主控台。
    
    ![Kibana 連結](images/opensearch-link.png)
    
2.  如有必要，請按一下_繼續進行 ... 預設 XX.XX.XX.XX.nip.io (不安全)_ 。
    
3.  在 OpenSearch 首頁中，按一下**首頁** -> **探索**。
    
    ![Kibana 儀表板點擊](images/discover-1.png)
    
4.  如上所示選取 _`verrazzano-applications*`_ 命名空間，然後搜尋 _books_ 並按 **Enter** 鍵或按一下**重新整理**。您應該會收到包含_工作簿_的日誌。 ![日誌結果](images/search-books.png)
    

## 任務 6：探索 Prometheus 主控台

1.  回到 Verrazzano 首頁，然後按一下 **Prometheus** 主控台。
    
    ![Prometheus 連結](images/prometheus-link.png)
    
2.  如果出現提示，請按一下**繼續 ... 預設 XX.XX.XX.XX.nip.io (不安全)** 。
    
3.  在 Prometheus 儀表板頁面上，_取得_ 類型至搜尋欄位，然後按一下您的自訂評量標準 _`application_org_books_bobby_BookResource_getBook_total`_ 。
    
    ![Prometheus 執行](images/prometheus-query.png)
    
4.  按一下**執行 (Execute)** 並檢查下方結果。您應該會看到度量的目前值，這表示端點已經完成多少要求。您也可以切換至_圖表_檢視，而非_主控台_模式。
    
    ![Prometheus 值](images/execute-query.png)
    
    > 您也可以將其他分析指標新增至儀表板。尋找清單中可用的預設度量。
    

## 作業 7：探索 Keycloak 主控台

1.  回到 Verrazzano 首頁，然後按一下 **Keycloak** 主控台。
    
    ![Keycloak 連結](images/keycloak-link.png)
    
2.  如果出現提示，請按一下**繼續 ... 預設 XX.XX.XX.XX.nip.io (不安全)** 。
    
3.  在「歡迎使用金鑰鎖定 (Welcome to Keycloak)」頁面上，按一下_管理主控台 (Administration Console)_ 。 ![Keycloak 家](images/keycloak-home.png)
    
4.  現在我們需要 Keycloak 主控台的使用者名稱與密碼。_使用者名稱_是 _keycloakadmin_ ，若要找出密碼，請返回 _Cloud Shell_ ，然後貼上下列命令來尋找 _Keycloak 主控台_的密碼。
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  複製密碼並返回開啟 _Keycloak 主控台_的瀏覽器。在_密碼_欄位中貼上密碼，輸入 _keycloakadmin_ 作為_使用者名稱_，然後按一下**登入**。
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  您可以在這裡檢視 Verrazzano 完成的預設配置。 ![Keycloak 首頁](images/keycloak-realms.png)
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月