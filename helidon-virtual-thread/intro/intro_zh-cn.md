# 简介

## 关于此研讨会

在本练习中，您将使用 Oracle Code Editor 使用 Helidon 4 中提供的 Helidon Nima WebServer 构建、运行和修改微服务。Nima 是从头开始编写的，用于利用 Java 19 的虚拟线程并提供简单的阻止编程模型。

您还将了解它如何与更复杂的反应性编程进行比较。

在此实验室中，您将使用 Helidon Project Starter 开发 Helidon Microprofile 应用程序。它是高度可定制的，提供各种选项，允许用户选择要添加到其项目的 Helidon 功能。然后，您将使用虚拟线程将应用程序迁移到在新 Helidon Nima WebServer 上运行的 Helidon 4。

本次研讨会旨在尽可能具有自我解释性，但可以随意要求澄清或协助。

估计时间：90 分钟

### 目标

*   浏览 OCI 代码编辑器
*   构建和运行 Nima 应用程序
*   构建和运行反应性应用程序
*   分析 Nima 应用程序的简单性
*   比较 Nima 和 Reactive 应用程序的堆栈跟踪
*   生成 Helidon MP 应用程序
*   将 Helidon 3 应用程序迁移到 Helidon 4

**关于 Helidon Nima**

Helidon 是 Oracle 推出的开源微服务框架，提供一组 Java 库，专门用于创建基于轻量级和快速微服务的应用。

在早期版本的 Helidon 开发人员可以挑选两种编程模型。[Helidon MP](https://helidon.io/docs/v3/#/mp/introduction) 是 Eclipse MicroProfile 标准的实现，具有 Jakarta EE 开发人员熟悉的 API。[Helidon SE](https://helidon.io/docs/v3/#/se/introduction) 是一个精简的反应性框架。

在 Helidon 4 中，Oracle 推出了 Níma，这是从头开始使用 JEP 425 虚拟线程的 Web 服务器。Nima 提供了易于使用的编程模型，具有出色的性能。对于虚拟线程，线程不再是稀缺资源，无法小心地池和管理。而是可以根据需要创建大量资源来处理近乎无限的并发请求。由于每个请求在其专用线程中运行，因此可以自由执行阻塞操作，例如调用数据库或其他服务。它可以通过简单的同步方式做到这一点，而无需担心阻塞平台线程并饥饿其他请求。您不再需要使用复杂的异步代码来实施低开销、高并发服务。

Helidon Níma Web 服务器打算取代 Helidon 生态系统中的 Netty。它还可以由其他框架用作嵌入式 Web 服务器组件。

### 先备条件

本练习假定您具有：

*   Oracle Cloud 账户

## 了解详细信息

*   [https://helidon.io](https://helidon.io)

## 确认

*   **作者** - Joe DiPol
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 2 月