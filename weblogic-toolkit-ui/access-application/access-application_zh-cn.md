# 测试应用程序部署

## 简介

### 关于 WebLogic 远程控制台

WebLogic 远程控制台是一个轻量级的开源控制台，可用于管理在任意位置（例如在物理或虚拟机、容器、Kubernetes 或 Oracle Cloud 中）运行的 WebLogic 服务器域。WebLogic 远程控制台不需要与 WebLogic 服务器域共存。

您可以在任意位置安装和运行 WebLogic 远程控制台，并使用 WebLogic REST API 连接到域。您只需启动桌面应用程序并连接到域的管理服务器即可。或者，您可以启动控制台服务器，在浏览器中启动控制台，然后连接到管理服务器。

WebLogic 服务器 12.2.1.3、12.2.1.4 和 14.1.1.0 完全支持 WebLogic 远程控制台。

**WebLogic 远程控制台的主要功能**

*   配置 WebLogic 服务器实例和集群
*   创建或修改 WDT 元数据模型
*   配置 WebLogic 服务器服务，如数据库连接 (JDBC) 和消息传送 (JMS)
*   部署和取消部署应用程序
*   启动和停止服务器和应用程序
*   监视服务器和应用程序的性能

在此实验室中，我们访问应用程序 _opdemo_ 并验证脱机内部部署域的成功迁移。我们还验证托管服务器云池之间的负载平衡。稍后，我们使用 WebLogic 远程控制台验证 kubernetes 环境中测试域资源的成功部署。

估计时间：10 分钟

观看下面的视频，快速浏览实验室。[测试应用程序部署](videohub:1_1khcsrbq)

### 目标

在本练习中，您将：

*   通过浏览器访问应用程序。
*   使用 WebLogic 远程控制台浏览 WebLogic 域。

## 任务 1：通过浏览器访问应用程序

在此任务中，我们访问 _opdemo_ 应用程序。单击刷新图标可向应用程序发出多个请求，以验证两个托管服务器云池之间的负载平衡。

1.  复制以下 URL 并将 _XX.XX.XX.XX_ 替换为您在上一个练习中记下的 IP。您可以看到以下输出。
    
        <copy>http://XX.XX.XX.XX/opdemo/?dsname=testDatasource</copy>
        
    
    ![打开申请](images/open-application.png)
    
2.  如果单击“Refresh（刷新）”图标，则可以查看两个托管服务器云池之间的负载平衡。 ![显示负载平衡](images/show-load-balancing.png)
    

## 任务 2：使用 WebLogic 远程控制台浏览 Kubernetes 集群上的 WebLogic 域

在此任务中，我们将探讨 WebLogic 远程控制台。我们在远程控制台中创建与 _Admin Server_ 的连接并验证 WebLogic 域中的资源。这将验证是否将内部部署域成功迁移到 Oracle Kubernetes 集群。

1.  要打开 WebLogic 远程控制台，请单击_活动_，在搜索框中键入 _WebLogic_ ，然后单击 _WebLogic 远程控制台_图标。
    
2.  单击 _Kiosk_ 下的 `Three dots`，然后选择_添加管理服务器连接提供程序_，然后单击_选择_。 ![管理服务器连接](images/adminserver-connection.png)
    
3.  输入以下数据，然后单击_确定_。  
    连接提供程序名称：AdminServer  
    用户名：weblogic  
    密码：welcome1  
    URL：`Copy_IP_From_TextFile`  
    ![连接详细资料](images/connection-details.png)
    
4.  单击_编辑树_图标，然后选择_服务_ -> _数据源_。您可以观察我们在内部部署域中看到的相同 Datasouce。 ![验证数据源](images/verify-datasources.png)
    
5.  显示域中正在运行的服务器。单击**监视树**图标，如图所示，然后选择**环境** -> **服务器**。您可以看到，我们的 **Admin Server** 和 2 个托管服务器云池正在运行。 ![正在运行的服务器](images/running-server-status.png)
    
6.  单击 **admin-server** ，您可以看到 WebLogic 版本为 **12.2.1.3.0** 。 ![服务器状态](images/wls-version.png)
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月