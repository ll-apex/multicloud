# 将 WebLogic 域部署到 Oracle Kubernetes 集群 (OKE)

## 简介

在此实验中，我们将 WebLogic 域部署到 kubernetes 集群。在主图像部分中，我们指定 oracle 帐户身份证明。在辅助映像部分中，我们指定 oracle 云账户身份证明。此处还指定集群的副本。

估计时间：10 分钟

### 目标

在本练习中，您将：

*   将 WebLogic 域部署到 Kubernetes 集群。

## 任务 1：将 WebLogic 域部署到 Oracle Kubernetes 集群

在此任务中，我们将 WebLogic 域的 Kubernetes 定制资源部署到 Kubernetes 集群。

1.  向下滚动，在“主映像”部分中输入以下内容，在_映像提取密钥名称_中输入 _domain-secret_ ，在_映像注册表提取用户名_和_映像注册表提取密码_中使用 Oracle 帐户用户名和密码。在_映像注册表提取电子邮件地址_中输入您的 Oracle 电子邮件 ID。这些是您用来接受 Oracle 容器注册表中 _weblogic_ 映像许可的相同凭证。 ![主要图像详细信息](images/primary-image-details.png)
    
    > **仅供参考：**  
    > 我们正在从 Oracle 容器注册表提取映像，因此我们指定身份证明，用于接受 WebLogic 服务器映像的许可协议。
    
2.  向下滚动，在“辅助映像”部分中，取消选中**指定辅助映像提取身份证明**对应的框。
    
3.  在_集群_部分中，单击_编辑_图标，如下所示。 ![集群调整大小](images/cluster-resize.png)
    
4.  输入 _2_ 作为_副本_，然后单击_确定_。在成功将 WebLogic 域部署到 Kubernetes 集群之后，副本的大小决定了处于_正在运行_状态的托管服务器数。 ![集群副本](images/cluster-replicas.png)
    
5.  在“数据源”部分中，双击可编辑两个数据源的_口令_。您可以在两个数据源中指定 _tiger_ 作为口令。完成后，单击_部署域_。 ![Datasoure 密码](images/datasource-password.png)
    
    > 这会将 WebLogic 域 test-domain 部署到 Kubernetes 名称空间 _test-domain-ns_ 。
    
6.  看到 _WebLogic 域部署到 Kubernetes 完成_窗口后，单击_确定_。 ![部署已完成](images/deployment-complete.png)
    
7.  返回终端，单击_活动_并选择_终端_窗口。复制以下命令并将其粘贴到终端。您应该会看到类似的输出，其中先为管理服务器运行自测程序的 pod，然后为托管服务器运行后面的 pod 处于_正在运行_状态。
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![云池状态](images/pod-status.png)
    
8.  您还可以通过 _WebLogic Kubernetes 工具包 UI_ 获取域状态。返回到 _WebLogic Kubernetes 工具包 UI_ ，然后单击_获取域状态_。 ![域状态](images/domain-status.png)
    
9.  在 "Domain Status"（域状态）窗口中，向下滚动以查看所有服务器 pod 的状态，然后单击 _OK（确定）_。 ![服务器状态](images/server-status.png)
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 10 月