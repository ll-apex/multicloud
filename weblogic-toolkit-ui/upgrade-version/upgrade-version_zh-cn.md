# 在 WebLogic 服务器版本中进行升级（可选）

## 简介

在本练习中，我们将修改主映像，并将 WebLogic 服务器映像与 _12.2.1.4-slim-ol8_ 标记一起使用。然后使用 WebLogic Kubernetes 工具包 UI 重新部署域。最后，我们通过 WebLogic 远程控制台验证新托管的服务器 pod 是否正在使用更新的 WebLogic 服务器映像。

估计时间：10 分钟

观看下面的视频，快速浏览实验室。[升级 WebLogic 服务器版本](videohub:1_5vonezmn)

### 目标

在本练习中，您将：

*   使用 WebLogic 服务器映像 (12.2.1.4) 作为主映像。
*   重新部署 WebLogic 域。

### 先备条件

*   访问在练习 1 中创建的 noVNC 远程桌面。

## 任务 1：输入新 WebLogic 服务器映像的详细信息作为主映像

在此任务中，我们将主映像更新为使用升级的 WebLogic Server 12.2.1.4.0 映像。

1.  返回 WebLogic Kubernetes 工具包 UI，单击 _WebLogic 域_。已将 WebLogic 服务器标记更改为 _12.2.1.4-slim-ol8_ 。 ![更新主图像标签](images/update-primary-image-tag.png)

## 任务 2：通过滚动重新启动服务器云池来更新已部署的应用程序

在此任务中，我们重新部署 WebLogic 域。稍后，我们使用 WebLogic 远程控制台验证服务器 pod 是否正在使用更新的 WebLogic Server 12.2.1.4.0 映像。

1.  单击_部署域_。这将重新部署该域。 ![重新部署域](images/redeploy-domain.png)
    
    > **仅供参考：**  
    > 更改了主映像时，我们会注意到逐一重新启动服务器。单击_部署域_时，它将启动_监视器作业_，该作业终止正在运行的管理服务器 pod，并为使用 WebLogic Server 12.2.1.4.0 映像的管理服务器创建新的 pod。自测器与托管服务器执行相同的过程。
    
2.  看到 _WebLogic 域部署到 Kubernetes 完成_窗口后，单击_确定_。 ![部署已完成](images/deployment-complete.png)
    
3.  返回到 _Terminal_ 并复制下面的命令并在终端中粘贴。您将看到逐个滚动重新启动服务器。首先，管理服务器云池终止并处于_正在运行_状态。
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![查看云池](images/view-pods.png)
    
4.  要验证管理服务器和托管服务器云池是否正在使用更新的 WebLogic 服务器映像，请单击 "Monitoring Tree"（监视树）图标，然后选择 _Environment（环境）_ -> _Servers（服务器）_ -> _admin-server_ 。您可以看到，它使用的是 12.2.1.4.0。 ![WLS 版本](images/wls-version.png)
    

Congratulation !!!

研讨会结束后。

我们希望您发现此研讨会非常有用。

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月