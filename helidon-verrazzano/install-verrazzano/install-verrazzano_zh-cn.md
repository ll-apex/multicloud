# 安装 Verrazzano

## 简介

此实验将指导您完成在 Oracle Cloud Infrastructure 中的 Kubernetes 集群上安装 Verrazzano 的步骤。

估计时间：15 分钟

### 关于产品/技术

Verrazzano 是一个端到端的企业容器平台，用于在多云和混合环境中部署云原生和传统应用。它由一系列精心策划的开源组件组成 - 许多您可能已经使用和信任，以及一些专门为将 Verrazzano 成为统一且易于使用的平台而编写的组件。

Verrazzano 包含以下功能：

*   混合和多集群负载管理
*   WebLogic、Coherence 和 Helidon 应用程序的特殊处理
*   多集群基础结构管理
*   集成和预有线应用监视
*   集成安全性
*   DevOps 和 GitOps 启用

Verrazzano 支持以下安装配置文件：开发 (`dev`)、生产 (`prod`) 和托管群集 (`managed-cluster`)。

下图介绍了 Verrazzano 安装配置文件。 ![安装概要信息](images/installprofile.png)

要更改以下任一命令中的配置文件，请将 _VZ\_PROFILE_ 环境变量设置为要安装的配置文件的名称。

有关 Verrazzano 配置选项的完整说明，请参见 [Verrazzano 定制资源定义](https://verrazzano.io/docs/reference/api/verrazzano/verrazzano/)。

在本练习中，我们将安装 _Verrazzano 的开发配置文件_，该配置文件具有以下特征：

*   通配符 (nip.io) DNS
*   自签名证书
*   系统组件和所有应用程序使用的共享可观察性堆栈
*   可观察性堆栈的临时存储（如果 pod 重新启动，则会丢失所有日志和度量）
*   它有一个轻量级安装。
*   用于评估。
*   单节点 Opensearch 集群拓扑。

Verrazzano 安装一组精选的开源组件。下表列出了每个组件、其版本和简要说明。

![Verrazzano 配置文件](images/verrazzano-profile.png " ") ![Verrazzano 配置文件](images/verrazzano-profiles.png " ")

根据 DNS 的选择，我们可以使用 nip.io（通配符 DNS）或 [Oracle OCI DNS](https://docs.cloud.oracle.com/en-us/iaas/Content/DNS/Concepts/dnszonemanagement.htm) 。在本练习中，我们将使用 nip.io（通配符 DNS）进行安装。

入站控制器是一种有助于提供对外部世界的 Docker 容器的访问（通过提供 IP 地址）。该入站将 IP 地址路由到不同的群集。

### 目标

在本练习中，您将：

*   设置 `kubectl` 以使用 Oracle Kubernetes 引擎集群
*   安装 Verrazzano vz CLI。
*   安装 Verrazzano 的开发 (`dev`) 配置文件。

### 先备条件

Verrazzano 需要以下项：

*   Kubernetes 集群和兼容的 `kubectl`。
*   Kubernetes worker 节点上至少有 2 个 CPU、100GB 磁盘存储和 16GB RAM。这足以安装 Verrazzano 的开发配置文件。根据您部署的应用程序的资源需求，这可能不足以部署应用程序。
*   在实验 1 中，您在 Oracle Cloud Infrastructure 上创建了一个 Kubernetes 集群。您将使用该 Kubernetes 集群 _cluster1_ 来安装 Verrazzano 的开发配置文件。

## 任务 1：配置 `kubectl` (Kubernetes Cluster CLI)

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
        10.0.10.101   Ready    node    11m   v1.26.2
        10.0.10.29    Ready    node    11m   v1.26.2
        10.0.10.37    Ready    node    11m   v1.26.2
        
    
    > 如果看到节点的信息，则配置成功。
    

## 任务 2：安装 Verrazzano CLI

1.  下载最新的 vz CLI。
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz</copy>
        
    
    输出结果应如下所示：
    
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
        

0 0 0 0 0 0 0 0 --:--:--:--:--:--:--:--:-- 0 100 39.7M 100 39.7M 0 0 0 23.6M 0 0:00:01 0:00:01 --:--:-- 32.7M \`\`

2.  下载校验和文件。
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        

输出结果应如下所示：

    ```bash
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    

0 0 0 0 0 0 0 0 --:--:--:--:--:--:--:--:-- 0 100 100 100 102 0 0 0 218 0 --:--:--:--:--:-- 218 \`\`\`

3.  根据校验和文件验证二进制文件。
    
        <copy>sha256sum -c verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        
    
    输出结果应如下所示：
    
        verrazzano-1.6.2-linux-amd64.tar.gz: OK
        
4.  解压缩并移至 vz 二进制目录，
    
        <copy>tar xvf verrazzano-1.6.2-linux-amd64.tar.gz
        cd ~/verrazzano-1.6.2/bin/</copy>
        
5.  测试以确保您安装的版本是最新的。
    
        <copy>./vz version</copy>
        
    
    输出结果应如下所示：
    
        Version: v1.6.2
        BuildDate: 2023-07-21T12:06:02Z
        GitCommit: 5cd8a2f8670000d2ef0b02404e3169699d87f26b
        

## 任务 3：安装 Verrazzano 开发配置文件

安装配置文件是一种众所周知的 Verrazzano 设置配置，可以通过名称进行引用，然后可以根据需要进行定制。

Verrazzano 支持以下安装配置文件：开发 (`dev`)、生产 (`prod`) 和托管群集 (`managed-cluster`)。

1.  使用 nip.io DNS 方法进行安装。复制以下命令并将其粘贴到 _Cloud Shell_ 中以安装 Verrazzano。
    
        <copy>./vz install -f - <<EOF
        apiVersion: install.verrazzano.io/v1beta1
        kind: Verrazzano
        metadata:
          name: example-verrazzano
        spec:
          profile: dev
        EOF
        </copy>
        
    
    输出结果应如下所示：
    
        Installing Verrazzano version v1.6.2
        Applying the file https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-platform-operator.yaml
        customresourcedefinition.apiextensions.k8s.io/verrazzanos.install.verrazzano.io created
        namespace/verrazzano-install created
        serviceaccount/verrazzano-platform-operator created
        clusterrole.rbac.authorization.k8s.io/verrazzano-managed-cluster created
        clusterrolebinding.rbac.authorization.k8s.io/verrazzano-platform-operator created
        service/verrazzano-platform-operator created
        service/verrazzano-platform-operator-webhook created
        deployment.apps/verrazzano-platform-operator created
        deployment.apps/verrazzano-platform-operator-webhook created
        mutatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-mysql-backup created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-operator-webhook created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-mysqlinstalloverrides created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-requirements-validator created
        Waiting for verrazzano-platform-operator to be ready before starting install - 23 seconds
        
    
    > 完成安装大约需要 15 到 20 分钟。此命令安装 Verrazzano 平台操作员并应用 Verrazzano 定制资源。安装日志将流式传输到命令窗口，直到安装完成或达到默认超时 (30m)。
    
2.  使 _Cloud Shell_ 保持打开状态，并使安装运行。
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月