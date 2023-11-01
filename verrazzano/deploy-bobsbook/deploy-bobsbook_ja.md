# Bobby's Booksサンプル・アプリケーションのデプロイ

## 概要

### Bobby's Booksアプリケーションについて

![Bobby's Booksアプリケーション](images/bobbyoverview.png " ")

[Bobby's Books](https://verrazzano.io/docs/samples/bobs-books/)は、次の3つの主要部分で構成されています:

*   バックエンドの_注文処理_アプリケーション。これは、RESTサービスと非常に単純なJSP UIを備えたJava EEアプリケーションで、MySQLデータベースにデータを格納します。このアプリケーションは、WebLogicサーバーで実行されます。
*   フロントエンドWebストア_「Robert's Books」_。これは一般書籍販売者です。これは、Helidonマイクロサービスとして実装されます。Coherenceからブック・データを取得し、Coherenceキャッシュ・ストアを使用してオーダー・マネージャのデータを保持し、React Web UIを持ちます。
*   フロントエンドのWebストアである_「Bobby's Books」_は、専門の児童書店です。これは、Helidonマイクロサービスとして実装されます。(異なる)Coherenceキャッシュ・ストアからブック・データを取得し、オーダー・マネージャと直接インタフェースし、JSF Web UIをWebLogicサーバー上で実行します。

このアプリケーションの詳細およびソース・コードについては、[Verrazzanoの例](https://github.com/verrazzano/examples)を参照してください。

### Verrazzanoおよびアプリケーションのデプロイメント

Verrazzanoは、[Open Application Model (OAM)](https://oam.dev/)を使用したアプリケーション定義をサポートしています。Verrrazzanoアプリケーションは、コンポーネントとアプリケーション構成で構成されています。

Verrazzanoを使用してアプリケーションをデプロイすると、プラットフォームによって、サービス・メッシュに接続、ネットワーク・ポリシーおよびイングレスが設定され、モニタリング・スタックが起動されてメトリック、ログおよびトレースが取得されます。Verrazzanoでは、OAMコンポーネントを使用して、関連するアプリケーション構成を定義することでアセンブルおよび構成されるシステムの機能ユニットを定義します。

### Verrazzanoコンポーネント

Verrazzano OAMコンポーネントは、アプリケーションの一般的な構成および環境要件を示す[Kubernetesカスタム・リソース](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)です。

次のコードは、この演習で使用するBobbyのBooksサンプル・アプリケーションの1つのコンポーネントを示しています。このリソースは、単一のエンドポイントを公開するHelidonアプリケーションを含む単一のDockerイメージによって実装されるコンポーネントを表します。

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
    

コンポーネントの各フィールドの簡単な説明:

*   **apiVersion** - コンポーネント・カスタム・リソース定義のバージョン
*   **kind** - コンポーネントのカスタム・リソース定義のStandard名
*   **metadata.name** - コンポーネントのカスタム・リソースの作成に使用される名前
*   **metadata.namespace** - このコンポーネントのカスタム・リソースの作成に使用されるネームスペース
*   **spec.workload.kind** - VerrazzanoHelidonWorkloadは、Kubernetesのステートレス・ワークロードを定義します
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name** - Kubernetesのステートレス・ワークロードの作成に使用される名前
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - 実装コンテナ
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - コンテナによって公開されるポート

Bobbysのbooksアプリケーションの完全なコンポーネントの説明は、[bobs-books-comp.yaml](https://github.com/verrazzano/verrazzano/blob/master/examples/bobs-books/bobs-books-comp.yaml)ファイルで確認できます。

### Verrazzanoアプリケーション構成

Verrazzanoアプリケーション構成は、環境固有のカスタマイズを提供するKubernetesカスタム・リソースです。次のコードは、この演習で使用するBobのBooksサンプルのアプリケーション構成を示しています。このリソースは、アプリケーションのbobs-booksネームスペースへのデプロイメントを指定します。

追加のランタイム機能は、ワークロードを増強するトレイトまたはランタイム・オーバーレイを使用して指定します。たとえば、イングレス・トレイトはイングレス・ホストおよびパスを指定し、メトリック・トレイトは、アプリケーション関連メトリックの取得に使用されるPrometheusスクレイパを提供します。

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

このラボでは、BobbyのBooksサンプル・アプリケーションをデプロイするプロセスについて説明します。

所要時間: 10分

### 目的

この演習では、次のことを行います。

*   ライセンスを受け入れて、Oracle Container Registryのリポジトリからイメージをダウンロードします。
*   Bobby's Booksサンプル・アプリケーションをデプロイします。
*   Bobby's Booksサンプル・アプリケーションのデプロイメントが成功したことを確認します。

### 事前設定

演習3を実行するには、次のものが必要です。

*   演習1を実行します。これにより、Oracle Cloud InfrastructureにOKEクラスタが作成されます。

\*演習1を実行します。演習1では、Oracle Cloud Infrastructure上のOKEクラスタにアクセスするようにkubectlを構成します。

*   Lab 2を実行します。これにより、VerrazzanoがKubernetesクラスタにインストールされます。
*   コマンドおよびURLを貼り付け、環境に応じて変更できるテキスト・エディタ。その後、変更したコマンドをコピーして貼り付け、_クラウド・シェル_で実行できます。

## タスク1: ライセンス契約に同意して、Oracle Container Registryのリポジトリからイメージをダウンロードします

_Bobby's Books_サンプル・アプリケーションのデプロイメントでは、イメージの例を使用します。これらのイメージにはOracle製品が含まれているため、bobs-booksファイル`bobs-books-comp.yaml`でこれらのイメージを使用する前に、ライセンス契約に同意する必要があります。

1.  Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/)のリンクをクリックしてサインインします。そのためには、Oracle Accountが必要です。
    
    ![ログイン](images/registrysignin.png " ")
    
2.  「ユーザー名」および「パスワード」フィールドに_Oracle Account資格証明_を入力し、_「サインイン」_をクリックします。後でこれらの資格証明を使用して、Kubernetesにシークレットを作成します。
    
    ![Oracle SSO](images/oraclesso.png " ")
    
3.  ホーム・ページで、_「Verrazzano」_を選択します。
    
    ![レジストリホームページ](images/registryhomepage.png " ")
    
4.  たとえば、example-bobbys-coherence、example-bobbys-front-end、example-bobs-books-order-managerおよびexample-roberts-coherenceリポジトリでは、言語として_「英語」_を選択し、_「続行」_をクリックします。
    
    ![続行](images/continue.png " ")
    
5.  「_Accept_」をクリックしてライセンス契約書に同意します。
    
    ![契約の受入](images/acceptagreement.png " ")
    
6.  次の図に示すように、Verrazzanoに関連するリポジトリのライセンス契約に同意したことを確認します。
    
    ![ライセンス契約の確認](images/verifyaggrement.png " ")
    
7.  Oracle Container Registryのホーム・ページで、_weblogic_を検索します
    
    ![検索 WebLogic](images/searchweblogic.png " ")
    
8.  次に示すように_「weblogic」_をクリックし、Verrazzano imagaesと同じようにライセンスを受け入れます。![「weblogic」をクリックします。](images/clickweblogic.png) ![weblogicの受け入れ](images/acceptagreement.png " ")
    

## タスク2: Bobby's Booksアプリケーションのデプロイ

構成ファイル`bobs-books-app.yaml`および`bobs-books-comp.yaml`があるソース・コードをダウンロードする必要があります。

1.  Bobby's Bookの例のVerrazzano OAMコンポーネントyamlファイルおよびVerrazzanoアプリケーション構成ファイルをダウンロードします。_「コピー」_をクリックし、次に示すようにクラウド・シェルにコマンドを貼り付けます:
    
        <copy>
        curl -LSs  https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-app.yaml >~/bobs-books-app.yaml
        curl -LSs https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-comp.yaml >~/bobs-books-comp.yaml
        cd ~
        </copy>
        
2.  すべてのKubernetesアーティファクトを個別のネームスペースに保持します。Bob's Booksサンプル・アプリケーションのネームスペースを作成します。ネームスペースは、クラスタを仮想サブクラスタに編成する方法です。クラスタ内に任意の数のネームスペースを持ち、それぞれを論理的に他のネームスペースから分離できますが、相互に通信する機能があります。また、Verrazzanoは、そのネームスペースVerrazzanoアーティファクトに格納することを認識する必要があります。そのため、Verrazzanoによって管理されるbobs-booksネームスペースを識別するラベルを追加する必要があります。ラベルは、意味があり、ユーザーに関連するオブジェクトの識別属性を指定するために使用されます。ここでは、bobs-bookネームスペースについて、ラベルをアタッチしています。このネームスペースは、Verrazzanoによって管理されるものとしてマークされます。_istio-injection=enabled_はIstioの「サイドカー」を有効にするため、Istioプロキシの確立に役立ちます。Istioプロキシを使用すると、Istioゲートウェイなどの他のIstioサービスにアクセスできます。前述の属性を使用してbobs-booksネームスペースにラベルを追加するには、次のコマンドをコピーして_クラウド・シェル_で実行します
    
        <copy>
        kubectl create namespace bobs-books
        kubectl label namespace bobs-books verrazzano-managed=true istio-injection=enabled
        </copy>
        
    
    ![ネームスペースの作成](images/createnamespace.png " ")
    
3.  次のコマンドをコピーして、スクリプトをダウンロードします。このスクリプトは、Oracle Container Registryのユーザーを認証します。認証が成功すると、dockerレジストリ・シークレットが作成されます。Dockerレジストリは、通常のコードの場合はGitHubのように、コンテナ(Kubernetesがプルできる)のイメージを格納およびバージョニングする方法です。ここでは、docker-registryシークレットを作成して、Oracle Container RegistryからBobbyのBooksサンプル・イメージをプルできるようにします。次のコマンドで_「コピー」_をクリックし、任意のテキスト・エディタに貼り付けて、ユーザー名とパスワードをタスク1で使用した電子メールIDとパスワードに置き換え、Oracle Container Registryからイメージをダウンロードするためのライセンス契約に同意します。次に、クラウド・シェルで、次のように変更したコマンドを貼り付けます:
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/deploy-bobsbook/create_secret.sh >~/create_secret.sh
        chmod 777 create_secret.sh
        ./create_secret.sh username 'password'    
        </copy>
        
    
    ![シークレットの作成](images/createsecret.png " ")
    
    > パスワードを一重引用符で囲んで入力してください。
    
4.  資格証明を使用して複数のKubernetesシークレットを作成する必要があります。BobbyのBooksアプリケーションでは、2つのWebLogicドメイン_bobby-front-end_と_bobs-bookstore_があります。WebLogicドメインの資格証明は、WebLogicドメイン・リソースの_webLogicCredentialsSecret_を使用してシークレットの名前が指定されているKubernetesシークレットに保持されます。また、ドメイン資格証明シークレットは、ドメインが実行されるネームスペースに作成する必要があります。WebLogicサーバー・ドメインで使用される_bobbys-front-end-weblogic-credentials_および_bobs-bookstore-weblogic-credentials_というシークレットを作成する必要があります。ユーザー名の値は`weblogic`で、パスワードはbobs-booksネームスペースでランダムに生成されます。Bobby's Booksアプリケーションは _mysql_データベースを使用します。そのため、ユーザー名値`weblogic`、ランダムに生成されるパスワード、およびbobs-booksネームスペースのJDBC URL _JDBC:mysql://mysql.bobs-books.svc.cluster.local:3306/books_を使用して、_mysql-credentials_という新しいシークレットを作成します。この値は、WebLogic DataSourceオブジェクトのJDBC接続文字列で使用されます。コマンドのブロックをコピーして_クラウド・シェル_に貼り付けてください。
    
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
        
    
    ![リソースの作成](images/createresource.png " ")
    
5.  3つのノードを持つKuberneterクラスタ_cluster1__verrazzano_があります。次に、BobbyのBooksコンテナ化アプリケーションを_cluster1__verrazzano_にデプロイします。そのためには、Kubernetesデプロイメント構成が必要です。このデプロイメントは、BobbyのBooksアプリケーションのインスタンスを作成および更新するようにKubernetesに指示します。ここでは、BobbyのBooksアプリケーションをデプロイするようにKubernetesに指示する`bobs-books-comp.yaml`ファイルがあります。次の2つのコマンドをコピーして貼り付けます。`bobs-books-comp.yaml`ファイルには、様々なOAMコンポーネントの定義が含まれています。ここで、OAMコンポーネントは、アプリケーションの一般的な構成および環境要件を記述するKubernetesカスタム・リソースです。`bobs-books-comp.yaml`ファイルについてさらに学習するには、この演習3の「概要」セクションのVerrazzanoコンポーネントを確認してください。
    
        <copy>kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books</copy>
        
    
    結果は次のようになります。
    
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
        
6.  `bobs-books-app.yaml`ファイルは、環境固有のカスタマイズを提供するVerrazzanoアプリケーション構成ファイルです。`bobs-books-app.yaml`ファイルについてさらに学習するには、この演習3の「概要」セクションのVerrazzanoアプリケーション構成を確認してください。
    
        <copy>kubectl apply -f ~/bobs-books-app.yaml -n bobs-books</copy>
        
    
    結果は次のようになります。
    
        $ kubectl apply -f ~/bobs-books-app.yaml -n bobs-books
        applicationconfiguration.core.oam.dev/bobs-books created
        $
        
7.  Bobby's Booksサンプル・アプリケーションのすべてのポッドが_「実行中」_状態になるまで待ちます。このコマンドは、成功する前に複数回繰り返す必要がある場合があります。WebLogicサーバーおよびCoherenceポッドの作成および準備には時間がかかる場合があります。この_kubectl_コマンドは、bobs-booksネームスペース内のすべてのポッドが_「実行中」_状態になるまで待機します。所要時間は4~5分です。5分以上待機している場合は、コマンドを再実行できます。
    
        <copy>kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s</copy>
        
    
    結果は次のようになります。
    
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
        

## タスク3: Bobby's Bookアプリケーションの正常なデプロイメントの確認

アプリケーション構成、ドメイン、Coherenceリソースおよびイングレス・トレイトがすべて存在することを確認します。

1.  _Bobby's Books_アプリケーションがbobs-booksネームスペースに正常にデプロイされていることを確認します。
    
        <copy>kubectl get ApplicationConfiguration -n bobs-books</copy>
        
    
    結果は次のようになります。
    
        $ kubectl get ApplicationConfiguration -n bobs-books
          NAME         AGE
          bobs-books   72m
        $
        
2.  両方のWebLogicドメインがbobs-booksネームスペース内に正常に作成されていることを確認します。
    
        <copy>kubectl get Domain -n bobs-books</copy>
        
    
    結果は次のようになります。
    
        $ kubectl get Domain -n bobs-books
          NAME               AGE
          bobbys-front-end   73m
          bobs-orders-wls    73m
        $
        
3.  両方のCoherenceクラスタがbobs-booksネームスペース内に正常に作成されていることを確認します。
    
        <copy>kubectl get Coherence -n bobs-books</copy>
        
    
    結果は次のようになります。
    
        $ kubectl get Coherence -n bobs-books
        NAME              CLUSTER           ROLE           REPLICAS  READY PHASE
        bobbys-coherence  bobbys-coherence  bobbys-coherence  1       1    Ready
        roberts-coherence roberts-coherence roberts-coherence 2       2    Ready
        $
        
4.  BobbyのBookアプリケーションのIngressTraitを取得するには、_クラウド・シェル_で次のコマンドを実行します。
    
        <copy>kubectl get IngressTrait -n bobs-books</copy>
        
    
    結果は次のようになります。
    
        $ kubectl get IngressTrait -n bobs-books
          NAME                               AGE
          bobby-wls-trait-79b67d9d88         76m
          bobs-orders-wls-trait-57b4d8cb4b   76m
          robert-helidon-trait-54d7bcd54b    76m
        $
        
5.  サービス・ポッドが正常に作成され、_「実行中」_状態に遷移していることを確認します。これには数分かかる場合があり、一部のサービスの終了および再起動が表示される場合があります。最後に、bobs-booksネームスペースに関連付けられているすべてのポッドが_「実行中」_ステータスにあることを確認します。_bobbys-helidon-stock-application_のポッド詳細をコピーしてください。
    
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
        
    
    > **bobbys-helidon-stock-application**のポッド名を書き留めます。このコンポーネントを再デプロイすると、このポッドが_「終了中」_ステータスになり、新しいポッドが開始され、演習7で_「実行中」_状態になります。
    
    _クラウド・シェル_は開いたままにしておきます。次のラボにも使用します。
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月