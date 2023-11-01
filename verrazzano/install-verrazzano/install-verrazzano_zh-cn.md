# 安装 Verrazzano

## 简介

此实验将指导您完成在 Oracle Cloud Infrastructure 中的 Kubernetes 集群上安装 Verrazzano 的步骤。

估计时间：20 分钟

### 关于产品/技术

Verrazzano 是一个端到端的企业容器平台，用于在多云和混合环境中部署云原生和传统应用。它由一系列精心策划的开源组件组成 - 许多您可能已经使用和信任，以及一些专门为将 Verrazzano 成为统一且易于使用的平台而编写的部件。

Verrazzano 包含以下功能：

*   混合和多集群负载管理
*   WebLogic、Coherence 和 Helidon 应用程序的特殊处理
*   多集群基础结构管理
*   集成和预有线应用监视
*   集成安全性
*   DevOps 和 GitOps 启用

### 目标

在本练习中，您将：

*   安装 Verrazzano 命令行工具。
*   安装 Verrazzano 的开发 (`dev`) 配置文件。
*   验证成功的 Verrazzano 安装。

### 先备条件

Verrazzano 需要以下项：

*   Kubernetes 集群和兼容的 `kubectl`。
*   Kubernetes worker 节点上至少有 2 个 CPU、100GB 磁盘存储和 16GB RAM。这足以安装 Verrazzano 的开发配置文件。根据您部署的应用程序的资源需求，这可能不足以部署应用程序。

在实验 1 中，您在 Oracle Cloud Infrastructure 上创建了一个 Kubernetes 集群。您将使用 Kubernetes 集群 \*cluster1\* 来安装 Verrazzano 的开发配置文件。 在实验 1 中，您创建了配置文件来访问 Oracle Cloud Infrastructure 上的 Kubernetes 集群。您将使用 Kubernetes 集群 \*cluster1\* 来安装 Verrazzano 的开发配置文件。

## 任务 1：安装 vz CLI

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
        

## 任务 2：安装 Verrazzano 开发配置文件

安装配置文件是一种众所周知的 Verrazzano 设置配置，可以通过名称进行引用，然后可以根据需要进行定制。

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
        \> 完成安装大约需要 15 到 20 分钟。此命令安装 Verrazzano 平台运算符并应用 Verrazzano 定制资源。 \> 完成安装大约需要 8 到 10 分钟。此命令安装 Verrazzano 平台操作员并应用 Verrazzano 定制资源。
2.  等待安装完成。安装日志将流式传输到命令窗口，直到安装完成或达到默认超时 (30m)。
    

## 任务 3：验证成功的 Verrazzano 安装

Verrazzano 在多个名称空间中安装多个对象。Verrazzano 组件安装在名称空间 _verrazzano-system_ 中。

1.  请验证与多个对象关联的所有云池的状态是否为_正在运行_。所有云池都将处于_正在运行_状态。
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    输出结果应如下所示：
    
        $   kubectl get pods -n verrazzano-system
        NAME                                    READY   STATUS  RESTARTS AGE
        coherence-operator-b5dc669c6-rk2sm      1/1     Running   1       23m
        fluentd-54f5x                           2/2     Running   1       11m
        fluentd-h7mgh                           2/2     Running   1       11m
        fluentd-xcdfz                           2/2     Running   0       11m
        oam-kubernetes-runtime-5b48f944b-cx7b9  1/1     Running   0       24m
        verrazzano-application-operator-665c5   1/1     Running   0       22m
        verrazzano-application-operator-webhook 1/1     Running   0       22m
        verrazzano-authproxy-67776ff58b-8l      3/3     Running   0       21m
        verrazzano-cluster-operator-67dc5695    1/1     Running   0       14m
        verrazzano-cluster-operator-webhook     1/1     Running   0       14m
        verrazzano-console-7d95c98cb9-9ql2      2/2     Running   0       20m
        verrazzano-monitoring-operator-59f      2/2     Running   0       21m
        vmi-system-es-master-0                  2/2     Running   0       19m
        vmi-system-grafana-7fd956b585-2tbgl     3/3     Running   0       19m
        vmi-system-kiali-dd87546d6-ddxss        2/2     Running   0       22m
        vmi-system-osd-7687d6fccf-nm7kt         2/2     Running   0       14m
        weblogic-operator-54979449f4-njgrq      2/2     Running   0       22m
        weblogic-operator-webhook-f7ff8c8cf     1/1     Running   0       22m
        
    
    将 _Cloud Shell_ 保持打开状态；我们需要它来支持 Lab 3。
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月