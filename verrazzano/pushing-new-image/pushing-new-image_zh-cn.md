# 将适用于 bobbys-helidon-stock-application 的 Docker 映像推送到 Oracle Cloud 容器注册表

## 简介

在实验 5 中，我们修改了 bobbys-helidon-stock-application，并构建了一个新的 Docker 映像。在此实验室中，我们将映像推送到 Oracle Cloud 容器注册表内的资料档案库。

估计时间：10 分钟

### 目标

在本练习中，您将：

*   生成验证令牌以登录 Oracle Cloud 容器注册表。
*   将 bobbys-helidon-stock-application Docker 映像推送到 Oracle Cloud 容器注册表资料档案库。

### 先备条件

您应该具有文本编辑器，可以在其中粘贴命令和 URL，并根据您的环境对其进行修改。然后，您可以复制并粘贴修改后的命令，以便在 _Cloud Shell_ 中运行它们。

## 任务 1：生成验证令牌以登录 Oracle Cloud 容器注册表

在此步骤中，我们将生成一个用于登录 Oracle Cloud 容器注册表的_验证令牌_。

1.  选择右上角的用户图标，然后选择_我的概要信息_。
    
    ![用户设置](images/user-settings.png " ")
    
2.  向下滚动并选择 _Auth Tokens_ 。
    
    ![验证标记](images/auth-token.png " ")
    
3.  单击_生成标记_。
    
    ![生成标记](images/generate-token.png " ")
    
4.  复制 _`helidon-stock-application-your_first_name`_ 并将其粘贴到_说明_框中，然后单击_生成令牌_。
    
    ![标记创建](images/token-create.png " ")
    
5.  在“生成的令牌”下选择_复制_并将其粘贴到文本文件中。我们以后不能复制它。
    
    ![复制标记](images/copy-token.png " ")
    

## 任务 2：将 bobbys-helidon-stock-application Docker 映像推送到 Oracle Cloud 容器注册表资料档案库

1.  在此实验的任务 1 中，您打开了 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 并确定了区域名称的端点并将其复制到文本编辑器。在我们的示例中，“Region Name（区域名称）”为 UK South（伦敦）。此任务需要此信息。 ![端点](images/end-point.png)
    
2.  复制以下命令并将其粘贴到文本编辑器中，然后将 **`END_POINT_OF_REGION_NAME`** 替换为区域的端点。
    
        <copy> docker login `END_POINT_OF_REGION_NAME`</copy>
        

3\. 在上一个练习中，您确定了租户名称空间。按如下所示创建用户名：\*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`/oracleidentitycloudservice/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\*。在此处，将 \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`\*\* 替换为租户的名称空间，将 \*\*\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\* 替换为您的 Oracle Cloud 账户用户名，然后从文本文件复制替换的用户名并将其粘贴到 \*Cloud Shell\* 中。对于“密码”, 请从文本编辑器或保存身份验证令牌的位置粘贴。!\[ 登录注册表 \](images/login-registry.png ”) 3\. 在上一个练习中，您确定了租户名称空间。按如下所示创建用户名：\`NAMESPACE\_OF\_YOUR\_TENANCY\`/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`。在执行 docker 登录时，您需要使用租户名称空间和用户名（小写）。在此处，将 \`NAMESPACE\_OF\_YOUR\_TENANCY\` 替换为租户的名称空间，将 \`YOUR\_ORACLE\_CLOUD\_USERNAME\` 替换为您的 Oracle Cloud 账户用户名，然后从文本编辑器复制替换的用户名并将其粘贴到 \*Cloud Shell\* 中。对于“Password（密码）”，请从文本编辑器或保存身份验证令牌的位置粘贴。

4.  返回到容器注册表，选择_汉堡菜单 -> 开发人员服务 -> 容器注册表_。
    
    ![容器注册表](images/container-registry.png " ")
    
5.  选择区间，然后单击_创建资料档案库_。
    
    ![创建资料档案库](images/repository-create.png " ")
    
6.  选择区间并输入 _`helidon-stock-application-your_first_name`_ 作为资料档案库名称，然后选择“访问权限”作为_公共_，然后单击_创建_。
    
    ![资料档案库数据](images/repository-data.png " ")
    
    创建系统信息库 _`helidon-stock-application-your_first_name`_ 后，您可以验证名称空间，它应与租户的名称空间相同。
    
    ！［验证名称空间］（图像/验证 -namespace.png" "）
    
7.  在实验 5 中，您已将 Docker 映像全名复制到文本编辑器。要将 Docker 映像推送到 Oracle Cloud 容器注册表内的资料档案库，请在文本编辑器中复制并粘贴以下命令，然后将 _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`helidon-stock-application-your_first_name`：1.0_ 替换为已保存在文本编辑器中的 Docker 映像全名。
    
        <copy>docker push `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/helidon-stock-application-your_first_name:1.0</copy>
        
    
    ![Docker 推送](images/docker-push.png " ")
    
    成功运行 _dockerush_ 命令后，展开 _`helidon-stock-application-your_first_name`_ 系统信息库，您会发现在此系统信息库中上载了新映像。
    
    将 _Cloud Shell_ 和容器注册表存储库页保持打开状态；在接下来的实验中，我们需要它们。
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 3 月