# コード・エディタでのHelidon MPアプリケーションの変更

## 概要

この演習では、コード・エディタでカスタム・エンドポイントをJAVAクラスに追加します。

見積時間: 10分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[コード・エディタでのHelidon MPアプリケーションの変更](videohub:1_sv1iug41)

### 目的

この演習では、次のことを行います。

*   Javaクラスへのカスタム・エンドポイントの追加
*   変更したアプリケーションの構築および実行

### 前提条件

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)が有効になっているアカウントが必要です。

## タスク1: カスタム・エンドポイントの追加

1.  コード・エディタで、_GreetResource.java_ファイルをクリックして開きます。 ![ファイルを開く](images/open-file.png)
    
2.  コードでわかるように、完全にMicroProfileベースです。つまり、すべての機能はPOJOおよび注釈を使用して実現されます。これらの注釈はStandardであるため、異なるベンダー間で移植できます。つまり、同じバージョンのMicroProfileをサポートする他のプラットフォームで実行されているコードを簡単に実行できます。詳細は、[ここ](https://microprofile.io/)を参照してください。
    
3.  5つのタイトルのグループからランダムにタイトルを提供する新しいエンドポイントを作成します。このエンドポイントを作成するには、次に示すように、行番号80で次のコードをコピーして貼り付けます。
    
        <copy>@Path("/title")
        @GET
        @Produces(MediaType.APPLICATION_JSON)
        public String getRandomTitle() {
            return new String[] {"Mr. ", "Mrs.", "Miss", "Dr.", "Dr. sc."} [(int)(Math.random()*5)];
        }</copy>
        
    
    ![コードを追加する](images/add-code.png)
    
4.  ファイルのコンテンツを保存するには、コード・エディタで_「ファイル」_→_「保存」_をクリックします。
    
    > AutoSaveは、コード・エディタでデフォルトで有効になっています。
    

## タスク2: アプリケーションの実行

1.  次のコマンドをコピーして端末に貼り付け、アプリケーションを実行します。
    
        <copy>mvn clean package
        java -jar target/myproject.jar</copy>
        
2.  新しい端末/コンソールを開き、次のコマンドを実行してアプリケーションを確認します。
    
        <copy>curl -X GET http://localhost:8080/greet/title</copy>
        Mr.
        
    
    > 5つのオプションのうち、タイトルがランダムに表示され、このコマンドを複数回実行できます。
    
3.  _「java -jar target/myproject.jar」コマンドが実行されている端末に`Ctrl + C`と入力して、**myproject**アプリケーションを停止します_。これは非常に重要であり、それ以外はラボの後半で問題に直面します。
    

## 確認

*   **作成者** - Dmitry Aleksandrov
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年4月