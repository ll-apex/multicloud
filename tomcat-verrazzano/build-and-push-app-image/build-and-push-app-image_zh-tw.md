# 將 Tomcat 應用程式映像檔推送至 Oracle Cloud Container Registry

## 簡介

在此實驗室中，您將使用 Tomcat 應用程式建立 Docker 映像檔，然後將該映像檔推送至 Oracle Cloud Container Registry 中的儲存區域。

預估時間：10 分鐘

### 目標

*   使用 Docker 建立及封裝應用程式。
*   產生認證權杖以登入 Oracle Cloud Container Registry。
*   將 Tomcat 應用程式 Docker 映像檔推送至您的 Oracle Cloud Container Registry 儲存區域。

### 先決條件

*   Docker
*   Oracle Cloud 帳戶

## 任務 1：下載應用程式原始碼

1.  複製並貼上下列命令以下載此研習的原始碼。
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/5DhoxZ22MseWaoSjG3EoBRf6DF5JbQ1QyR7tHa2qem-7IHb7nNEymF1ZSm2eA1ix/n/lrv4zdykjqrj/b/ankit-bucket/o/tomcat-examples.zip >~/tomcat-examples.zip</copy>
        
2.  複製並貼上下列命令，以解壓縮原始程式碼並將目前的目錄變更為應用程式資料夾。
    
        <copy>unzip ~/tomcat-examples.zip
        cd ~/tomcat-examples/</copy>
        

## 作業 2：建立 Tomcat 應用程式 Docker 映像檔

首先，我們會先準備要在 Verrazzano 上部署的 Docker 映像檔。

我們正在建立 Docker 映像檔，您將上傳至屬於您 OCI 帳戶的 Oracle Cloud Container Registry。若要這麼做，您必須建立反映登錄座標的影像名稱。

需要以下資訊：

*   租用戶命名空間
    
*   區域的端點
    
    > 將此資訊複製到文字檔，以便您可以在整個實驗室中參考該檔案。
    

1.  若要尋找租用戶的命名空間，請按一下_使用者_圖示 -> _租用戶_ (如圖所示) . 在**物件儲存設定值**中，您會發現命名空間。複製並儲存到您的文字檔，因為我們稍後也會使用。
    
    ![複製租用戶命名空間](images/copy-tenancynamespace.png " ")
    
2.  開啟 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 並判斷您「區域」名稱的端點，然後將其複製到文字檔中。在我們的範例中，區域名稱為 UK 南 (倫敦)。此任務需要此資訊。 ![端點](images/end-point.png)
    
3.  複製下列命令並將它貼到您的文字編輯器中。然後將 _`ENDPOINT_OF_YOUR_REGION`_ 取代為您區域名稱的端點，將 _`NAMESPACE_OF_YOUR_TENANCY`_ 取代為租用戶的命名空間，並將其取代為 _`your_first_name`_ 。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-your_first_name:v1 .</copy>
        
    
    命令就緒之後，請從 `~/tomcat-examples/` 目錄在 Cloud Shell 執行。建置會產生下列結果：
    
        $ cd ~/tomcat-examples/
        $ $ docker build -t lhr.ocir.io/tenancy-namespace/tomcat-example-ankit:v1 .
        Sending build context to Docker daemon  1.097MB
        Step 1/10 : FROM tomcat:8.0-alpine
        Trying to pull repository docker.io/library/tomcat ... 
        8.0-alpine: Pulling from docker.io/library/tomcat
        4fe2ade4980c: Pull complete 
        6fc58a8d4ae4: Pull complete 
        7d9bd64c803b: Pull complete 
        a22aedc5ac11: Pull complete 
        5bde63ae3587: Pull complete 
        69cb0c9b940a: Pull complete 
        Digest: sha256:d02a16c0147fcae13d812fa670a4b3c9944f5328b10a5a463ad697d2aa5bb063
        Status: Downloaded newer image for tomcat:8.0-alpine
        ---> 624fb61775c3
        Step 2/10 : LABEL maintainer="ankit.x.pandey@oracle.com"
        ---> Running in 20cc23726499
        Removing intermediate container 20cc23726499
        ---> 50245c696fb6
        Step 3/10 : ADD sample-webapp.war /usr/local/tomcat/webapps/
        ---> 727c55f91bb5
        Step 4/10 : RUN mkdir /data
        ---> Running in f3129a859e11
        Removing intermediate container f3129a859e11
        ---> 9ce0f5674f51
        Step 5/10 : ADD jmx_prometheus_javaagent-0.17.0.jar /data/jmx_prometheus_javaagent-0.17.0.jar
        ---> f03cc9ee1bee
        Step 6/10 : ADD prometheus-jmx-config.yaml /data/prometheus-jmx-config.yaml
        ---> 50c51ae6a148
        Step 7/10 : ENV JAVA_OPTS="-javaagent:/data/jmx_prometheus_javaagent-0.17.0.jar=8088:/data/prometheus-jmx-config.yaml"
        ---> Running in 5e9effd5d494
        Removing intermediate container 5e9effd5d494
        ---> 85ca06fcd965
        Step 8/10 : EXPOSE 8088
        ---> Running in 795325f82526
        Removing intermediate container 795325f82526
        ---> 19dfc6fd903c
        Step 9/10 : EXPOSE 8080
        ---> Running in 43be96f20275
        Removing intermediate container 43be96f20275
        ---> 7d9bcaa7a271
        Step 10/10 : CMD ["catalina.sh", "run"]
        ---> Running in 3e25cd78ab88
        Removing intermediate container 3e25cd78ab88
        ---> 516065fe1bf5
        Successfully built 516065fe1bf5
        Successfully tagged lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        $
        
4.  這會建立 Docker 映像檔，您可以存入您的本機儲存區域。
    
        $ docker images
        REPOSITORY                           TAG IMAGE ID      CREATED       SIZE
        lhr.ocir.io/name/tomcat-example-ankit v1 516065fe1bf5 2 minutes ago  147MB
        tomcat                       8.0-alpine  624fb61775c3 4 years ago    147MB
        
    
    將取代的完整影像名稱 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1` 複製到文字編輯器，因為您之後將需要它。
    

## 作業 3：產生登入 Oracle Cloud Container Registry 的認證權杖

在此步驟中，我們將產生一個_認證權杖_，供我們用來登入 Oracle Cloud Container Registry。

1.  選取右上角的「使用者圖示」，然後選取_我的設定檔_。
    
    ![我的設定檔](images/my-profile.png " ")
    
2.  向下捲動並選取_認證權杖_。
    
    ![認證權杖](images/auth-token.png " ")
    
3.  按一下_產生憑證_。
    
    ![產生權杖](images/generate-token.png " ")
    
4.  複製 _`tomcat-example-your_first_name`_ 並將其貼到_描述_方塊中，然後按一下_產生記號_。
    
    ![權杖建立](images/token-create.png " ")
    
5.  在「產生的記號」下選取_複製_，然後將其貼到文字編輯器中。我們無法稍後再複製。然後按一下_關閉 (Close)_ 。
    
    ![複製權杖](images/copy-token.png " ")
    

## 作業 4：將 Tomcat 應用程式 Docker 映像檔推送至您的容器登錄儲存區域

1.  在此實驗室的任務 2 中，您開啟了 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 並確定您「區域」名稱的端點，然後將其複製到文字檔中。在我們的範例中，區域名稱為 UK 南 (倫敦)。此任務需要此資訊。 ![端點](images/end-point.png)
    
2.  複製下列命令並將它貼到您的文字編輯器中，然後將 `ENDPOINT_OF_REGION_NAME` 取代為您區域的端點。
    
    > 在我們的範例中，區域名稱為 _UK South (London)_ ，端點為 _lhr.ocir.io_ 。您需要此任務的特定資訊。
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  在上一個步驟中，您也會決定租用戶命名空間。依下列方式輸入使用者名稱：`NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`。  
    
    *   將 `NAMESPACE_OF_YOUR_TENANCY` 取代為租用戶的命名空間
    *   將 `YOUR_ORACLE_CLOUD_USERNAME` 取代為您的 Oracle Cloud 帳戶使用者名稱，然後從文字編輯器複製取代的使用者名稱，然後將其貼到 _Cloud Shell_ 中。
    *   對於密碼，請從文字編輯器中複製並貼上驗證記號 (或任何您儲存的位置)。
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  瀏覽回「容器登錄」。在主控台中開啟導覽功能表，然後按一下**開發人員服務**。在**容器與使用者自建物件**底下，按一下**容器登錄**。 ![容器登錄](images/container-registry.png)
    
5.  選取區間，然後按一下**建立儲存區域**。 ![建立儲存區域](images/repository-create.png)
    
6.  選取區間並輸入 _`tomcat-example-your_first_name`_ 作為「儲存區域名稱」，然後選擇「以**公用**身分存取」，然後按一下**建立**。
    
    ![儲存區域描述](images/describe-repository.png)
    
7.  若要將 Docker 映像檔推送至 Oracle Cloud Container Registry 中的儲存區域，請在文字編輯器中複製並貼上下列命令，然後將 `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`tomcat-example-your_first_name`：1.0 取代為先前儲存的 Docker 映像檔完整名稱。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1</copy>
        
    
    結果應如下所示：
    
        $ docker push lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        The push refers to repository [lhr.ocir.io/tenancynamespace/tomcat-example-ankit]
        4b193f4c616d: Pushed 
        0469528628db: Pushed 
        cce8193c4190: Pushed 
        ca36c0db4673: Pushed 
        0136a6a85859: Pushed 
        98a0db77a14c: Pushed 
        9072514c7af0: Pushed 
        f6146a44a7d3: Pushed 
        0c3170905795: Pushed 
        df64d3292fd6: Pushed 
        v1: digest: sha256:65b562a7117870540f1807e0d796fe964e6428bda0ae290b8a6389bf637d1aba size: 2405
        $ 
        
8.  順利執行 _docker push_ 指令之後，請展開 _`tomcat-example-ankit:v1`_ 儲存庫，您將注意到新的映像檔已上傳至此儲存庫。
    

您現在可以**進入下一個實驗室**。

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月