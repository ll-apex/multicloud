# 使用 Verrazzano 访问和监视应用程序

## 简介

您已部署了 Hello Helidon 应用程序。在此实验室中，我们将访问应用程序并使用 Verrazzano 提供的多种管理工具进行验证。

估计时间：20 分钟

**Grafana**

Grafana 是一款开源解决方案，可用于运行数据分析，提取对大量数据有意义的指标，并在可定制的漂亮仪表盘的帮助下监视我们的应用。该工具有助于研究、分析和监视一段时间内的数据，技术上称为时间序列分析。用于通过提供相对数据来跟踪用户行为、应用程序行为、在生产环境或预生产环境中弹出错误的频率、弹出错误的类型以及上下文方案。

[https://grafana.com/grafana/](https://grafana.com/grafana/)

**OpenSearch 仪表盘**

OpenSearch 仪表盘是 OpenSearch 集群上索引的内容的可视化仪表盘。Verrazzano 创建 OpenSearch 仪表盘部署，以提供用于查询和可视化 OpenSearch 中收集的日志数据的用户界面。

要访问 OpenSearch 仪表盘控制台，请阅读[访问 Verrazzano](https://verrazzano.io/latest/docs/access/) 。

要通过 OpenSearch 仪表盘查看 OpenSearch 索引的记录，请创建一个索引模式以筛选所需索引下的记录。

在任务 3 中，我们将探讨 OpenSearch 仪表盘。

[https://opensearch.org/docs/latest/dashboards/index/](https://opensearch.org/docs/latest/dashboards/index/)

**Prometheus**

Prometheus 是一个监视和警报工具包。Prometheus 将其度量收集并存储为时间序列数据，即度量信息与记录时间戳一起存储，以及称为标签的可选键值对。

[https://prometheus.io/](https://prometheus.io/)

**Rancher**

Rancher 是一个平台，允许 Verrazzano 在生产中的多个 Kubernetes 集群上运行容器。它支持混合，这意味着内部部署集群的单一管理平台，托管在云服务上。

[https://rancher.com/](https://rancher.com/)

**嘉利**

Kiali 是 Istio 的用户界面，使用起来非常简单。通过 Verrazzano 控制台，您可以直接访问 Kiali。从那里，您可以看到流量流以及任何热点或故障区域。然后，您可以向下钻取以查看每个详细信息。Kiali 使用 Prometheus 中存储的 Envoy 中的度量数据来构建图形模型

### 目标

在本练习中，您将：

*   浏览 Rancher 控制台。
*   浏览 Grafana 控制台。
*   浏览 OpenSearch 仪表盘。
*   浏览 Prometheus 控制台。
*   浏览 Rancher 控制台。
*   浏览 Kiali 控制台。

### 先备条件

*   在 Oracle Cloud Infrastructure 上运行的 Kubernetes (OKE) 集群。
*   安装在 Kubernetes (OKE) 集群上的 Verrazzano。
*   已部署 Hello Helidon 应用程序。

## 任务 1：浏览 Rancher 控制台

Verrazzano 安装多个控制台。安装的端点存储在已安装的 Verrazzano 定制资源的 `Status` 字段中。

1.  可以使用以下命令获取这些控制台的端点：
    
        <copy>kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .</copy>
        
    
    输出结果应如下所示：
    
        $ kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .
        {
        "consoleUrl": "https://verrazzano.default.xx.xx.xx.xx.nip.io",
        "grafanaUrl": "https://grafana.vmi.system.default.xx.xx.xx.xx.nip.io",
        "keyCloakUrl": "https://keycloak.default.xx.xx.xx.xx.nip.io",
        "kialiUrl": "https://kiali.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchDashboardsUrl": "https://osd.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchUrl": "https://opensearch.vmi.system.default.xx.xx.xx.xx.nip.io",
        "prometheusUrl": "https://prometheus.vmi.system.default.xx.xx.xx.xx.nip.io",
        "rancherUrl": "https://rancher.default.xx.xx.xx.xx.nip.io"
        }
        $
        
2.  使用 `https://rancher.default.YOUR_UNIQUE_IP.nip.io` 打开 Rancher 控制台。
    
3.  Verrazzano _dev_ 配置文件使用自签名证书，因此您需要单击**高级**以接受风险并跳过警告。
    
    ![高级](images/rancher-advanced.png)
    
4.  单击 **Proceed to rancher default XX.XX.XX.XX.nip.io(unsafe)** 。如果无法使用此选项继续，只需在此 Chrome 浏览器窗口中的任意位置输入 _thisisunsafe_ （不安全）。当您在 chrome 浏览器窗口中输入时，您无法看到它，但是在您输入完 _thisisunsafe_ 后，您可以立即看到下一页。您可以在[此处](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)找到更多详细信息。
    
    ![继续](images/rancher-proceed.png)
    
5.  单击 _Log in with Keycloak_ 。 ![Rancher 登录](images/keycloak-login.png)
    
6.  由于它重定向到 Keycloak 控制台 URL 进行验证，因此请单击**高级**。
    
    ![Keycloak 验证](images/keycloak-advanced.png)
    
7.  单击 **Proceed to Keycloak default XX.XX.XX.nip.io(unsafe)** 。如果无法使用此选项继续，只需在此 Chrome 浏览器窗口中的任意位置输入 _thisisunsafe_ （不安全）。当您在 chrome 浏览器窗口中输入时，您无法看到它，但是在您输入完 _thisisunsafe_ 后，您可以立即看到下一页。您可以在[此处](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)找到更多详细信息。
    
    ![继续](images/keycloak-proceed.png)
    
8.  现在，我们需要 Verrazzano 控制台的用户名和密码。_用户名_为 _verrazzano_ ，要查找密码，请返回到 _Cloud Shell_ 并粘贴以下命令以查找 _Rancher Console_ 的密码。
    
        <copy>kubectl get secret --namespace verrazzano-system verrazzano -o jsonpath={.data.password} | base64 --decode; echo</copy>
        
9.  复制密码并返回到打开 _Rancher Console_ 的浏览器。将口令粘贴到 _Password_ 字段中，然后输入 _verrazzano_ 作为 _Username_ ，然后单击 **Sign In** 。
    
    ![SignIn](images/verrazzano-sign-in.png)
    
10.  从 rancher 控制台的主页，您可以查看 Verrazzano 链接。您可以从 rancher console.Click _汉堡菜单_ -> _Verrazzano_ 访问其中任何控制台。
    
    ![Rancher 主页](images/rancher-home.png)
    
11.  单击_应用程序_。此部分显示名称空间的所有应用程序，由 Verrazzano 管理。单击 _hello-helidon_ 名称空间中的 _hello-helidon_ 应用程序。 ![Helidon 应用程序](images/helidon-application.png)
    
12.  您可以查看与应用程序关联的云池。云池名称包含自动生成的唯一字符串，用于标识该特定副本。要查看云池的日志，请单击_三个点_ -> _查看日志_。 ![Helidon 组件](images/view-pod-logs.png)
    
13.  您可以查看应用程序的应用程序日志。如果看不到应用程序日志，请单击**设置**（带有齿轮图标的蓝色按钮），然后更改时间筛选器以显示容器开始时的所有日志条目。要查看与应用程序关联的组件，请单击_组件_。 ![应用程序日志](images/view-pod-log.png)
    
14.  此应用程序只有一个 component.To 查看相关资源是什么，单击_相关资源_。![Helidon 资源](images/helidon-resource.png) ![Helidon 资源](images/helidon-resources.png)
    
15.  单击 _Hamburgar 菜单_ -> _local_ 以打开 _Cluster Explorer_ 。_集群浏览器_允许您从 Rancher UI 查看和处理 Kubernetes 集群中的所有定制资源和 CRD。 ![Verrazzano 集群](images/verrazzano-cluster.png)
    
16.  该仪表盘概述了集群和部署的应用程序。资源数属于_用户名空间_，这实际上几乎是包括系统在内的所有资源。您可以按仪表盘顶部的名称空间进行筛选，但现在不需要这样做。单击左侧菜单中的**节点**项可查看节点的当前负载概览。
    
    ![集群浏览器](images/cluster-dashboard.png)
    
17.  整个部署对 OKE 群集没有任何影响。现在单击左侧菜单中的**工作量**，然后单击**部署**项以检查已部署的应用程序。
    
    ![节点](images/node.png)
    
18.  您可以查看多个部署。
    
    ![部署](images/deployment.png)
    

## 任务 2：浏览 Grafana 控制台

1.  单击_汉堡菜单_ -> _主页_。 ![住宅](images/ranchar-menu.png)
    
2.  在主页上，您将看到用于打开 _Grafana 控制台_的链接。单击 **Grafana Console** 的链接，如下所示：
    
    ![Grafana 主目录](images/grafana-link.png)
    
3.  单击**高级**。
    
    ![高级](images/grafana-advanced.png)
    
4.  单击 **Proceed to grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)** 。如果无法使用此选项继续，只需在此 Chrome 浏览器窗口中的任意位置输入 _thisisunsafe_ （不安全）。当您在 chrome 浏览器窗口中输入时，您无法看到它，但是在您输入完 _thisisunsafe_ 后，您可以立即看到下一页。您可以在[此处](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)找到更多详细信息。
    
    ![继续](images/grafana-proceed.png)
    
5.  此时将打开 Grafana 主页。单击左上角的**主页**。
    
    ![住宅](images/grafana-home.png)
    
6.  键入 `Helidon`，您将在_一般信息_下看到 **Helidon 监视仪表盘**。单击 **Helidon 监视仪表盘**。
    
    ![Helidon 仪表盘](images/search-helidon.png)
    
7.  观察 Helidon _quickstart-mp_ 应用程序的 JVM 详细信息，如状态、堆使用情况、运行时间、JVM 堆、线程计数、HTTP 请求等。这是专用于 Helidon 负载的预构建仪表盘。当然，您可以根据需要定制此仪表盘并添加定制诊断信息。
    
    ![仪表盘](images/helidon-dashboard.png)
    

## 任务 3：浏览 OpenSearch 仪表盘

1.  返回 Verrazzano 主页，然后单击 **OpenSearch 仪表盘**控制台。
    
    ![Kibana 链接](images/opensearch-link.png)
    
2.  如有必要，请单击 _Proceed to ... default XX.XX.XX.nip.io(unsafe)_ 。首次显示欢迎页面时，_OpenSearch 仪表盘_。它提供了用于尝试 OpenSearch 的内置示例数据，但您可以选择**自行浏览**选项，因为 Verrazzano 已完成必要的配置并且应用程序数据已可用。
    
    ![Kibana 欢迎页面](images/opensearch-proceed.png)
    
3.  在 OpenSearch 主页上，单击**主页** -> **搜索**。
    
    ![Kibana 面板单击](images/discover-1.png)
    
4.  选择名称空间 _verrazzano-application_ \*，如下所示。然后在搜索框中键入 _hello_ 以查看 hello helidon 应用程序的日志。
    
    ![日志结果](images/log-result.png)
    

## 任务 4：浏览 Prometheus 控制台

1.  返回 Verrazzano 主页，然后单击 **Prometheus** 控制台。
    
    ![Prometheus 链接](images/prometheus-link.png)
    
2.  如果出现提示，请单击 **Proceed to ... default XX.XX.XX.nip.io(unsafe)** 。
    
3.  在“Prometheus”仪表盘页面上，在搜索字段中键入 _vendor_ ，然后单击您的定制度量 _`vendor_requests_count_total`_ 。
    
    ![执行 Prometheus](images/prometheus-query.png)
    
4.  单击**执行**并检查下面的结果。您应看到度量的当前值。您还可以切换到 _Graph_ 视图而非 _Console_ 模式。
    
    ![促销值](images/execute-query.png)
    
    > 您还可以向仪表盘添加其他度量。搜索列表中的可用默认度量。
    

## 任务 5：浏览 Kiali 控制台

1.  返回 Verrazzano 主页并单击 **Kiali** 控制台。
    
    ![Rancher 链接](images/kiali-link.png)
    
2.  如果出现提示，请单击 **Proceed to ... default XX.XX.XX.nip.io(unsafe)** 。
    
3.  在左侧，单击“Graph（图形）”。
    
    ![Kiali 仪表盘](images/kiali-dashboard.png " ")
    
4.  在 "Namespace"（名称空间）下拉列表中，选中 _verrazzano-system_ 的框，使光标移到下拉列表中。 ![Bobs Namespace](images/verrazzano-namespace.png " ")
    
5.  您可以查看 _verrazzano-system_ 名称空间的图形视图。单击_图例图标_可查看图例视图，如屏幕截图中所示。
    
    ![图形视图](images/graphical-view.png " ")
    
6.  您可以在此处查看每种形状表示的内容，例如圆形表示_工作量_。
    
    ![图例视图](images/legend-view.png " ")
    
7.  在左侧，单击_应用程序_以查看部署的应用程序。
    
    ![应用程序](images/applications.png " ")
    

## 任务 6：浏览 Keycloak 控制台

1.  返回 Verrazzano 主页并单击 **Keycloak** 控制台。
    
    ![Keycloak 链接](images/keycloak-link.png)
    
2.  如果出现提示，请单击 **Proceed to ... default XX.XX.XX.nip.io(unsafe)** 。
    
3.  在“欢迎使用 Keycloak”页面上，单击_管理控制台_。 ![凯旋洛克](images/keycloak-home.png)
    
4.  现在我们需要 Keycloak 控制台的用户名和密码。_用户名_为 _keycloakadmin_ ，要查找密码，请返回到 _Cloud Shell_ 并粘贴以下命令以查找 _Keycloak Console_ 的密码。
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  复制密码并返回到打开 _Keycloak Console_ 的浏览器。将口令粘贴到_口令_字段中，然后输入 _keycloakadmin_ 作为_用户名_，然后单击**登录**。
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  您可以在此处查看 Verrazzano 完成的默认配置。 ![Keycloak Home](images/keycloak-realms.png)
    

恭喜您已完成 Verrazzano 实验室上的 Helidon 应用程序部署。

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 3 月