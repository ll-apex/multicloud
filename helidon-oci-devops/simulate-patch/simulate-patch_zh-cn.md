# 模拟补丁方案

## 介绍

此实验室将尝试模拟打补丁方案。举例说明，初始环境使用 _Open JDK_ 进行测试，最终迁移到 _Oracle JDK_ 准备生产时使用它。上一个构建和部署管道已将 _Open JDK 20_ 设置为要使用的 Java 风格。在本练习中，它将替换为 _Oracle JDK 20_ 。

估计时间：10 分钟

观看下面的视频，快速浏览实验室。[模拟补丁程序方案](videohub:1_4pecvhmf)

### 目标

在本练习中，您将：

*   修改 JDK 风格
*   验证部署管道中的 JDK 新样式

### 先备条件

*   Oracle 免费套餐（试用版）、付费版或 LiveLabs 云账户

## 任务 1：更改 JDK 安装程序

1.  如**练习 2** 中所示，在以前完成的**部署管道日志**中，观察到在日志底部附近使用的是 **Open JDK** 。 ![打开 jdk](images/open-jdk.png)
    
2.  在**代码编辑器**中，单击文件名 **`build_spec.yaml`** 以将其打开。注释掉环境变量 **`JDK20_TAR_GZ_INSTALLER`** ，该变量将 URL 路径设置为行号 #15 处的 **Open JDK** 安装程序的 URL 路径，取消注释 **`JDK20_TAR_GZ_INSTALLER`** ，将 URL 路径设置为行号 #16 下的 **Oracle JDK** 安装程序的 URL 路径，如下所示。 ![修改 jdk](images/modify-jdk.png)
    

## 任务 2：推送更改并触发 DevOps 管道

1.  复制以下命令并将其粘贴到终端**以提交和推送**更改。
    
        <copy>git add .
        git status
        git commit -m "Replace OpenJDK 20 with OracleJDK 20"
        git push</copy>
        
    
    > 此 Git 推送将触发管道。
    

## 任务 3：验证部署管道中的 JDK 新风格

1.  在新选项卡中打开[云控制台](https://cloud.oracle.com/)，单击 _Hamburger（汉堡）菜单_ -> _Developer Services（开发人员服务）_ -> _Projects（项目）_，位于 **DevOps** 下。 ![devops 项目](images/devops-project.png)
    
2.  选择在**练习 1** 中创建的区间，然后单击 _devops-project-helidon-ocw-hol-string_ 打开 **DevOps 项目**。 ![选择区间](images/select-compartment.png)
    
3.  在**最新构建历史记录**下，您将看到**运行**和状态为**已接受/正在进行**。单击最新的运行，如下所示。 ![构建历史记录](images/build-history.png)
    
4.  构建管道**完成全部三个阶段**后，您将看到如下所示的输出。 ![首先构建](images/build-run-first.png)
    
5.  在构建运行过程中，在第三阶段，单击**三个点**，然后单击**查看部署**，如下所示。这将打开部署管道。 ![查看部署](images/view-deployment.png)
    
6.  在此处可以查看**部署进度**。完成部署管道后，您将看到如下所示的输出。 ![部署运行](images/deployment-run.png)
    
    > 这成功将 Helidon 应用程序部署到 OCI 中的计算实例。
    
7.  要查看部署管道的日志，请单击靠近部署阶段的**三个点**，然后单击**查看详细信息**，如下所示。 ![查看日志](images/view-logs.png)
    
8.  向下滚动日志并验证 JDK 风格，应该为 **Oracle JDK** ，如下所示。 ![oracle jdk](images/oracle-jdk.png)
    

您现在可以**继续下一个实验。**

## 确认

*   **作者** - Keith Lustria
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月