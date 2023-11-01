# 將 Helidon 應用程式映像檔推送至 Oracle Cloud Container Registry

## 簡介

在此實驗室中，您將使用 Helidon 應用程式建立 Docker 映像檔，並將該映像檔推送至 Oracle Cloud Container Registry 中的儲存區域。

預估時間：10 分鐘

### 目標

*   使用 Docker 建立及封裝應用程式。
*   產生認證權杖以登入 Oracle Cloud Container Registry。
*   將 Helidon 應用程式 Docker 映像檔推送至您的 Oracle Cloud Container Registry 儲存區域。

### 先決條件

*   您在上一個實驗室中建立的 Helidon 應用程式
*   Docker
*   Oracle Cloud 帳戶

## 作業 1：建立 Helidon 應用程式 Docker 映像檔

首先，我們會先準備要在 Verrazzano 上部署的 Docker 映像檔。

我們正在建立 Docker 映像檔，您將上傳至屬於您 OCI 帳戶的 Oracle Cloud Container Registry。若要這麼做，您必須建立反映登錄座標的影像名稱。

需要以下資訊：

*   租用戶命名空間
*   區域的端點

1.  若要尋找租用戶的命名空間，請按一下_使用者_圖示 -> _租用戶_ (如圖所示) . 在**物件儲存設定值**中，您會發現命名空間。複製並儲存到您的文字檔，因為我們稍後也會使用。
    
    ![複製租用戶命名空間](images/copy-tenancynamespace.png " ")
    
2.  尋找_您區域的端點_。請參閱位於此 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 的表格。在顯示範例中，區域的端點是 _UK South (London)_ (作為區域名稱)，而其端點是 _lhr.ocir.io_ 。尋找您自己的_區域名稱_端點，然後將它儲存在文字檔中。您也需要這個實驗室才能上手。
    
    ![端點](images/end-point.png " ")
    
    > 您現在已同時具有區域的租用戶命名空間和端點。
    
3.  複製下列命令並將它貼到您的文字編輯器中。然後將 _`ENDPOINT_OF_YOUR_REGION`_ 取代為您區域名稱的端點，將 _`NAMESPACE_OF_YOUR_TENANCY`_ 取代為租用戶的命名空間，並將其取代為 _`your_first_name`_ 。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0 .</copy>
        
    
    命令就緒之後，從 _`~/quickstart-mp/`_ 目錄在 Cloud Shell 中執行。建置會產生下列結果：
    
        $ cd ~/quickstart-mp/
        $ docker build lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0 .
        > docker pull lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        [+] Building 107.5s (19/19) FINISHED                                                                                                            
        => [internal] load build definition from Dockerfile                                                                                       0.1s
        => => transferring dockerfile: 785B                                                                                                       0.1s
        => [internal] load .dockerignore                                                                                                          0.1s
        => => transferring context: 48B                                                                                                           0.0s
        => [internal] load metadata for docker.io/library/openjdk:11-jre-slim                                                                     3.7s
        => [internal] load metadata for docker.io/library/maven:3.6-jdk-11                                                                        2.8s
        => [auth] library/openjdk:pull token for registry-1.docker.io                                                                             0.0s
        => [auth] library/maven:pull token for registry-1.docker.io                                                                               0.0s
        => [stage-1 1/4] FROM docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527      33.3s
        => => resolve docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527               0.0s
        => => sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527 549B / 549B                                                 0.0s
        => => sha256:f3cdb8fd164057f4ef3e60674fca986f3cd7b3081d55875c7ce75b7a214fca6d 1.16kB / 1.16kB                                             0.0s
        => => sha256:9c9e40a31d4fa290f933d76f3b0a4183ba02a7298a309f6bfa892d618e7196ef 7.56kB / 7.56kB                                             0.0s
        => => sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717 31.36MB / 31.36MB                                          18.6s
        => => sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251 1.58MB / 1.58MB                                             1.6s
        => => sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c 211B / 211B                                                 0.7s
        => => sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358 47.13MB / 47.13MB                                          24.4s
        => => extracting sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717                                                  7.7s
        => => extracting sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251                                                  0.3s
        => => extracting sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c                                                  0.0s
        => => extracting sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358                                                  5.7s
        => [build 1/7] FROM docker.io/library/maven:3.6-jdk-11@sha256:1d29ccf46ef2a5e64f7de3d79a63f9bcffb4dc56be0ae3daed5ca5542b38aa2d            0.0s
        => [internal] load build context                                                                                                          0.1s
        => => transferring context: 13.99kB                                                                                                       0.1s
        => CACHED [build 2/7] WORKDIR /helidon                                                                                                    0.0s
        => [build 3/7] ADD pom.xml .                                                                                                              0.1s
        => [build 4/7] RUN mvn package -Dmaven.test.skip -Declipselink.weave.skip                                                                91.7s
        => [stage-1 2/4] WORKDIR /helidon                                                                                                         0.8s
        => [build 5/7] ADD src src                                                                                                                0.1s
        => [build 6/7] RUN mvn package -DskipTests                                                                                               10.8s
        => [build 7/7] RUN echo "done!"                                                                                                           0.5s
        => [stage-1 3/4] COPY --from=build /helidon/target/quickstart-mp-ankit.jar ./                                                                   0.1s
        => [stage-1 4/4] COPY --from=build /helidon/target/libs ./libs                                                                            0.1s
        => exporting to image                                                                                                                     0.1s
        => => exporting layers                                                                                                                    0.1s
        => => writing image sha256:587a079ad854fc79e768acda11fc05dd87d37013261249e778e80749798c2837                                               0.0s
        => => naming to lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0                                                                           0.0s
        
4.  這會建立 Docker 映像檔，您可以存入您的本機儲存區域。
    
        $ docker images
        
        REPOSITORY                                                                           TAG                               IMAGE ID       CREATED         SIZE
        lhr.ocir.io/tenancynamespace/quickstart-mp-ankit                                               1.0                               587a079ad854   5 minutes ago   243MB
        
    
    將取代的完整影像名稱 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-ankit:1.0` 複製到您的文字檔，因為您之後將需要它。
    

## 作業 2：產生登入 Oracle Cloud Container Registry 的認證權杖

在此步驟中，我們將產生一個_認證權杖_，供我們用來登入 Oracle Cloud Container Registry。

1.  選取右上角的「使用者圖示」，然後選取_我的設定檔_。
    
    ![我的設定檔](images/my-profile.png)
    
2.  向下捲動並選取_認證權杖_。
    
    ![認證權杖](images/auth-token.png)
    
3.  按一下_產生憑證_。
    
    ![產生記號](images/generate-token.png)
    
4.  複製 _`quickstart-mp-your_first_name`_ 並將其貼到「描述」方塊中，然後按一下_產生記號_。
    
    ![建立權杖](images/create-token.png)
    
5.  按一下「產生的記號」下的_複製_，然後將其貼到文字編輯器中。您之後無法複製，因此請確定您已儲存此權杖的複本。然後，按一下_關閉_。
    
    ![複製記號](images/copy-token.png)
    

## 作業 3：將 Helidon 應用程式 (quickstart-mp) Docker 映像檔推送至您的容器登錄儲存區域

1.  在此實驗室的任務 1 中，您開啟了 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 並決定「區域」名稱的端點，然後將其複製到文字編輯器。在我們的範例中，區域名稱為 UK 南 (倫敦)。此任務需要此資訊。 ![端點](images/end-point.png)
    
2.  複製下列命令並將它貼到您的文字編輯器中，然後將 `ENDPOINT_OF_REGION_NAME` 取代為您區域的端點。
    
    > 在我們的範例中，區域名稱為 _UK South (London)_ ，端點為 _lhr.ocir.io_ 。您需要此任務的特定資訊。
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  在上一個步驟中，您也會決定租用戶命名空間。依下列方式輸入使用者名稱：`NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`。  
    
    *   將 _`NAMESPACE_OF_YOUR_TENANCY`_ 取代為租用戶的命名空間
    *   將 _`YOUR_ORACLE_CLOUD_USERNAME`_ 取代為您的 Oracle Cloud 帳戶使用者名稱，然後從文字編輯器複製取代的使用者名稱，然後貼到 _Cloud Shell_ 中。
    *   對於密碼，請從文字編輯器中複製並貼上驗證記號 (或任何您儲存的位置)。
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  瀏覽回「容器登錄」。在主控台中開啟導覽功能表，然後按一下**開發人員服務**。在**容器與使用者自建物件**底下，按一下**容器登錄**。 ![容器登錄](images/container-registry.png)
    
5.  選取區間，然後按一下**建立 (Create)** 。 ![建立儲存區域](images/repository-create.png)
    
6.  選取區間並輸入 _`quickstart-mp-your_first_name`_ 作為「儲存區域名稱」，然後選擇「以**公用**身分存取」，然後按一下**建立儲存區域**。
    
    ![儲存區域描述](images/describe-repository.png)
    
7.  建立儲存庫 _`quickstart-mp-your_first_name`_ 之後，您可以在儲存庫清單中驗證其設定。
    
    ![驗證命名空間](images/verify-namespace.png)
    
8.  若要將 Docker 映像檔推送至 Oracle Cloud Container Registry 中的儲存區域，請在文字編輯器中複製並貼上下列命令，然後將 `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`quickstar-mp-your_first_name`：1.0 取代為先前儲存的 Docker 映像檔完整名稱。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0</copy>
        
    
    結果應如下所示：
    
        $ docker push lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        The push refers to a repository [lhr.ocir.io/tenancynamespace/quickstart-mp-ankit]
        0795b8384c47: Pushed
        131452972f9d: Pushed
        93c53f2e9519: Pushed
        3b78b65a4be9: Pushed
        e1434e7d0308: Pushed
        17679d5f39bd: Pushed
        300b011056d9: Pushed
        1.0: digest: sha256:355fa56eab185535a58c5038186381b6d02fd8e0bcb534872107fc249f98256a size: 1786
        
9.  順利執行 _docker push_ 指令之後，請展開 _`quickstart-mp-your_first_name`_ 儲存庫，您將注意到新的映像檔已上傳至此儲存庫。
    
10.  讓 _Cloud Shell_ 與容器登錄儲存區域頁面維持開啟，您將需要使用這些頁面進入下一個實驗室。
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月