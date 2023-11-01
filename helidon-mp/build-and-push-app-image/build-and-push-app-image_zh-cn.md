# 构建 Helidon 应用程序映像并将其推送到 Oracle Cloud 容器注册表

## 简介

在本练习中，您将使用 Helidon 应用构建_原生_ Docker 映像，并将该映像推送到 Oracle Cloud 容器注册表内的资料档案库。

估计时间：15 分钟

观看下面的视频，快速浏览实验室。[构建 Helidon 应用映像并将其推送到 Oracle Cloud 容器注册表](videohub:1_mh1brw5t)

### 目标

*   使用 Docker 构建和打包应用。
*   生成验证令牌以登录 Oracle Cloud 容器注册表。
*   将 Helidon 应用程序 Docker 映像推送到 Oracle Cloud 容器注册表资料档案库。

### 先备条件

*   在上一个练习中创建的 Helidon 应用程序
*   Docker
*   Oracle Cloud 账户

## 任务 1：构建 Helidon 应用 Docker 映像

我们正在创建 Docker 映像，您将此映像上载到属于您的 OCI 账户的 Oracle Cloud 容器注册表。为此，您需要创建一个反映注册表坐标的图像名称。

需要以下信息：

*   区域名称
*   租户名称空间
*   区域的端点
    
    > 将此信息复制到文本文件，以便可以在整个练习中引用它。
    

1.  找到_区域名称_。  
    您的_区域名称_位于 Oracle Cloud 控制台的右上角，在此示例中，该名称显示为_英国南部（伦敦）_。您可能有所不同。
    
    ![容器注册表](images/region-name.png)
    
2.  找到_租户名称空间_。  
    在控制台中，打开导航菜单并单击**开发人员服务**。在**容器和构件**下，单击**容器注册表**。
    
    ![租户名称空间](images/container-registry.png)
    
    > 租户名称空间列在区间中。复制并将其保存在文本文件中。您也会在下一个练习中使用此信息。 ![租户名称空间](images/name-space.png)
    
3.  找到_区域的端点_。  
    请参阅此 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 中记录的表。在所示的示例中，区域的端点是 _UK South（伦敦）_（作为区域名称），其端点是 _lhr.ocir.io_ 。找到您自己的_区域名称_的端点并将其保存在文本文件中。下一个实验室还需要它。
    
    ![端点](images/end-points.png)
    
    > 现在，您的区域同时具有租户名称空间和端点。
    
4.  复制以下命令并将其粘贴到您的文本文件中。然后，将 _`ENDPOINT_OF_YOUR_REGION`_ 替换为您区域名称的端点，将 _`NAMESPACE_OF_YOUR_TENANCY`_ 替换为租户的名称空间，将 _`your_first_name`_ 替换为您的名字。
    
    > 这将在 Docker 容器内完成完整构建。首次运行它时，需要一段时间，因为它正在下载所有 Maven 依赖项并将其高速缓存到 Docker 层。只要您不更改 pom.xml 文件，后续的构建速度就会快很多。如果修改了 pom，则将重新下载相关项。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0 -f Dockerfile.native .</copy>
        
    
    该命令准备就绪后，从 _`~/helidon-project/myproject/myproject`_ 目录在代码编辑器内的终端中运行。构建将在结束时生成以下结果：
    
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
        
5.  这将创建 Docker 映像，您可以在本地系统信息库中检查该映像。
    
        $ docker images
        REPOSITORY                     TAG IMAGE ID        CREATED           SIZE
        lhr.ocir.io/tn/myproject-ankit 1.0 aa8b6e7b04c0 About a minute ago   80.2MB
        
    
    将替换的完整图像名称 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0` 复制到文本编辑器，因为以后需要它。
    
6.  在终端中复制并粘贴以下命令，以便在代码编辑器的 Cloud Shell 中运行 docker 映像。
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    ![docker 运行](images/docker-run.png)
    
7.  打开新的终端/控制台，然后运行以下命令来检查应用程序：
    
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
        

## 任务 2：生成验证令牌以登录 Oracle Cloud 容器注册表

在此步骤中，我们将生成一个用于登录 Oracle Cloud 容器注册表的_验证令牌_。

1.  选择右上角的用户图标，然后选择_我的概要信息_。
    
    ![我的配置文件](images/my-profile.png " ")
    
2.  向下滚动并选择 _Auth Tokens_ 。
    
    ![验证标记](images/auth-token.png " ")
    
3.  单击_生成标记_。
    
    ![生成标记](images/generate-token.png " ")
    
4.  复制 _`myproject-your_first_name`_ 并将其粘贴到_说明_框中，然后单击_生成令牌_。
    
    ![标记创建](images/token-create.png " ")
    
5.  在“生成的令牌”下选择_复制_并将其粘贴到文本编辑器中。我们以后不能复制它。然后单击_关闭_。
    
    ![复制标记](images/copy-token.png " ")
    

## 任务 3：将 Helidon 应用程序 (myproject) Docker 映像推送到容器注册表资料档案库

1.  在此实验的任务 1 中，您打开了 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 并确定了区域名称的端点并将其复制到文本文件。在我们的示例中，“Region Name（区域名称）”为 UK South（伦敦）。此任务需要此信息。 ![端点](images/end-points.png)
    
2.  复制以下命令并将其粘贴到文本文件中，然后将 `ENDPOINT_OF_REGION_NAME` 替换为区域的端点。
    
    > 在我们的示例中，区域名称为 _UK South (London)_ ，端点为 _lhr.ocir.io_ 。您需要此任务的特定信息。
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  在上一步中，您还确定了租户名称空间。按如下方式输入用户名：`NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`。  
    
    *   将 `NAMESPACE_OF_YOUR_TENANCY` 替换为租户的名称空间
    *   将 `YOUR_ORACLE_CLOUD_USERNAME` 替换为您的 Oracle Cloud 账户用户名，然后从文本文件复制替换的用户名并将其粘贴到 _Cloud Shell_ 中。
    *   对于“密码”，请从您的文本文件（或您保存的任何位置）复制并粘贴“身份验证令牌”。
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  导航回容器注册表。在控制台中，打开导航菜单，然后单击**开发人员服务**。在**容器和构件**下，单击**容器注册表**。 ![容器注册表](images/container-registry.png)
    
5.  选择区间，然后单击**创建资料档案库**。 ![创建资料档案库](images/repository-create.png)
    
6.  选择区间并输入 _`myproject-your_first_name`_ 作为资料档案库名称，然后选择“访问权限”作为**公共**，然后单击**创建资料档案库**。
    
    ![资料档案库说明](images/describe-repository.png)
    
7.  创建系统信息库 _`myproject-your_first_name`_ 后，可以在系统信息库列表中验证其设置。
    
    ![验证名称空间](images/verify-namespace.png)
    
8.  要将 Docker 映像推送到 Oracle Cloud 容器注册表内的资料档案库，请在文本文件中复制并粘贴以下命令，然后将 `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/myproject-your\_first\_name：1.0 替换为您之前保存的 Docker 映像全名。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    该结果应该与此类似：
    
        $ docker push lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 The push refers to repository [lhr.ocir.io/tenancynamespace/myproject-ankit]
        0bf9e88b7284: Pushed 
        0acc08535a89: Pushed 
        1.0: digest: sha256:3e8cc0ff23d256499dff8d907150b639925687aed0c41008cbe1794bc02433e2 size: 735
        $
        
9.  成功运行 _dockerush_ 命令后，展开 _`myproject-your_first_name`_ 系统信息库，您将发现新映像已上载到此系统信息库。
    
    ![图像已上载](images/verify-push.png)
    

## 确认

*   **作者** - Dmitry Aleksandrov
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 4 月