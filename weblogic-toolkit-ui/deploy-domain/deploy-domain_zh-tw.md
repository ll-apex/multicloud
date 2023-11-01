# 將 WebLogic 網域部署至 Oracle Kubernetes 叢集 (OKE)

## 簡介

在此實驗室中，我們將 WebLogic 網域部署到 kubernetes 叢集。在主要映像檔區段中，我們指定 oracle 帳戶證明資料。在輔助映像檔區段中，我們指定 oracle cloud 帳戶證明資料。我們也會指定叢集的複本。

預估時間：10 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[將 WebLogic 網域部署到 OKE 叢集](videohub:1_wz94de1l)

### 目標

在此實驗室中，您將：

*   將 WebLogic 網域部署到 Kubernetes 叢集。

## 作業 1：將 WebLogic 網域部署至 Oracle Kubernetes 叢集

在這項任務中，我們將 WebLogic 網域的 Kubernetes 自訂資源部署到 Kubernetes 叢集。

1.  向下捲動並在「主要映像檔」區段中輸入下列內容，以_映像檔提取加密密碼名稱_輸入 _domain-secret_ ，然後在_映像檔登錄提取使用者名稱_和_映像檔登錄提取密碼_中使用 Oracle 帳戶使用者名稱和密碼。在 _Image Registry 提取電子郵件地址_中輸入您的 Oracle 電子郵件 ID。這些證明資料與您用來接受 Oracle Container Registry 中 _weblogic_ 映像檔的授權相同。 ![主要影像明細](images/primary-image-details.png)
    
    > **僅供參考：**  
    > 我們從 Oracle Container Registry 提取映像檔，以便指定證明資料，用來接受 WebLogic 伺服器映像檔的授權合約。
    
2.  向下捲動至「輔助映像檔」區段，取消勾選**指定輔助映像檔提取證明資料**的方塊。
    
3.  在_叢集_區段中，按一下_編輯_圖示 (如圖所示)。 ![調整叢集大小](images/cluster-resize.png)
    
4.  輸入 _2_ 作為_複本_，然後按一下_確定_。成功將 WebLogic 網域部署至 Kubernetes 叢集之後，複本大小會決定_執行中_狀態的受管理伺服器數目。 ![叢集複本](images/cluster-replicas.png)
    
5.  在「資料來源」區段中，按兩下以編輯兩個資料來源的_密碼_。您可以同時在兩個資料來源中提供 _tiger_ 作為密碼。完成之後，請按一下_部署網域_。 ![資料來源密碼](images/datasource-password.png)
    
    > 這會將 WebLogic 網域測試網域部署到 Kubernetes 命名空間 _test-domain-ns_ 。
    
6.  看到 _WebLogic Domain Deployment to Kubernetes Complete_ 視窗之後，按一下_確定_。 ![建置完成](images/deployment-complete.png)
    
7.  回到終端機，按一下_活動 (Activities)_ ，然後選取_終端機 (Terminal)_ 視窗。複製下列指令並將其貼上終端機。您應該會看到類似的輸出，其中先針對「管理伺服器」執行自我檢查程式的 Pod，之後的受管理伺服器 Pod 會處於_執行中_狀態。
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Pod 狀態](images/pod-status.png)
    
8.  您也可以透過 _WebLogic Kubernetes Toolkit UI_ 取得網域狀態。回到 _WebLogic Kubernetes Toolkit UI_ ，然後按一下_取得網域狀態_。 ![網域狀態](images/domain-status.png)
    
9.  在「網域狀態」視窗中，向下捲動以查看所有伺服器 Pod 的狀態，然後按一下_確定_。 ![伺服器狀態](images/server-status.png)
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月