# アプリケーション・アクセスおよびキュレートされたスタックによる変更の確認

## 概要

演習7では、bobbys-helidon-stock-applicationの変更を適用し、そのポッドは_「実行中」_状態です。この演習では、アプリケーション、VerrazzanoコンソールおよびGrafanaコンソールの変更を確認します。

所要時間:05分

### 目的

この演習では、次のことを行います。

*   Bobby's Booksアプリケーションの変更を確認します。
*   Grafanaコンソールで変更内容を確認します。

### 事前設定

*   コマンドとURLを貼り付け、環境に応じて変更できるテキスト・エディタが必要です。その後、変更したコマンドをコピーして貼り付け、_クラウド・シェル_で実行できます。

## タスク1: Bobby's Booksアプリケーションの変更の確認

1.  Bobbyの「Books」タブを開き、「Refresh」を選択します。そのタブを閉じた場合は、テキスト・エディタで次のコマンドをコピーして貼り付け、XX.XX.XX.XXをアプリケーションのEXTERNAL\_IPに置き換えます。ブック名はすべて大文字であることがわかります。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![ボビーの本](images/bobbysbooks.png " ")
    

## タスク2: Grafanaコンソールでの変更の確認

1.  Verrazzanoのホームページに戻ります。
    
    ![Verrazzanoホーム](images/verrazzao-home.png " ")
    
2.  Grafanaのリンクを選択して、_Grafanaコンソール_を開きます。
    
    ![Grafanaリンク](images/grafana-link.png " ")
    
3.  _「ホーム」_を選択し、_「Helidon」_と入力して、_「Helidonモニタリング・ダッシュボード」_を選択します。
    
    ![Helidonの検索](images/search-helidon.png " ")
    
4.  ServiceIDで、_bobs-books\_default\_bobs-books\_bobby-helidon_を選択し、インスタンスで新しく作成されたインスタンスを選択します。この場合、変更された_bobby-helidon-stock-application_に関する情報が表示されます。
    
    ![新しいコンポーネント](images/new-component.png " ")
    
    おめでとうございます!ラボが正常に完了しました。
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月