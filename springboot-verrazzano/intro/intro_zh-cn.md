# 简介

## 关于此研讨会

在此研讨会中，我们将探讨如何使用 Verrazzano 将 Spring Boot 应用程序部署到 Oracle Kubernetes 引擎 (OKE) 集群。

在当今的数字时代，企业正在寻求更快、更高效地开发和部署应用的方法。Kubernetes 已成为容器编排的实际标准，使组织可以部署、管理和扩展容器化应用。Oracle Kubernetes 引擎 (OKE) 提供托管 Kubernetes 服务，可简化 Oracle Cloud Infrastructure (OCI) 上容器化应用的部署和管理。

Spring Boot 是一种基于 Java 的流行框架，用于构建 Web 应用程序和微服务。它提供简化的开发体验，采用超配置约定方法，使其成为在 Kubernetes 上构建和部署容器化应用的绝佳选择。

Verrazzano 是一个开源的 Kubernetes 原生平台，它提供了完整的端到端解决方案来部署和管理云原生应用。它提供统一的应用拓扑视图以及一组用于部署、监视和管理的集成工具，从而简化了复杂多组件应用的部署。

本次研讨会旨在尽可能具有自我解释性，但可以随意要求澄清或协助。

估计时间：90 分钟

### 目标

*   设置 Oracle Cloud 免费套餐账户（如果您尚未这样做）。
*   在 OCI 上设置 OKE 集群
*   在集群上安装和配置 Verrazzano
*   为 Spring Boot 应用程序构建容器映像
*   使用 Verrazzano 将应用程序部署到 OKE 群集
*   使用 Verrazzano 的集成工具监视和管理应用程序

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。

## 了解详细信息

**关于 Springboot**

Spring Boot 是一种基于 Java 的流行框架，用于构建 Web 应用程序和微服务。它提供简化的开发体验，采用超配置约定方法，使其成为在 Kubernetes 上构建和部署容器化应用的绝佳选择。

Spring Boot 提供多种功能，可帮助您快速构建和部署应用，例如嵌入式服务器、自动配置，以及用于构建 Web 应用、消息传递系统和数据驱动型应用的各种库和工具。它还为测试和调试提供了丰富的工具，使其更易于编写强大可靠的代码。

Spring Boot 的主要优势之一是它能够轻松地与其他技术和框架集成，包括数据库系统、消息队列和缓存系统。这样，开发人员可以构建复杂的分布式系统来处理大规模负载并满足基于云的现代应用需求。

Spring Boot 专注于简单性和易用性，已成为希望构建现代云原生应用的开发人员和组织的一种热门选择。它有一个充满活力的贡献者和用户社区，并且正在不断发展以适应行业不断变化的需求。

**关于 Verrazzano**

Verrazzano 是一个端到端的企业容器平台，用于在多云和混合环境中部署云原生和传统应用。它由一系列精心策划的开源组件组成 - 许多您可能已经使用和信任，以及一些专门为将 Verrazzano 成为统一且易于使用的平台而编写的组件。

Verrazzano 包含以下功能：

*   混合和多集群负载管理
*   WebLogic、Coherence 和 Helidon 应用程序的特殊处理
*   多集群基础结构管理
*   集成和预有线应用监视
*   集成安全性
*   DevOps 和 GitOps 启用

![Verrazzano](images/verrazzano.png)

*   [https://spring.io/](https://spring.io/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 4 月