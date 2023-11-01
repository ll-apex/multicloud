# 修改 Helidon 應用程式以新增物件儲存支援

## 簡介

此實驗室的目標是要示範從 Helidon 應用程式**如何新增物件儲存體存取**。這是透過將用來儲存問候語的變數取代成現在成為新問候語容器的物件，並儲存**從物件儲存的儲存桶擷取**來完成。因為物件持續存在，所以應用程式會重新啟動時最後一個問候語詞值。如果沒有這項變更，且透過變數在記憶體中使用問候語，當應用程式重新啟動時，問候語會重設為預設值。

預估時間：15 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[物件儲存支援](videohub:1_p5v2wehm)

### 目標

在此實驗室中，您將：

*   修改 Helidon 應用程式以顯示它與 OCI 服務 (例如物件儲存) 的整合
*   驗證成功的物件儲存整合

### 必要條件

*   一個 Oracle Free Tier (Trial)、Paid 或 LiveLabs 雲端帳戶

## 作業 1：修改物件儲存整合的 Helidon 應用程式

1.  確認 **~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties** 中已正確設定 **oci.bucket.name** ，此設定應該已在實驗室 2/ 任務 3 的步驟 5 中設定。
    
2.  在**程式碼編輯器**中，按一下 **~/oci-mp/server/** 底下的 _`pom.xml`_ 檔案名稱來開啟檔案，然後在 **dependencies** 子句內新增 **Object Storage OCI SDK** 相依性，如下所示。
    
        <copy><dependency>
                    <groupId>com.oracle.oci.sdk</groupId>
                    <artifactId>oci-java-sdk-objectstorage</artifactId>
        </dependency></copy>
        
    
    ![新增相依性](images/add-dependency.png)
    
    > 保持縮排正確。
    
3.  我們會修改 _~/oci-mp/server/src/main/java/ocw/hol/mp/oci/server/GreetingProvider.java_ 檔案，以便新增 **Object Storage** 存取權。不過，我們還是有這項變更的原始程式碼。請複製並貼上下列程式碼，以便更新此檔案的必要變更。您可以閱讀下方的_注意事項_，瞭解此檔案中所做的變更。
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/source/GreetingProvider.java server/src/main/java/ocw/hol/mp/oci/server/</copy>
        
    
    ![已新增物件儲存](images/os-added.png)
    
    > **請閱讀：-**
    
    *   在建構子的引數區段中，新增 _ObjectStorage objectStorageClient_ 參數。由於這是 _@Injected_ 註解的一部分，因此 Helidon 會自動處理參數，並設定此參數以包含可用來與物件儲存服務通訊的從屬端，而無須新增幾行 **OCI SDK** 程式碼供該用途使用。
    *   在相同建構子的引數區段新增 **ConfigProperty** ，此區段會從配置中的 **oci.bucket.name** 特性擷取值。先前已在初始應用程式設定期間，從 **`devops_helidon_to_instance_ocw_hol`** 儲存庫目錄執行名為 **`update_config_values.sh`** 的公用程式程序檔時，填入 **microprofile-config.properties** 。
    *   使用 **getNamespace() 物件儲存 SDK** 方法，擷取物件儲存的命名空間，此命名空間稍後將用來擷取或儲存物件：
    
        <copy>public GreetingProvider(@ConfigProperty(name = "app.greeting") String message,
                            ObjectStorage objectStorageClient,
                            @ConfigProperty(name = "oci.bucket.name") String bucketName) {
        try {
            this.bucketName = bucketName;
            GetNamespaceResponse namespaceResponse =
                    objectStorageClient.getNamespace(GetNamespaceRequest.builder().build());
            this.objectStorageClient = objectStorageClient;
            this.namespaceName = namespaceResponse.getValue();
            LOGGER.info("Object storage namespace: " + namespaceName);
            setMessage(message);
        } catch (Exception e) {
            LOGGER.warning("Error invoking getNamespace from Object Storage: " + e);
        }
        }     </copy>
        
    
    *   在 GreetingProvider 類別的右側，已移除變數宣告：
    
        <copy>private final AtomicReference<String> message = new AtomicReference<>();</copy>
        
    
    *   以下面的區域全域變數取代。 _LOGGER_ 將用於記錄，而 _objectStorageClient_ 、_namespaceName_ 、_bucketName_ 及 _objectName_ 將用於_物件儲存 SDK_ 呼叫。
    
        <copy>private static final Logger LOGGER = Logger.getLogger(GreetingProvider.class.getName());
        private ObjectStorage objectStorageClient;
        private String namespaceName;
        private String bucketName;
        private final String objectName = "hello.txt";</copy>
        
    
    *   將 _getMessage()_ 的內容取代為以下程式碼。這將會使用 **getObject() SDK** 方法，從從儲存桶擷取的 **hello.txt** 物件擷取問候語。
    
        <copy>try {
        GetObjectResponse getResponse =
                objectStorageClient.getObject(
                        GetObjectRequest.builder()
                                .namespaceName(namespaceName)
                                .bucketName(bucketName)
                                .objectName(objectName)
                                .build());
        return new String(getResponse.getInputStream().readAllBytes());
        } catch (Exception e) {
            LOGGER.warning("Error invoking getObject from Object Storage: " + e);
            return "Hello-Error";
        }</copy>
        
    
    *   將無效 **setMessage (String message)** 方法的內容取代為以下程式碼。這會使用 **putObject() SDK** 方法，將包含問候語的 **hello.txt** 物件儲存在儲存桶中。
    
        <copy>try {
        byte[] contents = message.getBytes();
        PutObjectRequest putObjectRequest =
                PutObjectRequest.builder()
                        .namespaceName(namespaceName)
                        .bucketName(bucketName)
                        .objectName(objectName)
                        .putObjectBody(new ByteArrayInputStream(message.getBytes()))
                        .contentLength(Long.valueOf(contents.length))
                        .build();
        objectStorageClient.putObject(putObjectRequest);
        } catch (Exception e) {
            LOGGER.warning("Error invoking putObject from Object Storage: " + e);
        }</copy>
        
    
    *   已移除匯入 ( **匯入 java.util.concurrent.atomic.AtomicReference；** )，並將其取代為這些新匯入，以支援所有已新增的代碼。
    
        <copy>import java.io.ByteArrayInputStream;
        import java.util.logging.Logger;
        
        import com.oracle.bmc.objectstorage.ObjectStorage;
        import com.oracle.bmc.objectstorage.requests.GetNamespaceRequest;
        import com.oracle.bmc.objectstorage.requests.GetObjectRequest;
        import com.oracle.bmc.objectstorage.requests.PutObjectRequest;
        import com.oracle.bmc.objectstorage.responses.GetNamespaceResponse;
        import com.oracle.bmc.objectstorage.responses.GetObjectResponse;</copy>
        

## 作業 2：推送 Helidon 應用程式程式碼變更並觸發 DevOps 管線

1.  在終端機中複製並貼上下列命令**以確認和推送變更**。
    
        <copy>git add .
        git status
        git commit -m "Add support Object Storage integration"
        git push</copy>
        
    
    > 此 Git 推送將會觸發管線。
    
2.  請監控組建和部署管線日誌，等到 **DevOps 生命週期**完成。 ![部署運行](images/deployment-run.png)
    

## 作業 3：確認成功的物件儲存整合

使用曲線進行測試，然後檢查是否已將新的 **hello.txt** 物件新增至 **bucket** 。驗證物件的大小是否與問候語的大小相同。例如，如果問候語是 _Hello_ ，則大小應為 **5** 。如果問候語是 _Hola_ ，則大小應該是 **4** 。

1.  將建置節點 **`PUBLIC_IP`** 設定為環境變數。
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  複製並貼上下列命令來放置 **GET** 要求。您會有如下所示的執行結果。
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  在 **雲端主控台**中，按一下 \[ _漢堡功能表_ \] -> \[ _儲存_ \] -> \[ _儲存桶_ \]，如下所示。 ![桶功能表](images/bucket-menu.png)
    
4.  選取正確的區間，然後按一下 **app-bucket-helidonocw-hol-string** 以開啟**儲存桶**。 ![選取區間](images/select-compartment.png)
    
5.  檢查儲存桶現在包含一個物件 **hello.txt** ，而且大小為 **5 個位元組**，因為問候語是 **Hello** 。您也可以下載物件並確認內容確實是 _Hello_ 。 ![驗證大小](images/verify-size.png)
    
    > 請將此頁面保持開啟，因為我們會在下個步驟變更問候語之後，重新整理此頁面。
    
6.  複製並貼上下列命令，將 **Hello** 問候語取代為 **Hola** 。
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
7.  檢查儲存桶的 **hello.txt** 物件現在其大小為 **4 個位元組**，因為問候語以 **Hola** 取代。您也可以下載物件並驗證內容已變更為**霍拉**。 ![卓拉尺寸](images/hola-size.png)
    
8.  使用 **restart.sh** 工具重新啟動應用程式，示範問候語的值在**物件儲存**中仍持續存在。
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/restart.sh</copy>
        Created private.key and can be used to ssh to the deployment instance by running this command: "ssh -i private.key opc@xx.xx.xx.xx"
        FIPS mode initialized
        The authenticity of host 'xx.xx.xx.xx (xx.xx.xx.xx)' can't be established.
        ECDSA key fingerprint is SHA256:hJl8axCNhFcILDo+AwxMkodxhY+UxRD40d1ans83GTg.
        ECDSA key fingerprint is SHA1:IBUhyn05DaIs60GAQsruVXajhym.
        Are you sure you want to continue connecting (yes/no)? yes
        Warning: Permanently added 'xx.xx.xx.xx' (ECDSA) to the list of known hosts.
        Starting oci-mp-server.jar
        Helidon app is now running with pid 264792!
        Cleaning up ssh private.key
        
    
    ![重新啟動應用程式](images/restart-application.png)
    
    > 輸入_是_，當要求**您確定要繼續連線時 (是 / 否)？**
    
9.  呼叫預設的 Hello World 要求，並且**瞭解問候語還是 Hola** 。
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
10.  重新整理**雲端主控台**中的「儲存設定 (Bucket)」頁面，您會發現大小仍然是 **4** ，確認問候語仍然是 **Hola** 。 ![驗證持續性](images/verify-persistence.png)
    

**恭喜！** 您已完成研習。若要繼續進行**實驗室 6** ，請**刪除此研討會期間建立的所有資源**。

## 進一步瞭解

*   [Helidon OCI 整合](https://helidon.io/docs/v3/#/mp/integrations/oci)

## 確認

*   **作者** - Keith Lustria
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月