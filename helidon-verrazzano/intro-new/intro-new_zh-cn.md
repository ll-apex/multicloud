# 简介

## 关于此研讨会

此实验室向与会者提供了与 Oracle Kubernetes Engine、Verrazzano 和 Helidon 的入门级实践课程。您将学习如何使用 Verrazzano 在 OKE 上部署示例 hello helidon 应用程序。

在此实验室中，您将使用 Oracle Cloud 免费套餐账户在 Oracle Cloud Infrastructure (OCI) 上设置 Oracle Kubernetes 引擎 (OKE)。免费套餐账户足以探索和了解如何在企业级别运行和运行微服务应用程序。

本次研讨会的目标是，学生将学习使用 OKE 和 Verrazzano 的基本知识，并了解他们如何为您提供帮助。如果要了解有关这些项目功能的更多信息，请继续了解如何使用 Oracle 免费层云账户和 Oracle Cloud Infrastructure。

本次研讨会旨在尽可能具有自我解释性，但可以随意要求澄清或协助。

估计时间：60 分钟

### 目标

*   设置 Oracle Cloud 免费套餐账户（如果您尚未这样做）。
*   在 Oracle Cloud Infrastructure 上设置 Oracle Kubernetes 引擎实例。
*   安装 Verrazzano 平台。
*   部署 _Simple Hello Helidon_ 应用程序。
*   使用 Verrazzano 工具（包括 Rancher、Opensearch、Grafana 和 Prometheus）监视 _Simple Hello Helidon_ 应用程序。

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。

## 关于 Helidon

Helidon 是 Oracle 推出的开源微服务框架，提供一组 Java 库，专门用于创建基于轻量级和快速微服务的应用。该框架支持两种用于编写微服务的编程模型：Helidon SE 和 Helidon MP。

虽然 Helidon SE 设计为支持反应性编程模型的微框架，但 Helidon MP 是 MicroProfile 规范的实现。由于 MicroProfile 在 Java EE 中具有根目录，因此 MicroProfile API 采用一种熟悉的声明式方法，可以大量使用注释。这使得它成为 Java EE 开发人员的良好选择。

MicroProfile 的功能旨在实施微服务。您可以找到用于定义 REST 客户端、监视应用、读取技术和功能统计信息的 API 以及配置应用。Helidon 还为核心的 Microprofile API 集添加了其他 API，从而提供了编写现代云原生应用所需的所有功能。

> [MicroProfile](https://microprofile.io/) 标准基于 Jakarta EE。与 Jakarta EE 一样，MicroProfile 是开源的，由 Eclipse 基金会开发。与 Jakarta EE 一样，在实现标准的库或应用服务器上实施 MicroProfile。

## 关于 Verrazzano

Verrazzano 是一个端到端的企业容器平台，用于在多云和混合环境中部署云原生和传统应用。它由一系列精心策划的开源组件组成 - 许多您可能已经使用和信任，以及一些专门为将 Verrazzano 成为统一且易于使用的平台而编写的组件。

Verrazzano 包含以下功能：

*   混合和多集群负载管理
*   WebLogic、Coherence 和 Helidon 应用程序的特殊处理
*   多集群基础结构管理
*   集成和预有线应用监视
*   集成安全性
*   DevOps 和 GitOps 启用

![Verrazzano](images/verrazzano.png)

## 了解详细信息

*   [https://helidon.io](https://helidon.io)
*   [https://verrazzano.io/](https://verrazzano.io/)

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 3 月