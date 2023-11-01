# 简介

## 关于此研讨会

在此实验室中，您将使用 Helidon Project Starter 开发 HTTP 微服务应用程序。它是高度可定制的，提供各种选项，允许用户选择要添加到项目的 Helidon 功能。

然后，您将浏览 OCI 代码编辑器并在其中打开 Helidon 应用程序。您还将了解如何在 Oracle Cloud Infrastructure (OCI) Cloud Shell 中使用 GraalVM Enterprise Edition。

您将使用 maven 为 Helidon MP 应用程序构建 GraalVM 本机映像。然后，您将通过在现有 Java 类中添加定制端点来修改应用程序。稍后，您将构建此应用的原生 docker 映像并将其推送到 Oracle Cloud 容器映像注册表。您还将创建计算实例并将 docker 映像拉到此处，然后为此应用程序运行 docker 容器。

本次研讨会旨在尽可能具有自我解释性，但可以随意要求澄清或协助。

估计时间：90 分钟

### 目标

*   生成 Helidon MP 应用程序
*   浏览 OCI 代码编辑器
*   为 Helidon MP 应用程序创建本机映像
*   为 Helidon 应用程序创建 GraalVM 本机 docker 映像
*   创建虚拟机
*   为 Helidon MP 应用程序运行 docker 容器

### 先备条件

本练习假定您具有：

*   Oracle Cloud 账户

## 关于 Helidon

Helidon 是 Oracle 推出的开源微服务框架，提供一组 Java 库，专门用于创建基于轻量级和快速微服务的应用。该框架支持两种用于编写微服务的编程模型：Helidon SE 和 Helidon MP。

虽然 Helidon SE 设计为支持反应性编程模型的微框架，但 Helidon MP 是 MicroProfile 规范的实现。由于 MicroProfile 在 Java EE 中具有根目录，因此 MicroProfile API 采用一种熟悉的声明式方法，可以大量使用注释。这使得它成为 Java EE 开发人员的良好选择。

MicroProfile 的功能旨在实施微服务。您可以找到用于定义 REST 客户端、监视应用、读取技术和功能统计信息的 API 以及配置应用。Helidon 还为核心的 Microprofile API 集添加了其他 API，从而提供了编写现代云原生应用所需的所有功能。

> [MicroProfile](https://microprofile.io/) 标准基于 Jakarta EE。与 Jakarta EE 一样，MicroProfile 是开源的，由 Eclipse 基金会开发。与 Jakarta EE 一样，在实现标准的库或应用服务器上实施 MicroProfile。

## 了解详细信息

*   [https://helidon.io](https://helidon.io)

## 确认

*   **作者** - Dmitry Aleksandrov
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 4 月