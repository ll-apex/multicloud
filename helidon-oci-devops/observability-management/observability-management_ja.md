# ログおよびメトリック・エクスプローラの確認

## 概要

この演習では、Helidonアプリケーションのデプロイメントが成功したことを確認します。また、ログおよびメトリック・エクスプローラ・サービスについても確認します。

予想時間: 5分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[ログとメトリック・エクスプローラの詳細](videohub:1_7a0qaaif)

### 目的

この演習では、次のことを行います。

*   Helidonアプリケーションの正常なデプロイメントを確認します。
*   OCIメトリック・エクスプローラの詳細
*   OCIロギング・サービスの確認
*   Helidonアプリケーションの存続性と準備状況を検証します

### 事前設定

*   Oracle Free Tier(トライアル)、有料またはLiveLabsクラウド・アカウント
*   OCIコンソールに関する知識

## タスク1: Helidonアプリケーションへのアクセス

このタスクでは、curlを使用して、GETおよびPUT HTTPリクエストを実行するアプリケーションにアクセスします。

1.  **コード・エディタ**に戻り、ターミナルで次のコマンドをコピーして貼り付け、デプロイメント・ノード**`PUBLIC_IP`**を環境変数として設定します。
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  次のコマンドをコピーして貼り付け、**GETリクエスト**を配置します。次に示すような出力が表示されます。
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  こんにちは、**Joe**です。次に示すような出力が表示されます。
    
        <copy>curl http://$PUBLIC_IP:8080/greet/Joe</copy>
        {"message":"Hello Joe!","date":[2023,5,23]}
        
4.  Helloを別のグリーティングワード(**Hola**)に置き換えてください。次に示すような出力が表示されます。
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,23]}
        

## タスク2: OCIメトリック・エクスプローラの確認

このタスクでは、**Helidonメトリック**が**OCIモニタリング・サービス**にプッシュされていることを確認します。

1.  クラウド・コンソールで、次に示すように、_「ハンバーガー・メニュー」_→_「監視および管理」_→_「メトリック・エクスプローラ」_をクリックします。 ![メトリック・エクスプローラ](images/metrics-explorer.png)
    
2.  下にスクロールして**「問合せ」**セクションに移動し、正しいコンパートメントを選択してから、メトリック・ネームスペースとして**`helidon_metrics`**を選択し、メトリック名として**`request.count_counter`**を選択して問合せを作成します。次に、次に示すように_「チャートの更新」_をクリックします。 ![問合せの作成](images/create-query.png)
    
3.  上にスクロールすると、次のような出力が表示されます。これにより、リクエスト・カウンタのデータがグラフ形式で提供されます。 ![問合せエディタ](images/query-editor.png)
    
4.  データ表にデータを表示するには、次に示すように**「データ表の表示」**のボタンを切り替えます。ボタンを切り替えると、_「データ表の表示」_が**「グラフの表示」**に置き換わります。 ![データ表](images/data-table.png)
    

## タスク3: OCI Loggingサービスの確認

このタスクでは、ログ・グループ**app-log-group-helidon-ocw-hol**のログ**app-log-helidon-ocw-hol**を調査して、ロギング・サービスにメッセージをプッシュする**OCIロギングSDK**統合を検証します。

1.  クラウド・コンソールで、_「ハンバーガー・メニュー」_→_「監視および管理」_→_「ログ」_をクリックします。 ![ログ・メニュー](images/logs-menu.png)
    
2.  正しいコンパートメントを選択し、次に示すように_`app-log-group-helidon-ocw-hol`_を**「ログ・グループ」**として選択し、**「app-log-helidon-ocw-hol」**をクリックします。 ![コンパートメントの選択](images/select-compartment.png)
    
3.  **「時間によるフィルタ」**で、ドロップダウンから**「今日」**を選択すると、アプリケーション・ログが表示されます。 ![ログの表示](images/view-logs.png)
    

## タスク4: Helidonアプリケーションのヘルス・チェックによる稼働状況と準備状況の検証

Helidon **oci-mp**アプリケーションは、**健全性**または**準備**(あるいはその両方)を検証する**ヘルス・チェック**機能を追加します。プロジェクト内の_GreetLivenessCheck_および_GreetReadinessCheck_クラス・ファイルをそれぞれチェックして、それらの実行方法を確認できます。これは、Kubernetesなどのオーケストレータ環境でアプリケーションをマイクロサービスとして実行し、マイクロサービスが正常でない場合に再起動する必要があるかどうかを判断する場合に特に役立ちます。このラボに固有の準備状況チェックは、アプリケーションの起動後、_Helidonアプリケーションが正常に起動したかどうかを判断するために_DevOpsデプロイメント・パイプライン仕様で利用されます。_deployment\_spec.yaml_の**line #79**にあるコードをチェックして、動作を確認します。

1.  **コード・エディタ**に戻り、ターミナルで次のコマンドをコピーして貼り付け、デプロイメント・ノード**`PUBLIC_IP`**を環境変数として設定します。
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  次のコマンドをコピーして貼り付け、**liveness**をチェックします。次に示すような出力が表示されます。
    
        <copy>curl http://$PUBLIC_IP:8080/health/live</copy>
        {"status":"UP","checks":[{"name":"CustomLivenessCheck","status":"UP","data":{"time":1684391639448}}]}
        
3.  次のコマンドをコピーして貼り付け、**準備状況**を確認します。次に示すような出力が表示されます。
    
        <copy>curl http://$PUBLIC_IP:8080/health/ready</copy>
        {"status":"UP","checks":[{"name":"CustomReadinessCheck","status":"UP","data":{"time":1684391438298}}]}
        

**次の演習に進みます。**

## さらに学ぶ

*   [HelidonによるOCIアプリケーション](https://medium.com/helidon/oci-application-with-helidon-caa78cacaee5)
*   [Helidon MPメトリック](https://helidon.io/docs/v3/#/mp/metrics/metrics)
*   [Helidon MPメトリック・ガイド](https://helidon.io/docs/v3/#/mp/guides/metrics)
*   [Helidon MPヘルス](https://helidon.io/docs/v3/#/mp/health)
*   [Helidon MPヘルス・チェック・ガイド](https://helidon.io/docs/v3/#/mp/guides/health)

## 確認

*   **著者** - Keith Lustria
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月