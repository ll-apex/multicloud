# 将 WebLogic 操作员部署到 Oracle Kubernetes 集群 (OKE)

## 简介

在此实验室中，我们使用浏览器对 OCI CLI 进行验证，这将创建 _.oci/config_ 文件。由于我们将使用 kubectl 使用_本地访问_远程管理集群。它需要 _kubeconfig_ 文件。将使用 OCI CLI 生成此 kubeconfig 文件。然后，我们通过 WebLogic Kubernetes 工具包 UI 验证与 Kubernetes 集群的连接。最后，我们将 WebLogic Kubernetes 操作员安装到 Kubernetes 集群 (OKE)。

估计时间：10 分钟

### 目标

在本练习中，您将：

*   配置 kubectl（Kubernetes 集群 CLI）以连接到 Kubernetes 集群。
*   验证 WebLogic Kubernetes 工具包 UI 与 Kubernetes 集群的连接。
*   将 WebLogic Kubernetes 操作员安装到 Kubernetes 集群。

## 任务 1：配置 kubectl (Kubernetes Cluster CLI) 以连接到 Oracle Kubernetes 集群

在此任务中，我们在 _/home/opc_ 目录中创建配置文件 _.oci/config_ 和 _.kube/config_ 。使用此配置文件可以从此虚拟机访问 Oracle Kubernetes 集群 (OKE)。

1.  在 Firefox 中，打开云控制台，选择**汉堡包菜单** -> **开发人员服务** -> **Kubernetes 集群 (OKE)** ，如下所示。 ![OKE 图标](images/oke-icon.png)
    
2.  单击在练习 3 中创建的集群名称，然后单击**访问集群**。 ![访问集群](images/access-cluster.png)
    
3.  选择**本地访问**，然后单击**复制**，如下所示。 ![本地访问](images/local-access.png)
    
4.  单击**活动**，然后选择**终端**。 ![终端](images/click-terminal.png)
    
5.  将复制的命令粘贴到终端中。对于**是否要创建新配置文件？**，键入 **y** ，然后按 **Enter** 。对于**是否要通过浏览器登录来创建配置文件？**，键入 **y** ，然后按 **Enter** 。 ![OCI 配置](images/oci-config.png)
    
6.  在 Firefox 浏览器中，点击您的活动会话。
    
    > 您将看到_授权已完成_，如下所示。 ![授权完成](images/authorization-complete.png)
    
7.  在**为私钥输入密码短语**中，将其留空并按 **Enter** 键。 ![空密码短语](images/empty-passphrase.png)
    
8.  使用上箭头键再次运行 **oce Ce ...** 命令并重新运行该命令，直到您看到**写入 Kubeconfig 文件 /home/opc/.kube/config** 的新配置。 ![创建 KubeConfig](images/create-kubeconfig.png)
    
9.  复制并粘贴以下命令以更改 **~/.kube/config** 文件权限。
    
        <copy>chmod 600 ~/.kube/config</copy>
        

## 任务 2：查看 OKE 堆栈的 Oracle WebLogic Server 云资源

1.  在 OCI 控制台中，单击**导航菜单**，选择**开发人员服务**。在资源管理器组下，单击**堆栈**。
    
2.  选择包含堆栈的**区间**。
    
3.  单击堆栈的名称。
    
4.  导航到**应用程序信息**选项卡，可以查看堆栈创建的资源。复制**堡垒实例公共 IP** 并将其粘贴到文本文件中。 ![复制堡垒](images/copy-bastion.png)
    

## 任务 3：在终端中启用代理配置

在此任务中，我们将启用从远程桌面到 Oracle Kubernetes 集群节点的 **ssh 隧道**。

1.  打开终端，复制并粘贴以下命令以在远程桌面中创建私钥文件。
    
        <copy>vi ~/.ssh/opc.key</copy>
        
2.  按 **i** 以进入插入模式，并将私钥的内容粘贴到此文件中，然后按转义符，然后输入 **：wq** 以保存此文件。
    
3.  复制并粘贴以下命令以更改私钥文件权限。
    
        <copy>chmod 600 ~/.ssh/opc.key</copy>
        
4.  复制以下命令并将其粘贴到终端中，然后将 **bastion-public-ip** 替换为您在最后一个任务中复制的公共 IP。
    
        <copy>ssh -D 1088 -fCqN -i ~/.ssh/opc.key opc@bastion-public-ip</copy>
        
5.  打开 **~/.kube/config** 文件，然后在此文件中复制并粘贴以下内容，如下所示。
    
        <copy>proxy-url: socks5://localhost:1088</copy>
        
    
    ![代理配置](images/proxy-config.png)
    

## 任务 4：验证 WebLogic Kubernetes 工具包 UI 与 Oracle Kubernetes 集群的连接

在此任务中，我们验证从 `WebLogic Kubernetes Toolkit UI` 应用程序到 _Oracle Kubernetes Cluster(OKE)_ 的连接。

1.  返回 WebLogic Kubernetes 工具包 UI，单击_活动_，然后选择 WebLogic Kubernetes 工具包 UI 窗口。
    
2.  单击 _Kubernetes_ -> _客户端配置_，然后单击_验证连接_。 ![验证连接](images/verify-connectivity.png)
    
3.  看到_验证 Kubernetes 客户机连接成功_窗口后，单击_确定_。 ![已成功连接](images/successfully-connected.png)
    

## 任务 5：将 WebLogic Kubernetes 操作员更新到 Oracle Kubernetes 集群

本节支持在目标 Kubernetes 集群中安装 WebLogic Kubernetes 运算符（“操作员”）。

1.  单击 **WebLogic 运算符**。指定以下配置详细信息，然后单击**更新运算符**。
    
    **Kubernetes 名称空间** - 要将运算符安装到的 Kubernetes 名称空间。输入值 **wko-operator-ns** ，如下面的屏幕截图中所示。
    
    **Kubernetes 服务账户** - 操作员在发出 Kubernetes API 请求时使用的 Kubernetes 服务账户。按下面的屏幕截图所示输入值 **wko-operator-sa** 。
    
    **用于操作员安装的 Helm 发行版名称** \- 用于标识此安装的 Helm 发行版名称。输入如下屏幕截图中所示的 **wko-weblogic-operator** 值。
    
    ![WebLogic 操作员](images/weblogic-operator.png)
    
    > **仅供参考：**  
    > ！默认情况下，操作员的_要使用的图像标记_字段设置为与 GitHub 容器注册表上最新的操作员版本对应的图像标记。  
    > 操作员需要知道它将管理的 Kubernetes 集群中的哪个 WebLogic 域。它在 Kubernetes 名称空间级别执行此操作，因此操作员配置为管理的 Kubernetes 名称空间中的任何 WebLogic 域将由要安装的操作员实例管理。  
    > 对于 _Kubernetes 名称空间选择策略_字段，我们选择_标签选择器_，这意味着具有 _weblogic-operator=enabled_ 标签的任何 Kubernetes 名称空间将由该运算符管理。  
    > ![操作员图像](images/operator-image.png)
    
    > 通过启用“启用群集角色绑定”，操作员安装将创建操作员将用于所有托管名称空间的 Kubernetes ClusterRole 和 ClusterRoleBinding。  
    > 默认情况下，操作员的 REST API 未在 Kubernetes 集群外部公开。要允许公开 REST API，您可以启用_外部公开 REST API_ 。 ![角色绑定](images/role-binding.png)  
    
    > 使用此窗格可以覆盖操作员的 Java 日志记录配置，这在调试操作员的问题时很有用。  
    > ![Java 日志记录](images/java-logging.png)  
    
    > 有关 _WebLogic Kubernetes 操作员映像_、_Kubernetes 名称空间选择策略_、_WebLogic Kubernetes 角色绑定_、_外部 REST API 访问_、_第三方集成_和 _Java 日志记录_的详细信息，请参阅 [WebLogic Kubernetes 操作员](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/)文档。
    
2.  在看到 _WebLogic Kubernetes 操作员安装完成_后，单击_确定_。 ![已安装操作员](images/operator-installed.png)
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 10 月