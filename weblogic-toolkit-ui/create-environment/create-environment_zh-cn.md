# 设置实验室环境

## 简介

在本练习中，您将创建一个配置了所有必要网络资源的 3 节点 Kubernetes 集群。此外，您将在 Oracle Cloud 容器映像注册表中创建资料档案库。然后，您将生成验证令牌。此外，您还将接受 Oracle 容器注册表中 WebLogic 服务器映像的许可协议。

估计时间：15 分钟

观看下面的视频，快速浏览实验室。[设置实验室环境](videohub:1_zhvohpqq)

### 目标

在本练习中，您将：

*   创建 Oracle Kubernetes 集群。
*   在 Oracle Cloud 容器映像注册表中创建资料档案库。
*   生成验证标记。
*   接受 Oracle 容器注册表中 WebLogic 服务器映像的许可证。

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。
*   您必须具有 Oracle 账户。
*   您应该具有文本编辑器。

## 任务 1：创建 Oracle Kubernetes 集群

_快速创建_功能使用默认设置，根据需要使用新的网络资源创建_快速集群_。此方法是创建新集群的最快方法。如果您接受所有默认值，只需单击几下即可创建新集群。将自动创建集群的新网络资源以及一个节点池和三个 worker 节点。

1.  在控制台中，选择 _Hamburger Menu -> Developer Services -> Kubernetes Clusters (OKE)_ ，如下所示。
    
    ![汉堡菜单](images/hamburger-menu.png " ")
    
2.  在“集群列表”页中，选择区间，然后单击_创建集群_。
    
    > 您需要选择允许在其中创建集群的区间，以及 Oracle 容器注册表内的资料档案库。
    
    ![选择区间](images/select-compartment.png " ")
    
3.  在“创建集群解决方案”对话框中，选择_快速创建_并单击_提交_。
    
    ![启动工作流](images/launch-workflow.png " ")
    
    _快速创建_将创建具有默认设置的新集群以及新集群的新网络资源。
    
    在“集群创建”页上指定以下配置详细信息（请注意您在_配置_字段中放置的值）：
    
    *   **名称**：集群的名称。保留默认值。
    *   **区间**：区间的名称。选择允许您在其中创建资源的区间。
    *   **Kubernetes 版本**：Kubernetes 的版本。选择 _1.26.2_ 作为 Kubernetes 版本。
    *   **Kubernetes API 端点**：集群主节点是否可路由。选择_公共端点_值。
    *   **节点类型**：选择_托管_作为节点类型。
    *   **Kubernetes Worker 节点**：集群 worker 节点是否可路由。保留默认_私人员工_值。
    *   **配置**：用于节点池中每个节点的配置。配置确定分配给每个节点的 CPU 数和内存量。此列表仅显示您的租户中 OKE 支持的那些配置。选择 _VM.Standard.E4。Flex_ （通常在 Oracle 免费套餐账户中提供）。选择 1 个 OCPU 和 16 GB 作为内存量。
    *   **映像**：选择 Kubernetes 版本为 _1.26.2_ 的默认映像。
    *   **节点数**：要创建的 worker 节点数。保留默认值 _3_ 。
    
    ![快速集群](images/quick-cluster1.png " ") ![输入数据](images/enter-data.png " ")
    
4.  单击_下一步_可查看为新集群输入的详细信息。
    
5.  在_复查_页上，选中_创建基本集群_的复选框，然后单击_创建集群_以创建新的网络资源和新集群。
    
    ![复查集群](images/review-cluster.png " ")
    
    > 您将看到要为您创建的网络资源。等待启动创建节点池的请求，然后单击_关闭_。
    
    ![网络资源](images/network-resource.png " ")
    
    > 然后，新群集将显示在 _Cluster Details_ 页面上。创建主节点时，新群集的状态为_活动_（大约需要 7 分钟）。您不需要等待，请继续执行下一个任务。
    
    ![集群访问](images/cluster-access.png " ")
    

## 任务 2：创建资源库

在此任务中，您将创建公共资料档案库。在练习 5 中，我们将辅助映像推送到此资料档案库。

1.  在控制台中，选择 _Hamburger Menu_ -> _Developer Services_ -> _Container Registry_ ，如下所示。 ![容器注册表](images/container-registry.png)
    
2.  选择您的区间，允许您在其中创建资料档案库。单击_创建资料档案库_。 ![创建资料档案库](images/create-repository.png)
    
3.  输入 _`test-model-your_firstname`_ 作为资料档案库名称，并以_公共_身份输入访问权限，然后单击_创建_。 ![资料档案库详细资料](images/repository-details.png)
    
4.  资料档案库就绪后。请注意文本编辑器内文本文件中的租户名称空间。 ![附注租户 NameSpace](images/tenancy-namespace.png)
    

## 任务 3：生成验证令牌

在此任务中，我们将生成_验证令牌_。在练习 5 中，我们将使用此验证令牌将辅助映像推送到 Oracle Cloud 容器注册表资料档案库。

1.  选择右上角的用户图标，然后选择 _MyProfile_ 。
    
    ![我的配置文件](images/my-profile.png)
    
2.  向下滚动并选择 _Auth Tokens_ ，然后单击 _Generate Token（生成令牌）_。
    
    ![验证标记](images/auth-token.png)
    
3.  复制 _`test-model-your_first_name`_ 并将其粘贴到_说明_框中，然后单击_生成令牌_。
    
    ![创建标记](images/create-token.png)
    
4.  在“生成的令牌”下选择_复制_并将其粘贴到文本编辑器中。我们以后不能复制它。单击_关闭_。
    
    ![复制标记](images/copy-token.png)
    

## 任务 4：接受 WebLogic 服务器映像的许可证

在此任务中，我们接受 WebLogic 服务器映像的许可协议位于 Oracle 容器注册表中。与实验 3 中一样，我们将使用 WebLogic Server 12.2.1.3.0 映像作为主映像。因此，要访问 WebLogic 服务器映像，我们接受许可协议。

1.  单击 Oracle 容器注册表 [https://container-registry.oracle.com/](https://container-registry.oracle.com/) 的链接并登录。为此，您需要一个 Oracle 帐户。 ![容器注册表登录](images/container-registry-sign-in.png)
    
2.  在“用户名和密码”字段中输入 _Oracle 帐户凭据_，然后单击_登录_。 ![登录容器注册表](images/login-container-registry.png)
    
3.  在 Oracle 容器注册表的主页中，搜索 _weblogic_ 。 ![搜索 WebLogic](images/search-weblogic.png)
    
4.  按所示单击 _weblogic_ 并选择 _English_ 作为语言，然后单击 _Continue_ 。![单击 WebLogic](images/click-weblogic.png) ![选择语言](images/select-language.png)
    
5.  单击_接受_接受许可协议。 ![接受许可证](images/accept-license.png)
    

## 了解详细信息

_关于 Oracle Cloud Infrastructure Container Engine for Kubernetes_

Oracle Cloud Infrastructure Container Engine for Kubernetes 是一项完全托管、可扩展的高可用性服务，可用于将容器应用部署到云中。开发团队想要可靠地构建、部署和管理云原生应用时，请使用 Kubernetes 容器引擎（有时简称为 OKE）。指定您的应用程序所需的计算资源，OKE 在现有 OCI 租户中的 Oracle Cloud Infrastructure 上预配这些资源。

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月