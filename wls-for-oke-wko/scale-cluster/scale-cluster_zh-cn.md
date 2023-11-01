# 缩放 WebLogic 集群（可选）

## 简介

在本练习中，我们将扩展一个 WebLogic 集群。在此处，我们将值修改为 _3_ 并重新部署域。

估计时间：5 分钟

### 目标

在本练习中，您将：

*   缩放 WebLogic Cluster。

## 任务 1：使用 WebLogic Kubernetes 工具包 UI 扩展 WebLogic 集群

在此任务中，只需将_副本_值从 2 修改为 3，然后重新部署域。

1.  返回 WebLogic Kubernetes 工具包 UI，单击 _WebLogic 域_。转到_集群_部分，然后单击_编辑_图标。  
    ![集群调整大小](images/cluster-resize.png)
    
2.  将副本从 _2_ 更改为 _3_ ，然后单击 _OK_ 。 ![更改副本](images/change-replicas.png)
    
3.  要重新部署该域，请单击_部署域_。 ![重新部署域](images/redeploy-domain.png)
    
4.  看到 _WebLogic Domain Deployment to Kubernetes Complete（将域部署到 Kubernetes 完成）_窗口后，单击 _Ok（确定）_。 ![部署已完成](images/deployment-complete.png)
    
5.  返回到_终端_窗口，单击_活动_并选择_终端_窗口。复制以下命令并粘贴到终端。
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![查看缩放](images/view-scaling.png)
    
    > 您可以查看域的重新部署，启动自测程序作业，该作业启动为 test-domain-managed-server3 创建 pod 的过程，有时此 pod 进入_运行_状态。
    
6.  返回到打开应用程序页面的浏览器。单击“Refresh（刷新）”按钮，您将立即看到三个托管服务器之间的负载平衡。 ![新服务器](images/new-server.png)
    
7.  返回到 WebLogic 远程控制台，单击_监视树_ -> _环境_ -> _服务器_。您会注意到 managed-server3。 ![远程控制台](images/remote-console.png)
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 10 月