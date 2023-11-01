# 简介

## 关于此研讨会

在 Kubernetes 集群上部署 Tomcat 应用程序是一个复杂的过程，涉及管理多个容器、配置网络以及确保高可用性和可扩展性。为了简化此流程，许多组织正转向 Docker 和 Kubernetes 等容器化技术。

Docker 允许创建可在任何平台上部署的可移植容器映像，而 Kubernetes 则提供了强大的编排引擎，可以管理容器的部署和扩展。

Oracle Verrazzano 是一个云原生平台，提供了在 Kubernetes 集群上部署和运行容器化应用的统一管理体验。它提供了一组一致的工具和 API 来管理和监视多个集群中的应用，从而简化了复杂应用的部署。

在此研讨会中，我们将探讨使用 Verrazzano 将 Tomcat 应用程序 Docker 映像部署到 Oracle Kubernetes 集群的过程。我们将介绍创建 Docker 映像、配置 Verrazzano、部署应用程序以及监视其性能所涉及的步骤。

参加本研讨会后，您将清楚地了解如何使用 Verrazzano 在 Oracle Kubernetes 集群上部署容器化应用。

本次研讨会旨在尽可能具有自我解释性，但可以随意要求澄清或协助。

估计时间：90 分钟

### 目标

*   设置 Oracle Cloud 免费套餐账户（如果您尚未这样做）。
*   在 Oracle Cloud Infrastructure 上设置 Oracle Kubernetes 引擎实例。
*   安装 Verrazzano 生产配置文件。
*   创建示例 Tomcat 应用程序 docker 映像。
*   使用 Verrazzano 将 Tomcat 应用程序部署到 OKE。
*   浏览 Grafana、Prometheus 和 OpenSearch 仪表盘控制台。

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。

## 了解详细信息

**关于 Tomcat**

Apache Tomcat 是开源 Web 服务器和 Servlet 容器，广泛用于部署基于 Java 的 Web 应用程序。Tomcat 设计为轻量级、高效且易于使用，它为管理和部署 Web 应用提供了丰富的功能。

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

*   [https://tomcat.apache.org/](https://tomcat.apache.org/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月