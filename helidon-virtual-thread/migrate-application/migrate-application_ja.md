# Helidon 3アプリケーションを作成し、Helidon 4に移行します

## 概要

この演習では、Nettyに基づいて元のリアクティブWebサーバーで実行されているHelidon 3アプリケーションから開始します。その後、仮想スレッドを使用して、新しいHelidon Nima WebServerで実行されているHelidon 4にアプリケーションを移行します。

[Lab4ウォークスルー](videohub:1_zr1m00ba)

見積時間: 20分

### 目的

*   Helidon Starterを使用して、Helidon MicroProfileアプリケーションを生成、構築および実行します。
*   Helidon 3 MicroprofileアプリケーションをHelidon 4に移行します

### 事前設定

*   Oracle Cloudアカウント

## タスク1: Helidon 3アプリケーションの作成およびアプリケーションのビルド

1.  次のURLをコピーしてブラウザに貼り付け、Helidonプロジェクト・ページを開きます。
    
        <copy>https://helidon.io/starter/</copy>
        
2.  「プロジェクトの生成」で、「Helidonフレーバ」として_「Helidon MP」_を選択し、_「次へ」_をクリックします。
    
3.  「アプリケーション・タイプ」で、_「クイックスタート」_、_「次へ」_の順に選択します。
    
4.  「Media Support」で、_「Jackson」_を選択し、_「Next」_をクリックします。
    
5.  「プロジェクトのカスタマイズ」で、デフォルト値を選択し、_「ダウンロード」_をクリックします。これはウィンドウにポップアップし、この_myproject.zip_を選択した場所に保存します。このワークショップの残りの部分では、_myproject_名が使用されます。別の名前を選択する場合は、それぞれ変更してください。
    
6.  コード・エディタのHELIDON-LEVELUP-2023-MAINに戻り、_「ドキュメント」_をクリックします。 ![ドキュメントの選択](images/select-docs.png)
    
7.  _「ファイル」_→_「ファイルのアップロード」_をクリックし、このファイルを以前に保存した場所から_myproject.zip_を選択して、_「開く」_をクリックします。 ![ファイルのアップロード](images/upload-files.png)
    
8.  _docs_フォルダの下に _myproject.zip_ファイルが表示されます。 ![Zipファイル](images/zip-file.png)
    
9.  次のコマンドをコピーして貼り付け、ファイルを解凍します。
    
        <copy>cd ~/helidon-levelup-2023-main/docs/
        unzip myproject.zip</copy>
        
10.  myprojectフォルダから、次のコマンドを実行してプロジェクトをビルドします。PATHおよびJAVA\_HOME変数を設定した端末を使用してください。
    
        <copy>cd myproject
        mvn clean package</copy>
        
    
    > このコマンドの実行が終了すると、_BUILD SUCCESS_と表示されます。
    
11.  次のコマンドをコピーして端末に貼り付け、このアプリケーションを実行します。次のスクリーンショットに示すような出力が表示されます。
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    次のような出力が表示されます。
    
        $ java -jar target/myproject.jar
        2023.02.28 03:48:36 INFO io.helidon.common.LogConfig Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:48:39 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:48:40 INFO io.helidon.webserver.NettyWebServer Thread[#31,nioEventLoopGroup-2-1,10,main]: Channel '@default' started: [id: 0x5b66bd2a, L:/0.0.0.0:8080]
        2023.02.28 03:48:41 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 4816 milliseconds (since JVM startup).
        2023.02.28 03:48:41 INFO io.helidon.common.HelidonFeatures Thread[#32,features-thread,5,main]: Helidon MP 3.1.2 features: [CDI, Config, Health, JAX-RS, Metrics, Open API, Server]
        
12.  curlコマンドを実行する端末に戻り、次のコマンドを実行してアプリケーションを確認します。
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
13.  "java -jar target/myproject.jar"コマンドが実行されている端末に`Ctrl + C`と入力して、_myproject_アプリケーションを停止します。
    
14.  ファイル _src/main/java/com/example/myproject/GreetResource.java_を編集し、メソッド _createResponse(String who)_を検索して、スクリーンショットに示すように次の行を追加します。
    
        <copy>System.out.println("Running on thread " + Thread.currentThread());</copy>
        
    
    ![アプリケーションの変更](images/modify-application.png)
    
15.  ステップ10、11および12の説明に従って、アプリケーションを再構築、実行および実行します。
    
16.  サーバーを起動した端末のサーバー出力を確認します。スレッドの名前はhelidon-server-nです。これは、JAX-RSリクエストを処理するためにHelidonによって作成されたスレッド・プール内の従来のプラットフォーム・スレッドです。次のようなサーバー出力が表示されます。
    
        Running on thread Thread[#25,helidon-server-1,5,server]
        
17.  "java -jar target/myproject.jar"コマンドが実行されている端末に`Ctrl + C`と入力して、_myproject_アプリケーションを停止します。
    

## タスク2: Helidom MicroProfileアプリケーションのHelidon 4への移行

1.  myprojectの場合は、_pom.xml_ファイルを開き、親pomを _3.1.1_から _4.0.0-ALPHA5_に変更します。
    
        <parent>
            <groupId>io.helidon.applications</groupId>
            <artifactId>helidon-mp</artifactId>
            <version>4.0.0-ALPHA5</version>
            <relativePath/>
        </parent>
        
    
    ![POMバージョン](images/pom-version.png)
    
2.  src/main/resources/logging.propertiesを編集し、_io.helidon.common.HelidonConsoleHandler_を_io.helidon.logging.jul.HelidonConsoleHandler_に変更します。 ![ロギングの編集](images/edit-logging.png)
    
3.  これで、アプリケーションがHelidon 4に移行されました。次のコマンドをコピーして貼り付け、アプリケーションをビルドします。
    
        <copy>mvn clean package -DskipTests</copy>
        
4.  次のコマンドをコピーして貼り付け、アプリケーションを実行します。
    
        <copy>java --enable-preview  -jar target/myproject.jar</copy>
        
    
    次のような出力が表示されます。
    
        $ java --enable-preview  -jar target/myproject.jar
        2023.02.28 03:56:17 INFO io.helidon.logging.jul.JulProvider Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:56:21 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] http://0.0.0.0:8080 bound for socket '@default'
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] direct writes
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Started all channels in 19 milliseconds. 5131 milliseconds since JVM startup. Java 19.0.2+7-44
        2023.02.28 03:56:22 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 5144 milliseconds (since JVM startup).
        2023.02.28 03:56:22 INFO io.helidon.common.features.HelidonFeatures Thread[#28,features-thread,5,main]: Helidon MP 4.0.0-ALPHA5 features: [CDI, Config, Health, Metrics, Open API, Server, WebServer]
        
5.  サーバー出力を確認し、スレッドがVirtualThreadになったことを確認します。
    
6.  "java --enable-preview -jar target/myproject.jar"コマンドが実行されている端末に`Ctrl + C`と入力して、_myproject_アプリケーションを停止します。
    

Helidon仮想スレッド・ワークショップが完了しました。

## 確認

*   **作成者** - Joe DiPol
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年2月