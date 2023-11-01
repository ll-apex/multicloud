# VerrazzanoへのHelidonアプリケーションのデプロイ

## 概要

このラボでは、Helidon quickstart-mpアプリケーションをデプロイするプロセスについて説明します。

見積時間: 10分

### Verrazzanoおよびアプリケーションのデプロイメント

Verrazzanoは、[Open Application Model (OAM)](https://oam.dev/)を使用したアプリケーション定義をサポートしています。Verrazzanoアプリケーションは、コンポーネントおよびアプリケーション構成で構成されます。

Verrazzanoを使用してアプリケーションをデプロイすると、プラットフォームによって接続とネットワーク・ポリシー、サービス・メッシュのイングレスが設定され、モニタリング・スタックが起動されてメトリック、ログおよびトレースが取得されます。Verrazzanoでは、OAMコンポーネントを使用して、関連するアプリケーション構成を定義することでアセンブルおよび構成されるシステムの機能ユニットを定義します。

### Verrazzanoコンポーネント

Verrazzano OAMコンポーネントは、アプリケーションの一般的な構成および環境要件を示す[Kubernetesカスタム・リソース](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)です。

次のコードは、この演習で使用するHelidon _quickstart-mp_アプリケーションの単純なHelidonアプリケーション・コンポーネントを示しています。このリソースは、単一のエンドポイントを公開するHelidonアプリケーションを含む単一のDockerイメージによって実装されるコンポーネントを表します。

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
    

コンポーネントの各フィールドの簡単な説明:

*   **apiVersion** - コンポーネント・カスタム・リソース定義のバージョン
*   **kind** - コンポーネントのカスタム・リソース定義のStandard名
*   **metadata.name** - コンポーネントのカスタム・リソースの作成に使用される名前
*   **metadata.namespace** - このコンポーネントのカスタム・リソースの作成に使用されるネームスペース
*   **spec.workload.kind** - VerrazzanoHelidonWorkloadは、Kubernetesのステートレス・ワークロードを定義します
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name** - Kubernetesのステートレス・ワークロードの作成に使用される名前
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - 実装コンテナ
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - コンテナによって公開されるポート

### Verrazzanoアプリケーション構成

Verrazzanoアプリケーション構成は、環境固有のカスタマイズを提供するKubernetesカスタム・リソースです。次のコードは、この演習で使用するHelidonの_quickstart-mp_例のアプリケーション構成を示しています。このリソースは、hello-helidonネームスペースへのアプリケーションのデプロイメントを指定します。

追加のランタイム機能は、ワークロードを増強するトレイトまたはランタイム・オーバーレイを使用して指定します。たとえば、イングレス・トレイトはイングレス・ホストおよびパスを指定し、メトリック・トレイトは、アプリケーション関連メトリックの取得に使用されるPrometheusスクレイパを提供します。

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
    

アプリケーション構成の各フィールドの簡単な説明:

*   **apiVersion** - ApplicationConfigurationカスタム・リソース定義のバージョン
*   **kind** - アプリケーション構成のカスタム・リソース定義のStandard名
*   **metadata.name** - このアプリケーション構成リソースの作成に使用される名前
*   **metadata.namespace** - このアプリケーション構成カスタム・リソースに使用されるネームスペース
*   **spec.components** - ランタイム構成を指定するために利用されるアプリケーションのコンポーネントへの参照
*   **spec.components\[\].traits** - アプリケーションのコンポーネントに指定されたトレイト

トレイトについて知るために、イングレス・トレイトのフィールドを確認します:

*   **apiVersion** - OAMトレイト・カスタム・リソース定義のバージョン
*   **kind** - IngressTraitは、OAMアプリケーションのイングレス・トレイト・カスタム・リソース定義の名前です
*   **spec.rules.paths** - アプリケーションにアクセスするためのコンテキスト・パス

### 目的

この演習では、次のことを行います。

*   Verrazzano環境の正常なインストールを確認します。
*   Helidon _quickstart-mp_アプリケーションをデプロイします。
*   Helidon _quickstart-mp_アプリケーションのデプロイメントを確認します。

### 事前設定

この演習を実行するには、次のことが必要です。

*   Oracle Cloud Infrastructureで実行されているKubernetes (OKE)クラスタ。
*   VerrazzanoのインストールがKubernetes (OKE)クラスタで開始されました。
*   コンテナ・レジストリで使用可能なコンテナ・パッケージHelidon _quickstart-mp_アプリケーション。

## タスク1: Verrazzanoのインストールの成功の確認

Verrazzanoは複数のオブジェクトを複数のネームスペースにインストールします。Verrazzanoコンポーネントは、ネームスペース_verrazzano-system_にインストールされます。

1.  複数のオブジェクトに関連付けられたすべてのポッドが_「実行中」_ステータスであることを確認してください。次に示すように、_「実行中」_状態のポッドがあります。
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    結果は次のようになります。
    
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
        

## タスク2: Helidon quickstart-mpアプリケーションのデプロイ

1.  クラウド・シェル環境で、Verrazzano OAMコンポーネントのyamlファイルおよびVerrazzanoアプリケーション構成ファイルをダウンロードします:
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-app.yaml >~/hello-helidon-app.yaml
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-comp.yaml >~/hello-helidon-comp.yaml
        cd ~
        </copy>
        
2.  _hello-helidon-comp.yaml_のイメージ名を変更します。`vi`エディタを使用できます。
    
        <copy>vi ~/hello-helidon-comp.yaml</copy>
        
3.  `i`を使用して挿入モードを変更し、23行目のリポジトリ・パスを反映するようにイメージ名を変更します。
    
        image: "END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0"
        
    
    次に例を示します。
    
        image: "ocir.io/tenancynamespace/quickstart-mp-your_first_name:1.0"
        
4.  挿入終了モードの`Esc`を使用し、`:wq`と入力して変更を保存し、エディタを閉じます。
    
5.  Helidon quickstart-mpアプリケーションの`hello-helidon`ネームスペースを作成します。すべてのKubernetesアーティファクトを個別のネームスペースに保持します。
    
        <copy>
        kubectl create namespace hello-helidon
        </copy>
        
    
    > ネームスペースは、クラスタを仮想サブクラスタに編成する方法です。1つのクラスタ内に任意の数のネームスペースを持つことができ、それぞれが他のものから論理的に分離されますが、相互に通信できます。
    
6.  Verrazzanoは、そのネームスペースVerrazzanoアーティファクトに格納することを認識する必要があります。そのため、Verrazzanoによって管理される`hello-helidon`ネームスペースを識別するラベルを追加する必要があります。ラベルは、意味があり、ユーザーに関連するオブジェクトの識別属性を指定するために使用されます。
    
    ここでは、`hello-helidon`ネームスペースについて、ラベルをアタッチしています。これは、このネームスペースをVerrazzanoによって管理されるものとしてマークします。_istio-injection=enabled_はIstioの「サイドカー」を有効にするため、Istioプロキシの確立に役立ちます。Istioプロキシを使用すると、Istioゲートウェイなどの他のIstioサービスにアクセスできます。前述の属性を使用して`hello-helidon`ネームスペースにラベルを追加するには、次のコマンドをコピーしてクラウド・シェルで実行します:
    
        <copy>
        kubectl label namespace hello-helidon verrazzano-managed=true istio-injection=enabled
        </copy>
        
7.  次に、Helidonの_quickstart-mp_コンテナ化されたアプリケーションを_cluster1_にデプロイします。そのためには、Kubernetesデプロイメント構成が必要です。このデプロイメントは、Helidon _quickstart-mp_アプリケーションのインスタンスを作成および更新するようにKubernetesに指示します。ここでは、Kubernetesに指示する`hello-helidon-comp.yaml`ファイルがあります。
    
    Helidon _quickstart-mp_アプリケーションをデプロイするには、次に示す2つのコマンドをコピーして貼り付けます。`hello-helidon-comp.yaml`ファイルには、様々なOAMコンポーネントの定義が含まれています。ここで、OAMコンポーネントは、アプリケーションの一般的な構成および環境要件を記述するKubernetesカスタム・リソースです。
    
        <copy>kubectl apply -f ~/hello-helidon-comp.yaml -n hello-helidon</copy>
        
    
    `hello-helidon-app.yaml`ファイルは、環境固有のカスタマイズを提供するVerrazzanoアプリケーション構成ファイルです。
    
        <copy>kubectl apply -f ~/hello-helidon-app.yaml -n hello-helidon</copy>
        
8.  ポッドが_「実行中」_ステータスになるまで待ちます。この_kubectl_コマンドを使用して、hello-helidonネームスペース内のすべてのポッドが_「実行中」_状態になるまで待機します。約1~2分かかります。
    
        <copy>kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s</copy>
        
    
    ポッドの準備ができたら、同様のレスポンスが表示されます:
    
        $ kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s
        pod/hello-helidon-deployment-58fdd5cd4-94wjf condition met
        
    
    ポッドを直接リストして、そのステータスを確認することもできます:
    
        $ kubectl  get po -n hello-helidon
        NAME                                       READY   STATUS    RESTARTS   AGE
        hello-helidon-deployment-58fdd5cd4-94wjf   2/2     Running   0          34m
        

## タスク3: Helidon quickstart-mpアプリケーションの正常なデプロイメントの確認

1.  `help/allGreetings`エンドポイントを確認します。外部/ロード・バランサのIPおよびアプリケーション構成から構築されたURLを確認するには、次のコマンドを実行します。
    
        <copy>echo https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings</copy>
        
    
    これにより、RESTエンドポイントへの適切なURLが出力されます。たとえば:
    
        https://hello-helidon-appconf.hello-helidon.xx.xx.xx.xx.nip.io/help/allGreetings
        
2.  このリンクを使用して、ブラウザからテストします。ただし、自己署名証明書により、リスクを受け入れ、ブラウザがリクエスト処理を続行できるようにする必要があります。
    
    レスポンスは文字列のみであるため、`curl`を使用する方が簡単です。
    
        <copy>curl -k https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings; echo</copy>
        
    
    開発中に受け取ったものと同じ結果が表示されます。
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
3.  _クラウド・シェル_は開いたままにしておきます。次の演習に使用します。
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月