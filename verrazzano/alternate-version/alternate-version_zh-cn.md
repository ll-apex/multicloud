# 通过简化的解决方案更改版本

## 简介

在本练习中，我们将使用修改后的 bobbys-helidon-stock-application 映像。此映像存储在 Oracle Cloud 容器注册表中的公共资料档案库中。我们还使用修改后的 bobs-books-comp-mod.yaml 文件，该文件指向修改后的 bobbys-helidon-stock-application 映像。

### 目标

在本练习中，您将：

*   下载修改后的 bobs-books-comp-mod.yaml 文件。
*   使用 kubectl 应用更改

### 先备条件

*   运行 Lab 1，在 Oracle Cloud Infrastructure 上创建 OKE 集群。
*   运行 Lab 2，在 Kubernetes 集群上安装 Verrazzano。
*   运行部署 Bobs-Books 应用程序的 Lab 3。
*   您应该具有文本编辑器，可以在其中粘贴命令和 URL，并根据您的环境对其进行修改。然后，您可以复制并粘贴修改后的命令，以便在 _Cloud Shell_ 中运行它们。

## 任务 1：下载修改后的 bobs-books-comp-mod.yaml 文件

1.  运行以下命令下载修改后的 bobs-books-comp-mod.yaml 文件。
    
        <copy>curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/alternate-version/bobs-books-comp-mod.yaml >~/bobs-books-comp-mod.yaml</copy>
        
    
    ![下载文件](images/downloadfile.png " ")
    

## 任务 2：使用 kubectl 应用更改

1.  要应用更改，请在 _Cloud Shell_ 中复制并粘贴以下命令。应用更改时，新 pod 将初始化为新组件的请求，而与旧组件关联的 pod 将继续处理请求。之后，新云池将达到_正在运行_状态后，旧云池将开始_已终止_。最终，只有新云池将处于_正在运行_状态。
    
        <copy>kubectl apply -f ~/bobs-books-comp-mod.yaml -n bobs-books</copy>
        
    
    ![应用更改](images/applychanges.png " ")
    
    您可以在输出中观察；仅配置了 _component.core.oam.dev/bobby-helidon_ ，其他组件不变。
    
2.  要查看新 pod 如何初始化以及旧 pod 如何进入_终止_状态，请在 _Cloud Shell_ 中复制并粘贴以下命令。
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    将看到类似如下的输出：
    
        vera_zano@cloudshell:~ (us-ashburn-1)$ kubectl get pods -n bobs-books
        NAME                                               READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                                 2/2    Running  0         130m
        bobbys-front-end-adminserver                       4/4    Running  0         127m
        bobbys-front-end-managed-server1                   4/4    Running  0         126m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  0/2    PodInitializing  0         10s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Running  0         130m
        bobs-bookstore-adminserver                         4/4    Running  0         127m
        bobs-bookstore-managed-server1                     4/4    Running  0         126m
        mysql-65d864bf8c-xf64p                             2/2    Running  0         130m
        robert-helidon-bfdfb58b8-58qfs                     2/2    Running  0         130m
        robert-helidon-bfdfb58b8-lkw8m                     2/2    Running  0         130m
        roberts-coherence-0                                2/2    Running  0         130m
        roberts-coherence-1                                2/2    Running  0         130m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  1/2    Running  0         28s
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  2/2    Running  0         34s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        vera_zano@cloudshell:~ (us-ashburn-1)$
        
    
    在看到所有 pod 都处于 _Running_ 状态后，按 _CTRL + C_ 键可终止此命令。
    

将 _Cloud Shell_ 保持打开状态，因为我们上一个实验还需要它。

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 2 月