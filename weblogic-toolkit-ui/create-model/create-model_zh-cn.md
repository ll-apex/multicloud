# 修改 WKT UI 项目并创建模型文件

## 简介

在本练习中，我们将探讨内部部署 WebLogic 域。我们导航到管理控制台以查看 _test-domain_ 中部署的应用程序、数据源和服务器。我们还打开预先创建的 _`base_project.wktproj`_ ，该 _Project Settings_ 部分已有预填充值。然后，我们通过预检查脱机内部部署域来创建模型文件。最后，我们验证模型并准备要部署在 Oracle Kubernetes 集群 (OKE) 上的模型。

估计时间：15 分钟

观看下面的视频，快速浏览实验室。[在 OCI 上为 OKE 创建模型](videohub:1_qdch3qqg)

### 目标

在本练习中，您将：

*   浏览内部部署 WebLogic 域 _test-domain_ 。
*   打开基本 WKT 项目。
*   脱机内部部署域的自测。
*   验证和准备模型。

### 先备条件

要运行此实验，您必须：

*   访问在实验 2 中创建的 noVNC 远程桌面。

## 任务 1：浏览内部部署域

在此任务中，我们使用 WebLogic 管理控制台浏览内部部署 _test-domain_ 中的资源。

1.  在左侧，单击_箭头图标_。 ![剪贴板](images/clipboard.png)

> **重要提示** \- 您可以查看_剪贴板_，以便在主机和远程桌面之间复制和粘贴，我们使用_剪贴板_。例如，如果要从主机复制并将其粘贴到远程桌面中，则需要先将其粘贴到剪贴板中，然后才能将其粘贴到远程桌面中。再次单击_箭头图标_可隐藏_设置_选项。

2.  单击 _Oracle WebLogic Server_ 选项卡并输入 _weblogic/Welcome1%_ 作为 `Username/Password`，然后单击_登录_。您可以看到，我们有 WebLogic 服务器版本 _12.2.1.3.0_ 。  
    ![登录管理控制台](images/login-admin-console.png)
    
3.  要查看可用服务器，请展开_环境_，然后单击_服务器_。您可以看到，我们有一个包含 5 个托管服务器的动态集群。 ![查看服务器](images/view-servers.png)
    
4.  要查看数据源，请展开_服务_，然后单击_数据源_。 ![查看数据源](images/view-datasources.png)
    
5.  要查看部署的应用程序，请单击_部署_。您可以看到，我们有 _opdemo_ 作为已部署的应用程序。 ![查看部署](images/view-deployments.png)
    

## 任务 2：打开基本 WKT UI 项目

为了简化实验，我们创建了 _`base_project.wktproj`_ ，它预设了 docker、Java、Oracle 主目录和主映像标记的位置。在此任务中，我们打开 _`base_project.wktproj`_ 项目。

1.  单击_活动_，然后在搜索框中键入 **WebLogic** 。单击 _WebLogic Kubernetes 工具包 UI_ 的图标。 ![打开 WKTUI](images/open-wktui.png)
    
2.  要打开 _base\_project.wktproj_ 项目，请单击_文件_ -> _打开项目_。 ![打开项目](images/open-project.png)
    
3.  单击左侧的_下载_，然后选择 _base\_project.wktproj_ ，然后单击_打开项目_。 ![项目位置](images/project-location.png)
    
    > **For your information only:**  
    > As _Credential Story Policy_, we select **Store in Native OS Credentials Store**. It means the credentials (like for WebLogic Server and datasources) are only stored on the local machine.  
    > For _Where would you like the target Oracle Fusion Middleware domain to live?_, we select **Created in the container from the model in the image**. In this case, the set of model-related files are added to the image. So when the WebLogic Kubernetes Operator domain object is deployed, its inspector process runs and creates the WebLogic Server domain inside a running container on-the-fly.  
    > ![项目设置](images/project-settings.png) As _Kubernetes Environment Target Type_, we select **WebLogic Kubernetes Operator**. This means, you want this domain to be deployed in Kubernetes managed by the WebLogic Kubernetes Operator. This settings also determine what sections and their associated actions within the application, to display.  
    > we also specify the location for _JAVA HOME_ and _ORACLE\_HOME_. WebLogic Kubernetes Toolkit UI uses this directory when invoking the WebLogic Deployer Tooling and WebLogic Image Tool.  
    > To build new images, inspect images and interact with image repositories, the WKT UI application uses an image build tool, which defaults to docker.  
    > ![Kubernetes 集群类型](images/kubernetes-cluster-type.png)
    
4.  输入 _welcome1_ 作为 **Password** ，然后单击 _Unlock_ 。 ![开锁](images/unlock.png)
    

## 任务 3：脱机内部部署域的自测

在此任务中，我们将对内部部署域执行自测，这将创建由域配置组成的模型文件。

1.  在 WebLogic Kubernetes 工具包 UI 中，单击_模型_。 ![型号](images/click-model.png)
    
2.  单击_文件_ -> _添加模型_ -> _搜索模型（脱机）_。 ![搜索模型](images/discover-model.png)
    
3.  单击“打开”文件夹_图标_可打开_域主页_。 ![打开域 Hom](images/open-domain-home.png)
    
4.  在 "Home" 文件夹中，导航到 _`/home/opc/Oracle/Middleware/Oracle_Home/user_projects/domains/`_ 目录并选择 _test-domain_ 文件夹，然后单击 _Select_ 。单击_确定_。![导航位置](images/navigate-location.png) ![指定位置](images/specify-location.png)
    
    > 如果您在控制台中查看，您将看到这会调用 WebLogic 部署器工具以在脱机模式下自测域配置。
    
5.  您可以看到如下所示的窗口，最后，您将准备好模型。 ![查看模型](images/view-model.png)
    
    > 此 WDT 自测的结果是 model（域配置的元数据表示）、占位符，您可以在其中指定应用程序归档中的值（如数据源的口令）和应用程序。
    

## 任务 4：验证和准备模型

在此任务中，我们验证模型并准备要部署在 Oracle Kubernetes 集群 (OKE) 上的模型。

1.  单击_代码视图_并验证模型，单击_验证模型_。 ![验证模型](images/validate-model.png)
    
    > **仅供参考：**  
    > 验证模型将调用 WDT [验证模型工具](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/validate/)，该工具验证模型及其相关构件的格式是否正确，并为特定模型位置的有效属性和子文件夹提供帮助。
    
2.  看到_验证模型完成_窗口后，单击_确定_。 ![验证完成](images/validate-complete.png)
    
3.  要准备要部署在 Kubernetes 集群上的模型，请单击_准备模型_ ![准备模型](images/prepare-model.png)
    
    > **仅供参考：**  
    > 准备模型调用 WDT [Prepare Model Tool](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/prepare/) 以修改模型，以便在安装了 WebLogic Kubernetes Operator 或 Verrazzano 的 Kubernetes 集群中工作。  
    > “准备模型”执行以下操作：
    
    *   删除与目标环境不兼容的模型部分和字段。
    *   使用引用变量的模型标记替换端点值。
    *   使用引用 Kubernetes 密钥或变量中的字段的模型标记替换身份证明值。
    *   为应用程序变量、变量覆盖和密钥编辑器中显示的字段提供默认值。
    *   将拓扑信息提取到其用于生成用于部署域的资源文件的应用程序。
4.  看到_准备模型完成_窗口后，单击_确定_。 ![准备完成](images/prepare-complete.png)
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月