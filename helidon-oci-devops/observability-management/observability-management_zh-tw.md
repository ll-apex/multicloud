# 探索日誌和度量總管

## 簡介

在此實驗室中，您將驗證 Helidon 應用程式的成功部署。此外，您也將探索日誌和度量總管服務。

估計時間：5 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[探索日誌和度量總管](videohub:1_7a0qaaif)

### 目標

在此實驗室中，您將：

*   驗證成功部署 Helidon 應用程式。
*   探索 OCI 度量總管
*   探索 OCI 日誌記錄服務
*   驗證 Helidon 應用程式的效能和就緒度

### 先決條件

*   一個 Oracle Free Tier (Trial)、Paid 或 LiveLabs 雲端帳戶
*   熟悉 OCI 主控台

## 任務 1：存取 Helidon 應用程式

在這項任務中，我們將使用 curl 來存取應用程式以執行 GET 和 PUT HTTP 要求。

1.  回到**程式碼編輯器**，在終端機中複製並貼上下列命令，以將部署節點 **`PUBLIC_IP`** 設定為環境變數。
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  複製並貼上下列命令來放置 **GET 要求**。您會有如下所示的執行結果。
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  嗨，請命名為 **Joe** 。您會有如下所示的執行結果。
    
        <copy>curl http://$PUBLIC_IP:8080/greet/Joe</copy>
        {"message":"Hello Joe!","date":[2023,5,23]}
        
4.  將 Hello 取代為另一個問候語，例如 **Hola** 。您會有如下所示的執行結果。
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,23]}
        

## 作業 2：探索 OCI 度量總管

在這項任務中，您將驗證 **Helidon 指標**是否已推送至 **OCI 監控服務**。

1.  在雲端主控台中，按一下_漢堡功能表_ -> _可觀測性與管理_ -> _監督_底下的**度量總管**，如下所示。 ![度量總管](images/metrics-explorer.png)
    
2.  向下捲動至**查詢**區段，選取正確的區間，然後選取 **`helidon_metrics`** 作為度量命名空間，並選取 **`request.count_counter`** 作為度量名稱以建立查詢。然後按一下_更新圖表_，如下所示。 ![建立查詢](images/create-query.png)
    
3.  向上捲動，您會看到如下所示的類似輸出。這會提供您要求計數器之圖形格式的資料。 ![查詢編輯器](images/query-editor.png)
    
4.  若要顯示「資料表」中的資料，請切換**顯示資料表格**的按鈕，如下所示。當您切換按鈕時，_顯示資料表格_會取代為**顯示圖形**。 ![資料表格](images/data-table.png)
    

## 作業 3：探索 OCI 日誌記錄服務

在這項任務中，我們驗證 **OCI 記錄 SDK** 整合，將訊息推送至記錄日誌服務，方法是探索日誌群組 **app-log-group-helidon-ocw-hol** 上的日誌 **app-log-helidon-ocw-hol** 。

1.  在雲端主控台中，按一下_漢堡功能表_ -> _可觀測性與管理_ -> _日誌_。 ![日誌功能表](images/logs-menu.png)
    
2.  選取正確的區間，然後選取 _`app-log-group-helidon-ocw-hol`_ 作為**日誌群組**，然後按一下 **app-log-helidon-ocw-hol** ，如下所示。 ![選取區間](images/select-compartment.png)
    
3.  在**依時間篩選**中，從下拉式清單中選取**今天**，您可以查看應用程式日誌。 ![檢視日誌](images/view-logs.png)
    

## 作業 4：Helidon 應用程式的狀況檢查，以驗證效能和準備狀況

Helidon **oci-mp** 應用程式新增了**狀況檢查 (Health Check)** 功能以驗證**完整性**和 (或) **新功能**。您可以分別檢查專案中的 _GreetLivenessCheck_ 和 _GreetReadinessCheck_ 類別檔案，瞭解它們的執行方式。這在協調器環境 (例如 Kubernetes) 上執行應用程式作為微服務時特別有用，以判斷微服務如果不健康，是否需要重新啟動。此實驗室特有，應用程式啟動後，會在 DevOps 部署管線規格中使用整備度檢查_以判斷 Helidon 應用程式是否順利啟動_。請至 _deployment\_spec.yaml_ 中的 **line #79** 查看代碼以了解其運作方式。

1.  回到**程式碼編輯器**，在終端機中複製並貼上下列命令，以將部署節點 **`PUBLIC_IP`** 設定為環境變數。
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  複製並貼上下列命令以檢查**完整性**。您會有如下所示的執行結果。
    
        <copy>curl http://$PUBLIC_IP:8080/health/live</copy>
        {"status":"UP","checks":[{"name":"CustomLivenessCheck","status":"UP","data":{"time":1684391639448}}]}
        
3.  複製並貼上下列命令以檢查**新功能**。您會有如下所示的執行結果。
    
        <copy>curl http://$PUBLIC_IP:8080/health/ready</copy>
        {"status":"UP","checks":[{"name":"CustomReadinessCheck","status":"UP","data":{"time":1684391438298}}]}
        

您現在可以**繼續進行下一個實驗室。**

## 進一步瞭解

*   [OCI 應用程式與 Helidon](https://medium.com/helidon/oci-application-with-helidon-caa78cacaee5)
*   [Helidon MP 度量](https://helidon.io/docs/v3/#/mp/metrics/metrics)
*   [Helidon MP 指標指南](https://helidon.io/docs/v3/#/mp/guides/metrics)
*   [Helidon MP Health](https://helidon.io/docs/v3/#/mp/health)
*   [Helidon MP 健康檢查指南](https://helidon.io/docs/v3/#/mp/guides/health)

## 確認

*   **作者** - Keith Lustria
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月