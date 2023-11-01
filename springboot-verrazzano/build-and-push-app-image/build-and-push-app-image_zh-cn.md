# 将 Springboot 应用程序映像推送到 Oracle Cloud 容器注册表

## 简介

在本练习中，您将使用 Springboot 应用程序构建 Docker 映像，并将该映像推送到 Oracle Cloud 容器注册表内的资料档案库。

估计时间：10 分钟

### 目标

*   使用 Docker 构建和打包应用。
*   生成验证令牌以登录 Oracle Cloud 容器注册表。
*   将 Springboot 应用程序 Docker 映像推送到 Oracle Cloud 容器注册表资料档案库。

### 先备条件

*   Docker
*   Oracle Cloud 账户

## 任务 1：下载应用程序源代码和所需的 JDK

1.  复制并粘贴以下命令下载此研习会的源代码。
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/EeNNvCVLObhXjHIwu5M-J_zbSXJsbLop6wcsFFnDfneY3zqbAFfdKikZe-Q0GT3I/n/lrv4zdykjqrj/b/ankit-bucket/o/springboot-app.zip >~/springboot-app.zip</copy>
        
2.  复制并粘贴以下命令以解压缩源代码并将当前目录更改为应用程序文件夹。
    
        <copy>unzip ~/springboot-app.zip
        cd ~/springboot-app/</copy>
        
3.  我们将为 Springboot 应用程序创建 Docker 映像，但此应用程序使用特定版本的 JDK，我们不想更改构建新映像的 Docker 文件。因此，我们下载所需的 JDK。要下载所需的 JDK 版本，请复制下面的命令并将其粘贴到 Cloud Shell 中。
    
        <copy>wget https://download.java.net/java/ga/jdk11/openjdk-11_linux-x64_bin.tar.gz</copy>
        
4.  复制并粘贴以下命令以构建 Springboot 应用程序。
    
        <copy>mvn clean package</copy>
        

## 任务 2：构建 Springboot 应用程序 Docker 映像

我们将首先准备用于在 Verrazzano 上部署的 Docker 映像。

我们正在创建 Docker 映像，您将此映像上载到属于您的 OCI 账户的 Oracle Cloud 容器注册表。为此，您需要创建一个反映注册表坐标的图像名称。

需要以下信息：

*   租户名称空间
    
*   区域的端点
    
    > 将此信息复制到文本文件，以便可以在整个练习中引用它。
    

1.  要查找租户的名称空间，请单击_用户_图标 -> _租户_，如下所示。在**对象存储设置**中，您将找到名称空间。复制并将其保存在您的文本文件中，因为我们稍后也会使用它。
    
    ![复制租户名称空间](images/copy-tenancynamespace.png " ")
    
2.  找到_区域的端点_。  
    请参阅此 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 中记录的表。在所示的示例中，区域的端点是 _UK South（伦敦）_（作为区域名称），其端点是 _lhr.ocir.io_ 。找到您自己的_区域名称_的端点并将其保存在文本文件中。
    
    ![端点](images/end-points.png)
    
    > 现在，您的区域同时具有租户名称空间和端点。
    
3.  复制以下命令并将其粘贴到您的文本文件中。然后，将 _`ENDPOINT_OF_YOUR_REGION`_ 替换为您区域名称的端点，将 _`NAMESPACE_OF_YOUR_TENANCY`_ 替换为租户的名称空间，将 _`your_first_name`_ 替换为您的名字。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-your_first_name:v1 .</copy>
        
    
    命令准备就绪后，从 `~/springboot-app/` 目录在 Cloud Shell 中运行。生成将生成以下结果：
    
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
        
4.  这将创建 Docker 映像，您可以在本地系统信息库中检查该映像。
    
        $ docker images
        REPOSITORY                            TAG     IMAGE ID    CREATED      SIZE
        lhr.ocir.io/namespace/springboot-ankit v1    a701fa912f2 3 minutes ago 664MB
        ghcr.io/oracle/oraclelinux            7-slim 1d56b1a0fd8 3 weeks ago   138MB
        $ 
        
    
    将替换的完整映像名称 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1` 复制到文本文件，因为以后需要它。
    

## 任务 3：生成验证令牌以登录 Oracle Cloud 容器注册表

在此步骤中，我们将生成一个用于登录 Oracle Cloud 容器注册表的_验证令牌_。

1.  选择右上角的用户图标，然后选择_我的概要信息_。
    
    ![我的配置文件](images/my-profile.png " ")
    
2.  向下滚动并选择 _Auth Tokens_ 。
    
    ![验证标记](images/auth-token.png " ")
    
3.  单击_生成标记_。
    
    ![生成标记](images/generate-token.png " ")
    
4.  复制 _`springboot-your_first_name`_ 并将其粘贴到_说明_框中，然后单击_生成令牌_。
    
    ![标记创建](images/token-create.png " ")
    
5.  在“生成的令牌”下选择_复制_并将其粘贴到文本文件中。我们以后不能复制它。然后单击_关闭_。
    
    ![复制标记](images/copy-token.png " ")
    

## 任务 4：将 Springboot 应用程序 Docker 映像推送到容器注册表资料档案库

1.  在此实验的任务 1 中，您打开了 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 并确定了区域名称的端点并将其复制到文本文件。在我们的示例中，“Region Name（区域名称）”为 UK South（伦敦）。此任务需要此信息。 ![端点](images/end-points.png)
    
2.  复制以下命令并将其粘贴到文本编辑器中，然后将 `ENDPOINT_OF_REGION_NAME` 替换为区域的端点。
    
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
    
5.  选择区间，然后单击**创建**。 ![创建资料档案库](images/repository-create.png)
    
6.  选择区间并输入 _`springboot-your_first_name`_ 作为资料档案库名称，然后选择“访问权限”作为**公共**，然后单击**创建资料档案库**。
    
    ![资料档案库说明](images/describe-repository.png)
    
7.  要将 Docker 映像推送到 Oracle Cloud 容器注册表内的资料档案库，请在文本文件中复制并粘贴以下命令，然后将 _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/springboot-your\_firstname：1.0_ 替换为先前保存的 Docker 映像全名。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1</copy>
        
    
    该结果应该与此类似：
    
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
        
8.  成功运行 _dockerush_ 命令后，展开 _`springboot-ankit:v1`_ 系统信息库，您将发现新映像已上载到此系统信息库。
    

您现在可以**进入下一个实验**。

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月