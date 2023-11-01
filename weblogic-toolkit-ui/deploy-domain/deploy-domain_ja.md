# WebLogicドメインをOracle Kubernetesクラスタ(OKE)にデプロイ

## 概要

この演習では、WebLogicドメインをkubernetesクラスタにデプロイします。プライマリ・イメージ・セクションで、oracleアカウント資格証明を指定します。補助イメージ・セクションで、oracleクラウド・アカウント資格証明を指定します。ここでは、クラスタのレプリカも指定します。

見積時間: 10分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[OKEクラスタへのWebLogicドメインのデプロイ](videohub:1_wz94de1l)

### 目的

この演習では、次のことを行います。

*   WebLogicドメインをKubernetesクラスタにデプロイします。

## タスク1: WebLogicドメインのOracle Kubernetesクラスタへのデプロイ

このタスクでは、WebLogicドメインのKubernetesカスタム・リソースをKubernetesクラスタにデプロイします。

1.  下にスクロールして、「プライマリ・イメージ」セクションに次を入力し、_「イメージ・プル・シークレット名」_に_domain-secret_と入力し、_「イメージ・レジストリのプル・ユーザー名」_および_「イメージ・レジストリのプル・パスワード」_にOracleアカウントのユーザー名とパスワードを使用します。_「イメージ・レジストリのプルEメール・アドレス」_にOracle EメールIDを入力します。これらは、Oracle Container Registryの_weblogic_イメージのライセンスの受入れに使用した資格証明と同じです。 ![プライマリ・イメージの詳細](images/primary-image-details.png)
    
    > **参考情報:**  
    > Oracle Container Registryからイメージをプルするため、WebLogicサーバー・イメージのライセンス契約に同意するために使用した資格証明を指定します。
    
2.  「補助イメージ」セクションで下にスクロールし、**「補助イメージのプル資格証明の指定」**のボックスの選択を解除します。
    
3.  _「クラスタ」_セクションで、次に示すように_「編集」_アイコンをクリックします。 ![クラスタのサイズ変更](images/cluster-resize.png)
    
4.  _「2」_に_「レプリカ」_と入力し、_「OK」_をクリックします。レプリカのサイズは、WebLogicドメインをKubernetesクラスタに正常にデプロイした後、_「実行中」_状態の管理対象サーバーの数を決定します。 ![クラスタ・レプリカ](images/cluster-replicas.png)
    
5.  「データソース」セクションで、2つのデータソースの_パスワード_をダブルクリックして編集します。両方のデータソースで _tiger_をパスワードとして指定できます。完了したら、_「ドメインのデプロイ」_をクリックします。 ![Datasoureパスワード](images/datasource-password.png)
    
    > これにより、WebLogicドメイン・テスト・ドメインがKubernetesネームスペース_test-domain-ns_にデプロイされます。
    
6.  _WebLogic「Kubernetesへのドメイン・デプロイメントの完了」_ウィンドウが表示されたら、_「OK」_をクリックします。 ![デプロイメントの完了](images/deployment-complete.png)
    
7.  端末に戻り、_「アクティビティ」_をクリックして_「ターミナル」_ウィンドウを選択します。次のコマンドをコピーして、ターミナルを貼り付けます。同様の出力が表示されます。この出力では、イントロスペクタのポッドが最初に実行され、次に管理対象サーバー用のポッドが_「実行中」_状態になります。
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![ポッド・ステータス](images/pod-status.png)
    
8.  _WebLogic Kubernetes Toolkit UI_を使用してドメイン・ステータスを取得することもできます。_WebLogic Kubernetes Toolkit UI_に戻り、_「ドメイン・ステータスの取得」_をクリックします。 ![ドメイン・ステータス](images/domain-status.png)
    
9.  「ドメイン・ステータス」ウィンドウで、下にスクロールしてすべてのサーバー・ポッドのステータスを確認し、_「OK」_をクリックします。 ![サーバー・ステータス](images/server-status.png)
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月