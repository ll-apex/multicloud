# 将 Helidon 应用程序映像推送到 Oracle Cloud 容器注册表

## 简介

在本练习中，您将使用 Helidon 应用构建 Docker 映像，并将该映像推送到 Oracle Cloud 容器注册表内的资料档案库。

估计时间：10 分钟

### 目标

*   使用 Docker 构建和打包应用。
*   生成验证令牌以登录 Oracle Cloud 容器注册表。
*   将 Helidon 应用程序 Docker 映像推送到 Oracle Cloud 容器注册表资料档案库。

### 先备条件

*   在上一个练习中创建的 Helidon 应用程序
*   Docker
*   Oracle Cloud 账户

## 任务 1：构建 Helidon 应用 Docker 映像

我们将首先准备用于在 Verrazzano 上部署的 Docker 映像。

我们正在创建 Docker 映像，您将此映像上载到属于您的 OCI 账户的 Oracle Cloud 容器注册表。为此，您需要创建一个反映注册表坐标的图像名称。

需要以下信息：

*   租户名称空间
*   区域的端点

1.  要查找租户的名称空间，请单击_用户_图标 -> _租户_，如下所示。在**对象存储设置**中，您将找到名称空间。复制并将其保存在您的文本文件中，因为我们稍后也会使用它。
    
    ![复制租户名称空间](images/copy-tenancynamespace.png " ")
    
2.  找到_区域的端点_。请参阅此 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 中记录的表。在所示的示例中，区域的端点是 _UK South（伦敦）_（作为区域名称），其端点是 _lhr.ocir.io_ 。找到您自己的_区域名称_的端点并将其保存在文本文件中。下一个实验室还需要它。
    
    ![端点](images/end-point.png " ")
    
    > 现在，您的区域同时具有租户名称空间和端点。
    
3.  复制以下命令并将其粘贴到文本编辑器中。然后，将 _`ENDPOINT_OF_YOUR_REGION`_ 替换为您区域名称的端点，将 _`NAMESPACE_OF_YOUR_TENANCY`_ 替换为租户的名称空间，将 _`your_first_name`_ 替换为您的名字。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0 .</copy>
        
    
    命令准备就绪后，从 _`~/quickstart-mp/`_ 目录在 Cloud Shell 中运行。生成将生成以下结果：
    
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
        
4.  这将创建 Docker 映像，您可以在本地系统信息库中检查该映像。
    
        $ docker images
        
        REPOSITORY                                                                           TAG                               IMAGE ID       CREATED         SIZE
        lhr.ocir.io/tenancynamespace/quickstart-mp-ankit                                               1.0                               587a079ad854   5 minutes ago   243MB
        
    
    将替换的完整映像名称 `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-ankit:1.0` 复制到文本文件，因为以后需要它。
    

## 任务 2：生成验证令牌以登录 Oracle Cloud 容器注册表

在此步骤中，我们将生成一个用于登录 Oracle Cloud 容器注册表的_验证令牌_。

1.  选择右上角的用户图标，然后选择_我的概要信息_。
    
    ![我的配置文件](images/my-profile.png)
    
2.  向下滚动并选择 _Auth Tokens_ 。
    
    ![验证标记](images/auth-token.png)
    
3.  单击_生成标记_。
    
    ![生成标记](images/generate-token.png)
    
4.  复制 _`quickstart-mp-your_first_name`_ 并将其粘贴到“说明”框中，然后单击_生成令牌_。
    
    ![创建标记](images/create-token.png)
    
5.  单击“生成的令牌”下的_复制_并将其粘贴到文本编辑器中。您以后无法复制该令牌，因此请确保已保存此令牌的副本。然后，单击_关闭_。
    
    ![复制标记](images/copy-token.png)
    

## 任务 3：将 Helidon 应用程序 (quickstart-mp) Docker 映像推送到容器注册表资料档案库

1.  在此实验的任务 1 中，您打开了 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 并确定了区域名称的端点并将其复制到文本编辑器。在我们的示例中，“Region Name（区域名称）”为 UK South（伦敦）。此任务需要此信息。 ![端点](images/end-point.png)
    
2.  复制以下命令并将其粘贴到文本编辑器中，然后将 `ENDPOINT_OF_REGION_NAME` 替换为区域的端点。
    
    > 在我们的示例中，区域名称为 _UK South (London)_ ，端点为 _lhr.ocir.io_ 。您需要此任务的特定信息。
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  在上一步中，您还确定了租户名称空间。按如下方式输入用户名：`NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`。  
    
    *   将 _`NAMESPACE_OF_YOUR_TENANCY`_ 替换为租户的名称空间
    *   将 _`YOUR_ORACLE_CLOUD_USERNAME`_ 替换为您的 Oracle Cloud 账户用户名，然后从文本编辑器复制替换的用户名并将其粘贴到_云 Shell_ 中。
    *   对于“密码”，请从文本编辑器复制并粘贴“身份验证令牌”（或者您保存的任何位置）。
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  导航回容器注册表。在控制台中，打开导航菜单，然后单击**开发人员服务**。在**容器和构件**下，单击**容器注册表**。 ![容器注册表](images/container-registry.png)
    
5.  选择区间，然后单击**创建**。 ![创建资料档案库](images/repository-create.png)
    
6.  选择区间并输入 _`quickstart-mp-your_first_name`_ 作为资料档案库名称，然后选择“访问权限”作为**公共**，然后单击**创建资料档案库**。
    
    ![资料档案库说明](images/describe-repository.png)
    
7.  创建系统信息库 _`quickstart-mp-your_first_name`_ 后，可以在系统信息库列表中验证其设置。
    
    ![验证名称空间](images/verify-namespace.png)
    
8.  要将 Docker 映像推送到 Oracle Cloud 容器注册表内的资料档案库，请在文本编辑器中复制并粘贴以下命令，然后将 `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`quickstar-mp-your_first_name`：1.0 替换为您之前保存的 Docker 映像全名。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0</copy>
        
    
    该结果应该与此类似：
    
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
        
9.  成功运行 _dockerush_ 命令后，展开 _`quickstart-mp-your_first_name`_ 系统信息库，您将发现新映像已上载到此系统信息库。
    
10.  将 _Cloud Shell_ 和容器注册表存储库页保持打开状态；在接下来的练习中，您将需要它们。
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月