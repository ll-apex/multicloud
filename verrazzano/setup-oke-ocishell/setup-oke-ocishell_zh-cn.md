# 在 Oracle Cloud Infrastructure 上设置 Oracle Kubernetes 引擎实例

## 简介

此实验将指导您完成在 Oracle Cloud Infrastructure 上创建托管 Kubernetes 环境的步骤。

估计时间：15 分钟

### 关于产品/技术

Oracle Cloud Infrastructure Container Engine for Kubernetes 是一项完全托管、可扩展的高可用性服务，可用于将容器应用部署到云中。开发团队想要可靠地构建、部署和管理云原生应用时，请使用 Kubernetes 容器引擎（有时简称为 OKE）。指定您的应用程序所需的计算资源，OKE 在现有 OCI 租户中的 Oracle Cloud Infrastructure 上预配这些资源。

### 目标

在本练习中，您将：

*   创建 OKE（Oracle Kubernetes 引擎）实例。
*   打开 OCI Cloud Shell 并配置 `kubectl` 以与 Kubernetes 集群交互。

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。

要创建 Kubernetes (OKE) 的容器引擎，请完成以下步骤：

*   创建网络资源（VCN、子网、安全列表等）。
*   创建群集。
*   创建 `NodePool`。

此实验展示了_快速入门_功能如何为三节点 Kubernetes 集群创建和配置所有必需的资源。所有节点都将部署在不同的可用性域中，以确保高可用性。

有关 OKE 和定制群集部署的更多信息，请参见 [Oracle Container Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm) 文档。

## 任务 1：创建 OKE 群集

_快速创建_功能使用默认设置，根据需要使用新的网络资源创建_快速集群_。此方法是创建新集群的最快方法。如果您接受所有默认值，只需单击几下即可创建新集群。将自动创建集群的新网络资源以及一个节点池和三个 worker 节点。

1.  在控制台中，选择 _Hamburger Menu -> Developer Services -> Kubernetes Clusters (OKE)_ ，如下所示。
    
    ![汉堡菜单](images/hamburger-menu.png " ")
    
2.  在“集群列表”页中，选择所选区间，允许您创建集群，然后单击_创建集群_。
    
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
    *   **配置**：用于节点池中每个节点的配置。配置确定分配给每个节点的 CPU 数和内存量。此列表仅显示您的租户中 OKE 支持的那些配置。选择 _VM.Standard.E4。Flex_ （通常在 Oracle 免费套餐账户中提供）。选择 2 个 OCPU 和 32 GB 作为内存量。
    *   **映像**：选择 Kubernetes 版本为 _1.26.2_ 的默认映像。
    *   **节点数**：要创建的 worker 节点数。保留默认值 _3_ 。
    
    > _请非常小心不离开默认形状；默认形状太小，无法适应所有 VERRAZZANO 组件_
    
    ![快速集群](images/quick-cluster.png " ") ![节点编号](images/node-number.png " ")
    
4.  单击_下一步_可查看为新集群输入的详细信息。
    
5.  在_复查_页上，选中_创建基本集群_的复选框，然后单击_创建集群_以创建新的网络资源和新集群。
    
    ![复查集群](images/review-cluster.png " ")
    
    > 您将看到要为您创建的网络资源。等待启动创建节点池的请求，然后单击_关闭_。
    
    ![网络资源](images/network-resource.png " ")
    
    > 然后，新群集将显示在 _Cluster Details_ 页面上。创建主节点时，新群集的状态为_活动_（大约需要 7 分钟）。然后，您可以继续练习。
    
    ![集群预配](images/cluster-provision.png " ")
    
    ![集群访问](images/cluster-access.png " ")
    

## 任务 2：配置 `kubectl` (Kubernetes Cluster CLI)

Oracle Cloud Infrastructure (OCI) Cloud Shell 是基于 Web 浏览器的终端，可从 Oracle Cloud 控制台访问。Cloud Shell 通过预先验证的 Oracle Cloud Infrastructure CLI 和其他有用的工具（ _Git、kubectl、helm、OCI CLI_ ）可以访问 Linux shell 以完成 Verrazzano 教程。可以从控制台访问 Cloud Shell。Cloud Shell 将作为控制台的持久框架显示在 Oracle Cloud 控制台中，并且在导航到控制台的不同页面时保持活动状态。

您将使用 _Cloud Shell_ 完成此研讨会。

我们将使用 `kubectl` 通过 Cloud Shell 远程管理集群。它需要 `kubeconfig` 文件。这将使用预先验证的 OCI CLI 生成，因此在开始使用之前无需进行任何设置。

1.  单击集群详细信息页上的_访问集群_。
    
    > 如果离开该页面，则打开导航菜单，然后在_开发人员服务_下选择 _Kubernetes 集群 (OKE)_ 。选择集群并转至详细信息页。
    
    ![访问集群](images/access-cluster.png " ")
    
    > 此时将显示一个对话框，您可以在其中打开 Cloud Shell 并包含需要运行的定制 OCI 命令以创建 Kubernetes 配置文件。
    
2.  保留默认的 _Cloud Shell 访问_，然后首先选择_复制_链接将 `oci ce...` 命令复制到 Cloud Shell。
    
    ![复制 kubectl 配置](images/copy-config.png " ")
    
3.  现在，单击_启动 Cloud Shell_ 以打开内置控制台。然后关闭配置对话框，然后将命令粘贴到 _Cloud Shell_ 中。
    
    ![启动 Cloud Shell](images/launch-cloudshell.png " ")
    
4.  将该命令从剪贴板（Ctrl+V 或右键单击并复制）复制到 Cloud Shell 并运行该命令。
    
    例如，该命令如下所示：
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![kubectl 配置](images/kube-config.png " ")
    
5.  现在，检查 `kubectl` 是否正常工作，例如，使用 `get node` 命令。您可能需要多次运行此命令，直到看到类似于以下内容的输出。
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.192   Ready    node    11m   v1.26.2
        10.0.10.219   Ready    node    11m   v1.26.2
        10.0.10.90    Ready    node    11m   v1.26.2
        
    
    > 如果看到节点的信息，则配置成功。
    
6.  可以使用 Cloud Shell 右上角的控件随时最小化和恢复终端大小。
    
    ![云 shell](images/cloudshell.png " ")
    

将此 _Cloud Shell_ 保持打开状态；我们将将其用于更多实验。

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月