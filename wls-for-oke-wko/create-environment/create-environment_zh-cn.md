# 设置实验室环境

## 简介

在本练习中，您将在 Oracle Cloud 容器映像注册表中创建资料档案库。此外，您还将接受 Oracle 容器注册表中 WebLogic 服务器映像的许可协议。

估计时间：15 分钟

### 目标

在本练习中，您将：

*   在 Oracle Cloud 容器映像注册表中创建资料档案库。
*   接受 Oracle 容器注册表中 WebLogic 服务器映像的许可证。

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。
*   您必须具有 Oracle 账户。
*   您应该具有文本编辑器。

## 任务 1：创建资源库

在此任务中，您将创建公共资料档案库。在练习 5 中，我们将辅助映像推送到此资料档案库。

1.  在远程桌面中，要打开 Firefox，请单击**活动**，在搜索框中键入 **Fire** ，然后单击 **Firefox** ，如下所示。 ![打开 firefox](images/open-firefox.png)
    
2.  使用您的凭据登录 [Oracle Cloud 控制台](https://cloud.oracle.com)。
    
3.  在控制台中，选择 _Hamburger Menu_ -> _Developer Services_ -> _Container Registry_ ，如下所示。 ![容器注册表](images/container-registry.png)
    
4.  选择您的区间，允许您在其中创建资料档案库。单击_创建资料档案库_。 ![创建资料档案库](images/create-repository.png)
    
5.  输入 _`test-model-your_firstname`_ 作为资料档案库名称，并以_公共_身份输入访问权限，然后单击_创建_。 ![资料档案库详细资料](images/repository-details.png)
    
6.  资料档案库就绪后。请记下文本文件中的租户名称空间。 ![附注租户 NameSpace](images/tenancy-namespace.png)
    

## 任务 2：接受 WebLogic 服务器映像的许可证

在此任务中，我们接受 WebLogic 服务器映像的许可协议位于 Oracle 容器注册表中。与实验 5 中一样，我们将使用 WebLogic Server 12.2.1.3.0 映像作为主映像。因此，要访问 WebLogic 服务器映像，我们接受许可协议。

1.  在浏览器中复制并粘贴 Oracle 容器注册表 [https://container-registry.oracle.com/](https://container-registry.oracle.com/) 的链接并登录。为此，您需要一个 Oracle 帐户。 ![容器注册表登录](images/container-registry-sign-in.png)
    
2.  在“用户名和密码”字段中输入 _Oracle 帐户凭据_，然后单击_登录_。 ![登录容器注册表](images/login-container-registry.png)
    
3.  在 Oracle 容器注册表的主页中，搜索 _weblogic_ 。 ![搜索 WebLogic](images/search-weblogic.png)
    
4.  按所示单击 _weblogic_ 并选择 _English_ 作为语言，然后单击 _Continue_ 。![单击 WebLogic](images/click-weblogic.png) ![选择语言](images/select-language.png)
    
5.  单击_接受_接受许可协议。 ![接受许可证](images/accept-license.png)
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 10 月