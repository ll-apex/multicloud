# 准备设置

## 简介

此实验室将向您展示如何下载设置运行此研习会所需的资源所需的 Oracle 资源管理器 (ORM) 堆栈 zip 文件。然后，您可以创建计算实例和虚拟云网络 (VCN)，以便访问远程桌面。

估计时间：10 分钟

### 目标

*   下载 ORM 堆栈
*   使用资源管理器堆栈创建计算 + 网络

### 先备条件

本练习假定您具有：

*   Oracle 免费层级或付费云账户

## 任务 1：下载 Oracle Resource Manager (ORM) 堆栈 zip 文件

1.  单击下面的链接下载构建环境所需的资源管理器 zip 文件：
    
    _注 1：_如果为研习会提供一次堆栈下载，请使用此简单表达式。
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  保存到您的下载文件夹中。
    

## 任务 2：创建堆栈：计算 + 网络

1.  确定**任务 1：下载 Oracle Resource Manager (ORM) 堆栈 zip 文件**中下载的 ORM 堆栈 zip 文件。
    
2.  打开左上角的汉堡菜单。单击**开发人员服务**，然后选择**资源管理器** > **堆栈**。选择要安装堆栈的区间。单击**创建堆栈**。![菜单堆栈](images/menu-stack.png) ![选择区间](images/select-compartment.png)
    
3.  选择**我的配置**，然后选择**。Zip** 文件按钮，单击**浏览**链接，然后为文件浏览器选择下载的 zip 文件或拖放。单击**下一步**。 ![浏览 zip](images/browse-zip.png)
    
4.  输入或选择以下内容，然后单击**下一步**。
    
    **实例计数：**接受默认值 1。
    
    **选择可用性域：**从下拉列表中选择一个可用性域。
    
    **需要通过 SSH 进行远程访问？**对 "Remote Desktop Access"（仅远程桌面访问）保持 "Unchecked"（取消选中）- 默认值。
    
    **是否使用具有可调整 OCPU 计数的灵活实例配置？：**保持默认设置为选中状态（除非您计划使用固定配置）。
    
    **实例配置：**保留默认值，或者从下拉菜单（例如 VM.Standard.E4）的弹性配置列表中选择。弹性 )。
    
    **为每个实例选择 OCPU 计数：**接受显示的默认值，例如 (1) 将预配 1 个 OCPU 和 16GB 内存。
    
    **是否使用现有 VCN？：**请接受默认值，不选中此复选框。这将创建新的 VCN。![主要配置](images/main-config.png) ![实例配置](images/instance-shape.png)
    
5.  选择**运行应用**并单击**创建**。 ![运行应用](images/run-apply.png)
    

您现在可以进入下一个实验室。

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 10 月