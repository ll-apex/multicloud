# 將傳入控制器部署到 Oracle Kubernetes 叢集 (OKE)

## 簡介

在此實驗室中，我們安裝 _Traefik_ 傳入控制器。之後，我們會更新_傳入路由_以存取應用程式和管理伺服器。

預估時間：10 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[將傳入控制器部署到 OKE 叢集](videohub:1_4kih00fi)

### 目標

在此實驗室中，您將：

*   在 Kubernetes 叢集上安裝傳入控制器。
*   更新傳入路由。

## 作業 1：將傳入控制器安裝到 Oracle Kubernetes 叢集

在這項任務中，我們會安裝_傳入控制器_。

1.  按一下_傳入控制器_。您可以看到一些預先填入的值，讓它保持不變並按一下_安裝傳入控制器_。 ![安裝傳入控制器](images/install-ingress-controller.png)
    
    > **僅供參考：**  
    > 這會將 _traefik-operator_ 輸入控制器順利安裝至 Kubernetes 命名空間 _traefik-ns_ 。
    
2.  看到_傳入控制器安裝完成_之後，請按一下_確定_。 ![已安裝傳入控制器](images/ingress-controller-installed.png)
    

## 作業 2：更新傳入路由以存取應用模組

在這項任務中，我們會新增「存取管理主控台」和「應用程式」的傳入路由。最後，我們會取得可存取的端點。

1.  向下捲動並按一下 _+_ icone 以新增_傳入路由組態_。 ![新增傳入路由](images/add-ingress-routes.png)
    
2.  如上所示按一下「編輯」圖示以修改值。 ![編輯匯入](images/edit-ingress.png)
    
3.  輸入下列明細，然後按一下_確定_。  
    名稱：主控台  
    路徑表示式：/console  
    目標服務命名空間：test-domain-ns  
    目標服務：test-domain-admin-server  
    目標連接埠：7001  
    
    ![主控台傳入](images/console-ingress.png)
    
4.  同樣地，新增下列 _opdemo_ 傳入路由：  
    名稱：opdemo  
    路徑表示式：/opdemo  
    目標服務命名空間：test-domain-ns  
    目標服務：test-domain-cluster-cluster-1  
    目標連接埠：8001  
    ![Opdemo 傳入](images/opdemo-ingress.png)
    
5.  同樣地，新增下列 _remote-console_ 內送路由：  
    名稱：remote-console  
    路徑表示式：/  
    目標服務命名空間：test-domain-ns  
    目標服務：test-domain-admin-server  
    目標連接埠：7001  
    ![遠端主控台傳入](images/remote-console-ingress.png)
    
6.  若要更新「傳入路由」，請按一下_更新傳入路由_。 ![更新傳入路由](images/update-ingress-routes.png)
    
7.  在_更新現有的傳入路由_中，按一下_是_。
    
8.  看到_傳入路由更新完成_視窗之後，按一下_確定_。 ![更新傳入完成](images/update-ingress-complete.png)
    
    > 您必須註記此 IP，並將它儲存在文字檔中。
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月