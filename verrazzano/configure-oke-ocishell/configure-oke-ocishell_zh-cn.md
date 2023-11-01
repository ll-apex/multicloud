# 配置 KUBECTL 以与 Oracle Cloud Infrastructure (OCI) 上的 Oracle Container Engine for Kubernetes (OKE) 交互

## 简介

此实验将指导您完成创建配置文件的步骤，这些步骤允许访问 Oracle Cloud Infrastructure 上的 Kubernetes 环境。

### 关于产品/技术

Oracle Cloud Infrastructure Container Engine for Kubernetes 是一项完全托管、可扩展的高可用性服务，可用于将容器应用部署到云中。开发团队想要可靠地构建、部署和管理云原生应用时，请使用 Kubernetes 容器引擎（有时简称为 OKE）。指定您的应用程序所需的计算资源，OKE 在现有 OCI 租户中的 Oracle Cloud Infrastructure 上预配这些资源。

### 目标

在本练习中，您将：

*   打开 OCI Cloud Shell 并配置 `kubectl` 以与 Kubernetes 集群交互。

### 先备条件

*   本研习会假定您已就 LiveLabs 预留此研习会。

## 任务 1：配置 `kubectl` (Kubernetes Cluster CLI)

Oracle Cloud Infrastructure (OCI) Cloud Shell 是基于 Web 浏览器的终端，可从 Oracle Cloud 控制台访问。Cloud Shell 通过预先验证的 Oracle Cloud Infrastructure CLI 和其他有用的工具（ _Git、kubectl、helm、OCI CLI_ ）可以访问 Linux shell 以完成 Verrazzano 教程。可以从控制台访问 Cloud Shell。Cloud Shell 将作为控制台的持久框架显示在 Oracle Cloud 控制台中，并且在导航到控制台的不同页面时保持活动状态。

您将使用 _Cloud Shell_ 完成此研讨会。

我们将使用 `kubectl` 通过 Cloud Shell 远程管理集群。它需要 `kubeconfig` 文件。这将使用预先验证的 OCI CLI 生成，因此在开始使用之前无需进行任何设置。

1.  在控制台中，选择 _Hamburger Menu -> Developer Services -> Kubernetes Clusters (OKE)_ ，如下所示。
    
    ![汉堡菜单](../setup-oke-ocishell/images/hamburgermenu.png " ")
    
2.  选择要分配给您的区间。然后单击 _cluster1_ 群集。
    
3.  单击集群详细信息页上的_访问集群_。
    
    ![访问集群](../setup-oke-ocishell/images/accesscluster.png " ")
    
    > 此时将显示一个对话框，您可以在其中打开 Cloud Shell 并包含需要运行的定制 OCI 命令以创建 Kubernetes 配置文件。
    
4.  保留默认的 _Cloud Shell 访问_，然后首先选择_复制_链接将 `oci ce...` 命令复制到 Cloud Shell。
    
    ![复制 kubectl 配置](../setup-oke-ocishell/images/copyconfig.png " ")
    
5.  现在，单击_启动 Cloud Shell_ 以打开内置控制台。然后关闭配置对话框，然后将命令粘贴到 _Cloud Shell_ 中。
    
    ![启动 Cloud Shell](../setup-oke-ocishell/images/launchcloudshell.png " ")
    
6.  将该命令从剪贴板（Ctrl+V 或右键单击并复制）复制到 Cloud Shell 并运行该命令。
    
    例如，该命令如下所示：
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![kubectl 配置](../setup-oke-ocishell/images/kubeconfig.png " ")
    
7.  现在，检查 `kubectl` 是否正常工作，例如，使用 `get node` 命令。您可能需要多次运行此命令，直到看到类似于以下内容的输出。
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.17    Ready    node    11m   v1.21.5
        10.0.10.184   Ready    node    11m   v1.21.5
        10.0.10.31    Ready    node    11m   v1.21.5
        
    
    > 如果看到节点的信息，则配置成功。
    
8.  可以使用 Cloud Shell 右上角的控件随时最小化和恢复终端大小。
    
    ![云 shell](../setup-oke-ocishell/images/cloudshell.png " ")
    

将此 _Cloud Shell_ 保持打开状态；我们将将其用于更多实验。

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 2 月