# 生成 Helidon MP 项目并在代码编辑器中运行项目

## 简介

此实验将指导您完成创建 Helidon MP 应用程序的步骤。

估计时间：15 分钟

观看下面的视频，快速浏览实验室。[生成 Helidon MP 项目并在代码编辑器中运行项目](videohub:1_22nv8v4q)

### 关于产品/技术

Helidon 设计简单易用，可提供工具和示例来帮助您快速上手。由于 Helidon 只是在快速 Netty 核心上运行的库集合，因此没有额外的开销或 bloat。Helidon 支持 MicroProfile，并提供 JAX-RS、CDI 和 JSON-P/B 等熟悉的 API。我们的 MicroProfile 实施基于我们的快速 Helidon Reactive WebServer 运行。Reactive WebServer 提供了一个现代、功能强大的编程模型，并在 Netty 上运行。轻量级、灵活性和反应性强，Helidon WebServer 为您的微服务提供了简单易用的快速基础。

借助对运行状况检查、指标、跟踪和容错能力的支持，Helidon 可以满足您需要编写与 Prometheus、Jaeger/Zipkin 和 Kubernetes 集成的云就绪应用。

### 关于 Helidon 项目入门

项目启动器是用于创建 Helidon 项目的新 Web UI。它是高度可定制的，提供各种选项，允许用户选择要添加到项目的 Helidon 功能。最终用户将能够根据其特定需求生成项目。有关详细信息，请单击 [Helidon Starter](https://helidon.io/starter) 。

### 关于代码编辑器

代码编辑器允许您直接从 OCI 控制台编辑和部署各种 OCI 服务的代码。现在，无需在控制台和本地开发环境之间切换，即可更新服务工作流和脚本。这样，您可以轻松快速地构建原型云解决方案，探索新服务并完成快速编码任务。

代码编辑器与 Cloud Shell 的直接集成允许您访问预安装在 Cloud Shell 中的 GraalVM Enterprise Native Image 和 JDK 17 (Java Development Kit)。

### 关于 OCI Cloud Shell

[OCI Cloud Shell](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm) 是可从 Oracle Cloud 控制台访问的基于浏览器的终端。通过该服务，您可以使用预先验证的 OCI 命令行界面 (command-line interface，CLI) 和预安装的开发人员工具访问 Linux shell，并具有 5GB 存储。

自版本 22.2.0 起，GraalVM Enterprise JDK 17 和原生映像预安装在 Cloud Shell 中。

> GraalVM 企业版在 Oracle Cloud Infrastructure 上提供，无需额外付费。

### 目标

*   创建一个名为 Helidon Greeting 应用程序的 MicroProfile 支持的微服务
*   在代码编辑器中打开 Helidon 应用程序
*   更改云 shell 中的默认 JDK
*   配置所需的 Maven
*   运行并练习 Helidon 问候应用程序

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。

## 任务 1：使用项目启动器生成 Helidon 项目

1.  复制下面的 URL 并将其粘贴到浏览器中以打开“Helidon 项目”页。
    
        <copy>https://helidon.io/starter/</copy>
        
2.  在“生成项目”下，选择 _Helidon MP_ 作为 Helidon 风格，然后单击_下一步_。
    
3.  对于“应用程序类型”，选择_快速启动_，然后单击_下一步_。
    
4.  对于 "Media Support"，选择 _JSON-B_ ，然后单击 _Next_ 。
    
5.  对于“定制项目”，选择默认值并单击_下载_。这将弹出窗口，将此 _myproject.zip_ 保存到您选择的位置。在此研讨会的剩余部分中，将使用我的项目名称。如果选择其他名称，请分别更改。
    

## 任务 2：在本地构建和运行 Helidon 项目

1.  在云控制台中，单击_开发人员工具_图标，如图所示，然后单击_代码编辑器_。 ![代码编辑器](images/code-editor.png)
    
2.  在代码编辑器中，单击_终端_ -> _新建终端_。 ![打开终端](images/open-terminal.png)
    
3.  在终端中复制并粘贴以下命令，以创建 _myproject_ 文件夹。我们将在其中下载默认的 myproject.zip 文件。
    
        <copy>mkdir -p ~/helidon-project/myproject</copy>
        
4.  在代码编辑器中，单击 _File_ -> _Open_ 。 ![打开文件夹](images/open-folder.png)
    
5.  选择 _helidon-project_ 文件夹，然后单击 _Open（打开）_。 ![打开 Helidon](images/open-helidon.png)
    
6.  在代码编辑器中的 EXPLORER 窗口中，您可以看到 _HELIDON-PROJECT_ 。您可以在此处看到 _myproject_ 文件夹，单击该文件夹。现在单击_文件_ -> _上载文件 ..._ ，然后指定下载项目的位置并选择 zip 文件并单击_打开_。![我的项目](images/myproject.png) ![上载文件](images/upload-files.png)
    
7.  复制并粘贴以下命令以解压缩 _myproject.zip_ 文件。
    
        <copy>cd ~/helidon-project/myproject
        unzip myproject.zip
        </copy>
        
8.  展开 _myproject_ 文件夹以查看项目结构。 ![展开项目](images/expand-project.png)
    
9.  要运行此项目，我们将使用 Maven 3.8+ 和 JDK 17+。在 Oracle 云中，提供了各种 JDK。在此处，我们将选择 GraalVM JDK。在终端中复制并粘贴以下命令，以了解您的默认 JDK。
    
        <copy>csruntimectl java list</copy>
        
    
    ![列出 JDK](images/list-jdk.png)
    
    > 开始时带 \* _星号_的 JDK 是您的默认 JDK。如果有任何其他 JDK，则使用 graalvmeejdk，然后通过运行以下命令更改默认 JDK 版本。请使用显示的 graalvmeejdk 版本，因为它可能不同于命令中显示的版本。
    
        <copy>csruntimectl java set graalvmeejdk-17</copy>
        
10.  要配置所需的 maven，请在终端中复制并粘贴以下命令。
    
        <copy>cd ~/helidon-project/myproject/
        wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz
        tar -xzvf apache-maven-3.8.8-bin.tar.gz
        PATH=~/helidon-project/myproject/apache-maven-3.8.8/bin:$PATH</copy>
        
    
    ![配置 maven](images/configure-maven.png)
    
11.  要验证您的 JDK 和 Maven 版本是否正确，如下所示，请在终端中运行以下命令。
    
        <copy>mvn -v</copy>
        
    
    ![验证先决条件](images/verify-prerequisite.png)
    
12.  从 _myproject_ 文件夹中，运行以下命令来构建项目。
    
        <copy>cd ~/helidon-project/myproject/myproject
        mvn clean package</copy>
        
    
    ![构建项目](images/build-project.png)
    
    > 执行此命令后应看到 _BUILD SUCCESS_ 。
    
13.  将以下命令复制并粘贴到终端，以运行此应用程序。您将看到类似于下方屏幕截图中所示的输出。
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    ![运维项目](images/run-project.png)
    

> 请注意，开始时间为 4822 毫秒。我们将在以后将此时间与本机映像可执行文件进行比较。

14.  在代码编辑器中打开新的终端/控制台并运行以下命令来检查应用程序：
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        
15.  _通过在运行 "java -jar target/myproject.jar" 命令的终端中输入 `Ctrl + C` 来停止 **myproject** 应用程序_。很重要，你将在实验室里面对问题。
    

## 确认

*   **作者** - Dmitry Aleksandrov
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 4 月