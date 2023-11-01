# WebLogicクラスタのスケーリング(オプション)

## 概要

この演習では、WebLogicクラスタをスケーリングします。ここでは、値を_3_に変更し、ドメインを再デプロイします。

見積時間: 5分

### 目的

この演習では、次のことを行います。

*   WebLogicクラスタをスケーリングします。

## タスク1: WebLogic Kubernetes Toolkit UIを使用したWebLogicクラスタのスケーリング

このタスクでは、_Replica_値を2から3に変更して、ドメインを再配備するだけです。

1.  WebLogic Kubernetes Toolkit UIに戻り、_WebLogicドメイン_をクリックします。_「クラスタ」_セクションに移動し、_「編集」_アイコンをクリックします。  
    ![クラスタのサイズ変更](images/cluster-resize.png)
    
2.  レプリカを_2_から_3_に変更し、_「OK」_をクリックします。 ![レプリカの変更](images/change-replicas.png)
    
3.  ドメインを再デプロイするには、_「ドメインのデプロイ」_をクリックします。 ![ドメインの再デプロイ](images/redeploy-domain.png)
    
4.  _WebLogic「Kubernetesへのドメイン・デプロイメントの完了」_ウィンドウが表示されたら、_「OK」_をクリックします。 ![デプロイメントの完了](images/deployment-complete.png)
    
5.  _「ターミナル」_ウィンドウに戻り、_「アクティビティ」_をクリックして_「ターミナル」_ウィンドウを選択します。次のコマンドをコピーして、端末に貼り付けます。
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![スケーリングの表示](images/view-scaling.png)
    
    > ドメインの再デプロイメントによってイントロスペクタ・ジョブが開始され、test-domain-managed-server3のポッドの作成プロセスが開始され、しばらくするとこのポッドが_「実行中」_ステータスになります。
    
6.  アプリケーション・ページが開いているブラウザに戻ります。「リフレッシュ」ボタンをクリックすると、3つの管理対象サーバー間のロード・バランシングが表示されます。 ![新規サーバー](images/new-server.png)
    
7.  WebLogicリモート・コンソールに戻り、_「モニタリング・ツリー」_→_「環境」_→_「サーバー」_をクリックします。管理対象server3もここに表示されます。 ![リモートコンソール](images/remote-console.png)
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年10月