# 访问 Bobby 的 Books 示例应用程序 浏览 Verrazzano、Grafana 和 Kiali 控制台

## 简介

在实验室 3 中，我们部署了 Bobby 的 Book 应用程序。在本练习中，我们将访问该应用程序并验证其是否正常工作。稍后，我们将探讨 Verrazzano 和 Grafana 控制台。

估计时间：10 分钟

### 目标

在本练习中，您将：

*   访问 Bobby 的 Book 应用程序。
*   浏览 Verrazzano 控制台。
*   浏览 Grafana 控制台。

### 先备条件

\* 运行实验室 1，用于在 Oracle Cloud Infrastructure 上创建 OKE 集群。 \* 运行实验室 1，该实验室配置 kubectl 以访问 Oracle Cloud Infrastructure 上的 OKE 集群。

*   运行 Lab 2，在 Kubernetes 集群上安装 Verrazzano。
*   运行实验室 3，该实验室部署 Bobby 的 Book 应用程序。
*   您应该具有文本编辑器，可以在其中粘贴命令和 URL，并根据您的环境对其进行修改。然后，您可以复制并粘贴修改后的命令，以便在 _Cloud Shell_ 中运行它们。
*   我们建议使用 Firefox 打开 Bobby 书籍应用程序、Verrazano、Grafana 和 Kiali 控制台的 URL。但是，如果您想使用 Chrome，则需要使用未记录的“thisisunsafe”解决方法来允许 chrome 接受证书。

## 任务 1：访问 Bobby 的 Book 应用程序

1.  我们需要一个 `EXTERNAL_IP` 地址，通过该地址可以访问 Bobby 的 Book 应用程序。要获取 istio-ingressgateway 服务的 `EXTERNAL_IP` 地址，请复制以下命令并将其粘贴到 _Cloud Shell_ 中。
    
        <copy> kubectl get service \
        -n "istio-system" "istio-ingressgateway" \
        -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo</copy>
        
    
    输出结果应如下所示：
    
           $ kubectl get service \
           > -n "istio-system" "istio-ingressgateway" \
           > -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo
           XX.XX.XX.XX
        
2.  要打开 Robert 的书店主页，请复制以下 URL 并将 _XX.XX.XX.XX_ 替换为您在最后一个步骤中获得的 _EXTERNAL\_IP_ 地址，如下图中所示。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/</copy>
        
    
    ![Roberts 书籍页面](images/robertbooks.png " ")
    
3.  单击_高级_，如下所示：
    
    ![Robert Advanced](images/robertadvanced.png " ")
    
4.  选择 _Proceed to bobs-books.bobs-books（转至 bobs-books.bobs-books）。EXTERNAL\_IP .nip.io(unsafe)_ 可访问应用程序。如果您不获取此选项以继续，只需在此 Chrome 浏览器窗口的任何位置键入不带任何空间的 _thisisunsafe_ 。当您在 chrome 浏览器窗口中输入时，您无法看到它，但是在您输入完 _thisisunsafe_ 后，您可以立即看到应用程序页面。您可以在[此处](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)找到更多详细信息。
    
    ![Robert Unsafe](images/robertunsafe.png " ") ![Robert Page](images/robertpage.png " ")
    
5.  要打开 Bobby 的 Book Store 主页，请打开一个新选项卡并复制以下 URL，并将 _XX.XX.XX.XX_ 替换为您的 `EXTERNAL_IP` 地址，如下图中所示。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![Bobbys URL](images/bobbysurl.png " ")
    
    ![Bobby 书店](images/bobbysbookstore.png " ")
    
    > 将此页面保持打开状态，因为我们将在实验室 8 中使用它。
    
6.  要打开 Bobby 的 Book Order Manager UI，请打开一个新选项卡并复制以下 URL，然后将 _XX.XX.XX.XX_ 替换为您的 _EXTERNAL\_IP_ 地址，如下图中所示。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobs-bookstore-order-manager/orders</copy>
        
    
    ![订单经理](images/ordermanager.png " ")
    
7.  返回 _Bobby's Books_ 页面，让我们购买一本书。单击_工作簿_，如下图中所示。
    
    ![退出订单](images/checkoutorder.png " ")
    
8.  选择_双向_工作簿的图像，如下图所示。
    
    ![采购账簿](images/purchasebook.png " ")
    
9.  首先，单击_添加到购物车_，然后单击_结账_，如下图中所示。
    
    ![单击 addcart](images/clickaddcart.png " ")
    
10.  输入采购账簿的详细信息。对于_省（州）_，输入两位数状态代码，然后单击_提交订单_。
    

![提交订单](images/submitorder.png " ") 11。返回到_订单管理器_页并选择_刷新_按钮，以检查订单管理器中是否已成功记录订单。

![验证订单](images/verify-order.png " ")

## 任务 2：浏览 Rancher 控制台

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
    
10.  从 rancher 控制台的主页，您可以查看 Verrazzano 链接。单击 _Hamburger（汉堡）菜单_ -> _Verrazzano_ 。
    
    ![Rancher 主页](images/rancher-home.png)
    
11.  单击_应用程序_。此部分显示名称空间的所有应用程序，由 Verrazzano 管理。单击 _bobs-books_ 名称空间中的 _bobs-books_ 应用程序。 ![Bobs 应用程序](images/bobs-application.png)
    
12.  您可以查看与应用程序关联的云池。云池名称包含自动生成的唯一字符串，用于标识该特定副本。要查看 _bobbys-helidon-stok-application_ 云池的日志，请单击_三个点_ -> _查看日志_。 ![Bobbys 日志](images/bobs-logs.png)
    
13.  您可以查看应用程序的应用程序日志。如果看不到应用程序日志，请单击**设置**（带有齿轮图标的蓝色按钮），然后更改时间筛选器以显示容器开始时的所有日志条目。要查看与应用程序关联的组件，请单击_组件_。 ![Bobbys 日志](images/bobs-log.png)
    
14.  您可以在此处查看 _bobs-books_ 应用程序的所有组件。要查看相关资源是什么，请单击_相关资源_。![Bobs 资源](images/bobs-resource.png) ![Bobs 资源](images/bobs-resources.png)
    
15.  单击 _Hamburgar 菜单_ -> _local_ 以打开 _Cluster Explorer_ 。_集群浏览器_允许您从 Rancher UI 查看和处理 Kubernetes 集群中的所有定制资源和 CRD。 ![Verrazzano 集群](images/verrazzano-cluster.png)
    
16.  该仪表盘概述了集群和部署的应用程序。资源数属于_用户名空间_，这实际上几乎是包括系统在内的所有资源。您可以按仪表盘顶部的名称空间进行筛选，但现在不需要这样做。单击左侧菜单中的**节点**项可查看节点的当前负载概览。
    
    ![集群浏览器](images/cluster-dashboard.png)
    
17.  整个部署对 OKE 群集没有任何影响。现在单击左侧菜单中的**部署**项以检查已部署的应用程序。
    
    ![客户端节点](images/cluster-nodes.png)
    
18.  您可以查看多个部署。
    
    ![部署](images/cluster-deployments.png)
    

## 任务 3：浏览 Grafana 控制台

1.  单击 _Hamburgar 菜单_ -> _主页_以打开 Rancher 主页。 ![住宅](images/ranchar-menu.png)
    
2.  在主页上，您将看到用于打开 _Grafana 控制台_的链接。单击 **Grafana Console** 的链接，如下所示：
    
    ![Grafana 主目录](images/grafana-link.png)
    
3.  单击_高级_。
    
    ![Grafana 高级版](images/grafanaadvanced.png " ")
    
4.  选择 _grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)_ 。如果您不获取此选项以继续，只需键入不带任何空间的 _thisisunsafe_ 。
    
    ![Grafana 继续](images/grafanaproceed.png " ")
    
5.  此时将打开 Grafana 主页。选择左上方的_主页_。
    
    ![Grafana 主页](images/grafana-homepage.png " ")
    
6.  键入 _WebLogic_ ，您将在 _General_ 下看到 _WebLogic Server Dashboard_ 。选择 _WebLogic Server Dashboard_ 。
    
    ！［搜索 WebLogic］（图像/搜索 - weblogic.png" "）
    
    您可以在此处观察 _Domain_ 和 Running Servers，Deployed Applications，Server Name and their Status，Heap Usage，Running Time，JVM Heap 下的两个域。如果您的应用程序具有 JDBC 和 JMS 等资源，您还可以在此处获取有关它的详细信息。
    
    ![WebLogic 仪表盘](images/weblogic-dashboard.png " ")
    
7.  现在，选择 WebLogic 服务器仪表盘并键入 _Helidon_ ，您将看到 _Helidon 监视仪表盘_。选择 _Helidon 监视仪表盘_。
    
    ![Helidon](images/search-helidon.png " ")
    
    您可以在此处查看各种详细信息，例如应用程序的_状态_及其_正常运行时间_、垃圾收集器和标记清理总计及其时间、线程计数。
    
    ![Helidon 仪表盘](images/helidon-dashboard.png " ")
    
8.  现在，选择“Helidon 监视仪表盘”并键入 _Coherence_ ，您将看到 _Coherence 仪表盘主页_。选择 _Coherence Dashboard Main_ 。
    
    ![Coherence](images/search-coherence.png " ")
    
9.  在此处可以查看 _Coherence 集群_的详细信息。对于 Bobby 的 Books 应用程序，我们有两个 Coherence 集群，一个用于 Bobby 的 Books，另一个用于 Robert 的 Books。您需要为 _Cluster Name_ 选择下拉菜单来查看这两个群集。
    
    ![Bobby Cluster](images/bobby-cluster.png " ")
    
    ![Robert Cluster](images/robert-cluster.png " ")
    

## 任务 4：浏览 Kiali 控制台

1.  返回 Verrazzano 主页并单击 **Kiali** 控制台。
    
    ![Rancher 链接](images/kiali-link.png)
    
2.  在左侧，单击“Graph（图形）”。
    
    ![Kiali 仪表盘](images/kiali-dashboard.png " ")
    
3.  在 "Namespace"（名称空间）下拉列表中，选中 _bobs-books_ 的框，使该游标在下拉列表中移动。 ![Bobs Namespace](images/bobs-namespace.png " ")
    
4.  您可以查看 _bobs-books_ 应用程序的图形视图。单击_图例_可查看“图例”视图。
    
    ![图形视图](images/graphical-view.png " ")
    
5.  您可以在此处查看每种形状表示的内容，例如圆形表示_工作量_。
    
    ![图例视图](images/legend-view.png " ")
    
6.  在左侧，单击_应用程序_可查看所有已部署的应用程序。
    
    ![应用程序](images/kiali-applications.png " ")
    

## 任务 5：浏览 OpenSearch 仪表盘

1.  返回 Verrazzano 主页，然后单击 **OpenSearch 仪表盘**控制台。
    
    ![Kibana 链接](images/opensearch-link.png)
    
2.  如有必要，请单击 _Proceed to ... default XX.XX.XX.nip.io(unsafe)_ 。
    
3.  在 OpenSearch 主页上，单击**主页** -> **搜索**。
    
    ![Kibana 面板单击](images/discover-1.png)
    
4.  按所示选择 _`verrazzano-applications*`_ 名称空间，然后搜索_工作簿_并按 **Enter** 或单击**刷新**。您应获取包含 _books_ 的日志。 ![日志结果](images/search-books.png)
    

## 任务 6：浏览 Prometheus 控制台

1.  返回 Verrazzano 主页，然后单击 **Prometheus** 控制台。
    
    ![Prometheus 链接](images/prometheus-link.png)
    
2.  如果出现提示，请单击 **Proceed to ... default XX.XX.XX.nip.io(unsafe)** 。
    
3.  在“Prometheus”仪表盘页面上，在搜索字段中键入 _get_ ，然后单击您的定制度量 _`application_org_books_bobby_BookResource_getBook_total`_ 。
    
    ![执行 Prometheus](images/prometheus-query.png)
    
4.  单击**执行**并检查下面的结果。您应看到度量的当前值，即端点已完成多少请求。您还可以切换到 _Graph_ 视图而非 _Console_ 模式。
    
    ![促销值](images/execute-query.png)
    
    > 您还可以向仪表盘添加其他度量。搜索列表中的可用默认度量。
    

## 任务 7：浏览 Keycloak 控制台

1.  返回 Verrazzano 主页并单击 **Keycloak** 控制台。
    
    ![Keycloak 链接](images/keycloak-link.png)
    
2.  如果出现提示，请单击 **Proceed to ... default XX.XX.XX.nip.io(unsafe)** 。
    
3.  在“欢迎使用 Keycloak”页面上，单击_管理控制台_。 ![凯旋洛克](images/keycloak-home.png)
    
4.  现在我们需要 Keycloak 控制台的用户名和密码。_用户名_为 _keycloakadmin_ ，要查找密码，请返回到 _Cloud Shell_ 并粘贴以下命令以查找 _Keycloak Console_ 的密码。
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  复制密码并返回到打开 _Keycloak Console_ 的浏览器。将口令粘贴到_口令_字段中，然后输入 _keycloakadmin_ 作为_用户名_，然后单击**登录**。
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  您可以在此处查看 Verrazzano 完成的默认配置。 ![Keycloak Home](images/keycloak-realms.png)
    

## 确认

*   **作者** - Ankit Pandey
*   **贡献者** - Maciej Gruszka、Sid Joshi
*   **上次更新者/日期** - Ankit Pandey，2023 年 8 月