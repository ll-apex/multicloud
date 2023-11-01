# Helidon MPプロジェクトを生成し、コード・エディタでプロジェクトを実行します

## 概要

このラボでは、Helidon MPアプリケーションを作成するステップについて説明します。

見積時間: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[Helidon MPプロジェクトの生成およびコード・エディタでのプロジェクトの実行](videohub:1_22nv8v4q)

### 製品・技術について

Helidonは、使いやすいように設計されており、ツールや例を使用してすぐに作業を開始できます。Helidonは高速Nettyコアで実行されているライブラリの集合であるため、余分なオーバーヘッドや膨張はありません。HelidonはMicroProfileをサポートし、JAX-RS、CDI、JSON-P/Bなどの使い慣れたAPIを提供します。MicroProfile実装は、高速Helidon Reactive WebServerで実行されます。リアクティブWebServerは、最新の機能的なプログラミング・モデルを提供し、Netty上で動作します。軽量で柔軟でリアクティブなHelidon WebServerは、マイクロサービスの使いやすく高速な基盤を提供します。

ヘルス・チェック、メトリック、トレースおよびフォルト・トレランスのサポートにより、Helidonには、Prometheus、Jaeger/ZipkinおよびKubernetesと統合されたクラウド対応アプリケーションを記述するために必要なものがあります。

### Helidon Project Starterについて

Project Starterは、Helidonプロジェクトを作成するための新しいWeb UIです。高度にカスタマイズ可能で、ユーザーがプロジェクトに追加するHelidon機能を選択できる様々なオプションを提供します。エンド・ユーザーは、特定のニーズにあわせてプロジェクトを生成できます。詳細は、[「Helidon Starter」](https://helidon.io/starter)をクリックしてください。

### コード・エディタについて

コード・エディタを使用すると、OCIコンソールから直接、様々なOCIサービスのコードを編集およびデプロイできます。コンソールとローカル開発環境を切り替えることなく、サービス・ワークフローおよびスクリプトを更新できるようになりました。これにより、クラウド・ソリューションの迅速なプロトタイプ作成、新しいサービスの探索、迅速なコーディング・タスクの実行が簡単になります。

コード・エディタをクラウド・シェルと直接統合することで、クラウド・シェルに事前にインストールされているGraalVM Enterprise Native ImageおよびJDK 17 (Java Development Kit)にアクセスできます。

### OCI Cloud Shellについて

[OCI Cloud Shell](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm)は、Oracle Cloudコンソールからアクセスできるブラウザベースのターミナルです。事前認証済のOCIコマンドライン・インタフェース(CLI)を備えたLinuxシェルと、事前インストール済の開発者ツールにアクセスでき、5GBのストレージが付属しています。

バージョン22.2.0では、GraalVM Enterprise JDK 17およびネイティブ・イメージがクラウド・シェルに事前にインストールされています。

> GraalVM Enterpriseは、追加コストなしでOracle Cloud Infrastructure上で使用できます。

### 目的

*   Helidon Greetingアプリケーションと呼ばれるMicroProfileでサポートされているマイクロサービスを作成します
*   コード・エディタでHelidonアプリケーションを開きます
*   クラウド・シェルでのデフォルトのJDKの変更
*   必要なMavenの構成
*   Helidon Greetingアプリの実行と実行

### 事前設定

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)が有効になっているアカウントが必要です。

## タスク1: プロジェクト・スタータを使用したHelidonプロジェクトの生成

1.  次のURLをコピーしてブラウザに貼り付け、Helidonプロジェクト・ページを開きます。
    
        <copy>https://helidon.io/starter/</copy>
        
2.  「プロジェクトの生成」で、「Helidonフレーバ」として_「Helidon MP」_を選択し、_「次へ」_をクリックします。
    
3.  「アプリケーション・タイプ」で、_「クイックスタート」_、_「次へ」_の順に選択します。
    
4.  「Media Support」で、「_JSON-B_」を選択し、「_Next_」をクリックします。
    
5.  「プロジェクトのカスタマイズ」で、デフォルト値を選択し、_「ダウンロード」_をクリックします。これはウィンドウにポップアップし、この_myproject.zip_を選択した場所に保存します。このワークショップの残りの部分では、myproject名が使用されます。別の名前を選択する場合は、それぞれ変更してください。
    

## タスク2: helidonプロジェクトのローカルでのビルドおよび実行

1.  クラウド・コンソールで、次に示すように_「開発者ツール」_アイコンをクリックし、_「コード・エディタ」_をクリックします。 ![コード・エディタ](images/code-editor.png)
    
2.  コード・エディタで、_「Terminal」_→_「New Terminal」_をクリックします。 ![ターミナルを開く](images/open-terminal.png)
    
3.  次のコマンドをコピーして端末に貼り付け、_myproject_フォルダを作成します。ここでデフォルトの myproject.zipファイルをダウンロードします。
    
        <copy>mkdir -p ~/helidon-project/myproject</copy>
        
4.  コード・エディタで、_「ファイル」_→_「開く」_をクリックします。 ![フォルダを開く](images/open-folder.png)
    
5.  _helidon-project_フォルダを選択し、_「Open」_をクリックします。 ![Helidonを開く](images/open-helidon.png)
    
6.  コード・エディタの「EXPLORER」ウィンドウで、_「HELIDON-PROJECT」_を表示できます。ここで_「myproject」_フォルダを参照してクリックします。_「ファイル」_→_「ファイルのアップロード..」_をクリックし、プロジェクトをダウンロードした場所を指定し、zipファイルを選択して_「開く」_をクリックします。![マイプロジェクト](images/myproject.png) ![ファイルのアップロード](images/upload-files.png)
    
7.  次のコマンドをコピーして貼り付け、_myproject.zip_ファイルを解凍します。
    
        <copy>cd ~/helidon-project/myproject
        unzip myproject.zip
        </copy>
        
8.  _myproject_フォルダを展開して、プロジェクト構造を表示します。 ![プロジェクトの展開](images/expand-project.png)
    
9.  このプロジェクトを実行するには、Maven 3.8+およびJDK 17+を使用します。Oracleクラウドには、様々なJDKが用意されています。ここでは、GraalVM JDKを選択します。端末で次のコマンドをコピーして貼り付け、デフォルトのJDKを確認します。
    
        <copy>csruntimectl java list</copy>
        
    
    ![JDKのリスト](images/list-jdk.png)
    
    > 最初に\* _asterisk_を持つJDKは、デフォルトのJDKです。他のJDK、graalvmeejdkがある場合は、次のコマンドを実行してデフォルトのJDKバージョンを変更します。graalvmeejdkの表示されたバージョンを使用してください。これは、コマンドの表示と異なる場合があります。
    
        <copy>csruntimectl java set graalvmeejdk-17</copy>
        
10.  必要なmavenを構成するには、次のコマンドをコピーして端末に貼り付けます。
    
        <copy>cd ~/helidon-project/myproject/
        wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz
        tar -xzvf apache-maven-3.8.8-bin.tar.gz
        PATH=~/helidon-project/myproject/apache-maven-3.8.8/bin:$PATH</copy>
        
    
    ![mavenの構成](images/configure-maven.png)
    
11.  次に示すように、正しいバージョンのJDKおよびMavenがあることを確認するには、ターミナルで次のコマンドを実行します。
    
        <copy>mvn -v</copy>
        
    
    ![前提条件の確認](images/verify-prerequisite.png)
    
12.  _myproject_フォルダから、次のコマンドを実行してプロジェクトを構築します。
    
        <copy>cd ~/helidon-project/myproject/myproject
        mvn clean package</copy>
        
    
    ![プロジェクトをビルドして](images/build-project.png)
    
    > このコマンドの実行が終了すると、_BUILD SUCCESS_と表示されます。
    
13.  次のコマンドをコピーして端末に貼り付け、このアプリケーションを実行します。次のスクリーンショットに示すような出力が表示されます。
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    ![プロジェクトの実行](images/run-project.png)
    

> 開始時間は4822ミリ秒です。この時間を、後でネイティブ・イメージの実行可能ファイルと比較します。

14.  コード・エディタで新しい端末/コンソールを開き、次のコマンドを実行してアプリケーションを確認します。
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        
15.  _「java -jar target/myproject.jar」コマンドが実行されている端末に`Ctrl + C`と入力して、**myproject**アプリケーションを停止します_。これは非常に重要であり、それ以外はラボの後半で問題に直面します。
    

## 確認

*   **作成者** - Dmitry Aleksandrov
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年4月