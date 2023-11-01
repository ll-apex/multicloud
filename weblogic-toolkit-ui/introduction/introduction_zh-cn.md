# 简介

## 关于此研讨会

本研讨会将内部部署 WebLogic 服务器域的端到端迁移到容器，并使它可以在 OCI 中通过 Oracle Container Engine for Kubernetes (OKE) 运行。我们演示了 WebLogic Kubernetes 工具包 UI 以及 WebLogic 部署工具和 Weblogic Kubernetes 操作员的图形界面。我们演示了如何使用一组面向 DevOps 的工具简化并加快迁移过程。

![实验室流程](images/lab-flow.png)

估计时间：120 分钟

观看下面的视频，快速浏览实验室。[完整研讨会](videohub:1_q1mmkimy)

### 关于产品/技术

WebLogic Kubernetes 工具包 (WKT) 是一组开源工具，可帮助您预配基于 WebLogic 的应用程序，以便在 Kubernetes 集群的 Linux 容器中运行。WKT 包括以下工具：  

*   [WebLogic 部署工具 (Deploy Tooling，WDT)](https://github.com/oracle/weblogic-deploy-tooling) - 一组单用途生命周期工具，可从 WebLogic 域的单个元数据模型表示形式中操作。
*   [WebLogic 映像工具 (Image Tool，WIT)](https://github.com/oracle/weblogic-image-tool) - 用于创建用于运行 WebLogic 域的 Linux 容器映像的工具。
*   [WebLogic Kubernetes 操作员 (WKO)](https://github.com/oracle/weblogic-kubernetes-operator) - 一种 Kubernetes 运算符，允许 WebLogic 域在 Kubernetes 集群中原生运行。

WKT UI 提供了一个图形用户界面，可包装 WKT 工具、Docker、Helm 和 Kubernetes 客户端 (kubectl) 并帮助您完成模型创建和修改过程属于 WebLogic 域，创建用于运行域的 Linux 容器映像，以及设置和部署在 Kubernetes 集群中部署和访问域所需的软件和配置。

### 目标

*   从应用市场映像创建虚拟机
*   在 Oracle Cloud Infrastructure 上设置 Oracle Kubernetes 引擎实例
*   修改 WKT UI 项目并创建模型文件
*   创建辅助映像并在 Oracle 容器映像注册表中进行推送
*   将 WebLogic 操作员部署到 Oracle Kubernetes 集群 (OKE)
*   将 WebLogic 域部署到 Oracle Kubernetes 集群 (OKE)
*   将入站控制器部署到 Oracle Kubernetes 集群 (OKE)
*   显示托管服务器云池与浏览 WebLogic-Remote-Console 之间的负载平衡
*   缩放 WebLogic 集群
*   通过滚动重新启动新映像来更新已部署的应用程序

## 了解详细信息

*   [WebLogic Kubernetes 工具包 UI](https://oracle.github.io/weblogic-toolkit-ui/)

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月