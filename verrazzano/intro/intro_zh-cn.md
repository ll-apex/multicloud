# 简介

## 关于此研讨会

此实验室展示了如何在单个 Kubernetes 集群上安装 Verrazzano 平台并使用 Verrazzano 概念部署示例应用程序。

[Bobby’s Books](https://verrazzano.io/docs/samples/bobs-books/) 示例应用程序包含三个主要部分：

*   后端“订单处理”应用，它是具有 REST 服务的 Java EE 应用，也是非常简单的 JSP UI，用于将数据存储在 MySQL 数据库中。此应用程序在 WebLogic 服务器上运行。
*   前端网店“Robert’s Books”，这是一家普通书卖家。这是作为 Helidon 微服务实施的，该服务从 Coherence 获取书籍数据，使用 Coherence 高速缓存存储为订单管理器保存数据，并具有 React Web UI。
*   前端网店“比比书”，是一家专业儿童书店。这是作为 Helidon 微服务实现的，该服务从（不同的）Coherence 高速缓存存储获取工作簿数据，与订单管理器直接交互，并在 WebLogic 服务器上运行 JSF Web UI。

### 关于产品/技术

Verrazzano 是一个端到端的企业容器平台，用于在多云和混合环境中部署云原生和传统应用。它由一系列精心策划的开源组件组成 - 许多您可能已经使用和信任，以及一些专门为将 Verrazzano 成为统一且易于使用的平台而编写的部件。

Verrazzano 包含以下功能：

*   混合和多集群负载管理
*   WebLogic、Coherence 和 Helidon 应用程序的特殊处理
*   多集群基础结构管理
*   集成和预有线应用监视
*   集成安全性
*   DevOps 和 GitOps 启用

![Verrazzano](images/verrazzano.png)

### 目标

1\. 在 Oracle Cloud Infrastructure 上设置 Oracle Kubernetes 引擎实例。 1\. 配置 \`kubectl\` 以与 Oracle Cloud Infrastructure 上的 Oracle Kubernetes Engine 实例交互。 2. 安装并了解 Verrazzano 平台。3. 部署 \*Bobby 的 Books\* 示例应用程序。4. 修改和重新部署基于 Helidon 的 \*Stock\* 应用程序组件。

## 了解详细信息

*   [Verrazzano](https://verrazzano.io/)

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月