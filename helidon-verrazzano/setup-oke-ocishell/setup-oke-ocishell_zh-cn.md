# 在 Oracle Cloud Infrastructure 上设置 Oracle Kubernetes 引擎实例

## 简介

在本练习中，您将创建一个配置了所有必要网络资源的 3 节点 Kubernetes 集群。这些节点将部署在不同的容错域中，以确保高可用性。

有关 OKE 和定制群集部署的更多信息，请参见 [Oracle Kubernetes Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm) 文档。

估计时间：15 分钟

### 关于产品/技术

Oracle Cloud Infrastructure Container Engine for Kubernetes 是一项完全托管、可扩展的高可用性服务，可用于将容器应用部署到云中。开发团队想要可靠地构建、部署和管理云原生应用时，请使用 Kubernetes 容器引擎（有时简称为 OKE）。指定您的应用程序所需的计算资源，OKE 在现有 OCI 租户中的 Oracle Cloud Infrastructure 上预配这些资源。

### 目标

您将使用_快速创建_集群功能创建具有所需网络资源、节点池和三个 worker 节点的 OKE（Oracle Kubernetes 引擎）实例。_快速创建_方法是创建新集群的最快方法。如果您接受所有默认值，只需单击几下即可创建新集群。

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。

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
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月