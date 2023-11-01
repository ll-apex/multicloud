# 将入站控制器部署到 Oracle Kubernetes 集群 (OKE)

## 简介

在本练习中，我们将安装 _Traefik_ 入站控制器。稍后，我们将更新_入站路由_以访问应用程序和管理服务器。

估计时间：10 分钟

### 目标

在本练习中，您将：

*   在 Kubernetes 集群上安装入站控制器。
*   更新入站路由。

## 任务 1：将入站控制器安装到 Oracle Kubernetes 集群

在此任务中，我们将安装_入站控制器_。

1.  单击_入站控制器_。您可以看到一些预填充的值，使其保持不变，然后单击 _Install Ingress Controller_ 。 ![安装入站控制器](images/install-ingress-controller.png)
    
    > **仅供参考：**  
    > 这将成功将 _traefik-operator_ 入站控制器安装到 Kubernetes 名称空间 _traefik-ns_ 。
    
2.  看到 _Ingress Controller Installation Complete_ 后，单击 _Ok_ 。 ![已安装入站控制器](images/ingress-controller-installed.png)
    

## 任务 2：更新入站路由以访问应用程序

在此任务中，我们将添加用于访问管理控制台和应用程序的入站路由。最后，我们获取了可访问的端点。

1.  向下滚动并单击 _+_ 图标以添加_入站路由配置_。 ![添加入站路由](images/add-ingress-routes.png)
    
2.  单击“Edit（编辑）”图标，如图所示修改值。 ![编辑入站](images/edit-ingress.png)
    
3.  输入以下详细信息，然后单击_确定_。  
    名称：console  
    路径表达式：/console  
    目标服务名称空间：test-domain-ns  
    目标服务：test-domain-admin-server  
    目标端口：7001  
    
    ![控制台入站](images/console-ingress.png)
    
4.  以类似的方式，同时添加以下 _opdemo_ 入站路由：  
    名称：opdemo  
    路径表达式：/opdemo  
    目标服务名称空间：test-domain-ns  
    目标服务：test-domain-cluster-cluster-1  
    目标端口：8001  
    ![运营演示入站](images/opdemo-ingress.png)
    
5.  同样，请添加以下 _remote-console_ 入站路由：  
    名称：远程控制台  
    路径表达式：/  
    目标服务名称空间：test-domain-ns  
    目标服务：test-domain-admin-server  
    目标端口：7001  
    ![远程控制台入站](images/remote-console-ingress.png)
    
6.  要更新入站路由，请单击_更新入站路由_。 ![更新入站路由](images/update-ingress-routes.png)
    
7.  在_更新现有入站路由_中，单击_是_。
    
8.  看到_入站路由更新完成_窗口后，单击_确定_。 ![更新入站已完成](images/update-ingress-complete.png)
    
    > 您需要记下此 IP 并将其保存在文本文件中。
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 10 月