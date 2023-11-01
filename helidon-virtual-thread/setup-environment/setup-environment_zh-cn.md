# 设置环境

## 简介

本练习将指导您完成设置研讨会所需环境的步骤。

[Lab1 演练](videohub:1_far2bboa)

估计时间：10 分钟

### 关于代码编辑器

代码编辑器允许您直接从 OCI 控制台编辑和部署各种 OCI 服务的代码。现在，无需在控制台和本地开发环境之间切换，即可更新服务工作流和脚本。这样，您可以轻松快速地构建原型云解决方案，探索新服务并完成快速编码任务。

代码编辑器与 Cloud Shell 的直接集成允许您访问预安装在 Cloud Shell 中的 GraalVM Enterprise Native Image 和 JDK 17 (Java Development Kit)。

### 目标

*   设置代码编辑器
*   下载所需的 Maven 和 JDK 版本
*   下载 Helidon 源代码

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。
*   您必须使用 Firefox 作为浏览器才能打开代码编辑器。

## 任务 1：设置代码编辑器

1.  在云控制台中，单击代码编辑器图标，如下所示。 ![代码编辑器](images/code-editor.png)
    
2.  在代码编辑器中，单击 "Terminal"（终端）-> "New Terminal"（新建终端）。 ![打开终端](images/open-terminal.png)
    

## 任务 2：下载所需的 Maven 和 JDK 版本

1.  复制以下命令并在终端中粘贴。它下载所需的 JDK 和 Maven 版本。
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/archive/jdk-19.0.2_linux-x64_bin.tar.gz
        tar -xzvf jdk-19.0.2_linux-x64_bin.tar.gz</copy>
        

## 任务 3：下载 Helidon 源代码

1.  复制以下命令并在终端中粘贴以下载 helidon 应用程序的源代码。
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  复制并粘贴以下命令以解压缩 _helidon-levelup-2023-main.zip_ 。
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

您现在可以_进入练习 2_ 。

## 确认

*   **作者** - Joe DiPol
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 2 月