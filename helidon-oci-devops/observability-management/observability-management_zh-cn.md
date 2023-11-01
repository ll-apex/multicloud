# 浏览日志和度量浏览器

## 简介

在本练习中，您将验证 Helidon 应用程序是否已成功部署。此外，您将了解日志和度量浏览器服务。

估计时间：5 分钟

观看下面的视频，快速浏览实验室。[浏览日志和指标浏览器](videohub:1_7a0qaaif)

### 目标

在本练习中，您将：

*   验证 Helidon 应用程序是否已成功部署。
*   浏览 OCI 度量浏览器
*   了解 OCI 日志记录服务
*   验证 Helidon 应用程序的投入和准备情况

### 先备条件

*   Oracle 免费套餐（试用版）、付费版或 LiveLabs 云账户
*   熟悉 OCI 控制台

## 任务 1：访问 Helidon 应用程序

在此任务中，我们将使用 curl 访问应用程序以执行 GET 和 PUT HTTP 请求。

1.  返回到**代码编辑器**，在终端中复制并粘贴以下命令，将部署节点 **`PUBLIC_IP`** 设置为环境变量。
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  复制并粘贴以下命令以放置 **GET 请求**。您将具有如下所示的类似输出。
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  您好，这里是 **Joe** 。您将具有如下所示的类似输出。
    
        <copy>curl http://$PUBLIC_IP:8080/greet/Joe</copy>
        {"message":"Hello Joe!","date":[2023,5,23]}
        
4.  将 Hello 替换为另一个问候语，即 **Hola** 。您将具有如下所示的类似输出。
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,23]}
        

## 任务 2：浏览 OCI 度量浏览器

在此任务中，您将验证 **Helidon 指标**是否已推送到 **OCI 监视服务**。

1.  在云控制台中，单击_汉堡菜单_ -> _观察和管理_ -> _监视_下的**度量浏览器**，如下所示。 ![度量浏览器](images/metrics-explorer.png)
    
2.  向下滚动以转到**查询**部分，选择正确的区间，然后选择 **`helidon_metrics`** 作为度量名称空间，然后选择 **`request.count_counter`** 作为度量名称来构建查询。然后单击_更新图表_，如下所示。 ![创建查询](images/create-query.png)
    
3.  向上滚动，您将看到类似输出，如下所示。这提供了请求计数器图形格式的数据。 ![查询编辑器](images/query-editor.png)
    
4.  要在数据表中显示数据，请切换**显示数据表**的按钮，如下所示。在切换按钮时，_显示数据表_将替换为**显示图形**。 ![数据表](images/data-table.png)
    

## 任务 3：浏览 OCI 日志记录服务

在此任务中，我们将验证 **OCI 日志记录 SDK** 集成，通过浏览日志组 **app-log-group-helidon-ocw-hol** 上的日志 **app-log-group-helidon-ocw-hol** 将消息推送到日志记录服务。

1.  在云控制台中，单击_汉堡菜单_ -> _观察和管理_ -> _日志_。 ![日志菜单](images/logs-menu.png)
    
2.  选择正确的区间，然后选择 _`app-log-group-helidon-ocw-hol`_ 作为**日志组**，然后单击 **app-log-helidon-ocw-hol** ，如下所示。 ![选择区间](images/select-compartment.png)
    
3.  在**按时间筛选**中，从下拉列表中选择**今天**，您可以查看应用程序日志。 ![查看日志](images/view-logs.png)
    

## 任务 4：对 Helidon 应用程序进行健康检查以验证 liveness 和 readiness

Helidon **oci-mp** 应用程序添加了**健康检查**功能来验证**活动**和/或**就绪**。您可以分别在项目中检查 _GreetLivenessCheck_ 和 _GreetReadinessCheck_ 类文件，以查看它们是如何完成的。在 Kubernetes 之类的业务流程环境上以微服务形式运行应用程序，以确定微服务在不健康的情况下是否需要重新启动时，这将特别有用。特定于此实验，在应用程序启动_以确定 Helidon 应用程序是否成功启动_后，就可以在 DevOps 部署管道规范中利用就绪状态检查。在 _deployment\_spec.yaml_ 中的 **line #79** 中查看代码，了解其实际作用。

1.  返回到**代码编辑器**，在终端中复制并粘贴以下命令，将部署节点 **`PUBLIC_IP`** 设置为环境变量。
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  复制并粘贴以下命令以检查 **liveness** 。您将具有如下所示的类似输出。
    
        <copy>curl http://$PUBLIC_IP:8080/health/live</copy>
        {"status":"UP","checks":[{"name":"CustomLivenessCheck","status":"UP","data":{"time":1684391639448}}]}
        
3.  复制并粘贴以下命令以检查**就绪**。您将具有如下所示的类似输出。
    
        <copy>curl http://$PUBLIC_IP:8080/health/ready</copy>
        {"status":"UP","checks":[{"name":"CustomReadinessCheck","status":"UP","data":{"time":1684391438298}}]}
        

您现在可以**继续下一个实验。**

## 了解详细信息

*   [使用 Helidon 的 OCI 应用](https://medium.com/helidon/oci-application-with-helidon-caa78cacaee5)
*   [Helidon MP 度量](https://helidon.io/docs/v3/#/mp/metrics/metrics)
*   [Helidon MP 度量指南](https://helidon.io/docs/v3/#/mp/guides/metrics)
*   [Helidon MP 健康状况](https://helidon.io/docs/v3/#/mp/health)
*   [Helidon MP 健康检查指南](https://helidon.io/docs/v3/#/mp/guides/health)

## 确认

*   **作者** - Keith Lustria
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月