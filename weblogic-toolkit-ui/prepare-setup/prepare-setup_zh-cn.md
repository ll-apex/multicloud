# 准备设置

## 简介

此实验室将向您展示如何下载设置运行此研习会所需的资源所需的 Oracle 资源管理器 (ORM) 堆栈 zip 文件。此研讨会需要计算实例和虚拟云网络 (VCN)。

估计时间：10 分钟

### 目标

*   下载 ORM 堆栈
*   配置现有虚拟云网络 (VCN)

### 先备条件

本练习假定您具有：

*   Oracle 免费层级或付费云账户

## 任务 1：下载 Oracle Resource Manager (ORM) 堆栈 zip 文件

1.  单击下面的链接下载构建环境所需的资源管理器 zip 文件：
    
    _注 1：_如果为研习会提供一次堆栈下载，请使用此简单表达式。
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  保存到您的下载文件夹中。
    

我们强烈建议使用此堆栈创建包含实例的自包含/专用 VCN。跳至_任务 3_ 以遵循我们的建议。如果您希望使用现有 VCN，请继续执行下面所示的下一步，以便使用所需的出站规则更新现有 VCN。

## 任务 2：向现有 VCN 添加安全规则

此研讨会要求提供一定数量的端口，这是使用创建专用 VCN 的默认 ORM 堆栈执行可以满足的要求。要使用现有 VCN，应将以下端口添加到入站规则。

| 港口 | 说明 |
| :-- | :-- |
| 22 | SSH |
| 80 | noVNC 远程桌面（NGINX 代理） |
| 6080 | noVNC 远程桌面 |

1.  请转至 _Networking >> Virtual Cloud Networks_ 。
2.  选择您的网络。
3.  在“Resources（资源）”下，选择“Security Lists（安全列表）”。
4.  单击“Create Security List（创建安全列表）”按钮下的“Default Security Lists（默认安全列表）”
5.  单击“Add Ingress Rule（添加入站规则）”按钮
6.  输入以下信息：
    *   源 CIDR：0.0.0.0/0
    *   目标端口范围：_请参阅上表_
7.  单击“Add Ingress Rules（添加入站规则）”按钮。

## 任务 3：设置计算

使用上述两个任务中的详细信息，转到实验室_环境设置_，使用 Oracle Resource Manager (ORM) 和以下选项之一设置您的研习会环境：

*   创建堆栈：_计算 + 网络_
*   创建堆栈：使用现有 VCN _仅计算_，其中安全列表已根据上面的_任务 2_ 更新

您现在可以进入下一个实验室。

## 确认

*   **作者** - NA Technology 的 LiveLabs 平台主管 Rene Fontcha
*   **贡献者** - Meghana Banka
*   **上次更新者/日期** - Rene Fontcha，LiveLabs Platform Lead，NA Technology，2022 年 2 月