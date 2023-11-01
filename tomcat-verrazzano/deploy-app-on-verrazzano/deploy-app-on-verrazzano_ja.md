# TomcatアプリケーションをVerrazzanoにデプロイ

## 概要

このラボでは、Tomcatサンプル・アプリケーションdockerイメージをデプロイするプロセスについて説明します。

見積時間: 10分

### Verrazzanoおよびアプリケーションのデプロイメント

Verrazzanoは、[Open Application Model (OAM)](https://oam.dev/)を使用したアプリケーション定義をサポートしています。Verrazzanoアプリケーションは、コンポーネントおよびアプリケーション構成で構成されます。

Verrazzanoを使用してアプリケーションをデプロイすると、プラットフォームによって接続とネットワーク・ポリシー、サービス・メッシュのイングレスが設定され、モニタリング・スタックが起動されてメトリック、ログおよびトレースが取得されます。Verrazzanoでは、OAMコンポーネントを使用して、関連するアプリケーション構成を定義することでアセンブルおよび構成されるシステムの機能ユニットを定義します。

### Verrazzanoコンポーネント

Verrazzano OAMコンポーネントは、アプリケーションの一般的な構成および環境要件を示す[Kubernetesカスタム・リソース](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)です。

次のコードは、この演習で使用するtomcatサンプル・アプリケーションの単純なtomcatアプリケーション・コンポーネントを示しています。このリソースは、単一のエンドポイントを公開するTomcatアプリケーションを含む単一のDockerイメージによって実装されるコンポーネントを記述します。

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: tomcat-component
    spec:
      workload:
        apiVersion: core.oam.dev/v1alpha2
        kind: ContainerizedWorkload
        metadata:
          name: tomcat-workload
          labels:
            app: tomcat
            version: v2
        spec:
          containers:
          - name: tomcat-container
            image: "ENDPOINT_OF_REGION/TENANCY_NAMESPACE/tomcat-example-your_firstname:v1"
            ports:
              - containerPort: 8080
                name: ecnj
    

コンポーネントの各フィールドの簡単な説明:

*   **apiVersion** - コンポーネント・カスタム・リソース定義のバージョン
*   **kind** - コンポーネントのカスタム・リソース定義のStandard名
*   **metadata.name** - コンポーネントのカスタム・リソースの作成に使用される名前
*   **spec.workload.kind** - ContainerizedWorkloadは、Kubernetesのステートレス・ワークロードを定義します
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - コンテナによって公開されるポート

### Verrazzanoアプリケーション構成

Verrazzanoアプリケーション構成は、環境固有のカスタマイズを提供するKubernetesカスタム・リソースです。次のコードは、この演習で使用するtomcatアプリケーションの例のアプリケーション構成を示しています。このリソースは、Tomcat-nsネームスペースへのアプリケーションのデプロイメントを指定します。

追加のランタイム機能は、ワークロードを増強するトレイトまたはランタイム・オーバーレイを使用して指定します。たとえば、イングレス・トレイトはイングレス・ホストおよびパスを指定し、メトリック・トレイトは、アプリケーション関連メトリックの取得に使用されるPrometheusスクレイパを提供します。

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: tomcat-appconf
      annotations:
        version: v1.0.0
        description: "Verrazzano demo application"
    spec:
      components:
        - componentName: tomcat-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                  scraper: verrazzano-system/vmi-system-prometheus-0
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: tomcat-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
            - trait:
                apiVersion: core.oam.dev/v1alpha2
                kind: ManualScalerTrait
                spec:
                  replicaCount: 2
    

アプリケーション構成の各フィールドの簡単な説明:

*   **apiVersion** - ApplicationConfigurationカスタム・リソース定義のバージョン
*   **kind** - アプリケーション構成のカスタム・リソース定義のStandard名
*   **metadata.name** - このアプリケーション構成リソースの作成に使用される名前
*   **spec.components** - ランタイム構成を指定するために利用されるアプリケーションのコンポーネントへの参照
*   **spec.components\[\].traits** - アプリケーションのコンポーネントに指定されたトレイト

トレイトについて知るために、イングレス・トレイトのフィールドを確認します:

*   **apiVersion** - OAMトレイト・カスタム・リソース定義のバージョン
*   **kind** - IngressTraitは、OAMアプリケーションのイングレス・トレイト・カスタム・リソース定義の名前です
*   **spec.rules.paths** - アプリケーションにアクセスするためのコンテキスト・パス

### 目的

この演習では、次のことを行います。

*   tomcatサンプル・アプリケーションをデプロイします。
*   Tomcatサンプル・アプリケーションのデプロイメントを確認します。

### 事前設定

この演習を実行するには、次のことが必要です。

*   Oracle Cloud Infrastructureで実行されているKubernetes (OKE)クラスタ。
*   VerrazzanoのインストールがKubernetes (OKE)クラスタで開始されました。
*   コンテナ・レジストリで使用可能なコンテナ・パッケージ_`tomcat-example-your_first_name`_アプリケーション。

## タスク1: Tomcatサンプル・アプリケーションのデプロイ

1.  tomcatサンプル・アプリケーションの`tomcat-ns`ネームスペースを作成します。すべてのKubernetesアーティファクトを個別のネームスペースに保持します。
    
        <copy>kubectl create namespace tomcat-ns</copy>
        
    
    > ネームスペースは、クラスタを仮想サブクラスタに編成する方法です。1つのクラスタ内に任意の数のネームスペースを持つことができ、それぞれが他のものから論理的に分離されますが、相互に通信できます。
    
2.  Verrazzanoは、そのネームスペースVerrazzanoアーティファクトに格納することを認識する必要があります。そのため、Verrazzanoによって管理される`tomcat-ns`ネームスペースを識別するラベルを追加する必要があります。ラベルは、意味があり、ユーザーに関連するオブジェクトの識別属性を指定するために使用されます。
    
    ここでは、`tomcat-ns`ネームスペースについて、ラベルをアタッチしています。これは、このネームスペースをVerrazzanoによって管理されるものとしてマークします。_istio-injection=enabled_はIstioの「サイドカー」を有効にするため、Istioプロキシの確立に役立ちます。Istioプロキシを使用すると、Istioゲートウェイなどの他のIstioサービスにアクセスできます。前述の属性を使用して`tomcat-ns`ネームスペースにラベルを追加するには、次のコマンドをコピーしてクラウド・シェルで実行します:
    
        <copy>kubectl label namespace tomcat-ns verrazzano-managed=true istio-injection=enabled</copy>
        
3.  _~/tomcat-examples/_には、Tomcatsampleアプリケーション用の構成ファイルがあります。ラボ2の一部として、appliationコンポーネント用の新しいDockerイメージを構築しました。また、そのDockerイメージをOracle Cloud Container Registryリポジトリにプッシュしました。この演習では、Oracle Cloud Container Registryリポジトリから新しいDockerイメージを取得するように_tomcat-comp.yaml_ファイルを変更します。_tomcat-comp.yaml_ファイルを変更するには、次のコマンドをコピーして_クラウド・シェル_に貼り付けます。
    
        <copy>vi ~/tomcat-examples/tomcat-comp.yaml</copy>
        
4.  ラボ3の一部として、Dockerイメージのフルネームを保存しました。次の行をコピーして、テキスト・エディタに貼り付ける必要があります。次に、`docker image full name`をDockerイメージ名に置き換える必要があります。次に、変更した行をコピーし、_i_を押して`*tomcat-comp.yaml*`ファイルにテキストを挿入します。出力を行番号 **19**で貼り付け(インデントを保持するようにしてください)、_Esc_を押してから、_:wq_と入力してファイルを保存します。
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![行挿入](images/insert-line.png " ")
    
5.  ここで、Tomcatコンテナ化されたアプリケーションを_cluster1_にデプロイします。そのためには、Kubernetesデプロイメント構成が必要です。このデプロイメントは、Tomcatアプリケーションのインスタンスを作成および更新するようにKubernetesに指示します。ここでは、Kubernetesに指示する`tomcat-comp.yaml`ファイルがあります。
    
    tomcatサンプル・アプリケーションをデプロイするには、次に示す2つのコマンドをコピーして貼り付けます。`tomcat-comp.yaml`ファイルには、様々なOAMコンポーネントの定義が含まれています。ここで、OAMコンポーネントは、アプリケーションの一般的な構成および環境要件を記述するKubernetesカスタム・リソースです。
    
        <copy>kubectl apply -f ~/tomcat-examples/tomcat-comp.yaml -n tomcat-ns</copy>
        
    
    `tomcat-app.yaml`ファイルは、環境固有のカスタマイズを提供するVerrazzanoアプリケーション構成ファイルです。
    
        <copy>kubectl apply -f ~/tomcat-examples/tomcat-app.yaml -n tomcat-ns</copy>
        
6.  ポッドが_「実行中」_ステータスになるまで待ちます。この_kubectl_コマンドを使用して、すべてのポッドが_tomcat-ns_ネームスペース内で_「実行中」_状態になるまで待機します。約1~2分かかります。
    
        <copy>kubectl wait --for=condition=Ready pods --all -n tomcat-ns --timeout=600s</copy>
        
    
    ポッドの準備ができたら、同様のレスポンスが表示されます:
    
        $ kubectl wait --for=condition=Ready pods --all -n tomcat-ns --timeout=600s
        pod/tomcat-workload-57d75d66d9-598lq condition met
        pod/tomcat-workload-57d75d66d9-b2m4l condition met
        
    
    ポッドを直接リストして、そのステータスを確認することもできます:
    
        <copy>kubectl  get po -n tomcat-ns</copy>
        
    
        $ kubectl  get po -n tomcat-ns
        NAME                               READY   STATUS    RESTARTS   AGE
        tomcat-workload-57d75d66d9-598lq   2/2     Running   0          57s
        tomcat-workload-57d75d66d9-b2m4l   2/2     Running   0          54s
        $
        

## タスク2: Tomcatアプリケーションの正常なデプロイメントの確認

1.  `/sample-webapp/`エンドポイントを確認します。外部/ロード・バランサのIPおよびアプリケーション構成から構築されたURLを確認するには、次のコマンドを実行します。
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io tomcat-ns-tomcat-appconf-gw -n tomcat-ns -o jsonpath={.spec.servers[0].hosts[0]})/sample-webapp/</copy>
        
    
    これにより、RESTエンドポイントへの適切なURLが出力されます。たとえば:
    
        https://tomcat-appconf.tomcat-ns.xx.xx.xx.xx.nip.io/sample-webapp/
        
2.  ブラウザにURLを貼り付け、_「クリックしてサーブレットを起動します」_をクリックします。 ![アプリケーションURL](images/application-url.png)
    
3.  _「ホスト名」_および_「IPアドレス」_が表示されます。別の_Incognitoウィンドウ_で同じURLを開くと、このリクエストは別のサーバー・インスタンスによって処理されます。![ホスト名](images/host-name.png) ![ロード・バランシングの表示](images/show-loadbalancing.png)
    

> 2つのレプリカ間のロード・バランシングを示しています。

**次の演習に進む**ことができます。

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月