# 在 WebLogic 伺服器版本中進行升級 (可選擇)

## 簡介

在此實驗室中，我們修改了主要映像檔，並搭配 _12.2.1.4-slim-ol8_ 標記使用 WebLogic 伺服器映像檔。然後，我們會使用 WebLogic Kubernetes Toolkit UI 重新部署網域。最後，我們會驗證新管理的伺服器 Pod 是否透過 WebLogic Remote Console 使用更新的 WebLogic 伺服器映像檔。

預估時間：10 分鐘

### 目標

在此實驗室中，您將：

*   使用 WebLogic 伺服器影像 (12.2.1.4) 作為主要影像。
*   重新部署 WebLogic 網域。

### 先決條件

*   存取實驗室 1 中建立的 noVNC 遠端桌面。

## 作業 1：輸入新 WebLogic 伺服器影像作為主要影像的詳細資訊

在這項任務中，我們將主要影像更新為使用已升級的 WebLogic Server 12.2.1.4.0 映像檔。

1.  回到 WebLogic Kubernetes Toolkit UI，按一下 _WebLogic 網域_。將 WebLogic 伺服器標記變更為 _12.2.1.4-slim-ol8_ 。 ![更新主要影像標記](images/update-primary-image-tag.png)

## 作業 2：輪流重新啟動伺服器 Pod 以更新建置的應用程式

在這項任務中，我們重新部署 WebLogic 網域。之後，請使用 WebLogic Remote Console 驗證伺服器 Pod 是否使用更新的 WebLogic Server 12.2.1.4.0 映像檔。

1.  按一下_部署網域_。這將會重新部署網域。 ![重新部署網域](images/redeploy-domain.png)
    
    > **僅供參考：**  
    > 當我們變更了主要映像檔時，我們將會注意到輪流重新啟動伺服器一次。當您按一下_部署網域_時，它會啟動一個終止執行中管理伺服器 Pod 的 _Introspector 工作_，並為使用 WebLogic Server 12.2.1.4.0 映像檔的管理伺服器建立新的 Pod。自我檢查程式在兩個受管理伺服器上都執行相同的程序。
    
2.  看到 _WebLogic 網域部署到 Kubernetes 完成_視窗之後，請按一下_確定_。 ![建置完成](images/deployment-complete.png)
    
3.  回到 _Terminal_ ，然後複製下方指令並貼到終端機中。您會注意到輪流重新啟動伺服器一次。首先，管理伺服器 Pod 會終止並處於_執行中_狀態。
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![檢視 Pod](images/view-pods.png)
    
4.  若要驗證「管理伺服器」和「受管理伺服器」Pod 使用更新的 WebLogic 伺服器影像，請按一下「監督樹狀結構」圖示，然後選取_環境_ -> _伺服器_ -> _admin-server_ 。您可以看到，它正在使用 12.2.1.4.0。 ![WLS 版本](images/wls-version.png)
    

Congratulation !!!

這是研討會結束。

我們希望您能找到這個研討會。

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 10 月