# 測試應用程式建置

## 簡介

### 關於 WebLogic 遠端主控台

WebLogic 遠端主控台是輕量型的開源主控台，可用來管理您的 WebLogic 伺服器網域，例如在實體或虛擬機器、容器、Kubernetes 或 Oracle Cloud 中執行。WebLogic 遠端主控台不需要與 WebLogic 伺服器網域共置。

您可以隨處安裝並執行 WebLogic Remote Console，並使用 WebLogic REST API 連線至您的網域。您只要啟動桌面應用程式並連線至網域的管理伺服器即可。或者，您可以啟動主控台伺服器、在瀏覽器中啟動主控台，然後連線至「管理伺服器」。

WebLogic Server 12.2.1.3、12.2.1.4 和 14.1.1.0 完全支援 WebLogic Remote Console。

**WebLogic 遠端主控台的主要功能**

*   設定 WebLogic 伺服器執行處理和叢集
*   建立或修改 WDT 描述資料模型
*   設定 WebLogic 伺服器服務，如資料庫連線 (JDBC) 和訊息 (JMS)
*   部署及取消部署應用程式
*   啟動和停止伺服器和應用程式
*   監督伺服器和應用程式效能

在此實驗室中，我們存取應用系統 _opdemo_ ，並確認成功移轉離線企業內部部署網域。我們也驗證受管理伺服器 Pod 之間的負載平衡。之後，我們使用 WebLogic Remote Console 驗證在 kubernetes 環境中成功部署測試網域資源。

預估時間：10 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[測試應用程式部署](videohub:1_1khcsrbq)

### 目標

在此實驗室中，您將：

*   透過瀏覽器存取應用程式。
*   使用 WebLogic Remote Console 探索 WebLogic 網域。

## 作業 1：透過瀏覽器存取應用程式

在這項任務中，我們存取了 _opdemo_ 應用程式。我們會按一下重新整理圖示，對應用程式提出多個要求，驗證兩個受管理伺服器 Pod 之間的負載平衡。

1.  複製下方的 URL，並將 _XX.XX.XX.XX_ 取代為您的 IP，您在最後一個實驗室中註記下來。您可以看到下方的輸出。
    
        <copy>http://XX.XX.XX.XX/opdemo/?dsname=testDatasource</copy>
        
    
    ![開啟應用程式](images/open-application.png)
    
2.  如果您按一下「重新整理」圖示，即可看到兩個受管理伺服器 Pod 之間的負載平衡。 ![顯示負載平衡](images/show-load-balancing.png)
    

## 作業 2：使用 WebLogic 遠端主控台探索 Kubernetes 叢集上的 WebLogic 網域

在這項任務中，我們將探索 WebLogic 遠端主控台。我們會在「遠端主控台」中建立 _Admin Server_ 連線，並驗證 WebLogic 網域中的資源。這樣可以驗證企業內部部署網域成功移轉至 Oracle Kubernetes 叢集。

1.  若要開啟 WebLogic 遠端主控台，請按一下_活動_，在搜尋方塊中輸入 _WebLogic_ ，然後按一下 _WebLogic 遠端主控台_圖示。
    
2.  按一下 _Kiosk_ 底下的 `Three dots`，然後選取_新增管理伺服器連線提供者_，然後按一下_選擇_。 ![管理伺服器連線](images/adminserver-connection.png)
    
3.  輸入下列資料，然後按一下_確定_。  
    連線提供者名稱：AdminServer  
    使用者名稱：weblogic  
    密碼：welcome1  
    URL：`Copy_IP_From_TextFile`  
    ![連線詳細資訊](images/connection-details.png)
    
4.  按一下_編輯樹狀結構_圖示，然後選取_服務_ -> _資料來源_。您可以觀察到與企業內部部署網域相同的資料來源。 ![驗證資料來源](images/verify-datasources.png)
    
5.  顯示網域中執行的伺服器。按一下顯示的**監督樹狀結構**圖示，然後選取**環境** -> **伺服器**。您可以看到**管理伺服器**和 2 個受管理伺服器 Pod 正在執行中。 ![執行中伺服器](images/running-server-status.png)
    
6.  按一下 **admin-server** ，您會看到 WebLogic 版本為 **12.2.1.3.0** 。 ![伺服器狀態](images/wls-version.png)
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月