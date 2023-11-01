# 构建 Helidon MP 应用原生映像

## 简介

在本练习中，您将学习如何在本地 linux 计算机上设置环境。

估计时间：10 分钟

### 目标

*   配置所需的 JDK 和 Maven。
*   下载应用程序源代码。

### 先备条件

*   您必须具有 IDE 才能修改、构建和运行项目。

## 任务 1：下载所需的 Maven 和 JDK 版本

1.  在 IDE 中，打开终端/控制台。
    
2.  复制以下命令并在终端中粘贴。它下载所需的 JDK 和 Maven 版本，并将 PATH 变量设置为使用所需的 Maven 和 JDK。
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/latest/jdk-19_linux-x64_bin.tar.gz
        tar -xzvf jdk-19_linux-x64_bin.tar.gz</copy>
        

## 任务 2：下载 Helidon 源代码

1.  复制以下命令并在终端中粘贴以下载 helidon 应用程序的源代码。
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  复制并粘贴以下命令以解压缩 _helidon-levelup-2023-main.zip_ 。
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

您现在可以_进入下一个实验_。

## 确认

*   **作者** - Joe DiPol
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 2 月