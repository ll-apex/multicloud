# Helidon Nimaおよびリアクティブ・アプリケーションの構築と実行

## 概要

このラボでは、Oracle Cloud Infrastructure内のOracle Code EditorでNimaおよびReactive Helidonアプリケーションを構築および実行するプロセスについて説明します。

[Lab2ウォークスルー](videohub:1_e88ydqwt)

見積時間: 15分

### 目的

この演習では、次のことを行います。

*   Helidon Nimaアプリケーションのビルド、実行およびテスト
*   Helidon Reactiveアプリケーションの構築、実行、テスト
*   リアクティブアプリケーションと比較して、Nimaアプリケーションのシンプルさを分析

### 事前設定

この演習を実行するには、次のことが必要です。

*   Oracle Cloudアカウント
*   演習1を完了し、必要なJDKおよびmavenをインストールします。

## タスク1: Nimaアプリケーションの構築および実行

1.  コード・エディタで_「ファイル」_→_「開く」_をクリックします。 ![プロジェクトを開く](images/open-project.png)
    
2.  _helidon-levelup-2023-main_フォルダを選択し、_「開く」_をクリックします。 ![Helidonプロジェクト](images/helidon-project.png)
    
    > このフォルダを開いているときにコード・エディタに表示される警告/問題は無視してください。
    
3.  新しい端末を開き、次のコマンドをコピーして貼り付けてPATHおよびJAVA\_HOME変数を設定します。
    
        <copy>PATH=~/jdk-19.0.2/bin:~/apache-maven-3.8.3/bin:$PATH
        JAVA_HOME=~/jdk-19.0.2</copy>
        
4.  次のコマンドをコピーして端末で実行し、必要なJDKおよびMavenバージョンが正しく構成されていることを確認します。
    
        <copy>mvn -v</copy>
        
    
    次のような出力が表示されます。
    
        $ mvn -v 
        Apache Maven 3.8.3 (ff8e977a158738155dc465c6a97ffaf31982d739)
        Maven home: /home/ankit_x_pa/apache-maven-3.8.3
        Java version: 19.0.2, vendor: Oracle Corporation, runtime: /home/ankit_x_pa/jdk-19.0.2
        Default locale: en_US, platform encoding: UTF-8
        OS name: "linux", version: "4.14.35-2047.520.3.1.el7uek.x86_64", arch: "amd64", family: "unix"
        $ 
        
    
    > _必要なJDKおよびMavenバージョンがあるため、アプリケーションのビルドおよび実行にはこの端末のみを使用します。_
    
5.  次のコマンドをコピーして貼り付け、nimaアプリケーションをビルドします。
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    次のような出力が表示されます。
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-blocking ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/nima/target/example-nima-blocking.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  7.562 s
        [INFO] Finished at: 2023-02-27T11:42:52Z
        [INFO] ------------------------------------------------------------------------
        
6.  次のコマンドをコピーして端末に貼り付け、アプリケーションのブロッキングnimaバージョンを実行します。
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    次のような出力が表示されます。
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.27 11:43:18.589 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:43:18.902 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:43:19.700 [0x314d2373] http://127.0.0.1:33193 bound for socket '@default'
        2023.02.27 11:43:19.703 [0x314d2373] direct writes
        2023.02.27 11:43:19.722 Helidon Níma 4.0.0-ALPHA5
        
7.  サーバーが実行されているポート番号を書き留めます(@defaultのログ・エントリを参照)。たとえば、出力ではポート番号は33193です。同様に、サーバーのポート番号を確認します。
    
8.  新しいターミナルを開くには、_「ターミナル」_→_「新しいターミナル」_をクリックします。この端末を使用して、_curl_コマンドを実行します。 ![新規ターミナル](images/new-terminal.png)
    
9.  次のコマンドをコピーして新しい端末に貼り付けます。_`<port>`_をサーバーポートに置き換えることを忘れないでください。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    次のような出力が表示されます。
    
        curl http://localhost:33193/one
        remote_1
        $
        
10.  次のコマンドをコピーして新しい端末に貼り付けます。_`<port>`_をサーバーポートに交換することを忘れないでください。
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    次のような出力が表示されます。
    
        $ curl http://localhost:33193/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > 結果の順序を確認します。名前が示すように、このリクエストはリモート・リソースを順番に複数回呼び出します。
    
11.  次のコマンドをコピーして新しい端末に貼り付けます。_`<port>`_をサーバーポートに交換することを忘れないでください。
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    次のような出力が表示されます。
    
        $ curl http://localhost:33193/parallel
        Combined results: [remote_21, remote_18, remote_12, remote_13, remote_14, remote_15, remote_16, remote_17, remote_19, remote_20]
        $
        
    
    > 結果の順序を確認します。名前が示すように、このリクエストはリモート・リソースをパラレルで複数回呼び出します。
    
12.  \*java -jar \*コマンドが実行されている端末で_\[Ctrl\]を押しながら\[C\]_を押してサーバーを停止します。
    

## タスク2: リアクティブ・アプリケーションの構築および実行

1.  次のコマンドをコピーして貼り付け、nimaアプリケーションをビルドします。
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    次のような出力が表示されます。
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-reactive ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/reactive/target/example-nima-reactive.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  6.196 s
        [INFO] Finished at: 2023-02-27T11:51:17Z
        [INFO] ------------------------------------------------------------------------
        $
        
2.  次のコマンドをコピーして端末に貼り付け、アプリケーションのリアクティブ・バージョンを実行します。
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    次のような出力が表示されます。
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.27 11:51:26.227 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:51:26.723 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:51:27.286 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.27 11:51:27.618 Channel '@default' started: [id: 0x53fadd43, L:/0.0.0.0:45765]
        
3.  サーバーが実行されているポート番号を書き留めます(@defaultのログ・エントリを参照)。たとえば、出力ではポート番号は45765です。同様に、サーバーのポート番号を確認します。
    
4.  curlコマンドを実行するためにタスク1で開いたターミナルに戻ります。
    
5.  次のコマンドをコピーして新しい端末に貼り付けます。_`<port>`_をサーバーポートに交換することを忘れないでください。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    次のような出力が表示されます。
    
        $ curl http://localhost:45765/one
        remote_1
        $
        
6.  次のコマンドをコピーして新しい端末に貼り付けます。_`<port>`_をサーバーポートに交換することを忘れないでください。
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    次のような出力が表示されます。
    
        $ curl http://localhost:45765/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > 結果の順序を確認します。名前が示すように、このリクエストはリモート・リソースを順番に複数回呼び出します。
    
7.  次のコマンドをコピーして新しい端末に貼り付けます。_`<port>`_をサーバーポートに交換することを忘れないでください。
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    次のような出力が表示されます。
    
        $ curl http://localhost:45765/parallel
        Combined results: [remote_17, remote_16, remote_13, remote_20, remote_12, remote_19, remote_18, remote_14, remote_21, remote_15]
        $
        
    
    > 結果の順序を確認します。名前が示すように、このリクエストはリモート・リソースをパラレルで複数回呼び出します。
    
8.  \*java -jar \*コマンドが実行されている端末で_\[Ctrl\]を押しながら\[C\]_を押してサーバーを停止します。
    

## タスク3: Nimaアプリケーションのシンプルさの分析

**ブロックとリアクティブ**

同じタスクのNíma(ブロッキング)とHelidon SE(リアクティブ)の実装を比較します。

*   どちらの実装も、Helidon WebClientを使用してRESTコールを実行します
*   ブロッキングの実装は簡単です。
    *   実行フローは、コード内の文の順序によって反映されます。
    *   複雑な、または意味的に豊富なライブラリ呼び出しに対する呼び出しはありません
    *   デバッグは簡単です。
*   リアクティブ・バージョンでは、リアクティブ・ライブラリを十分に理解する必要があります。
    *   Multi、flatMap、エラー処理などが含まれます。
    *   リアクティブ・ハンドラの使用により、制御フローは明らかではなくなりました
    *   flatMapの同時実行性の有効化/禁止機能の理解が必要です。
    *   デバッグはより困難

1.  _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ファイルを開き、アプリケーションのnimaバージョンでエンドポイントがどのように実装されているかを確認します。 ![Nima Block](images/nima-block.png)
    
2.  _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ファイルを開き、アプリケーションのリアクティブ・バージョンでエンドポイントがどのように実装されているかを確認します。 ![リアクティブ・サービス](images/reactive-service.png)
    
3.  _「オープン・エディタ」_セクションで、_BlockingService.java_ファイルを右クリックし、_「比較用に選択」_を選択します。 ![比較の選択](images/select-compare.png)
    
4.  _ReactiveService.java_ファイルを右クリックし、_「選択済と比較」_を選択します。 ![選択内容を比較](images/compare-selected.png)
    
5.  リアクティブ・コードがブロックよりも複雑であることを確認します(仮想スレッド)。 ![コードの比較](images/compare-code.png)
    
    > BlockingServiceおよびReactiveServiceのメソッド1、順序およびパラレルをそれぞれ確認します。どのように動作するかをご確認ください!
    

_次の演習に進む_ことができます。

## 確認

*   **作成者** - Joe DiPol
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年2月