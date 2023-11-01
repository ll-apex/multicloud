# 创建辅助映像并将其推送到 Oracle 容器映像注册表

## 简介

**主映像** - 包含 Oracle Fusion Middleware 软件的映像。它用作为域运行 WebLogic 服务器的所有容器的基础。

**辅助映像** \- 提供 WebLogic 部署工具软件和模型文件的映像。在运行时，辅助映像的内容将与主映像的内容合并。![图像结构](images/image-structure.png)

在此实验室中，我们使用 WebLogic 服务器 12.2.1.3.0-ol8 映像作为主映像。此外，我们还将创建辅助映像，并使用生成的验证令牌将其推送到 Oracle 容器映像注册表资料档案库。

估计时间：10 分钟

### 目标

在本练习中，您将：

*   创建辅助映像并将映像推送到 Oracle Cloud 容器映像注册表。

## 任务 1：准备辅助映像并推送辅助映像

在此任务中，我们将创建辅助映像，此映像将推送到 Oracle Cloud 容器注册表。

1.  单击_图像_。对于主映像，我们将使用下面的 _weblogic_ Image.So 将默认值保留在 _Primary Image_ 部分下方，如下所示
    
        <copy>container-registry.oracle.com/middleware/weblogic:12.2.1.3-ol8</copy>
        
    
    ![主要图像](images/primary-image.png)
    
    > **仅供参考：**  
    > 主映像是用于运行域的映像。一个主映像可用于数百个域。主映像包含 OS、JDK 和 FMW 软件安装。
    
2.  要创建辅助映像标记，我们需要以下信息：
    
    *   区域端点
    *   租户名称空间
3.  找到_区域的端点_。请参阅此 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 中记录的表。在所示的示例中，区域的端点是 _UK South（伦敦）_（作为区域名称），其端点是 _lhr.ocir.io_ 。找到您自己的_区域名称_的端点并将其保存在文本文件中。下一个实验室还需要它。
    
    ![端点](images/end-point.png " ")
    
    > 现在，您的区域同时具有租户名称空间和端点。
    
4.  在练习 3 中，您已注意到文本文件中的租户名称空间。如果不是，则要查找租户的名称空间，请选择_汉堡菜单_ -> _开发人员服务_ -> _容器注册表_，如下所示。选择您创建的资料档案库，您将按如下所示找到名称空间。 ![租户名称空间](images/tenancy-namespace.png)
    
5.  现在，您的区域同时具有租户名称空间和端点。复制以下命令并将其粘贴到您的文本文件中。然后将 `END_POINT_OF_YOUR_REGION` 替换为区域名称的端点 `NAMESPACE_OF_YOUR_TENANCY`，将其替换为租户的名称空间。单击_辅助映像_选项卡，如下所示。 ![辅助选项卡](images/auxiliary-tab.png)
    
        <copy>END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/test-model-your_first_name:v1</copy>
        

> 例如，在我的情况下，辅助映像标记是 `lhr.ocir.io/tenancynamespace/test-model-ankit:v1`。

6.  在步骤 4 中，您还确定了租户名称空间。按如下方式输入辅助映像注册表推送用户名：`NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`。  
    

*   将 `NAMESPACE_OF_YOUR_TENANCY` 替换为租户的名称空间
*   将 `YOUR_ORACLE_CLOUD_USERNAME` 替换为您的 Oracle Cloud 账户用户名，然后从文本文件复制替换的用户名并将其粘贴到_辅助映像注册表推送用户名_中。

> 例如，在我的情况下，**辅助映像注册表推送用户名**为 `tenancynamespace/lab.user@oracle.com`。

*   对于密码，请从文本文件复制并粘贴验证令牌（或者您保存的任何位置），然后将其粘贴到**辅助映像注册表推送用户名**中。 ![辅助映像详细信息](images/auxiliary-image-details.png)

7.  单击_创建辅助映像_。 ![创建辅助映像](images/create-auxiliary-image.png)
    
8.  由于已在练习 2 中准备模型，因此请单击_否_。 ![准备模型](images/prepare-model.png)
    
9.  选择要保存 _WebLogic 部署器_的_下载_文件夹，然后单击_选择_，如下所示。 ![WDT 地点](images/wdt-location.png)
    
10.  成功创建辅助映像后，在_创建辅助映像完成_窗口中单击_确定_。 ![已创建辅助](images/auxiliary-created.png)
    
    > **仅供参考：**  
    > 辅助映像特定于域。辅助映像包含定义域的数据。
    
11.  单击_推送辅助映像_可在 Oracle Cloud 容器映像注册表内的资料档案库中推送映像。 ![推送辅助](images/push-auxiliary.png)
    
12.  成功推送映像后，在_推送映像完成_窗口中，单击_确定_。 ![辅助推送](images/auxiliary-pushed.png)
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 10 月