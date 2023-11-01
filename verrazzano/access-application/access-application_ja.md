# Bobby's Booksサンプル・アプリケーションへのアクセス Verrazzano、GrafanaおよびKialiコンソールの確認

## 概要

演習3では、BobbyのBookアプリケーションをデプロイしました。この演習では、アプリケーションにアクセスして、動作していることを確認します。その後、VerrazzanoおよびGrafanaコンソールを確認します。

所要時間: 10分

### 目的

この演習では、次のことを行います。

*   Bobby's Bookアプリケーションにアクセスします。
*   Verrazzanoコンソールを確認します。
*   Grafanaコンソールを確認します。

### 前提条件

\*演習1を実行します。これにより、Oracle Cloud InfrastructureにOKEクラスタが作成されます。 \*演習1を実行します。演習1では、Oracle Cloud Infrastructure上のOKEクラスタにアクセスするようにkubectlを構成します。

*   Lab 2を実行します。これにより、VerrazzanoがKubernetesクラスタにインストールされます。
*   Bobby's BookアプリケーションをデプロイするLab 3を実行します。
*   コマンドとURLを貼り付け、環境に応じて変更できるテキスト・エディタが必要です。その後、変更したコマンドをコピーして貼り付け、_クラウド・シェル_で実行できます。
*   Bobby's Booksアプリケーション、Verrazano、Grafana、Kiali ConsoleのURLを開くには、Firefoxを使用することをお薦めします。ただし、Chromeを使用する場合は、ドキュメント化されていない「thisisunsafe」回避策を使用して、chromeが証明書を受け入れるようにする必要があります。

## タスク1: Bobby's Bookアプリケーションへのアクセス

1.  Bobby's Bookアプリケーションにアクセスできる`EXTERNAL_IP`アドレスが必要です。istio-ingressgatewayサービスの`EXTERNAL_IP`アドレスを取得するには、次のコマンドをコピーして_クラウド・シェル_に貼り付けます。
    
        <copy> kubectl get service \
        -n "istio-system" "istio-ingressgateway" \
        -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo</copy>
        
    
    結果は次のようになります。
    
           $ kubectl get service \
           > -n "istio-system" "istio-ingressgateway" \
           > -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo
           XX.XX.XX.XX
        
2.  Robertのブック・ストア・ホームページを開くには、次のURLをコピーし、_XX.XX.XX.XX_を最後のステップで取得した_EXTERNAL\_IP_アドレスに置き換えます(次の図を参照)。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/</copy>
        
    
    ![Roberts Booksページ](images/robertbooks.png " ")
    
3.  次に示すように、_「詳細」_をクリックします。
    
    ![Robert Advanced](images/robertadvanced.png " ")
    
4.  _「Bobs-books.bobs-booksに進む」を選択します。EXTERNAL\_IP .nip.io(安全でない)_: アプリケーションにアクセスします。続行するためにこのオプションが表示されない場合は、このクロムブラウザウィンドウ内の任意の場所にスペースを入れずに _thisisunsafe_と入力します。クロムブラウザウィンドウに入力すると表示されなくなりますが、_thisisunsafe_と入力するとすぐにアプリケーションページが表示されます。詳細は、[こちら](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates)をご覧ください。
    
    ![Robert Unsafe](images/robertunsafe.png " ") ![Robert Page](images/robertpage.png " ")
    
5.  Bobbyのブック・ストア・ホームページを開くには、次の図に示すように、新しいタブを開き、次のURLをコピーして、_XX.XX.XX.XX_を`EXTERNAL_IP`アドレスに置き換えます。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![Bobbys URL](images/bobbysurl.png " ")
    
    ![ボビー書店](images/bobbysbookstore.png " ")
    
    > このページはLab 8で使用するため、開いたままにしておきます。
    
6.  BobbyのBook Order Manager UIを開くには、新しいタブを開き、次のURLをコピーし、次の図に示すように_XX.XX.XX.XX_を_EXTERNAL\_IP_アドレスに置き換えます。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobs-bookstore-order-manager/orders</copy>
        
    
    ![オーダー・マネージャ](images/ordermanager.png " ")
    
7.  _Bobby's Books_ページに戻り、本を購入します。次の図に示すように、_「ブック」_をクリックします。
    
    ![チェックアウト](images/checkoutorder.png " ")
    
8.  次の図に示すように、_「Twilight」_ブックのイメージを選択します。
    
    ![購買台帳](images/purchasebook.png " ")
    
9.  まず、次の図に示すように、_「カートに追加」_、_「チェックアウト」_の順にクリックします。
    
    ![「addcart」をクリックします。](images/clickaddcart.png " ")
    
10.  ブックを購入するための詳細を入力します。_「自分の都道府県」_に、2桁の状態コードを入力し、_「オーダーの発行」_をクリックします。
    

![オーダーの送信](images/submitorder.png " ") 11._「オーダー・マネージャ」_ページに戻り、_「リフレッシュ」_ボタンを選択して、オーダーがオーダー・マネージャに正常に記録されているかどうかを確認します。

![オーダーの検証](images/verify-order.png " ")

## タスク2: Rancherコンソールの確認

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
    
10.  ランサー・コンソールのホーム・ページから、Verrazzanoリンクを表示できます。_「ハンバーガー・メニュー」_→_「Verrazzano」_をクリックします。
    
    ![ランチャー ホーム](images/rancher-home.png)
    
11.  _「アプリケーション」_をクリックします。この項では、ネームスペースを持つすべてのアプリケーションを示し、Verrazzanoによって管理されます。_bobs-books_名前空間内の _bobs-books_アプリケーションをクリックします。 ![Bobsアプリケーション](images/bobs-application.png)
    
12.  アプリケーションに関連付けられたポッドを表示できます。ポッド名には、その特定のレプリカを識別するための自動生成された一意の文字列が含まれます。_bobbys-helidon-stok-application_ポッドのログを表示するには、_「3つのドット」_→_「ログの表示」_をクリックします。 ![Bobbysログ](images/bobs-logs.png)
    
13.  アプリケーションのアプリケーション・ログを表示できます。アプリケーション・ログが表示されない場合は、**「設定」**(歯車アイコン付きの青色のボタン)をクリックし、時間フィルタを変更してコンテナ開始からのすべてのログ・エントリを表示します。アプリケーションに関連付けられたコンポーネントを表示するには、_「コンポーネント」_をクリックします。 ![Bobbysログ](images/bobs-log.png)
    
14.  ここで、_bobs-books_アプリケーションのすべてのコンポーネントを表示できます。関連リソースを表示するには、_「関連リソース」_をクリックします。![Bobsリソース](images/bobs-resource.png) ![Bobs Resources](images/bobs-resources.png)
    
15.  _「ハンバーガー・メニュー」_→_「ローカル」_をクリックして、_「クラスタ・エクスプローラ」_を開きます。_クラスタ・エクスプローラ_では、Rancher UIからKubernetesクラスタ内のすべてのカスタム・リソースおよびCRDを表示および操作できます。 ![Verrazzanoクラスタ](images/verrazzano-cluster.png)
    
16.  ダッシュボードには、クラスタおよびデプロイ済アプリケーションの概要が表示されます。リソースの数は_「ユーザー・ネームスペース」_に属し、システムを含むほぼすべてのリソースです。ダッシュボードの上部にあるネームスペースでフィルタできますが、ここでは必要ありません。左側のメニューの「**ノード**」項目をクリックして、ノードの現在の負荷の概要を取得します。
    
    ![クラスタ・エクスプローラ](images/cluster-dashboard.png)
    
17.  デプロイメント全体がOKEクラスタに影響を与えません。左側のメニューの**「デプロイメント」**項目をクリックして、デプロイされたアプリケーションを確認します。
    
    ![clustrノード](images/cluster-nodes.png)
    
18.  複数のデプロイメントを表示できます。
    
    ![デプロイメント](images/cluster-deployments.png)
    

## タスク3: Grafanaコンソールの確認

1.  _「ハンバーガー・メニュー」_→_「ホーム」_をクリックして、Rancherホームページを開きます。 ![ホーム](images/ranchar-menu.png)
    
2.  ホーム・ページには、_Grafanaコンソール_を開くためのリンクが表示されます。次に示すように、**Grafanaコンソール**のリンクをクリックします:
    
    ![Grafanaホーム](images/grafana-link.png)
    
3.  _「詳細」_をクリックします。
    
    ![Grafana拡張](images/grafanaadvanced.png " ")
    
4.  _「grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)」_を選択します。続行するためにこのオプションが表示されない場合は、スペースなしで _thisisunsafe_と入力します。
    
    ![Grafanaの続行](images/grafanaproceed.png " ")
    
5.  Grafanaのホームページが開きます。左上にある_「ホーム」_を選択します。
    
    ![Grafanaホームページ](images/grafana-homepage.png " ")
    
6.  「_WebLogic_」と入力すると、「_General_」の下に「_WebLogic Server Dashboard_」が表示されます。_WebLogic「サーバー・ダッシュボード」_を選択します。
    
    !\[検索WebLogic\](images/search-weblogic.png" ")
    
    ここでは、_「ドメイン」_と「実行中のサーバー」、「デプロイされたアプリケーション」、「サーバー名とそのステータス」、「ヒープ使用量」、「実行時間」、「JVMヒープ」の2つのドメインを確認できます。アプリケーションにJDBCやJMSなどのリソースがある場合は、ここでその詳細を取得することもできます。
    
    ![WebLogicダッシュボード](images/weblogic-dashboard.png " ")
    
7.  次に、WebLogicサーバー・ダッシュボードを選択し、_Helidon_と入力すると、_Helidonモニタリング・ダッシュボード_が表示されます。_「Helidonモニタリング・ダッシュボード」_を選択します。
    
    ![Helidon](images/search-helidon.png " ")
    
    ここでは、アプリケーションの_「ステータス」_とその_「稼働時間」_、ガベージ・コレクタ、スイープ合計のマークとその時間、スレッド数などの様々な詳細を確認できます。
    
    ![Helidonダッシュボード](images/helidon-dashboard.png " ")
    
8.  次に、「Helidonモニタリング・ダッシュボード」を選択し、_「Coherence」_と入力すると、_「Coherenceダッシュボード・メイン」_が表示されます。_「Coherenceダッシュボード・メイン」_を選択します。
    
    ![コヒーレンス](images/search-coherence.png " ")
    
9.  ここでは、_Coherenceクラスタ_の詳細を確認できます。Bobby's Booksアプリケーションには、Bobby's Books用とRobert's Books用の2つのCoherenceクラスタがあります。両方のクラスタを表示するには、_「クラスタ名」_のドロップダウン・メニューを選択する必要があります。
    
    ![Bobby Cluster](images/bobby-cluster.png " ")
    
    ![Robert Cluster](images/robert-cluster.png " ")
    

## タスク4: Kialiコンソールの確認

1.  Verrazzanoのホームページに戻り、**「Kiali」**コンソールをクリックします。
    
    ![ランサーリンク](images/kiali-link.png)
    
2.  左側の「グラフ」をクリックします。
    
    ![Kialiダッシュボード](images/kiali-dashboard.png " ")
    
3.  「ネームスペース」ドロップダウンで、_bobs-books_のボックスを選択し、カーサーをドロップダウンの外側に移動します。 ![Bobsネームスペース](images/bobs-namespace.png " ")
    
4.  _bobs-books_アプリケーションのグラフィカル・ビューを表示できます。_「凡例」_をクリックして、「凡例」ビューを表示します。
    
    ![グラフィカル表示](images/graphical-view.png " ")
    
5.  ここでは、円が_ワークロード_を表すように、各シェイプが表すものを表示できます。
    
    ![凡例ビュー](images/legend-view.png " ")
    
6.  左側で、_「アプリケーション」_をクリックして、デプロイされたすべてのアプリケーションを表示します。
    
    ![アプリケーション](images/kiali-applications.png " ")
    

## タスク5: OpenSearchダッシュボードの確認

1.  Verrazzanoのホームページに戻り、**OpenSearch Dashboards**コンソールをクリックします。
    
    ![Kibanaリンク](images/opensearch-link.png)
    
2.  必要に応じて、_「続行...デフォルトXX.XX.XX.XX.nip.io(安全でない)」_をクリックします。
    
3.  OpenSearchホームページで、**「ホーム」**→**「Discover」**をクリックします。
    
    ![Kibanaダッシュボードのクリック](images/discover-1.png)
    
4.  次に示すように_`verrazzano-applications*`_ネームスペースを選択し、_ブック_を検索して**\[Enter\]**を押すか、**「リフレッシュ」**をクリックします。_ブック_を含むログが表示されます。 ![ログ結果](images/search-books.png)
    

## タスク6: Prometheusコンソールの確認

1.  Verrazzanoのホームページに戻り、**「Prometheus」**コンソールをクリックします。
    
    ![プロメテウス・リンク](images/prometheus-link.png)
    
2.  プロンプトが表示されたら、**「Proceed to ... default XX.XX.XX.nip.io(unsafe)」**をクリックします。
    
3.  Prometheusダッシュボード・ページで、検索フィールドに_get_と入力し、カスタム・メトリック_`application_org_books_bobby_BookResource_getBook_total`_をクリックします。
    
    ![Prometheus実行](images/prometheus-query.png)
    
4.  **「実行」**をクリックし、次の結果を確認します。メトリックの現在の値が表示されます。これは、エンドポイントによって完了されたリクエストの数を意味します。_コンソール_・モードのかわりに_「グラフ」_ビューに切り替えることもできます。
    
    ![プロメテウス値](images/execute-query.png)
    
    > ダッシュボードに別のメトリックを追加することもできます。リストで使用可能なデフォルトのメトリックをDiscoverします。
    

## タスク7: Keycloakコンソールの確認

1.  Verrazzanoのホームページに戻り、**Keycloak**コンソールをクリックします。
    
    ![Keycloakリンク](images/keycloak-link.png)
    
2.  プロンプトが表示されたら、**「Proceed to ... default XX.XX.XX.nip.io(unsafe)」**をクリックします。
    
3.  Keycloakへようこそページで_管理コンソール_をクリックします。 ![Keycloakホーム](images/keycloak-home.png)
    
4.  Keycloakコンソールのユーザー名とパスワードが必要です。_ユーザー名_は_keycloakadmin_で、パスワードを確認するには、_クラウド・シェル_に戻り、次のコマンドを貼り付けて_Keycloakコンソール_のパスワードを確認します。
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  パスワードをコピーして、_Keycloakコンソール_が開いているブラウザに戻ります。_「パスワード」_フィールドにパスワードを貼り付け、_keycloakadmin_に_「ユーザー名」_と入力し、**「サインイン」**をクリックします。
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  ここでは、Verrazzanoによって行われたデフォルト構成を表示できます。 ![Keycloakホーム](images/keycloak-realms.png)
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月