# 在 Verrazzano 上部署 Helidon 应用程序

## 简介

此实验将指导您完成 Helidon 快速入门 mp 应用程序的部署过程。

估计时间：10 分钟

### Verrazzano 和应用程序部署

Verrazzano 支持使用[打开应用程序模型 (OAM)](https://oam.dev/) 的应用程序定义。Verrazzano 应用程序由组件和应用程序配置组成。

使用 Verrazzano 部署应用程序时，平台会设置连接和网络策略、服务网格中的入站，并建立监视堆栈来捕获度量、日志和跟踪。Verrazzano 使用 OAM 组件定义系统的功能单元，然后通过定义关联的应用程序配置来组合和配置这些单元。

### Verrazzano 组件

Verrazzano OAM 组件是描述应用程序的一般组合和环境要求的 [Kubernetes 定制资源](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)。

以下代码显示了此实验中使用的 Helidon _quickstart-mp_ 应用程序的简单 Helidon 应用程序组件。此资源描述了一个组件，该组件由包含公开单个端点的 Helidon 应用程序的单个 Docker 映像实施。

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: hello-helidon-component
      namespace: hello-helidon
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: hello-helidon-workload
          labels:
            app: hello-helidon
        spec:
          deploymentTemplate:
            metadata:
              name: hello-helidon-deployment
            podSpec:
              containers:
                - name: hello-helidon-container
                  image: "END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp:1.0"
                  ports:
                    - containerPort: 8080
                      name: http
    

组件的每个字段的简要说明：

*   **apiVersion** - 组件定制资源定义的版本
*   **kind** - 组件定制资源定义的标准名称
*   **metadata.name** - 用于创建组件的定制资源的名称
*   **metadata.namespace** - 用于创建此组件的定制资源的名称空间
*   **spec.workload.kind** -VerrazzanoHelidonWorkload 定义 Kubernetes 的无状态工作负荷
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name** - 用于创建 Kubernetes 无状态工作负荷的名称
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - 实施容器
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - 容器公开的端口

### Verrazzano 应用程序配置

Verrazzano 应用程序配置是 Kubernetes 定制资源，可提供特定于环境的定制设置。以下代码显示了此练习中使用的 Helidon _quickstart-mp_ 示例的应用程序配置。此资源指定将应用程序部署到 hello-helidon 名称空间。

其他运行时功能是使用增大工作负荷的特性或运行时重叠指定的。例如，入站特性指定入站主机和路径，而度量特性提供用于获取应用程序相关度量的 Prometheus 刮板。

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: hello-helidon-appconf
      namespace: hello-helidon
      annotations:
        version: v1.0.0
        description: "Hello Helidon application"
    spec:
      components:
        - componentName: hello-helidon-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                    port: 8080
                    scraper: verrazzano-system/vmi-system-prometheus-0
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: hello-helidon-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/help/allGreetings"
                          pathType: Prefix
    

应用程序配置中每个字段的简要说明：

*   **apiVersion** -ApplicationConfiguration 定制资源定义的版本
*   **kind** - 应用程序配置定制资源定义的标准名称
*   **metadata.name** - 用于创建此应用程序配置资源的名称
*   **metadata.namespace** - 用于此应用程序配置定制资源的名称空间
*   **spec.components** - 对用于指定运行时配置的应用程序组件的引用
*   **spec.components\[\].traits** - 为应用程序组件指定的特性

要探究特征，我们可以检查入站特征的字段：

*   **apiVersion** -OAM 特性定制资源定义的版本
*   **kind** -IngressTrait 是 OAM 应用程序入站特性定制资源定义的名称
*   **spec.rules.paths** - 用于访问应用程序的上下文路径

### 目标

在本练习中，您将：

*   验证 Verrazzano 环境已成功安装。
*   部署 Helidon _quickstart-mp_ 应用程序。
*   验证 Helidon _quickstart-mp_ 应用程序的部署。

### 先备条件

要运行此实验，您必须：

*   在 Oracle Cloud Infrastructure 上运行的 Kubernetes (OKE) 集群。
*   Verrazzano 安装在 Kubernetes (OKE) 集群上开始。
*   容器打包的 Helidon _quickstart-mp_ 应用程序在容器注册表中可用。

## 任务 1：验证成功的 Verrazzano 安装

Verrazzano 在多个名称空间中安装多个对象。Verrazzano 组件安装在名称空间 _verrazzano-system_ 中。

1.  请验证与多个对象关联的所有云池的状态是否为_正在运行_。您将具有处于_正在运行_状态的云池，如下所示。
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    输出结果应如下所示：
    
        $   kubectl get pods -n verrazzano-system
        NAME                                     READY   STATUS    RESTARTS  AGE
        coherence-operator-b5dc669c6-rk2sm       1/1     Running     4       46m
        fluentd-54f5x                            2/2     Running     2       46m
        fluentd-h7mgh                            2/2     Running     1       46m
        fluentd-xcdfz                            2/2     Running     0       46m
        oam-kubernetes-runtime-5b48f944b-cx7b9   1/1     Running     0       46m
        verrazzano-application-operator-665c5c94 1/1     Running     0       46m
        verrazzano-application-operator-webhook  1/1     Running     0       46m
        verrazzano-authproxy-67776ff58b-8lkzs    3/3     Running     0       46m
        verrazzano-cluster-operator-67dc569555   1/1     Running     0       46m
        verrazzano-cluster-operator-webhook-57f  1/1     Running     0       46m
        verrazzano-console-7d95c98cb9-9ql2x      2/2     Running     0       46m
        verrazzano-monitoring-operator-59ff9576  2/2     Running     0       46m
        vmi-system-es-master-0                   2/2     Running     0       46m
        vmi-system-grafana-7fd956b585-2tbgl      3/3     Running     0       46m
        vmi-system-kiali-dd87546d6-ddxss         2/2     Running     0       46m
        vmi-system-osd-7687d6fccf-nm7kt          2/2     Running     0       46m
        weblogic-operator-54979449f4-njgrq       2/2     Running     0       46m
        weblogic-operator-webhook-f7ff8c8cf      1/1     Running     0       46m
        

## 任务 2：部署 Helidon 快速入门 mp 应用程序

1.  在 Cloud Shell 环境中下载 Verrazzano OAM 组件 yaml 文件和 Verrazzano 应用程序配置文件：
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-app.yaml >~/hello-helidon-app.yaml
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-comp.yaml >~/hello-helidon-comp.yaml
        cd ~
        </copy>
        
2.  修改 _hello-helidon-comp.yaml_ 中的映像名称。可以使用 `vi` 编辑器：
    
        <copy>vi ~/hello-helidon-comp.yaml</copy>
        
3.  使用 `i` 更改插入模式并修改映像名称以反映第 23 行中的系统信息库路径：
    
        image: "END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0"
        
    
    例如：
    
        image: "ocir.io/tenancynamespace/quickstart-mp-your_first_name:1.0"
        
4.  使用 `Esc` 退出插入模式，然后键入 `:wq` 保存更改并关闭编辑器。
    
5.  为 Helidon 快速启动 mp 应用程序创建 `hello-helidon` 名称空间。我们将所有 Kubernetes Artefacts 保留在一个单独的命名空间中。
    
        <copy>
        kubectl create namespace hello-helidon
        </copy>
        
    
    > 名称空间是一种将集群组织到虚拟子集群的方法。在一个集群中可以有任意数量的名称空间，每个名称空间以逻辑方式与其他名称空间分离，但可以相互通信。
    
6.  我们需要让 Verrazzano 知道我们存储在名称空间 Verrazzano Artifact 中。因此，我们需要添加标识由 Verrazzano 管理的 `hello-helidon` 名称空间的标签。标签用于指定对用户有意义且相关的对象的标识属性。
    
    在此处，对于 `hello-helidon` 名称空间，我们将为其附加一个标签，该标签将此名称空间标记为由 Verrazzano 管理。 _istio-injection=enabled_ 启用 Istio "sidecar"，因此有助于建立 Istio 代理。通过 Istio 代理，我们可以访问其他 Istio 服务，例如 Istio 网关。要使用上述属性将标签添加到 `hello-helidon` 名称空间，请复制以下命令并在 Cloud Shell 中运行该标签：
    
        <copy>
        kubectl label namespace hello-helidon verrazzano-managed=true istio-injection=enabled
        </copy>
        
7.  现在，我们希望在 _cluster1_ 上部署 Helidon _quickstart-mp_ 容器化应用程序。为此，我们需要 Kubernetes 部署配置。此部署指示 Kubernetes 为 Helidon _quickstart-mp_ 应用程序创建和更新实例。此处提供了 `hello-helidon-comp.yaml` 文件，用于指示 Kubernetes。
    
    要部署 Helidon _quickstart-mp_ 应用程序，请复制并粘贴以下两个命令，如下所示。`hello-helidon-comp.yaml` 文件包含各种 OAM 组件的定义，其中 OAM 组件是描述应用程序的一般组合和环境要求的 Kubernetes 定制资源。
    
        <copy>kubectl apply -f ~/hello-helidon-comp.yaml -n hello-helidon</copy>
        
    
    `hello-helidon-app.yaml` 文件是一个 Verrazzano 应用程序配置文件，它提供特定于环境的定制。
    
        <copy>kubectl apply -f ~/hello-helidon-app.yaml -n hello-helidon</copy>
        
8.  等待 pod 处于_正在运行_状态。使用此 _kubectl_ 命令可等待所有 pod 在 hello-helidon 名称空间内处于 _Running_ 状态。大约需要 1-2 分钟。
    
        <copy>kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s</copy>
        
    
    当云池准备就绪时，您可以看到类似的响应：
    
        $ kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s
        pod/hello-helidon-deployment-58fdd5cd4-94wjf condition met
        
    
    还可以直接列出 pod 来检查其状态：
    
        $ kubectl  get po -n hello-helidon
        NAME                                       READY   STATUS    RESTARTS   AGE
        hello-helidon-deployment-58fdd5cd4-94wjf   2/2     Running   0          34m
        

## 任务 3：验证 Helidon 快速启动 mp 应用程序的成功部署

1.  验证 `help/allGreetings` 端点。要确定从外部/负载平衡器 IP 和应用程序配置构建的 URL，请执行以下命令：
    
        <copy>echo https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings</copy>
        
    
    这将输出到 REST 端点的正确 URL，例如：
    
        https://hello-helidon-appconf.hello-helidon.xx.xx.xx.xx.nip.io/help/allGreetings
        
2.  使用此链接可从浏览器进行测试。但是，由于自签名证书，您需要接受风险并允许浏览器继续请求处理。
    
    您可能会发现使用 `curl` 更容易，因为响应只是字符串：
    
        <copy>curl -k https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings; echo</copy>
        
    
    您应该会看到您在开发期间收到的相同结果：
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
3.  将 _Cloud Shell_ 保持打开状态；我们将在下一个实验中使用它。
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月