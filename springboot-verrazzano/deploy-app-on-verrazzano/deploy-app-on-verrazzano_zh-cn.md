# 在 Verrazzano 上部署 Springboot 应用程序

## 简介

此实验将指导您完成将 Springboot 应用程序 docker 映像部署到 OKE 的过程。

估计时间：10 分钟

### Verrazzano 和应用程序部署

Verrazzano 支持使用[打开应用程序模型 (OAM)](https://oam.dev/) 的应用程序定义。Verrazzano 应用程序由组件和应用程序配置组成。

使用 Verrazzano 部署应用程序时，平台会设置连接和网络策略、服务网格中的入站，并建立监视堆栈来捕获度量、日志和跟踪。Verrazzano 使用 OAM 组件定义系统的功能单元，然后通过定义关联的应用程序配置来组合和配置这些单元。

### Verrazzano 组件

Verrazzano OAM 组件是描述应用程序的一般组合和环境要求的 [Kubernetes 定制资源](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)。

以下代码显示了用于此实验中所用 Tomcat 样例应用程序的简单 Springboot 应用程序组件。此资源介绍了由单个 Docker 映像实施的组件，该映像包含公开单个端点的 Springboot 应用程序。

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: springboot-component
    spec:
      workload:
        apiVersion: core.oam.dev/v1alpha2
        kind: ContainerizedWorkload
        metadata:
          name: springboot-workload
          labels:
            app: springboot
            version: v1
        spec:
          containers:
          - name: springboot-container
            image: "EndpointOfYourRegion/TenancyNamespace/springboot-your_firstname:v1"
            ports:
              - containerPort: 8080
                name: springboot
    

组件的每个字段的简要说明：

*   **apiVersion** - 组件定制资源定义的版本
*   **kind** - 组件定制资源定义的标准名称
*   **metadata.name** - 用于创建组件的定制资源的名称
*   **spec.workload.kind** -ContainerizedWorkload 定义 Kubernetes 的无状态工作负荷
*   **spec.workload.spec.containers.ports.containerPort** - 容器公开的端口

### Verrazzano 应用程序配置

Verrazzano 应用程序配置是 Kubernetes 定制资源，可提供特定于环境的定制设置。以下代码显示了此练习中使用的 Springboot 应用程序示例的应用程序配置。此资源指定将应用程序部署到 Springboot 名称空间。

其他运行时功能是使用增大工作负荷的特性或运行时重叠指定的。例如，入站特性指定入站主机和路径，而度量特性提供用于获取应用程序相关度量的 Prometheus 刮板。

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: springboot-appconf
      annotations:
        version: v1.0.0
        description: "Spring Boot application"
    spec:
      components:
        - componentName: springboot-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                  scraper: verrazzano-system/vmi-system-prometheus-0
                  path: "/actuator/prometheus"
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: springboot-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
    

应用程序配置中每个字段的简要说明：

*   **apiVersion** -ApplicationConfiguration 定制资源定义的版本
*   **kind** - 应用程序配置定制资源定义的标准名称
*   **metadata.name** - 用于创建此应用程序配置资源的名称
*   **spec.components** - 对用于指定运行时配置的应用程序组件的引用
*   **spec.components\[\].traits** - 为应用程序组件指定的特性

要探究特征，我们可以检查入站特征的字段：

*   **apiVersion** -OAM 特性定制资源定义的版本
*   **kind** -IngressTrait 是 OAM 应用程序入站特性定制资源定义的名称
*   **spec.rules.paths** - 用于访问应用程序的上下文路径

### 目标

在本练习中，您将：

*   部署 Springboot 应用程序。
*   验证 Springboot 应用程序的部署。

### 先备条件

要运行此实验，您必须：

*   在 Oracle Cloud Infrastructure 上运行的 Kubernetes (OKE) 集群。
*   Verrazzano 安装在 Kubernetes (OKE) 集群上开始。
*   容器打包的 _`springboot-your_firstname`_ 应用程序在容器注册表中可用。

## 任务 1：部署 Spring 引导应用程序

1.  为 Springboot 应用程序创建 `springboot` 名称空间。我们将所有 Kubernetes Artefacts 保留在一个单独的命名空间中。
    
        <copy>kubectl create namespace springboot</copy>
        
    
    > 名称空间是一种将集群组织到虚拟子集群的方法。在一个集群中可以有任意数量的名称空间，每个名称空间以逻辑方式与其他名称空间分离，但可以相互通信。
    
2.  我们需要让 Verrazzano 知道我们存储在名称空间 Verrazzano Artifact 中。因此，我们需要添加标识由 Verrazzano 管理的 `springboot` 名称空间的标签。标签用于指定对用户有意义且相关的对象的标识属性。
    
    在此处，对于 `springboot` 名称空间，我们将为其附加一个标签，该标签将此名称空间标记为由 Verrazzano 管理。 _istio-injection=enabled_ 启用 Istio "sidecar"，因此有助于建立 Istio 代理。通过 Istio 代理，我们可以访问其他 Istio 服务，例如 Istio 网关。要使用上述属性将标签添加到 `springboot` 名称空间，请复制以下命令并在 Cloud Shell 中运行该标签：
    
        <copy>kubectl label namespace springboot verrazzano-managed=true istio-injection=enabled</copy>
        
3.  在 _~/springboot-app_ 中，我们有 Springboot 应用程序的配置文件。在实验 2 中，我们为应用程序组件构建了一个新的 Docker 映像。此外，我们还将该 Docker 映像推送到 Oracle Cloud 容器注册表存储库。现在，在本练习中，我们将修改 _springboot-comp.yaml_ 文件，以便从 Oracle Cloud Container Registry 资料档案库获取新的 Docker 映像。要修改 _springboot-comp.yaml_ 文件，请复制以下命令并将其粘贴到 _Cloud Shell_ 中。
    
        <copy>vi ~/springboot-app/springboot-comp.yaml</copy>
        
4.  在实验 2 中，您保存了 Docker 映像全名。您需要复制以下行并将其粘贴到文本文件中。然后，您需要将 `docker image full name` 替换为 Docker 映像名称。然后复制修改的行，然后按 _i_ 将文本插入到 `*springboot-comp.yaml*` 文件中。将输出粘贴到行号 **19** （确保保留缩进），然后按 _Esc_ ，然后键入 _：wq_ 保存文件。
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![插入行](images/insert-line.png " ")
    
5.  现在，我们希望在 _cluster1_ 上部署 Springboot 容器化应用程序。为此，我们需要 Kubernetes 部署配置。此部署指示 Kubernetes 为 Springboot 应用程序创建和更新实例。此处提供了 `springboot-comp.yaml` 文件，用于指示 Kubernetes。
    
    要部署 Springboot 应用程序，请复制并粘贴以下两个命令，如下所示。`springboot-comp.yaml` 文件包含各种 OAM 组件的定义，其中 OAM 组件是描述应用程序的一般组合和环境要求的 Kubernetes 定制资源。
    
        <copy>kubectl apply -f ~/springboot-app/springboot-comp.yaml -n springboot</copy>
        
    
    `springboot-app.yaml` 文件是一个 Verrazzano 应用程序配置文件，它提供特定于环境的定制。
    
        <copy>kubectl apply -f ~/springboot-app/springboot-app.yaml -n springboot</copy>
        
6.  等待 pod 处于_正在运行_状态。使用此 _kubectl_ 命令可等待所有 pod 在 _springboot_ 名称空间内处于 _Running_ 状态。大约需要 1-2 分钟。
    
        <copy>kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s</copy>
        
    
    当云池准备就绪时，您可以看到类似的响应：
    
        $ kubectl wait --for=condition=Ready pods --all -n kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s --timeout=600s
        pod/springboot-workload-5c4ffc8954-mbgbb condition met
        pod/springboot-workload-bfb9d487-dwp7q condition met
        
    
    还可以直接列出 pod 来检查其状态：
    
        <copy>kubectl  get po -n springboot</copy>
        
    
        $ kubectl  get po -n springboot
        NAME                                 READY   STATUS    RESTARTS   AGE
        springboot-workload-bfb9d487-dwp7q   2/2     Running   0          68s
        $
        

## 任务 2：验证 Springboot 应用程序的成功部署

1.  验证 `/facts` 端点。要确定从外部/负载平衡器 IP 和应用程序配置构建的 URL，请执行以下命令：
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io springboot-springboot-appconf-gw -n springboot -o jsonpath={.spec.servers[0].hosts[0]})/facts</copy>
        
    
    这将输出到 REST 端点的正确 URL，例如：
    
        https://springboot-appconf.springboot.xx.xx.xx.xx.nip.io/facts
        
2.  将 URL 粘贴到浏览器中，然后单击浏览器中的_刷新_按钮。每次单击“刷新”按钮时，都会提供有关 Verrazzano 的随机事实。![应用程序 URL](images/application-url.png) ![另一个事实](images/another-fact.png)
    

您现在可以**进入下一个实验**。

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月