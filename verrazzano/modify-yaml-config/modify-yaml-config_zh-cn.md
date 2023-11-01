# 修改 Bobby 的 Books 配置 YAML 文件

## 简介

在实验 6 中，我们推出了针对 _bobby-helidon-stock-application_ 的更新 Docker 映像。现在，我们希望 Verrazzano 重新部署更新的应用程序和组件，而不影响服务。为此，我们需要配置 YAML 文件，以便 Verrazzano 拿起新映像并为其启动云池。云池处于_正在运行_状态后，它将终止与上一个应用程序和组件关联的云池。

估计时间：10 分钟

### 目标

在本练习中，您将：

*   修改 `bobs-books-comp.yaml` 文件。
*   使用 `kubectl` 应用更改。

### 先备条件

您应该具有文本编辑器，可以在其中粘贴命令和 URL，并根据您的环境对其进行修改。然后，您可以复制并粘贴修改后的命令，以便在 _Cloud Shell_ 中运行它们。

## 任务 1：修改 bobs-books-comp.yaml 文件

1.  我们有一个应用程序配置文件 _bobs-books-comp.yaml_ 。在实验室 2 中，我们下载了应用程序 YAML 文件。要更改为包含 yaml 文件的主目录，请复制以下命令并将其粘贴到 _Cloud Shell_ 中。
    
        <copy>cd ~</copy>
        
2.  在此位置，我们有 bobs-books 应用程序的配置文件。在实验 5 中，我们修改了 bobbys-helidon-stock-application，并为该组件构建了一个新的 Docker 映像。在实验 6 中，我们将该 Docker 映像推送到 Oracle Cloud 容器注册表资料档案库。现在，在本练习中，我们将修改 _bobs-books-comp.yaml_ 文件，以便从 Oracle Cloud 容器注册表存储库获取新的更新 Docker 映像。要修改 _bobs-books-comp.yaml_ 文件，请复制以下命令并将其粘贴到 _Cloud Shell_ 中。
    
        <copy>vi bobs-books-comp.yaml</copy>
        
    
    ![打开文件](images/openfile.png " ")
    
3.  在实验 5 中，您保存了 Docker 映像全名。您需要复制以下行并将其粘贴到文本编辑器中。然后，您需要将 `docker image full name` 替换为 Docker 映像名称。然后复制修改的行，然后按 _i_ 将文本插入到 `*bobs-books-comp.yaml*` 文件中。将输出粘贴到第 148 行（确保保留缩进），然后使用 _#_ 注释掉现有行号 147（如下图所示），然后按 _Esc_ 键，然后键入 _：wq_ 保存文件。
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![插入行](images/insert-line.png " ")
    

## 任务 2：使用 `kubectl` 应用更改

1.  要应用更改，请在 _Cloud Shell_ 中复制并粘贴以下命令。应用更改时，新 pod 将初始化为新组件的请求，而与旧组件关联的 pod 将继续处理请求。之后，新云池将达到_正在运行_状态后，旧云池将开始_已终止_。最终，只有新云池将处于_正在运行_状态。
    
        <copy>kubectl apply -f bobs-books-comp.yaml -n bobs-books</copy>
        
    
    输出结果应如下所示：
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh unchanged
        component.core.oam.dev/robert-helidon unchanged
        component.core.oam.dev/bobby-coh unchanged
        component.core.oam.dev/bobby-helidon configured
        component.core.oam.dev/bobby-wls unchanged
        component.core.oam.dev/bobs-mysql-configmap unchanged
        component.core.oam.dev/bobs-mysql-service unchanged
        component.core.oam.dev/bobs-mysql-deployment unchanged
        component.core.oam.dev/bobs-orders-configmap unchanged
        component.core.oam.dev/bobs-orders-wls unchanged
        $
        
    
    您可以在输出中观察；仅配置了 _component.core.oam.dev/bobby-helidon_ ，其他组件不变。
    
2.  要查看新 pod 如何初始化以及旧 pod 如何进入_终止_状态，请在 _Cloud Shell_ 中复制并粘贴以下命令。
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    将看到类似如下的输出：
    
        $ kubectl get pods -n bobs-books
        NAME                                         READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                           2/2    Running      0         130m
        bobbys-front-end-adminserver                 4/4    Running      0         127m
        bobbys-front-end-managed-server1             4/4    Running      0         126m
        bobbys-helidon-stock-application-64fb55-zzp  0/2    PodInitializing  0     10s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Running      0         130m
        bobs-bookstore-adminserver                   4/4    Running      0         127m
        bobs-bookstore-managed-server1               4/4    Running      0         126m
        mysql-65d864bf8c-xf64p                       2/2    Running      0         130m
        robert-helidon-bfdfb58b8-58qfs               2/2    Running      0         130m
        robert-helidon-bfdfb58b8-lkw8m               2/2    Running      0         130m
        roberts-coherence-0                          2/2    Running      0         130m
        roberts-coherence-1                          2/2    Running      0         130m
        bobbys-helidon-stock-application-64fb55-zzp  1/2    Running      0         28s
        bobbys-helidon-stock-application-64fb55-zzp  2/2    Running      0         34s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        $
        
    
    在看到所有 pod 都处于 _Running_ 状态后，按 _CTRL + C_ 键可终止此命令。
    
    将 _Cloud Shell_ 保持打开状态，因为我们上一个实验还需要它。
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月