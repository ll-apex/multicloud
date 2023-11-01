# 部署 Bobby 的 Books 示例应用程序

## 介绍

### 关于 Bobby 的 Books 应用程序

![Bobby 书籍应用程序](images/bobbyoverview.png " ")

[Bobby's Books](https://verrazzano.io/docs/samples/bobs-books/) 包含三个主要部分：

*   后端_“订单处理”_应用，它是具有 REST 服务的 Java EE 应用，也是非常简单的 JSP UI，用于将数据存储在 MySQL 数据库中。此应用程序在 WebLogic 服务器上运行。
*   前端网店 _“Robert’s Books”_，是一家普通图书销售商。这是作为 Helidon 微服务实施的，该服务从 Coherence 获取书籍数据，使用 Coherence 高速缓存存储为订单管理器保存数据，并具有 React Web UI。
*   前端网店_“Bobby’s Books”_，是一家儿童书店。这是作为 Helidon 微服务实现的，该服务从（不同的）Coherence 高速缓存存储获取工作簿数据，与订单管理器直接交互，并在 WebLogic 服务器上运行 JSF Web UI。

有关此应用程序的更多信息和源代码，请参见 [Verrazzano 示例](https://github.com/verrazzano/examples)。

### Verrazzano 和应用程序部署

Verrazzano 支持使用[打开应用程序模型 (OAM)](https://oam.dev/) 的应用程序定义。Verrrazzano 应用程序由组件和应用程序配置组成。

在使用 Verrazzano 部署应用程序时，平台在服务网格中设置连接、网络策略和入站，并建立监视堆栈以获取度量、日志和跟踪。Verrazzano 使用 OAM 组件定义系统的功能单元，然后通过定义关联的应用程序配置来组合和配置这些单元。

### Verrazzano 组件

Verrazzano OAM 组件是描述应用程序的一般组合和环境要求的 [Kubernetes 定制资源](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)。

以下代码显示了此练习中使用的 Bobby 工作簿示例应用程序的一个组件。此资源描述了一个组件，该组件由包含公开单个端点的 Helidon 应用程序的单个 Docker 映像实施。

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: bobby-helidon
      namespace: bobs-books
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: bobbys-helidon-stock-application
          labels:
            app: bobbys-helidon-stock-application
        spec:
          deploymentTemplate:
            metadata:
              name: bobbys-helidon-stock-application
            podSpec:
              containers:
                - name: bobbys-helidon-stock-application
                  image: container-registry.oracle.com/verrazzano/example-bobbys-helidon-stock-application:0.1.12-1-20210624160519-017d358
                  imagePullPolicy: IfNotPresent
                  ports:
                    - containerPort: 8080
                      name: http
                  env:
                    - name: BACKEND_PORT
                      value: "8001"
                    - name: BACKEND_HOSTNAME
                      value: bobs-bookstore-cluster-cluster-1.bobs-books.svc.cluster.local
                    - name: COH_CLUSTER
                      value: bobbys-coherence
                    - name: COH_CACHE_CONFIG
                      value: coherence-cache-config.xml
                    - name: COH_POF_CONFIG
                      value: pof-config.xml
              imagePullSecrets:
                - name: bobs-books-repo-credentials
    

组件的每个字段的简要说明：

*   **apiVersion** - 组件定制资源定义的版本
*   **kind** - 组件定制资源定义的标准名称
*   **metadata.name** - 用于创建组件的定制资源的名称
*   **metadata.namespace** - 用于创建此组件的定制资源的名称空间
*   **spec.workload.kind** -VerrazzanoHelidonWorkload 定义 Kubernetes 的无状态工作负荷
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name** - 用于创建 Kubernetes 无状态工作负荷的名称
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - 实施容器
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - 容器公开的端口

您可以在 [bobs-books-comp.yaml](https://github.com/verrazzano/verrazzano/blob/master/examples/bobs-books/bobs-books-comp.yaml) 文件中找到 Bobbys 工作簿应用程序的完整组件说明。

### Verrazzano 应用程序配置

Verrazzano 应用程序配置是 Kubernetes 定制资源，可提供特定于环境的定制设置。以下代码显示了此练习中使用的 Bob 工作簿示例的应用程序配置。此资源指定将应用程序部署到 bobs-books 名称空间。

其他运行时功能是使用增大工作负荷的特性或运行时重叠指定的。例如，入站特性指定入站主机和路径，而度量特性提供用于获取应用程序相关度量的 Prometheus 刮板。

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: bobs-books
      namespace: bobs-books
      annotations:
        version: v1.0.0
        description: "Bob's Books"
    spec:
      components:
        - componentName: robert-helidon
          traits:
            - trait:
                apiVersion: core.oam.dev/v1alpha2
                kind: ManualScalerTrait
                spec:
                  replicaCount: 2
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
        - componentName: robert-coh
        - componentName: bobby-coh
        - componentName: bobby-helidon
        - componentName: bobby-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobbys-front-end"
                          pathType: Prefix
        - componentName: bobs-orders-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobs-bookstore-order-manager/orders"
                          pathType: Prefix
        - componentName: bobs-orders-configmap
        - componentName: bobs-mysql-deployment
        - componentName: bobs-mysql-service
        - componentName: bobs-mysql-configmap
    

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

此实验将指导您完成部署 Bobby 工作簿示例应用程序的过程。

估计时间：10 分钟

### 目标

在本练习中，您将：

*   接受从 Oracle 容器注册表中的资料档案库下载映像的许可证。
*   部署 Bobby 的 Books 示例应用程序。
*   验证 Bobby 的 Books 示例应用程序是否已成功部署。

### 先备条件

要运行 Lab 3，您必须具有：

*   运行 Lab 1，在 Oracle Cloud Infrastructure 上创建 OKE 集群。

\* 运行实验室 1，将 kubectl 配置为访问 Oracle Cloud Infrastructure 上的 OKE 集群。

*   运行 Lab 2，在 Kubernetes 集群上安装 Verrazzano。
*   文本编辑器，可在其中根据您的环境粘贴命令和 URL 并修改它们。然后，您可以复制并粘贴修改后的命令，以便在 _Cloud Shell_ 中运行它们。

## 任务 1：接受许可协议，从 Oracle 容器注册表中的资料档案库下载映像

对于部署 _Bobby's Books_ 示例应用程序，我们将使用示例图像。由于这些映像包含 Oracle 产品，因此在 bobs-books 文件 `bobs-books-comp.yaml` 中使用这些映像之前，您需要接受许可协议。

1.  单击 Oracle 容器注册表 [https://container-registry.oracle.com/](https://container-registry.oracle.com/) 的链接并登录。为此，您需要一个 Oracle 帐户。
    
    ![登录](images/registrysignin.png " ")
    
2.  在“用户名和密码”字段中输入您的 _Oracle 帐户凭据_，然后单击_登录_。稍后，我们将使用这些身份证明在 Kubernetes 中创建密钥。
    
    ![Oracle SSO](images/oraclesso.png " ")
    
3.  在主页上，选择 _Verrazzano_ 。
    
    ![注册表主页](images/registryhomepage.png " ")
    
4.  例如，bobbys-coherence、example-bobbys-front-end、example-bobs-books-order-manager 和 example-roberts-coherence 系统信息库，选择 _English_ 作为语言，然后单击_继续_。
    
    ![继续](images/continue.png " ")
    
5.  单击_接受_接受许可协议。
    
    ![接受协议](images/acceptagreement.png " ")
    
6.  验证您是否已接受与 Verrazzano 相关的系统信息库的许可协议，如下图所示。
    
    ![验证许可协议](images/verifyaggrement.png " ")
    
7.  在 Oracle 容器注册表的主页中，搜索 _weblogic_
    
    ![搜索 WebLogic](images/searchweblogic.png " ")
    
8.  如图所示，单击 _weblogic_ 并接受您的 Verrazzano 映像许可证。![单击 weblogic](images/clickweblogic.png) ![接受 weblogic](images/acceptagreement.png " ")
    

## 任务 2：部署 Bobby 的 Books 应用程序

我们需要下载源代码，其中包含配置文件 `bobs-books-app.yaml` 和 `bobs-books-comp.yaml`。

1.  下载 Bobby 书籍示例的 Verrazzano OAM 组件 yaml 文件和 Verrazzano 应用程序配置文件。单击_复制_，然后将命令粘贴到 Cloud Shell 中，如下所示：
    
        <copy>
        curl -LSs  https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-app.yaml >~/bobs-books-app.yaml
        curl -LSs https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-comp.yaml >~/bobs-books-comp.yaml
        cd ~
        </copy>
        
2.  我们将所有 Kubernetes Artifact 保留在单独的名称空间中。为 Bob 的 Books 示例应用程序创建一个名称空间。名称空间是一种将集群组织到虚拟子集群的方法。在一个集群中可以有任意数量的名称空间，每个名称空间以逻辑方式与其他名称空间分离，但可以相互通信。另外，我们需要让 Verrazzano 知道我们存储在名称空间 Verrazzano Artifact 中。因此，我们需要添加标识由 Verrazzano 管理的 bobs-books 名称空间的标签。标签用于指定对用户有意义且相关的对象的标识属性。在此，对于 bobs-book 名称空间，我们将为其附加一个标签，它将此名称空间标记为由 Verrazzano 管理。 _istio-injection=enabled_ 启用 Istio "sidecar"，因此有助于建立 Istio 代理。使用 Istio 代理，我们可以访问其他 Istio 服务，例如 Istio 网关等。要使用上述属性将标签添加到 bobs-books 名称空间，请复制以下命令并在 _Cloud Shell_ 中运行该标签
    
        <copy>
        kubectl create namespace bobs-books
        kubectl label namespace bobs-books verrazzano-managed=true istio-injection=enabled
        </copy>
        
    
    ![创建名称空间](images/createnamespace.png " ")
    
3.  复制以下命令下载脚本。此脚本对 Oracle 容器注册表的用户进行验证。如果验证成功，则会创建 docker 注册表密钥。Docker 注册表是一种存储和版本化映像的方式，例如 GitHub 表示普通代码，但对于容器（Kubernetes 可以拉取）。在此处，我们将创建 docker 注册表密钥，以允许从 Oracle 容器注册表提取 Bobby 的 Books 示例映像。在以下命令上单击_复制_，将其粘贴到您选择的任何文本编辑器中，并将用户名和密码分别替换为您在任务 1 中使用的电子邮件 ID 和密码，以接受从 Oracle 容器注册表下载映像的许可协议。然后，在 Cloud Shell 中，粘贴修改后的命令，如下所示：
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/deploy-bobsbook/create_secret.sh >~/create_secret.sh
        chmod 777 create_secret.sh
        ./create_secret.sh username 'password'    
        </copy>
        
    
    ![创建密钥](images/createsecret.png " ")
    
    > 请用单引号输入口令。
    
4.  我们需要使用身份证明创建多个 Kubernetes 密钥。在 Bobby 的 Books 应用程序中，我们有两个 WebLogic 域 _bobby-front-end_ 和 _bobs-bookstore_ 。WebLogic 域的凭证保留在 Kubernetes 密钥中，其中使用 WebLogic 域资源中的 _webLogicCredentialsSecret_ 指定密钥的名称。此外，必须在将运行域的名称空间中创建域身份证明密钥。我们需要创建 WebLogic 服务器域使用的名为 _bobbys-front-end-weblogic-credentials_ 和 _bobs-bookstore-weblogic-credentials_ 的密钥，其用户名值为 `weblogic`，并且在 bobs-books 名称空间中随机生成的口令。Bobby 的 Books 应用程序使用 _mysql_ 数据库。因此，我们创建了一个名为 _mysql-credentials_ 的新密钥，其用户名值为 `weblogic`，随机生成的口令以及 bobs-books 名称空间中的 JDBC URL _jdbc:mysql：//mysql.bobs-books.svc.cluster.local:3306/books_ 。我们将在 WebLogic DataSource 对象的 JDBC 连接字符串中使用此值。请将命令块复制并粘贴到 _Cloud Shell_ 中。
    
        <copy>
        export WLS_USERNAME=weblogic
        export WLS_PASSWORD=$((< /dev/urandom tr -dc 'A-Za-z0-9"'\''/<=>?\^_`|~' | head -c10);(date +%S))
        echo $WLS_PASSWORD
        kubectl create secret generic bobbys-front-end-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic bobs-bookstore-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic mysql-credentials \
            --from-literal=username=$WLS_USERNAME \
            --from-literal=password=$WLS_PASSWORD \
            --from-literal=url=jdbc:mysql://mysql.bobs-books.svc.cluster.local:3306/books \
            -n bobs-books
        cd ~
        </copy>
        
    
    ![创建资源](images/createresource.png " ")
    
5.  我们有一个包含三个节点的 Kuberneter 群集 _cluster1_ _verrazzano_。现在，我们希望在 _cluster1_ _verrazzano_ 上部署 Bobby 的 Books 容器化应用程序。为此，我们需要 Kubernetes 部署配置。此部署指示 Kubernetes 为 Bobby 的 Books 应用程序创建和更新实例。这里有 `bobs-books-comp.yaml` 文件，指示 Kubernetes 部署 Bobby 的 Books 应用程序。复制并粘贴以下两个命令，如下所示。`bobs-books-comp.yaml` 文件包含各种 OAM 组件的定义，其中 OAM 组件是描述应用程序的一般组合和环境要求的 Kubernetes 定制资源。要了解有关 `bobs-books-comp.yaml` 文件的更多信息，请查看此实验 3 的“简介”部分中的“Verrazzano 组件”。
    
        <copy>kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books</copy>
        
    
    输出结果应如下所示：
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh created
        component.core.oam.dev/robert-helidon created
        component.core.oam.dev/bobby-coh created
        component.core.oam.dev/bobby-helidon created
        component.core.oam.dev/bobby-wls created
        component.core.oam.dev/bobs-mysql-configmap created
        component.core.oam.dev/bobs-mysql-service created
        component.core.oam.dev/bobs-mysql-deployment created
        component.core.oam.dev/bobs-orders-configmap created
        component.core.oam.dev/bobs-orders-wls created
        $
        
6.  `bobs-books-app.yaml` 文件是一个 Verrazzano 应用程序配置文件，它提供特定于环境的定制设置。要了解有关 `bobs-books-app.yaml` 文件的详细信息，请查看此实验 3 的“简介”部分中的“Verrazzano 应用程序配置”。
    
        <copy>kubectl apply -f ~/bobs-books-app.yaml -n bobs-books</copy>
        
    
    输出结果应如下所示：
    
        $ kubectl apply -f ~/bobs-books-app.yaml -n bobs-books
        applicationconfiguration.core.oam.dev/bobs-books created
        $
        
7.  等待 Bobby 的 Books 示例应用程序中的所有 pod 处于 _Running_ 状态。您可能需要多次重复此命令才能成功。WebLogic 服务器和 Coherence 云池可能需要一些时间才能创建和就绪。此 _kubectl_ 命令将等待所有 pod 在 bobs-books 名称空间内处于 _Running_ 状态。花费大约 4-5 分钟。如果等待的时间超过 5 分钟，则可以再次重新运行该命令。
    
        <copy>kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s</copy>
        
    
    输出结果应如下所示：
    
          $ kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s
          pod/bobbys-coherence-0 condition met
          pod/bobbys-front-end-adminserver condition met
          pod/bobbys-front-end-managed-server1 condition met
          pod/bobbys-helidon-stock-application-5f74cbcc8b-cw4x4 condition met
          pod/bobs-bookstore-adminserver condition met
          pod/bobs-bookstore-managed-server1 condition met
          pod/mysql-6bc8f9f785-n4qjh condition met
          pod/robert-helidon-65b8874988-7x5vj condition met
          pod/robert-helidon-65b8874988-vnntp condition met
          pod/roberts-coherence-0 condition met
          pod/roberts-coherence-1 condition met
          $
        

## 任务 3：验证 Bobby 的 Book 应用程序的成功部署

验证应用程序配置、域、Coherence 资源和入站特性是否都存在。

1.  验证 _Bobby's Books_ 应用程序是否已成功部署到 bobs-books 名称空间中。
    
        <copy>kubectl get ApplicationConfiguration -n bobs-books</copy>
        
    
    输出结果应如下所示：
    
        $ kubectl get ApplicationConfiguration -n bobs-books
          NAME         AGE
          bobs-books   72m
        $
        
2.  验证是否已在 bobs-books 名称空间中成功创建了两个 WebLogic 域。
    
        <copy>kubectl get Domain -n bobs-books</copy>
        
    
    输出结果应如下所示：
    
        $ kubectl get Domain -n bobs-books
          NAME               AGE
          bobbys-front-end   73m
          bobs-orders-wls    73m
        $
        
3.  验证是否已在 bobs-books 名称空间中成功创建了两个 Coherence 集群。
    
        <copy>kubectl get Coherence -n bobs-books</copy>
        
    
    输出结果应如下所示：
    
        $ kubectl get Coherence -n bobs-books
        NAME              CLUSTER           ROLE           REPLICAS  READY PHASE
        bobbys-coherence  bobbys-coherence  bobbys-coherence  1       1    Ready
        roberts-coherence roberts-coherence roberts-coherence 2       2    Ready
        $
        
4.  要获取 Bobby 的 Book 应用程序的 IngressTrait，请在 _Cloud Shell_ 中运行以下命令。
    
        <copy>kubectl get IngressTrait -n bobs-books</copy>
        
    
    输出结果应如下所示：
    
        $ kubectl get IngressTrait -n bobs-books
          NAME                               AGE
          bobby-wls-trait-79b67d9d88         76m
          bobs-orders-wls-trait-57b4d8cb4b   76m
          robert-helidon-trait-54d7bcd54b    76m
        $
        
5.  验证是否已成功创建服务云池并转换为_正在运行_状态。请注意，这可能需要几分钟时间，您可能会看到一些服务终止并重新启动。最后，您将观察与 bobs-books 名称空间关联的所有 pod 都处于_正在运行_状态。请复制 _bobbys-helidon-stock-application_ 的 pod 详细信息。
    
        <copy>kubectl get pods -n bobs-books</copy>
        
    
        $ kubectl get pods -n bobs-books
        NAME                                          READY STATUS    RESTARTS   AGE
        bobbys-coherence-0                             2/2  Running   0          7m51s
        bobbys-front-end-adminserver                   4/4  Running   0          5m28s
        bobbys-front-end-managed-server1               4/4  Running   0          4m30s
        bobbys-helidon-stock-application-5f74cbc-cw4x4 2/2  Running   0          7m54s
        bobs-bookstore-adminserver                     4/4  Running   0          4m31s
        bobs-bookstore-managed-server1                 4/4  Running   0          3m41s
        mysql-6bc8f9f785-n4qjh                         2/2  Running   0          5m52s
        robert-helidon-65b8874988-7x5vj                2/2  Running   0          7m53s
        robert-helidon-65b8874988-vnntp                2/2  Running   0          7m54s
        roberts-coherence-0                            2/2  Running   0          7m52s
        roberts-coherence-1                            2/2  Running   0          7m51s
        $
        
    
    > 请注意 **bobbys-helidon-stock-application** 的 pod 名称。重新部署此组件时，您会发现此 pod 将进入_终止_状态，新 pod 将启动并在 Lab 7 中处于_正在运行_状态。
    
    将 _Cloud Shell_ 保持打开状态；我们还将其用于后续实验室。
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月