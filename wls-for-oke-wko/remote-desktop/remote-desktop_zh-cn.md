# 设置计算实例

## 简介

此实验室将向您展示如何设置 **Oracle WebLogic Suite for OKE BYOL** 堆栈，以生成运行研习会所需的 Oracle Cloud 对象。

估计时间：15 分钟

### 目标

在本练习中，您将：

*   生成验证标记。
*   在 Vault 中创建密钥
*   创建堆栈：适用于 OKE BYOL 的 Oracle WebLogic Suite
*   连接到计算实例

### 先备条件

本练习假定您具有：

*   Oracle Cloud 账户
*   您已生成了一对 SSH 密钥。
*   您已完成：**实验室：准备设置**

## 任务 1：生成验证令牌

在此任务中，我们将生成_验证令牌_。在练习 5 中，我们将使用此验证令牌将辅助映像推送到 Oracle Cloud 容器注册表资料档案库。此外，我们还使用此验证令牌将 WebLogic docker 映像提取到 Oracle Cloud Infrastructure Registry（也称为容器注册表）。

1.  选择右上角的用户图标，然后选择 _MyProfile_ 。
    
    ![我的配置文件](images/my-profile.png)
    
2.  向下滚动并选择 _Auth Tokens_ ，然后单击 _Generate Token（生成令牌）_。
    
    ![验证标记](images/auth-token.png)
    
3.  复制 _`test-model-your_first_name`_ 并将其粘贴到_说明_框中，然后单击_生成令牌_。
    
    ![创建标记](images/create-token.png)
    
4.  在“生成的令牌”下选择_复制_并将其粘贴到文本文件中。我们以后不能复制它。单击_关闭_。
    
    ![复制标记](images/copy-token.png)
    
    > 在下一个任务中，我们将此验证令牌存储在 **Secret** 中。
    

## 任务 2：在 Vault 中创建密钥

密钥是用于 Oracle Cloud Infrastructure 服务的身份证明，例如密码、证书、SSH 密钥或**验证令牌**。

1.  在 OCI 控制台中，单击**汉堡菜单** -> **身份与安全性** -> **Vault** 。 ![菜单储存库](images/menu-vault.png)
    
2.  在**列表范围**下选择区间，然后单击**创建 Vault** 。
    
3.  输入 Vault 的名称，然后单击 **Create Vault** 。 ![创建 Vault](images/create-vault.png)
    
4.  看到您创建的 Vault 处于**活动**状态后，单击 Vault 名称，如下所示。 ![vault 名称](images/vault-name.png)
    
5.  在 Vault 中，单击**主加密密钥**，然后单击**创建密钥**。 ![菜单项](images/menu-mek.png)
    
6.  输入 **key** 的名称，然后单击**创建密钥**，如下所示。 ![创建 mek](images/create-mek.png)
    
7.  在看到新主加密密钥的状态更改为 **Enabled** 之后，单击 **Resources** 下的 **Secrets** ，如下所示。 ![菜单密钥](images/menu-secret.png)
    
8.  单击**创建密钥**。
    
9.  输入密钥的名称并选择新的加密密钥，并将验证令牌输入为**秘密内容**，如下所示。单击**创建密钥**。 ![创建密钥](images/create-secret.png)
    

## 任务 3：创建堆栈：适用于 OKE BYOL 的 Oracle WebLogic Suite

1.  在 OCI 控制台中，单击**汉堡菜单** -> **市场** -> **所有应用程序**。 ![菜单市场](images/menu-marketplace.png)
    
2.  在搜索框中键入 **WebLogic OKE** ，然后单击 **Oracle WebLogic Suite for OKE BYOL** 。 ![菜单堆栈](images/menu-stack.png)
    
3.  选择可用的最新版本并选择区间，并勾选此框接受条款和条件。单击**启动堆栈**。 ![启动堆栈](images/launch-stack.png)
    
4.  在“堆栈信息”部分中，保留所有内容默认值，然后单击**下一步**。 ![堆栈信息](images/stack-info.png)
    
5.  输入 **wko** 作为资源名称前缀，然后单击“浏览”以选择 SSH 公共密钥。 ![前缀 -ssh](images/prefix-ssh.png)
    
    > 确保使用 **wko** 作为前缀，因为我们将在进一步的练习中使用此前缀。
    
6.  在 Verrazzano 集成中，保留 **Enable Verrazzano** 的默认未选中复选框。 ![uncheck verrazzano](images/uncheck-verrazzano.png)
    
7.  在“网络”中，选择**创建新 VCN** 作为虚拟云网络策略，并将所有内容保留默认值。 ![网络](images/network.png)
    
8.  在容器集群 (OKE) 配置中，选择以下项。
    
    **Kubernetes 版本：** \- 保留默认 **v1.26.2** 。
    
    **Non-WebLogic 节点池配置（必需）** \- 选择 **VM.Standard.E4。Flex** 作为配置并选择 **1** 作为 OCPU 数，并选择 **16** 作为内存量。
    
    **非 WebLogic 云池的 NodePool 中的节点** \- 保留默认值 **1** 。
    
    ![非博客](images/non-weblogic.png)
    
    **创建 WebLogic 节点池配置** - 将默认框保留为选中状态。
    
    **WebLogic 节点池配置（必需）** \- 选择 **VM.Standard.E4。Flex** 作为配置并选择 **1** 作为 OCPU 数，并选择 **16** 作为内存量。
    
    **NodePool 中 WebLogic pod 的节点** \- 保留默认值 **1** 。
    
    ![博客](images/weblogic-pool.png)
    
9.  在管理实例中，输入或选择以下内容，如下所示。**计算实例的可用性域** - 从下拉列表中选择可用性域。
    
    **管理实例计算配置（必需）** \- 选择 **VM.Standard.E4。Flex** 作为配置并选择 **1** 作为 OCPU 数，并选择 **16** 作为内存量。
    
    ![管理实例](images/admin-instance.png)
    
    **堡垒实例配置（必需）** \- 选择 **VM.Standard.E4。Flex** 作为配置并选择 **1** 作为 OCPU 数，并选择 **16** 作为内存量。
    
    ![野兽](images/bastion.png)
    
10.  在“File System（文件系统）”部分中，选择以下内容，如下所示。**文件系统的可用性域** - 从下拉列表中选择可用性域。 ![文件 avd](images/file-avd.png)
    
11.  在“Registry (OCIR)”部分中，输入以下内容，如下所示。
    
    **注册表用户名** - Kubernetes 用于访问容器映像注册表的用户名，该注册表格式为 {identity domain name}/{username}。如果您的租户使用的是 Oracle Identity Cloud Service，请使用格式 oracleidentitycloudservice/{username}。
    
    **OCIR 验证令牌区间** - 选择具有 OCIR 验证令牌的区间。
    
    **OCIR 验证令牌的已验证密钥** - 包含您为用户生成的用于访问映像注册表的 OCIR 验证令牌的密钥。
    
    ![密钥](images/ocir-secret.png)
    
12.  在 OCI 策略中，选中 **OCI 策略**对应的框。它创建从 Vault 读取密钥和管理自治事务处理数据库的策略（如果适用）。单击**下一步**。
    
13.  在“复查”部分中，选中**运行应用**的框，然后单击**创建**。 ![运行应用](images/run-apply.png)
    
    > 这将创建一个作业，该作业将在堆栈中创建所需的资源。您不需要等待，请继续执行下一个任务，
    

## 任务 4：访问图形远程桌面

为了便于执行本研习会，您的 VM 实例已预先配置了远程图形桌面，可使用手提电脑或工作站上的任何现代浏览器进行访问。请按照以下详细说明登录。

1.  打开左上角的汉堡菜单。单击**开发人员服务**，然后选择**资源管理器** > **堆栈**。
    
2.  单击在练习 1 中创建的堆栈名称。 ![单击堆栈](images/click-stack.png)
    
3.  导航到**应用程序信息**选项卡，然后复制**远程桌面 URL** 并将其粘贴到新的浏览器选项卡中。 ![桌面 URL](images/desktop-url.png)
    
    > 现在，您需要遵循此远程桌面中的所有指令。
    

您现在可以进入下一个实验室。

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 10 月