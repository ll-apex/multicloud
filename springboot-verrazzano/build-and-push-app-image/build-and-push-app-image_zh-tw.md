# 將 Springboot 應用程式映像檔推送至 Oracle Cloud Container Registry

## 簡介

在此實驗室中，您將使用 Springboot 應用程式建立 Docker 映像檔，然後將該映像檔推送至 Oracle Cloud Container Registry 中的儲存區域。

預估時間：10 分鐘

### 目標

*   使用 Docker 建立及封裝應用程式。
*   產生認證權杖以登入 Oracle Cloud Container Registry。
*   將 Springboot 應用程式 Docker 映像檔推送至您的 Oracle Cloud Container Registry 儲存區域。

### 先決條件

*   Docker
*   Oracle Cloud 帳戶

## 作業 1：下載應用程式原始碼和必要的 JDK

1.  複製並貼上下列命令以下載此研習的原始碼。
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/EeNNvCVLObhXjHIwu5M-J_zbSXJsbLop6wcsFFnDfneY3zqbAFfdKikZe-Q0GT3I/n/lrv4zdykjqrj/b/ankit-bucket/o/springboot-app.zip >~/springboot-app.zip</copy>
        
2.  複製並貼上下列命令，以解壓縮原始程式碼並將目前的目錄變更為應用程式資料夾。
    
        <copy>unzip ~/springboot-app.zip
        cd ~/springboot-app/</copy>
        
3.  我們即將為 Springboot 應用程式建立 Docker 映像檔，但此應用程式使用特定版本的 JDK，而我們不想變更建置新映像檔的 Docker 檔案。所以，我們會下載必要的 JDK。若要下載必要的 JDK 版本，請複製下方命令並將其貼到 Cloud Shell 中。
    
        <copy>wget https://download.java.net/java/ga/jdk11/openjdk-11_linux-x64_bin.tar.gz</copy>
        
4.  複製並貼上下列指令以建置 Springboot 應用程式。
    
        <copy>mvn clean package</copy>
        

## 作業 2：建置 Springboot 應用程式 Docker 映像檔

首先，我們會先準備要在 Verrazzano 上部署的 Docker 映像檔。

我們正在建立 Docker 映像檔，您將上傳至屬於您 OCI 帳戶的 Oracle Cloud Container Registry。若要這麼做，您必須建立反映登錄座標的影像名稱。

需要以下資訊：

*   租用戶命名空間
    
*   區域的端點
    
    > 將此資訊複製到文字檔，以便您可以在整個實驗室中參考該檔案。
    

1.  若要尋找租用戶的命名空間，請按一下_使用者_圖示 -> _租用戶_ (如圖所示) . 在**物件儲存設定值**中，您會發現命名空間。複製並儲存到您的文字檔，因為我們稍後也會使用。
    
    ![複製租用戶命名空間](images/copy-tenancynamespace.png " ")
    
2.  尋找_您區域的端點_。  
    請參閱此 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 所記載的表格。在顯示範例中，區域的端點是 _UK South (London)_ (作為區域名稱)，而其端點是 _lhr.ocir.io_ 。尋找您自己的_區域名稱_端點，然後將它儲存在文字檔中。
    
    ![端點](images/end-points.png)
    
    > 您現在已同時具有區域的租用戶命名空間和端點。
    
3.  複製下列命令並將其貼到您的文字檔中。然後將 _`ENDPOINT_OF_YOUR_REGION`_ 取代為您區域名稱的端點，將 _`NAMESPACE_OF_YOUR_TENANCY`_ 取代為租用戶的命名空間，並將其取代為 _`your_first_name`_ 。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-your_first_name:v1 .</copy>
        
    
    命令就緒之後，請從 `~/springboot-app/` 目錄在 Cloud Shell 執行。建置會產生下列結果：
    
        $ cd ~/springboot-app/
        $ docker build -t lhr.ocir.io/tenancy-namespace/springboot-ankit:v1 .
        Sending build context to Docker daemon  206.7MB
        Step 1/14 : FROM ghcr.io/oracle/oraclelinux:7-slim AS build_base
        Trying to pull repository ghcr.io/oracle/oraclelinux ... 
        7-slim: Pulling from ghcr.io/oracle/oraclelinux
        6cb086706000: Pull complete 
        Digest: sha256:4353fdc8664c386c0a443eb40b10a7662b4eb8d6eb5d6dcefe218e9783132c71
        Status: Downloaded newer image for ghcr.io/oracle/oraclelinux:7-slim
        ---> 1d56b1a0fd84
        Step 2/14 : RUN yum update -y && yum-config-manager --save --setopt=ol7_ociyum_config.skip_if_unavailable=true     && yum install -y tar unzip gzip     && yum clean all; rm -rf /var/cache/yum     && mkdir -p /license
        ---> Running in 92c013a8e84f
        Loaded plugins: ovl
        No packages marked for update
        Loaded plugins: ovl
        ===================================== main =====================================
        [main]
        alwaysprompt = True
        assumeno = False
        assumeyes = False
        autocheck_running_kernel = True
        autosavets = True
        bandwidth = 0
        bugtracker_url = https://linux.oracle.com
        cache = 0
        cachedir = /var/cache/yum/x86_64/7Server
        check_config_file_age = True
        clean_requirements_on_remove = False
        ----------------------------------------------------------------------
        Step 3/14 : ENV JAVA_HOME=/usr/java
        ---> Running in 96b99fba7e50
        Removing intermediate container 96b99fba7e50
        ---> 1cc6c7a63b89
        Step 4/14 : ENV PATH $JAVA_HOME/bin:$PATH
        ---> Running in 4a88eb052547
        Removing intermediate container 4a88eb052547
        ---> 48e7fa9b7b0c
        Step 5/14 : ARG JDK_BINARY="${JDK_BINARY:-openjdk-11_linux-x64_bin.tar.gz}"
        ---> Running in e922e8b35bfd
        Removing intermediate container e922e8b35bfd
        ---> 6888b690a4b0
        Step 6/14 : COPY ${JDK_BINARY} jdk.tar.gz
        ---> e9c5ffd0f2a5
        Step 7/14 : ENV JDK_DOWNLOAD_SHA256=3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e
        ---> Running in a7a89b057f25
        Removing intermediate container a7a89b057f25
        ---> 12ecfaa002bf
        Step 8/14 : RUN set -eux     echo "Checking JDK hash";     echo "${JDK_DOWNLOAD_SHA256} *jdk.tar.gz" | sha256sum --check -;     echo "Installing JDK";     mkdir -p "$JAVA_HOME";     tar xzf jdk.tar.gz --directory "${JAVA_HOME}" --strip-components=1;     rm -f jdk.tar.gz;
        ---> Running in a6a590adeee6
        + echo '3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e *jdk.tar.gz'
        + sha256sum --check -
        jdk.tar.gz: OK
        + echo 'Installing JDK'
        + mkdir -p /usr/java
        Installing JDK
        + tar xzf jdk.tar.gz --directory /usr/java --strip-components=1
        + rm -f jdk.tar.gz
        Removing intermediate container a6a590adeee6
        ---> 1f37a6cb044d
        Step 9/14 : COPY LICENSE.txt /license/
        ---> dc69f24f5be6
        Step 10/14 : COPY THIRD_PARTY_LICENSES.txt /license/
        ---> 5ef683dbda22
        Step 11/14 : ARG JAR_FILE=target/*.jar
        ---> Running in 3b80032f8310
        Removing intermediate container 3b80032f8310
        ---> 8eca16289bd7
        Step 12/14 : COPY ${JAR_FILE} app.jar
        ---> dcb7e3ed0871
        Step 13/14 : ENTRYPOINT ["java","-jar","/app.jar"]
        ---> Running in 2191623bf524
        Removing intermediate container 2191623bf524
        ---> 11e59e19cfb4
        Step 14/14 : USER 1000
        ---> Running in 16446779b92b
        Removing intermediate container 16446779b92b
        ---> a701fa912f2e
        Successfully built a701fa912f2e
        Successfully tagged lhr.ocir.io/tenancynamespace/springboot-ankit:v1
        $
        
4.  這會建立 Docker 映像檔，您可以存入您的本機儲存區域。
    
        $ docker images
        REPOSITORY                            TAG     IMAGE ID    CREATED      SIZE
        lhr.ocir.io/namespace/springboot-ankit v1    a701fa912f2 3 minutes ago 664MB
        ghcr.io/oracle/oraclelinux            7-slim 1d56b1a0fd8 3 weeks ago   138MB
        $ 
        
    
    將取代的完整影像名稱 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1` 複製到您的文字檔，因為您之後將需要它。
    

## 作業 3：產生登入 Oracle Cloud Container Registry 的認證權杖

在此步驟中，我們將產生一個_認證權杖_，供我們用來登入 Oracle Cloud Container Registry。

1.  選取右上角的「使用者圖示」，然後選取_我的設定檔_。
    
    ![我的設定檔](images/my-profile.png " ")
    
2.  向下捲動並選取_認證權杖_。
    
    ![認證權杖](images/auth-token.png " ")
    
3.  按一下_產生憑證_。
    
    ![產生權杖](images/generate-token.png " ")
    
4.  複製 _`springboot-your_first_name`_ 並將其貼到_描述_方塊中，然後按一下_產生記號_。
    
    ![權杖建立](images/token-create.png " ")
    
5.  選取「產生的記號」下的_複製_，然後將其貼到文字檔中。我們無法稍後再複製。然後按一下_關閉 (Close)_ 。
    
    ![複製權杖](images/copy-token.png " ")
    

## 作業 4：將 Springboot 應用程式 Docker 映像檔植入您的容器登錄儲存區域

1.  在此實驗室的任務 1 中，您開啟了 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 並決定「區域」名稱的端點，然後將其複製到文字檔中。在我們的範例中，區域名稱為 UK 南 (倫敦)。此任務需要此資訊。 ![端點](images/end-points.png)
    
2.  複製下列命令並將它貼到您的文字編輯器中，然後將 `ENDPOINT_OF_REGION_NAME` 取代為您區域的端點。
    
    > 在我們的範例中，區域名稱為 _UK South (London)_ ，端點為 _lhr.ocir.io_ 。您需要此任務的特定資訊。
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  在上一個步驟中，您也會決定租用戶命名空間。依下列方式輸入使用者名稱：`NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`。  
    
    *   將 `NAMESPACE_OF_YOUR_TENANCY` 取代為租用戶的命名空間
    *   將 `YOUR_ORACLE_CLOUD_USERNAME` 取代為您的 Oracle Cloud 帳戶使用者名稱，然後從您的文字檔複製取代的使用者名稱，然後將其貼到 _Cloud Shell_ 中。
    *   對於密碼，請從文字檔 (或任何您儲存的位置) 複製並貼上驗證記號。
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  瀏覽回「容器登錄」。在主控台中開啟導覽功能表，然後按一下**開發人員服務**。在**容器與使用者自建物件**底下，按一下**容器登錄**。 ![容器登錄](images/container-registry.png)
    
5.  選取區間，然後按一下**建立 (Create)** 。 ![建立儲存區域](images/repository-create.png)
    
6.  選取區間並輸入 _`springboot-your_first_name`_ 作為「儲存區域名稱」，然後選擇「以**公用**身分存取」，然後按一下**建立儲存區域**。
    
    ![儲存區域描述](images/describe-repository.png)
    
7.  若要將 Docker 映像檔推送至 Oracle Cloud Container Registry 中的儲存區域，請在文字檔中複製並貼上下列命令，然後將 _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/springboot-your\_firstname：1.0_ 取代為先前儲存的 Docker 映像檔完整名稱。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1</copy>
        
    
    結果應如下所示：
    
        $ docker push lhr.ocir.io/namespace/springboot-ankit:v1
        The push refers to repository [lhr.ocir.io/namespace/springboot-ankit]
        31118271414e: Pushed 
        e6144652ec48: Pushed 
        a5ac4d4576aa: Pushed 
        2f93ab3a0c42: Pushed 
        3ed60ad88e51: Pushed 
        f47db30f116a: Pushed 
        f50ba2e0b2f9: Pushed 
        v1: digest: sha256:96aacff31cb255ea815213aba837f16f40d73b14d67449d4744ed811c7a864c8 size: 1795
        $ 
        
8.  順利執行 _docker push_ 指令之後，請展開 _`springboot-ankit:v1`_ 儲存庫，您將注意到新的映像檔已上傳至此儲存庫。
    

您現在可以**進入下一個實驗室**。

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月