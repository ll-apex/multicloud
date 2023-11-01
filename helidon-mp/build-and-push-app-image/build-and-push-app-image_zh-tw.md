# 建立 Helidon 應用程式映像檔並推送至 Oracle Cloud Container Registry

## 簡介

在此實驗室中，您將使用 Helidon 應用程式建立_原生_ Docker 映像檔，並將該映像檔推送至 Oracle Cloud Container Registry 中的儲存區域。

預估時間：15 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[建置 Helidon 應用系統映像檔並推送至 Oracle Cloud Container Registry](videohub:1_mh1brw5t)

### 目標

*   使用 Docker 建立及封裝應用程式。
*   產生認證權杖以登入 Oracle Cloud Container Registry。
*   將 Helidon 應用程式 Docker 映像檔推送至您的 Oracle Cloud Container Registry 儲存區域。

### 先決條件

*   您在上一個實驗室中建立的 Helidon 應用程式
*   Docker
*   Oracle Cloud 帳戶

## 作業 1：建立 Helidon 應用程式 Docker 映像檔

我們正在建立 Docker 映像檔，您將上傳至屬於您 OCI 帳戶的 Oracle Cloud Container Registry。若要這麼做，您必須建立反映登錄座標的影像名稱。

需要以下資訊：

*   區域名稱
*   租用戶命名空間
*   區域的端點
    
    > 將此資訊複製到文字檔，以便您可以在整個實驗室中參考該檔案。
    

1.  尋找您的_區域名稱_。  
    您的_區域名稱_位於 Oracle Cloud 主控台的右上角，本範例顯示為_英國南部 (倫敦)_ 。你可能不同。
    
    ![容器登錄](images/region-name.png)
    
2.  尋找_租用戶命名空間_。  
    在主控台中開啟導覽功能表，然後按一下**開發人員服務**。在**容器與使用者自建物件**底下，按一下**容器登錄**。
    
    ![租用戶命名空間](images/container-registry.png)
    
    > 區間中會列出租用戶命名空間。複製並將其儲存在文字檔中。您將會在下一個實驗室中使用此資訊。 ![租用戶命名空間](images/name-space.png)
    
3.  尋找_您區域的端點_。  
    請參閱此 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 所記載的表格。在顯示範例中，區域的端點是 _UK South (London)_ (作為區域名稱)，而其端點是 _lhr.ocir.io_ 。尋找您自己的_區域名稱_端點，然後將它儲存在文字檔中。您也需要這個實驗室才能上手。
    
    ![端點](images/end-points.png)
    
    > 您現在已同時具有區域的租用戶命名空間和端點。
    
4.  複製下列命令並將其貼到您的文字檔中。然後將 _`ENDPOINT_OF_YOUR_REGION`_ 取代為您區域名稱的端點，將 _`NAMESPACE_OF_YOUR_TENANCY`_ 取代為租用戶的命名空間，並將其取代為 _`your_first_name`_ 。
    
    > 這會在 Docker 容器內完整建置。第一次執行時，會花費一些時間，因為它正在下載所有 Maven 相依性，並將它們快取到 Docker 層中。只要您沒有變更 pom.xml 檔案，後續的組建就會更快。如果修改 pom，將會重新下載相依性。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0 -f Dockerfile.native .</copy>
        
    
    命令就緒之後，從 _`~/helidon-project/myproject/myproject`_ 目錄在程式碼編輯器內的終端機中執行。組建會在最後產生下列結果：
    
        $ docker build -t lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 -f Dockerfile.native .
        
        [1/7] Initializing...                                                                                   (15.7s @ 0.14GB)
        Version info: 'GraalVM 22.3.0 Java 17 CE'
        Java version info: '17.0.5+8-jvmci-22.3-b08'
        C compiler: gcc (redhat, x86_64, 11.3.1)
        Garbage collector: Serial GC
        2 user-specific feature(s)
        - io.helidon.integrations.graal.mp.nativeimage.extension.WeldFeature
        - io.helidon.integrations.graal.nativeimage.extension.HelidonReflectionFeature
        2023.04.06 05:41:01 INFO io.helidon.common.LogConfig Thread[main,5,main]: Logging at initialization configured using classpath: /logging.properties
        [2/7] Performing analysis...  [**********]                                                             (202.8s @ 1.92GB)
        18,812 (92.77%) of 20,278 classes reachable
        27,564 (63.52%) of 43,392 fields reachable
        87,900 (62.22%) of 141,268 methods reachable
        1,068 classes,   565 fields, and 6,864 methods registered for reflection
            65 classes,    70 fields, and    58 methods registered for JNI access
            6 native libraries: dl, m, pthread, rt, stdc++, z
        [3/7] Building universe...                                                                              (27.6s @ 3.15GB)
        [4/7] Parsing methods...      [*****]                                                                   (22.5s @ 3.00GB)
        [5/7] Inlining methods...     [***]                                                                     (11.9s @ 1.84GB)
        [6/7] Compiling methods...    [************]                                                           (156.5s @ 3.05GB)
        [7/7] Creating image...                                                                                 (15.6s @ 2.44GB)
        35.03MB (45.80%) for code area:    57,947 compilation units
        39.02MB (51.01%) for image heap:  477,987 objects and 128 resources
        2.44MB ( 3.19%) for other data
        76.49MB in total
        ------------------------------------------------------------------------------------------------------------------------
        Top 10 packages in code area:                               Top 10 object types in image heap:
        1.63MB sun.security.ssl                                     7.71MB byte[] for code metadata
        1.20MB com.sun.media.sound                                  4.60MB java.lang.Class
        1.17MB java.util                                            3.93MB java.lang.String
        822.87KB java.lang.invoke                                     3.41MB byte[] for java.lang.String
        717.54KB com.sun.crypto.provider                              3.22MB byte[] for general heap data
        517.57KB io.helidon.config                                    1.58MB com.oracle.svm.core.hub.DynamicHubCompanion
        510.02KB java.util.concurrent                                 1.13MB byte[] for reflection metadata
        481.49KB jdk.proxy4                                           1.03MB byte[] for embedded resources
        474.98KB java.lang                                          915.61KB java.util.HashMap$Node
        468.42KB com.sun.org.apache.xerces.internal.impl            781.21KB java.lang.String[]
        26.70MB for 671 more packages                                9.83MB for 4584 more object types
        ------------------------------------------------------------------------------------------------------------------------
                                31.0s (6.6% of total time) in 59 GCs | Peak RSS: 4.80GB | CPU load: 1.60
        ------------------------------------------------------------------------------------------------------------------------
        Produced artifacts:
        /helidon/target/myproject (executable)
        /helidon/target/myproject.build_artifacts.txt (txt)
        ========================================================================================================================
        Finished generating 'myproject' in 7m 48s.
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  08:04 min
        [INFO] Finished at: 2023-04-06T05:48:33Z
        [INFO] ------------------------------------------------------------------------
        Removing intermediate container e400c5c6897b
        ---> 20099e4619d6
        Step 10/15 : RUN echo "done!"
        ---> Running in a8eddd448e48
        done!
        Removing intermediate container a8eddd448e48
        ---> ebfd3064dc68
        Step 11/15 : FROM scratch
        ---> 
        Step 12/15 : WORKDIR /helidon
        ---> Running in 46be56a98462
        Removing intermediate container 46be56a98462
        ---> eaf15b746a1c
        Step 13/15 : COPY --from=build /helidon/target/myproject .
        ---> a69ac5933048
        Step 14/15 : ENTRYPOINT ["./myproject"]
        ---> Running in 71633a601e7f
        Removing intermediate container 71633a601e7f
        ---> cd9f22bfa4b3
        Step 15/15 : EXPOSE 8080
        ---> Running in 4b9763eb49fa
        Removing intermediate container 4b9763eb49fa
        ---> aa8b6e7b04c0
        Successfully built aa8b6e7b04c0
        Successfully tagged lhr.ocir.io/lrv4zdykjqrj/myproject-ankit:1.0
        
5.  這會建立 Docker 映像檔，您可以存入您的本機儲存區域。
    
        $ docker images
        REPOSITORY                     TAG IMAGE ID        CREATED           SIZE
        lhr.ocir.io/tn/myproject-ankit 1.0 aa8b6e7b04c0 About a minute ago   80.2MB
        
    
    將取代的完整影像名稱 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0` 複製到文字編輯器，因為您之後將需要它。
    
6.  在終端機中複製並貼上下列命令，在 Cloud Shell 的程式碼編輯器中執行碼頭影像。
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    ![碼頭運行](images/docker-run.png)
    
7.  開啟新的終端機 / 主控台並執行下列命令來檢查應用程式：
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        

## 作業 2：產生登入 Oracle Cloud Container Registry 的認證權杖

在此步驟中，我們將產生一個_認證權杖_，供我們用來登入 Oracle Cloud Container Registry。

1.  選取右上角的「使用者圖示」，然後選取_我的設定檔_。
    
    ![我的設定檔](images/my-profile.png " ")
    
2.  向下捲動並選取_認證權杖_。
    
    ![認證權杖](images/auth-token.png " ")
    
3.  按一下_產生憑證_。
    
    ![產生權杖](images/generate-token.png " ")
    
4.  複製 _`myproject-your_first_name`_ 並將其貼到_描述_方塊中，然後按一下_產生記號_。
    
    ![權杖建立](images/token-create.png " ")
    
5.  在「產生的記號」下選取_複製_，然後將其貼到文字編輯器中。我們無法稍後再複製。然後按一下_關閉 (Close)_ 。
    
    ![複製權杖](images/copy-token.png " ")
    

## 作業 3：將 Helidon 應用程式 (myproject) Docker 映像檔植入您的容器登錄儲存區域

1.  在此實驗室的任務 1 中，您開啟了 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 並決定「區域」名稱的端點，然後將其複製到文字檔中。在我們的範例中，區域名稱為 UK 南 (倫敦)。此任務需要此資訊。 ![端點](images/end-points.png)
    
2.  複製下列命令並將其貼到您的文字檔中，然後將 `ENDPOINT_OF_REGION_NAME` 取代為您區域的端點。
    
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
    
5.  選取區間，然後按一下**建立儲存區域**。 ![建立儲存區域](images/repository-create.png)
    
6.  選取區間並輸入 _`myproject-your_first_name`_ 作為「儲存區域名稱」，然後選擇「以**公用**身分存取」，然後按一下**建立儲存區域**。
    
    ![儲存區域描述](images/describe-repository.png)
    
7.  建立儲存庫 _`myproject-your_first_name`_ 之後，您可以在儲存庫清單中驗證其設定。
    
    ![驗證命名空間](images/verify-namespace.png)
    
8.  若要將 Docker 映像檔推送至 Oracle Cloud Container Registry 中的儲存區域，請在文字檔中複製並貼上下列命令，然後將 `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/myproject-your\_first\_name：1.0 取代為先前儲存的 Docker 映像檔完整名稱。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    結果應如下所示：
    
        $ docker push lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 The push refers to repository [lhr.ocir.io/tenancynamespace/myproject-ankit]
        0bf9e88b7284: Pushed 
        0acc08535a89: Pushed 
        1.0: digest: sha256:3e8cc0ff23d256499dff8d907150b639925687aed0c41008cbe1794bc02433e2 size: 735
        $
        
9.  順利執行 _docker push_ 指令之後，請展開 _`myproject-your_first_name`_ 儲存庫，您將注意到新的映像檔已上傳至此儲存庫。
    
    ![已上傳影像](images/verify-push.png)
    

## 確認

*   **作者** - Dmitry Aleksandrov
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 4 月