# 擴充 WebLogic 叢集 (選擇性)

## 簡介

在此實驗室中，我們調整 WebLogic 叢集。在此，我們會將值修改為 _3_ ，然後重新部署網域。

預估時間：5 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[調整 WebLogic 叢集大小](videohub:1_mcl3p6td)

### 目標

在此實驗室中，您將：

*   調整 WebLogic 叢集。

## 作業 1：使用 WebLogic Kubernetes Toolkit UI 擴展 WebLogic 叢集

在這項任務中，您只需要將 _Replica_ 值從 2 修改為 3，然後再次重新部署網域。

1.  回到 WebLogic Kubernetes Toolkit UI，按一下 _WebLogic 網域_。前往_叢集_區段，然後按一下_編輯_圖示。  
    ![調整叢集大小](images/cluster-resize.png)
    
2.  將複本從 _2_ 變更為 _3_ ，然後按一下_確定_。 ![變更複本](images/change-replicas.png)
    
3.  若要重新部署網域，請按一下_部署網域_。 ![重新部署網域](images/redeploy-domain.png)
    
4.  看到 _WebLogic 網域部署到 Kubernetes 完成_視窗之後，請按一下_確定_。 ![建置完成](images/deployment-complete.png)
    
5.  回到_終端機_視窗，按一下_活動_，然後選取_終端機_視窗。複製下列指令並將其貼入終端機。
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![檢視縮放比例](images/view-scaling.png)
    
    > 您可以查看並重新部署網域，啟動自我檢查程式工作，啟動建立用於測試網域管理之 Pod 的處理作業 -server3，此 Pod 可進入_執行中_狀態。
    
6.  回到開啟應用程式頁面的瀏覽器。按一下「重新整理」按鈕，您將立即看到三部受管理伺服器之間的負載平衡。 ![新伺服器](images/new-server.png)
    
7.  返回 WebLogic Remote Console，按一下_監督樹狀結構_ -> _環境_ -> _伺服器_。您也會注意到管理：server3。 ![遠程控制台](images/remote-console.png)
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月