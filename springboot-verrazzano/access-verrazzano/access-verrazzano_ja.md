# Verrazzanoを使用したアプリケーションへのアクセスおよびモニター

## 概要

Springbootアプリケーションをデプロイしました。このラボでは、アプリケーションにアクセスし、Verrazzanoが提供する複数の管理ツールを使用して確認します。

見積時間: 15分

**Grafana**

Grafanaは、データ分析を実行するためのオープンソース・ソリューションであり、膨大な量のデータを把握し、カスタマイズ可能な優れたダッシュボードを利用してアプリケーションを監視するメトリックを取得します。このツールは、技術的には時系列分析と呼ばれる、ある時間のデータを調査、分析および監視するのに役立ちます。ユーザーの動作、アプリケーション動作、本番環境または本番前の環境で発生したエラーの頻度、ポップアップするエラーのタイプ、および相対データを提供することでコンテキスト・シナリオを追跡するのに役立ちます。

[https://grafana.com/grafana/](https://grafana.com/grafana/)

**OpenSearchダッシュボード**

OpenSearchダッシュボードは、OpenSearchクラスタで索引付けされたコンテンツのビジュアライゼーション・ダッシュボードです。Verrazzanoは、OpenSearchで収集されたログ・データを問い合せて視覚化するためのユーザー・インタフェースを提供するOpenSearchダッシュボード・デプロイメントを作成します。

OpenSearchダッシュボード・コンソールにアクセスするには、[「Verrazzanoへのアクセス」](https://verrazzano.io/latest/docs/access/)を参照してください。

OpenSearchダッシュボードを使用してOpenSearch索引のレコードを表示するには、索引パターンを作成して、必要な索引でレコードをフィルタします。

タスク3で、OpenSearchダッシュボードを確認します。

[https://opensearch.org/docs/latest/dashboards/index/](https://opensearch.org/docs/latest/dashboards/index/)

**プロメテウス**

Prometheusは監視およびアラート・ツールキットです。Prometheusは、そのメトリックを時系列データとして収集および格納します。つまり、メトリック情報は、ラベルと呼ばれるオプションのキーと値のペアとともに、記録されたタイムスタンプで格納されます。

[https://prometheus.io/](https://prometheus.io/)

**ランチャー**

Rancherは、Verrazzanoが本番環境の複数のKubernetesクラスタでコンテナを実行できるようにするプラットフォームです。ハイブリッドを実現することで、オンプレミス・クラスタを一元管理し、クラウド・サービスでホストできます。

[https://rancher.com/](https://rancher.com/)

**キアリ**

KialiはIstioのユーザーインターフェースであり、非常に使いやすいです。VerrazzanoコンソールからKialiに直接アクセスできます。そこから、交通の流れやホットスポットやトラブルエリアを見ることができます。その後、ドリルダウンしてそれぞれの詳細を表示できます。Kialiは、Prometheusに格納されているEnvoyのメトリック・データを使用してグラフィカル・モデルを構築します。

### 目的

この演習では、次のことを行います。

*   Rancherコンソールを確認します。
*   Prometheusコンソールを確認します。
*   Grafanaコンソールを確認します。
*   OpenSearchダッシュボードを確認します。
*   Kialiコンソールを確認します。
*   Keycloakコンソールを確認します。

### 事前設定

*   Oracle Cloud Infrastructureで実行されているKubernetes (OKE)クラスタ。
*   VerrazzanoはKubernetes (OKE)クラスタにインストールされています。
*   Springbootサンプル・アプリケーションをデプロイしました。

## タスク1: Rancherコンソールの確認

Verrazzanoは複数のコンソールをインストールします。インストールのエンドポイントは、インストールされたVerrazzanoカスタム・リソースの`Status`フィールドに格納されます。

1.  次のコマンドを使用して、これらのコンソールのエンドポイントを取得できます:
    
        <copy>kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .</copy>
        
    
    結果は次のようになります。
    
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
        
2.  `https://rancher.default.YOUR_UNIQUE_IP.nip.io`を使用して、Rancherコンソールを開きます。
    
3.  Verrazzano _dev_プロファイルは自己署名証明書を使用するため、**「詳細」**をクリックしてリスクを受け入れ、警告をスキップする必要があります。
    
    ![拡張](images/rancher-advanced.png)
    
4.  **「ランサーのデフォルトXX.XX.XX.XX.nip.io(unsafe)に進む」**をクリックします。続行するためにこのオプションが表示されない場合は、このクロムブラウザウィンドウのどこかにスペースを入れずに _thisisunsafe_と入力します。クロムブラウザウィンドウに入力すると表示されなくなりますが、_thisisunsafe_の入力が完了するとすぐに次のページが表示されます。詳細は、[こちら](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)をご覧ください。
    
    ![続行](images/rancher-proceed.png)
    
5.  _「Log in with Keycloak」_をクリックします。 ![ランサーログイン](images/keycloak-login.png)
    
6.  認証のためにKeycloakコンソールURLにリダイレクトされるため、**「詳細」**をクリックします。
    
    ![Keycloak認証](images/keycloak-advanced.png)
    
7.  **「Proceed to Keycloak default XX.XX.XX.XX.nip.io(unsafe)」**をクリックします。続行するためにこのオプションが表示されない場合は、このクロムブラウザウィンドウのどこかにスペースを入れずに _thisisunsafe_と入力します。クロムブラウザウィンドウに入力すると表示されなくなりますが、_thisisunsafe_の入力が完了するとすぐに次のページが表示されます。詳細は、[こちら](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)をご覧ください。
    
    ![続行](images/keycloak-proceed.png)
    
8.  Verrazzanoコンソールのユーザー名とパスワードが必要です。_ユーザー名_は_verrazzano_で、パスワードを確認するには、_クラウド・シェル_に戻り、次のコマンドを貼り付けて_ランサー・コンソール_のパスワードを確認します。
    
        <copy>kubectl get secret --namespace verrazzano-system verrazzano -o jsonpath={.data.password} | base64 --decode; echo</copy>
        
9.  パスワードをコピーして、_ランサー・コンソール_が開いているブラウザに戻ります。_「パスワード」_フィールドにパスワードを貼り付け、_verrazzano_に_「ユーザー名」_と入力し、**「サインイン」**をクリックします。
    
    ![SignIn](images/verrazzano-sign-in.png)
    
10.  ランサー・コンソールのホーム・ページから、Verrazzanoリンクを表示できます。ランサーconsole.Click _「ハンバーガー」メニュー_ -> _Verrazzano_からこれらのコンソールにアクセスできます。
    
    ![ランチャー ホーム](images/rancher-home.png)
    
11.  _「アプリケーション」_をクリックします。この項では、ネームスペースを持つすべてのアプリケーションを示し、Verrazzanoによって管理されます。_springboot_名前空間内の _springboot-appconf_アプリケーションをクリックします。 ![Sprinngbootアプリケーション](images/springboot-application.png)
    
12.  アプリケーションに関連付けられたポッドを表示できます。ポッド名には、その特定のレプリカを識別するための自動生成された一意の文字列が含まれます。ポッドのログを表示するには、_「3つのドット」_→_「ログの表示」_をクリックします。 ![Springbootコンポーネント](images/view-pod-logs.png)
    
13.  アプリケーションのアプリケーション・ログを表示できます。アプリケーション・ログが表示されない場合は、**「設定」**(歯車アイコン付きの青色のボタン)をクリックし、時間フィルタを変更してコンテナ開始からのすべてのログ・エントリを表示します。アプリケーションに関連付けられたコンポーネントを表示するには、_「コンポーネント」_をクリックします。 ![アプリケーション・ログ](images/view-pod-log.png)
    
14.  このアプリケーションには、関連するリソースを表示するcomponent.Toビューが1つのみあります。_「関連リソース」_をクリックします。![Springbootリソース](images/springboot-resource.png) ![Springbootリソース](images/springboot-resources.png)
    
15.  _「ハンバーガー・メニュー」_→_「ローカル」_をクリックして、_「クラスタ・エクスプローラ」_を開きます。_クラスタ・エクスプローラ_では、Rancher UIからKubernetesクラスタ内のすべてのカスタム・リソースおよびCRDを表示および操作できます。 ![Verrazzanoクラスタ](images/verrazzano-cluster.png)
    
16.  ダッシュボードには、クラスタおよびデプロイ済アプリケーションの概要が表示されます。リソースの数は_「ユーザー・ネームスペース」_に属し、システムを含むほぼすべてのリソースです。ダッシュボードの上部にあるネームスペースでフィルタできますが、ここでは必要ありません。左側のメニューの「**ノード**」項目をクリックして、ノードの現在の負荷の概要を取得します。
    
    ![クラスタ・エクスプローラ](images/cluster-dashboard.png)
    
17.  デプロイメント全体がOKEクラスタに影響を与えません。左側のメニューの**「ワークロード」**→**「デプロイメント」**項目をクリックして、デプロイされたアプリケーションを確認します。
    
    ![ノード](images/cluster-nodes.png)
    
18.  複数のデプロイメントを表示できます。
    
    ![デプロイメント](images/deployment.png)
    

## タスク2: Grafanaコンソールの確認

1.  _「ハンブルク・メニュー」_→_「ホーム」_をクリックします。 ![ホーム](images/ranchar-menu.png)
    
2.  ホーム・ページには、_Grafanaコンソール_を開くためのリンクが表示されます。次に示すように、**Grafanaコンソール**のリンクをクリックします:
    
    ![Grafanaホーム](images/grafana-link.png)
    
3.  **「詳細」**をクリックします。
    
    ![拡張](images/grafana-advanced.png)
    
4.  **「Proceed to grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)」**をクリックします。続行するためにこのオプションが表示されない場合は、このクロムブラウザウィンドウのどこかにスペースを入れずに _thisisunsafe_と入力します。クロムブラウザウィンドウに入力すると表示されなくなりますが、_thisisunsafe_の入力が完了するとすぐに次のページが表示されます。詳細は、[こちら](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)をご覧ください。
    
    ![続行](images/grafana-proceed.png)
    
5.  Grafanaホームで、_「ホーム」_をクリックし、次に示すように_「JVM(マイクロメーター)」_を選択します。![Grafanaホーム・リンク](images/grafana-home-link.png) ![JVMメトリック](images/jvm-metrics.png)
    
6.  次に示すように、アプリケーションの稼働時間、使用済ヒープ、JVMメモリー・データなどのメトリックを表示できます。![JVM 1](images/jvm-1.png) ![JVM 2](images/jvm-2.png)
    
7.  単純に、JVMをクリックし、次に示すように_「アプリケーション・ステータス」_ダッシュボードを開きます。 ![アプリケーションのステータス](images/application-status.png)
    
8.  アプリケーション・ステータスは、「CPU使用済」、「使用済メモリー」、「使用済ディスク」および「HTTPリクエスト/秒」などの情報で、次に示すように表示できます。 ![アプリケーション・データ](images/application-data.png)
    

## タスク3: OpenSearchダッシュボードの確認

1.  Verrazzanoのホームページに戻り、**OpenSearch Dashboards**コンソールをクリックします。
    
    ![Kibanaリンク](images/opensearch-link.png)
    
2.  必要に応じて、_「続行...デフォルトXX.XX.XX.XX.nip.io(安全でない)」_をクリックします。初めて_OpenSearchダッシュボード_にようこそページが表示されます。OpenSearchを試す組込みのサンプル・データを提供しますが、Verrazzanoは必要な構成を完了し、アプリケーション・データがすでに使用可能であるため、**「自分で探索」**オプションを選択できます。
    
    ![Kibanaようこそページ](images/opensearch-proceed.png)
    
3.  OpenSearchホームページで、**「ホーム」**→**「Discover」**をクリックします。
    
    ![Kibanaダッシュボードのクリック](images/discover-1.png)
    
4.  クラウド・シェルでエンドポイントに対して次のHTTPリクエストを起動します。リクエストは複数回実行できます。
    
        <copy>curl -k https://$(kubectl get gateways.networking.istio.io springboot-springboot-appconf-gw -n springboot -o jsonpath={.spec.servers[0].hosts[0]})/facts; echo</copy>
        
    
    > 結果に表示される内容は、任意の単語を選択して次のステップで検索できます。ここでは、_bridge_という単語を検索しますが、結果から任意の単語を検索できます。
    
5.  次に示すように_`verrazzano-application*`_ネームスペースを選択し、Springbootアプリケーションのログ(`bridge` )をフィルタ・テキストボックスに検索します。**\[Enter\]**を押すか、**「リフレッシュ」**をクリックします。少なくとも1つの結果を取得する必要があります。![アプリケーションのステータス](images/verrazzano-application.png) ![ログ結果](images/log-result.png)
    

## タスク4: Prometheusコンソールの確認

1.  Verrazzanoのホームページに戻り、**「Prometheus」**コンソールをクリックします。
    
    ![プロメテウス・リンク](images/prometheus-link.png)
    
2.  プロンプトが表示されたら、**「Proceed to ... default XX.XX.XX.nip.io(unsafe)」**をクリックします。
    
3.  Prometheusダッシュボード・ページで、検索フィールドに_http_と入力し、メトリック_`http_server_requests_seconds_count`_をクリックして_「実行」_をクリックします。
    
    ![Prometheus実行](images/prometheus-query.png)
    
4.  **「実行」**をクリックし、次の結果を確認します。メトリックの現在の値が表示されます。これは、HTTPリクエストを処理する合計期間(秒)を意味します。_コンソール_・モードのかわりに_「グラフ」_ビューに切り替えることもできます。
    
    ![プロメテウス値](images/execute-query.png)
    
    > ダッシュボードに別のメトリックを追加することもできます。リストで使用可能なデフォルトのメトリックをDiscoverします。
    

## タスク5: Kialiコンソールの確認

1.  Verrazzanoのホームページに戻り、**「Kiali」**コンソールをクリックします。
    
    ![ランサーリンク](images/kiali-link.png)
    
2.  プロンプトが表示されたら、**「Proceed to ... default XX.XX.XX.nip.io(unsafe)」**をクリックします。
    
3.  左側の「グラフ」をクリックします。
    
    ![Kialiダッシュボード](images/kiali-dashboard.png " ")
    
4.  「ネームスペース」ドロップダウンで、_verrazzano-system_のボックスを選択し、カーサーをドロップダウンの外側に移動します。 ![Bobsネームスペース](images/verrazzano-namespace.png " ")
    
5.  _verrazzano-system_名前空間のグラフィカルビューを表示できます。_「凡例」_をクリックして、「凡例」ビューを表示します。
    
    ![グラフィカル表示](images/graphical-view.png " ")
    
6.  ここでは、円が_ワークロード_を表すように、各シェイプが表すものを表示できます。
    
    ![凡例ビュー](images/legend-view.png " ")
    
7.  左側で、_「アプリケーション」_をクリックしてデプロイ済アプリケーションを表示します。
    
    ![アプリケーション](images/applications.png " ")
    

## タスク6: Keycloakコンソールの確認

1.  Verrazzanoのホームページに戻り、**Keycloak**コンソールをクリックします。
    
    ![Keycloakリンク](images/keycloak-link.png)
    
2.  プロンプトが表示されたら、**「Proceed to ... default XX.XX.XX.nip.io(unsafe)」**をクリックします。
    
3.  Keycloakへようこそページで_管理コンソール_をクリックします。 ![Keycloakホーム](images/keycloak-home.png)
    
4.  Keycloakコンソールのユーザー名とパスワードが必要です。_ユーザー名_は_keycloakadmin_で、パスワードを確認するには、_クラウド・シェル_に戻り、次のコマンドを貼り付けて_Keycloakコンソール_のパスワードを確認します。
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  パスワードをコピーして、_Keycloakコンソール_が開いているブラウザに戻ります。_「パスワード」_フィールドにパスワードを貼り付け、_keycloakadmin_に_「ユーザー名」_と入力し、**「サインイン」**をクリックします。
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  ここでは、Verrazzanoによって行われたデフォルト構成を表示できます。 ![Keycloakホーム](images/keycloak-realms.png)
    

VerrazzanoラボでのSpringbootアプリケーションのデプロイメントが完了しました。

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月