# 將 bobbys-helidon-stock-application 的 Docker 映像檔推送至 Oracle Cloud Container Registry

## 簡介

在實驗室 5 中，我們修改了 bobbys-helidon-stock-application，並建立新的 Docker 映像檔。在此實驗室中，我們會將該映像檔推送至 Oracle Cloud Container Registry 中的儲存區域。

預估時間：10 分鐘

### 目標

在此實驗室中，您將：

*   產生認證權杖以登入 Oracle Cloud Container Registry。
*   將 bobbys-helidon-stock-application Docker 映像檔推送至您的 Oracle Cloud Container Registry 儲存區域。

### 先決條件

您應該有一個文字編輯器，您可以在其中根據您的環境貼上指令和 URL 並加以修改。接著您可以複製並貼上修改的命令，在 _Cloud Shell_ 中執行這些命令。

## 作業 1：產生登入 Oracle Cloud Container Registry 的認證權杖

在此步驟中，我們將產生一個_認證權杖_，供我們用來登入 Oracle Cloud Container Registry。

1.  選取右上角的「使用者圖示」，然後選取_我的設定檔_。
    
    ![使用者設定值](images/user-settings.png " ")
    
2.  向下捲動並選取_認證權杖_。
    
    ![認證權杖](images/auth-token.png " ")
    
3.  按一下_產生憑證_。
    
    ![產生權杖](images/generate-token.png " ")
    
4.  複製 _`helidon-stock-application-your_first_name`_ 並將其貼到_描述_方塊中，然後按一下_產生記號_。
    
    ![權杖建立](images/token-create.png " ")
    
5.  選取「產生的記號」下的_複製_，然後將其貼到文字檔中。我們無法稍後再複製。
    
    ![複製權杖](images/copy-token.png " ")
    

## 作業 2：將 bobbys-helidon-stock-application Docker 映像檔推送至您的 Oracle Cloud Container Registry 儲存區域

1.  在此實驗室的任務 1 中，您開啟了 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 並決定「區域」名稱的端點，然後將其複製到文字編輯器。在我們的範例中，區域名稱為 UK 南 (倫敦)。此任務需要此資訊。 ![端點](images/end-point.png)
    
2.  複製下列命令並將它貼到您的文字編輯器中，然後將 **`END_POINT_OF_REGION_NAME`** 取代為您區域的端點。
    
        <copy> docker login `END_POINT_OF_REGION_NAME`</copy>
        

3\. 在先前的實驗室中，您決定了租用戶命名空間。將使用者名稱設為如下：\*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`/oracleidentitycloudservice/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\*. 將 \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`\*\* 取代為您租用戶的命名空間，並以您的 Oracle Cloud 帳戶使用者名稱取代 \*\*\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\*，然後從您的文字檔複製取代的使用者名稱，然後將其貼到 \*Cloud Shell\* 中。若為「密碼」，請從文字編輯器或您儲存的位置貼上「認證記號」。！\[Login Registry\] (images/login-registry.png " ") 3\. 在先前的實驗室中，您決定了租用戶命名空間。使用下列方式建立使用者名稱：\`NAMESPACE\_OF\_YOUR\_TENANCY\`/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`。執行 docker 登入時，您需要以小寫方式使用租用戶命名空間和使用者名稱。將 \`NAMESPACE\_OF\_YOUR\_TENANCY\` 取代為您租用戶的命名空間，將 \`YOUR\_ORACLE\_CLOUD\_USERNAME\` 取代為您的 Oracle Cloud 帳戶使用者名稱，然後從文字編輯器複製取代的使用者名稱，然後將其貼到 \*Cloud Shell\* 中。若為「密碼」，請從文字編輯器或儲存它的位置貼上認證記號。

4.  回到容器登錄，選取_漢堡功能表 -> 開發人員服務 -> 容器登錄_。
    
    ![容器登錄](images/container-registry.png " ")
    
5.  選取區間，然後按一下_建立儲存區域_。
    
    ![建立儲存區域](images/repository-create.png " ")
    
6.  選取區間並輸入 _`helidon-stock-application-your_first_name`_ 作為「儲存區域名稱」，然後選擇「以_公用_身分存取」，然後按一下_建立_。
    
    ![儲存庫資料](images/repository-data.png " ")
    
    儲存區域 _`helidon-stock-application-your_first_name`_ 建立之後，您可以驗證命名空間，而且必須與租用戶的命名空間相同。
    
    ！\[ 驗證命名空間 \] (images/verify-namespace.png" ")
    
7.  在 Lab 5 中，您會將 Docker 影像全名複製到文字編輯器中。若要將 Docker 映像檔推送至 Oracle Cloud Container Registry 中的儲存區域，請在文字編輯器中複製並貼上下列命令，然後將 _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`helidon-stock-application-your_first_name`：1.0_ 取代為已儲存在文字編輯器中的 Docker 映像檔完整名稱。
    
        <copy>docker push `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/helidon-stock-application-your_first_name:1.0</copy>
        
    
    ![Docker 推送](images/docker-push.png " ")
    
    順利執行 _docker push_ 指令之後，請展開 _`helidon-stock-application-your_first_name`_ 儲存庫，您將注意到此儲存庫中有已上傳的新影像。
    
    讓 _Cloud Shell_ 與容器登錄儲存區域頁面維持開啟，我們需要使用這些頁面進入下一個實驗室。
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 3 月