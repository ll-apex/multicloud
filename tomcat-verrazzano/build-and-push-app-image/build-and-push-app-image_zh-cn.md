# 将 Tomcat 应用程序映像推送到 Oracle Cloud 容器注册表

## 简介

在本练习中，您将使用您的 Tomcat 应用程序构建 Docker 映像，并将该映像推送到 Oracle Cloud 容器注册表内的资料档案库。

估计时间：10 分钟

### 目标

*   使用 Docker 构建和打包应用。
*   生成验证令牌以登录 Oracle Cloud 容器注册表。
*   将 Tomcat 应用程序 Docker 映像推送到 Oracle Cloud 容器注册表资料档案库。

### 先备条件

*   Docker
*   Oracle Cloud 账户

## 任务 1：下载应用程序源代码

1.  复制并粘贴以下命令下载此研习会的源代码。
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/5DhoxZ22MseWaoSjG3EoBRf6DF5JbQ1QyR7tHa2qem-7IHb7nNEymF1ZSm2eA1ix/n/lrv4zdykjqrj/b/ankit-bucket/o/tomcat-examples.zip >~/tomcat-examples.zip</copy>
        
2.  复制并粘贴以下命令以解压缩源代码并将当前目录更改为应用程序文件夹。
    
        <copy>unzip ~/tomcat-examples.zip
        cd ~/tomcat-examples/</copy>
        

## 任务 2：构建 Tomcat 应用程序 Docker 映像

我们将首先准备用于在 Verrazzano 上部署的 Docker 映像。

我们正在创建 Docker 映像，您将此映像上载到属于您的 OCI 账户的 Oracle Cloud 容器注册表。为此，您需要创建一个反映注册表坐标的图像名称。

需要以下信息：

*   租户名称空间
    
*   区域的端点
    
    > 将此信息复制到文本文件，以便可以在整个练习中引用它。
    

1.  要查找租户的名称空间，请单击_用户_图标 -> _租户_，如下所示。在**对象存储设置**中，您将找到名称空间。复制并将其保存在您的文本文件中，因为我们稍后也会使用它。
    
    ![复制租户名称空间](images/copy-tenancynamespace.png " ")
    
2.  打开 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 并确定区域名称的端点并将其复制到文本文件。在我们的示例中，“Region Name（区域名称）”为 UK South（伦敦）。此任务需要此信息。 ![端点](images/end-point.png)
    
3.  复制以下命令并将其粘贴到文本编辑器中。然后，将 _`ENDPOINT_OF_YOUR_REGION`_ 替换为您区域名称的端点，将 _`NAMESPACE_OF_YOUR_TENANCY`_ 替换为租户的名称空间，将 _`your_first_name`_ 替换为您的名字。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-your_first_name:v1 .</copy>
        
    
    命令准备就绪后，从 `~/tomcat-examples/` 目录在 Cloud Shell 中运行。生成将生成以下结果：
    
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
        
4.  这将创建 Docker 映像，您可以在本地系统信息库中检查该映像。
    
        $ docker images
        REPOSITORY                           TAG IMAGE ID      CREATED       SIZE
        lhr.ocir.io/name/tomcat-example-ankit v1 516065fe1bf5 2 minutes ago  147MB
        tomcat                       8.0-alpine  624fb61775c3 4 years ago    147MB
        
    
    将替换的完整图像名称 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1` 复制到文本编辑器，因为以后需要它。
    

## 任务 3：生成验证令牌以登录 Oracle Cloud 容器注册表

在此步骤中，我们将生成一个用于登录 Oracle Cloud 容器注册表的_验证令牌_。

1.  选择右上角的用户图标，然后选择_我的概要信息_。
    
    ![我的配置文件](images/my-profile.png " ")
    
2.  向下滚动并选择 _Auth Tokens_ 。
    
    ![验证标记](images/auth-token.png " ")
    
3.  单击_生成标记_。
    
    ![生成标记](images/generate-token.png " ")
    
4.  复制 _`tomcat-example-your_first_name`_ 并将其粘贴到_说明_框中，然后单击_生成令牌_。
    
    ![标记创建](images/token-create.png " ")
    
5.  在“生成的令牌”下选择_复制_并将其粘贴到文本编辑器中。我们以后不能复制它。然后单击_关闭_。
    
    ![复制标记](images/copy-token.png " ")
    

## 任务 4：将 Tomcat 应用程序 Docker 映像推送到容器注册表资料档案库

1.  在此实验的任务 2 中，您打开了 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 并确定了区域名称的端点并将其复制到文本文件。在我们的示例中，“Region Name（区域名称）”为 UK South（伦敦）。此任务需要此信息。 ![端点](images/end-point.png)
    
2.  复制以下命令并将其粘贴到文本编辑器中，然后将 `ENDPOINT_OF_REGION_NAME` 替换为区域的端点。
    
    > 在我们的示例中，区域名称为 _UK South (London)_ ，端点为 _lhr.ocir.io_ 。您需要此任务的特定信息。
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  在上一步中，您还确定了租户名称空间。按如下方式输入用户名：`NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`。  
    
    *   将 `NAMESPACE_OF_YOUR_TENANCY` 替换为租户的名称空间
    *   将 `YOUR_ORACLE_CLOUD_USERNAME` 替换为您的 Oracle Cloud 账户用户名，然后从文本编辑器复制替换的用户名，然后将其粘贴到_云 Shell_ 中。
    *   对于“密码”，请从文本编辑器复制并粘贴“身份验证令牌”（或者您保存的任何位置）。
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  导航回容器注册表。在控制台中，打开导航菜单，然后单击**开发人员服务**。在**容器和构件**下，单击**容器注册表**。 ![容器注册表](images/container-registry.png)
    
5.  选择区间，然后单击**创建资料档案库**。 ![创建资料档案库](images/repository-create.png)
    
6.  选择区间并输入 _`tomcat-example-your_first_name`_ 作为资料档案库名称，然后选择“访问权限”作为**公共**，然后单击**创建**。
    
    ![资料档案库说明](images/describe-repository.png)
    
7.  要将 Docker 映像推送到 Oracle Cloud 容器注册表内的资料档案库，请在文本编辑器中复制并粘贴以下命令，然后将 `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`tomcat-example-your_first_name`：1.0 替换为您之前保存的 Docker 映像全名。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1</copy>
        
    
    该结果应该与此类似：
    
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
        
8.  成功运行 _dockerush_ 命令后，展开 _`tomcat-example-ankit:v1`_ 系统信息库，您将发现新映像已上载到此系统信息库。
    

您现在可以**进入下一个实验**。

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月