# 修改 Helidon 应用程序以添加对象存储支持

## 简介

此实验室旨在演示 Helidon 应用**如何添加对象存储访问**。这是通过以下方式完成的：将用于将问候语的变量替换为对象，该对象现在将成为新的问候语容器，并存储**从对象存储桶中检索**。由于对象持久存在，因此应用程序重新启动后，最后一个问候字值将保持不变。如果不进行此更改，并且通过变量在内存中使用问候语，则在应用程序重新启动时，问候语将重置为默认值。

估计时间：15 分钟

观看下面的视频，快速浏览实验室。[对象存储支持](videohub:1_p5v2wehm)

### 目标

在本练习中，您将：

*   修改 Helidon 应用以显示与对象存储等 OCI 服务的集成
*   验证成功的对象存储集成

### 先决条件

*   Oracle 免费套餐（试用版）、付费版或 LiveLabs 云账户

## 任务 1：修改对象存储集成的 Helidon 应用程序

1.  验证 **oci.bucket.name** 是否已在 **~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties** 中正确配置，应该已在练习 2/任务 3 的步骤 5 中设置该配置。
    
2.  在**代码编辑器**中，单击 **~/oci-mp/server/** 下的文件名 _`pom.xml`_ 打开它，并在 **dependencies** 子句中添加 **Object Storage OCI SDK** 依赖项，如下所示。
    
        <copy><dependency>
                    <groupId>com.oracle.oci.sdk</groupId>
                    <artifactId>oci-java-sdk-objectstorage</artifactId>
        </dependency></copy>
        
    
    ![添加依赖关系](images/add-dependency.png)
    
    > 确保缩进正确。
    
3.  我们修改文件 _~/oci-mp/server/src/main/java/ocw/hol/mp/oci/server/GreetingProvider.java_ 以添加**对象存储**访问权限。为了节省时间，我们有此更改的源代码。请复制并粘贴以下代码，以便使用所需的更改更新此文件。您可以阅读下面的_注意_，其中描述了我们在此文件中所做的更改。
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/source/GreetingProvider.java server/src/main/java/ocw/hol/mp/oci/server/</copy>
        
    
    ![对象存储已添加](images/os-added.png)
    
    > **请读取：-**
    
    *   在构造器的参数部分中，添加了 _ObjectStorage objectStorageClient_ 参数。由于这是 _@Injected_ 注释的一部分，因此 Helidon 将自动处理和设置该参数以包含可用于与对象存储服务通信的客户端，而无需为此目的添加几行 **OCI SDK** 代码。
    *   在同一构造器的参数部分中，添加了 **ConfigProperty** ，这将从配置中的 **oci.bucket.name** 属性中提取值。在初始应用程序设置期间，当从 **`devops_helidon_to_instance_ocw_hol`** 系统信息库目录执行了名为 **`update_config_values.sh`** 的实用程序脚本时，这已在 **microprofile-config.properties** 中填充。
    *   使用 **getNamespace() 对象存储 SDK** 方法，检索对象存储的名称空间，因为以后将使用它来检索或存储对象：
    
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
        
    
    *   在 GreetingProvider 类下方，删除了变量声明：
    
        <copy>private final AtomicReference<String> message = new AtomicReference<>();</copy>
        
    
    *   将其替换为如下所示的本地全局变量。 _LOGGER_ 将用于日志记录，而 _objectStorageClient_ 、_namespaceName_ 、_bucketName_ 和 _objectName_ 将用于 _Object Storage SDK_ 调用。
    
        <copy>private static final Logger LOGGER = Logger.getLogger(GreetingProvider.class.getName());
        private ObjectStorage objectStorageClient;
        private String namespaceName;
        private String bucketName;
        private final String objectName = "hello.txt";</copy>
        
    
    *   将 _getMessage()_ 的内容替换为以下代码。这将使用 **getObject() SDK** 方法从从存储桶中提取的 **hello.txt** 对象中检索问候语。
    
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
        
    
    *   将 void **setMessage(String message)** 方法的内容替换为以下代码。这将使用 **putObject() SDK** 方法将包含问候语的 **hello.txt** 对象存储到存储桶中。
    
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
        
    
    *   已删除导入 ( **import java.util.concurrent.atomic.AtomicReference；** )，并将其替换为这些新导入，以支持已添加的所有新代码。
    
        <copy>import java.io.ByteArrayInputStream;
        import java.util.logging.Logger;
        
        import com.oracle.bmc.objectstorage.ObjectStorage;
        import com.oracle.bmc.objectstorage.requests.GetNamespaceRequest;
        import com.oracle.bmc.objectstorage.requests.GetObjectRequest;
        import com.oracle.bmc.objectstorage.requests.PutObjectRequest;
        import com.oracle.bmc.objectstorage.responses.GetNamespaceResponse;
        import com.oracle.bmc.objectstorage.responses.GetObjectResponse;</copy>
        

## 任务 2：推送 Helidon 应用程序代码更改并触发 DevOps 管道

1.  在终端**中复制并粘贴以下命令以提交并推送更改**。
    
        <copy>git add .
        git status
        git commit -m "Add support Object Storage integration"
        git push</copy>
        
    
    > 此 Git 推送将触发管道。
    
2.  等到通过监视构建和部署管道日志完成 **DevOps 生命周期**。 ![部署运行](images/deployment-run.png)
    

## 任务 3：验证对象存储集成是否成功

使用 curl 进行测试，并检查是否已将新的 **hello.txt** 对象添加到 **bucket** 。验证对象的大小是否与问候语的大小相同。例如，如果问候词为 _Hello_ ，则大小应为 **5** 。如果问候词为 _Hola_ ，则大小应为 **4** 。

1.  将部署节点 **`PUBLIC_IP`** 设置为环境变量。
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  复制并粘贴以下命令以放置 **GET** 请求。您将具有如下所示的类似输出。
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  在**云控制台**中，单击_汉堡菜单_ -> _存储_ -> _存储桶_，如下所示。 ![存储桶菜单](images/bucket-menu.png)
    
4.  选择正确的区间，然后单击 **app-bucket-helidonocw-hol-string** 以打开**桶**。 ![选择区间](images/select-compartment.png)
    
5.  检查存储桶现在是否包含对象 **hello.txt** ，并且大小为 **5 字节**，因为问候语为 **Hello** 。您还可以下载对象并验证内容是否确实是_您好_。 ![验证大小](images/verify-size.png)
    
    > 保留此页打开，因为在下一步中更改问候语后，我们会刷新此页。
    
6.  复制并粘贴以下命令以将 **Hello** 问候语替换为 **Hola** 。
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
7.  检查存储桶的 **hello.txt** 对象现在的大小是否为 **4 字节**，因为问候语已替换为 **Hola** 。您还可以下载对象并验证内容是否已更改为 **Hola** 。 ![hola 大小](images/hola-size.png)
    
8.  使用 **restart.sh** 工具重新启动应用程序，以演示问候词的值将保留，因为它保留在**对象存储**中。
    
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
        
    
    ![重新启动应用程序](images/restart-application.png)
    
    > 在要求 _Are you sure you want to continue connection (yes/no)？_ 时，输入 **yes** 。
    
9.  调用默认 Hello World 请求，**注意问候字仍然是 Hola** 。
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
10.  在**云控制台**中刷新“存储桶”页，您会发现该大小仍为 **4** ，这确认问候语仍为 **Hola** 。 ![验证持久性](images/verify-persistence.png)
    

**祝贺您！**您已完成了研习会。如果需要，可以继续执行**练习 6** ，这将**删除本研习会期间创建的所有资源**。

## 了解详细信息

*   [Helidon OCI 集成](https://helidon.io/docs/v3/#/mp/integrations/oci)

## 确认

*   **作者** - Keith Lustria
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月