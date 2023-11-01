# Helidonアプリケーションの開発

## 概要

このラボでは、Helidon MPアプリケーションを作成するステップについて説明します。

見積時間: 15分

### 製品・技術について

Helidonは、使いやすいように設計されており、ツールや例を使用してすぐに作業を開始できます。Helidonは高速Nettyコアで実行されているライブラリの集合であるため、余分なオーバーヘッドや膨張はありません。HelidonはMicroProfileをサポートし、JAX-RS、CDI、JSON-P/Bなどの使い慣れたAPIを提供します。MicroProfile実装は、高速Helidon Reactive WebServerで実行されます。リアクティブWebServerは、最新の機能的なプログラミング・モデルを提供し、Netty上で動作します。軽量で柔軟でリアクティブなHelidon WebServerは、マイクロサービスの使いやすく高速な基盤を提供します。

ヘルス・チェック、メトリック、トレースおよびフォルト・トレランスのサポートにより、Helidonには、Prometheus、Jaeger/ZipkinおよびKubernetesと統合されたクラウド対応アプリケーションを記述するために必要なものがあります。

### Helidon CLIについて

Helidon CLIでは、一連の原型から選択することで、Helidonプロジェクトを簡単に作成できます。また、継続的なコンパイルおよびアプリケーションの再起動を実行する開発者ループをサポートしているため、ソース・コードの変更を簡単に反復できます。

CLIは、インストールを容易にするためにスタンドアロン実行可能ファイルとして配布されます(GraalVMを使用してコンパイルされます)。現在、Linux、Mac、Windows用のダウンロードとして利用可能です。バイナリをダウンロードするだけで、PATHからアクセス可能な場所にバイナリをインストールできます。

### 目的

*   Helidon CLIのインストール
*   Helidon GreetingというMicroProfileでサポートされているマイクロサービスを作成します
*   Helidon Greetingアプリの実行と実行
*   ヘルスおよびメトリック・データの表示
*   アプリケーションへの新機能の追加

### 事前設定

*   HelidonにはJava 11以上が必要です
*   Maven 3.6.x

> **注意: アプリケーション・ビルドに既知の問題があるため、3.8.xバージョンを使用しないでください。**

*   Javaおよび`mvn`はパス内にあります。
*   Windowsユーザーには、Visual C++ Redistributable Runtimeも必要です。  
    詳細は、[Windows上のHelidon](https://helidon.io/docs/v2/#/about/04_windows)を参照してください。

## タスク1: Helidon CLIのインストール

1.  Helidon CLIのインストール
    
    macOSの場合:
    
        <copy>
        curl -O https://helidon.io/cli/latest/darwin/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Linuxの場合:
    
        <copy>
        curl -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Windowsの場合:
    
        <copy>
        PowerShell -Command Invoke-WebRequest -Uri "https://helidon.io/cli/latest/windows/helidon.exe" -OutFile "C:\Windows\system32\helidon.exe"
        </copy>
        

## タスク2: Helidonグリーティング・アプリケーションの作成

1.  コンソールで次のように入力します。
    
        <copy>helidon init --version 2.4.0 </copy>
        
    
    > 潜在的な問題を回避するには、この演習の環境でテストされた特定のHelidonバージョンを定義します。
    
2.  このデモでは、MicroProfileでサポートされているマイクロサービスを作成するため、**Helidon MP Flavor**のオプション**(2)**を選択します:
    
        Helidon flavor
        (1) SE 
        (2) MP 
        Enter selection (Default: 1): 2
        
3.  ほとんどの機能では、デフォルトのアンサーに対して**(2)クイックスタート**、**\[Enter\]**の順に選択します。OSユーザー名を使用するため、デフォルトのパッケージ名とプロジェクト・グループ名は異なる場合があります。パッケージ名を書き留めておくと、新しいJavaクラスの作成時に使用する必要があります。
    
        Select archetype
        (1) bare | Minimal Helidon MP project suitable to start from scratch 
        (2) quickstart | Sample Helidon MP project that includes multiple REST operations 
        (3) database | Helidon MP application that uses JPA with an in-memory H2 database 
        Enter selection (Default: 1): 2
        Project name (Default: quickstart-mp): 
        Project groupId (Default: me.user-helidon): 
        Project artifactId (Default: quickstart-mp): 
        Project version (Default: 1.0-SNAPSHOT): 
        Java package name (Default: me.user_name.mp.quickstart): 
        Switch directory to /home/user/quickstart-mp to use CLI
        
        Start development loop? (Default: n):
        $
        
    
    > **development loop**には、ここでデフォルト(**n**)を受け入れます。開発ループは、この演習の後半で開始します。
    
    これで、完全に機能するマイクロサービスMavenプロジェクトができました。
    

        quickstart-mp
        ├── Dockerfile
        ├── Dockerfile.jlink
        ├── Dockerfile.native
        ├── README.md
        ├── app.yaml
        ├── pom.xml
        └── src
            ├── main
            │   ├── java
            │   │   └── me
            │   │       └── buzz
            │   │           └── mp
            │   │               └── quickstart
            │   │                   ├── GreetResource.java
            │   │                   ├── GreetingProvider.java
            │   │                   └── package-info.java
            │   └── resources
            │       ├── META-INF
            │       │   ├── beans.xml
            │       │   ├── microprofile-config.properties
            │       │   └── native-image
            │       │       └── reflect-config.json
            │       └── logging.properties
            └── test
                └── java
                    └── me
                        └── buzz
                            └── mp
                                └── quickstart
                                    └── MainTest.java
    
    

## タスク3: Helidonグリーティング・アプリケーションの実行

1.  同じコンソール/端末から、quickstart-mpディレクトリに移動し、次のコマンドを実行します。
    
        <copy> cd quickstart-mp
        </copy>
        
2.  JDK11+の場合
    
        <copy>
        mvn package
        java -jar target/quickstart-mp.jar
        </copy>
        

### アプリケーションの演習

1.  新しい端末/コンソールを開き、次のコマンドを実行してアプリケーションを確認します。
    
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
        

### ヘルスおよびメトリック・データのレビュー

1.  同じターミナル/コンソールで、次のコマンドを実行してヘルスおよびメトリックを確認します:
    
        <copy>
        curl -s -X GET http://localhost:8080/health
        </copy>
        {"outcome":"UP",...
        . . .
        
    
        # Prometheus Format
        <copy>
        curl -s -X GET http://localhost:8080/metrics
        </copy>
        # TYPE base:gc_g1_young_generation_count gauge
        . . .
        
    
        # JSON Format
        <copy>
        curl -H 'Accept: application/json' -X GET http://localhost:8080/metrics
        </copy>
        {"base":...
        . . .
        
2.  "java -jar target/quickstart-mp.jar"コマンドが実行されている端末に`Ctrl + C`と入力して、_quickstart-mp_アプリケーションを停止します。
    

## タスク4: アプリケーションの変更

1.  お気に入りのIDEを開き、**microprofile-config.properties**ファイルに移動します。
    
    ![構成ファイル](images/config.jpg)
    
2.  コンソール/ターミナルで、プロジェクト・フォルダに移動し、次のように入力します。
    
        <copy>helidon dev</copy>
        
    
    > これにより、前のタスクで説明した**開発ループ**が開始されます。
    
3.  プロパティ_app.greeting_を「Hello Oracle」に変更し、ファイルを保存します。
    
        <copy>app.greeting=Hello Oracle</copy>
        
    
    ![HelidonDev](images/properties.jpg)
    
    > ファイルを変更するたびに、**Helidon CLI**が変更があることを認識し、アプリケーションを再コンパイルして再実行します。Helidonは小さいので、すべてがすぐに起こります。
    
4.  コンソール/ターミナルで、次を入力します:
    
        <copy>curl -X GET http://localhost:8080/greet</copy>
        
    
    結果は次のようになります。
    
        {"message":"Hello Oracle World!"}
        
    
    > `CTRL+C`を使用して開発ループを停止してください
    
5.  プロジェクトフォルダに戻り、**GreetResource.java**ファイルを開きます。
    
    > 純粋なMicroProfile互換コードであることがわかります。
    
    ![ModifyJava](images/greet-resource.jpg)
    
6.  異なる言語での異なる挨拶に役立つ新しいエンドポイントを作成します。この新機能を作成するには、次のコードを使用して**GreetHelpResource**という新しいクラスを作成します。
    
        <copy>
        package me.user_name.me.quickstart;
        import java.util.Arrays;
        import java.util.List;
        import java.util.logging.Logger;
        
        import javax.enterprise.context.ApplicationScoped;
        import javax.ws.rs.GET;
        import javax.ws.rs.Path;
        
        import org.eclipse.microprofile.metrics.annotation.Counted;
        
        @ApplicationScoped
        @Path("/help")
        public class GreetHelpResource {
        
            Logger LOGGER = Logger.getLogger(GreetHelpResource.class.getName());
        
            @GET
            @Path("/allGreetings")
            @Counted(name = "helpCalled", description = "How many time help was called")
            public String getAllGreetings(){
                LOGGER.info("Help requested!");
                return Arrays.toString(List.of("Hello","Привет","Hola","Hallo","Ciao","Nǐ hǎo", "Marhaba","Olá").toArray());
            }
        }
        </copy>
        
    
    > このクラスには、異なる言語のグリーティングのリストを返すメソッド_getAllGreetings_が1つのみあります。コードをコピーする際は、必要なパッケージ名をクラスの上部に追加してください。
    
7.  アプリケーションをビルドして実行します:
    
        <copy>
        mvn package -DskipTests
        java -jar target/quickstart-mp.jar
        </copy>
        
8.  次のコマンドを実行して、結果を確認します:
    
        <copy>curl http://localhost:8080/help/allGreetings</copy>
        
    
    予期した結果:
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
9.  メトリックを見ると、新しいカウンタが表示されます。このエンドポイントが呼び出されるたびに、値が増分されます。
    
        curl http://localhost:8080/metrics
        
        ...
        # TYPE application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total counter
        # HELP application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total How many time help was called
        application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total 1
        ...
        
10.  コンソールに、このコールに関するINFOログ行が表示されます。
    
        INFO me.buzz.mp.quickstart.GreetHelpResource Thread[helidon-4,5,server]: Help requested!
        
    
    新しいエンドポイントが追加されました。
    
    ![NewEndpoint](images/logs-output.jpg)
    
    > Helidonとそのツールの操作は簡単で高速です!
    
11.  端末/コンソールを開いたままにして、Verrazzanoインストール・ラボに進みます。
    

## 確認

*   **作成者** - Dmitry Aleksandrov
*   **貢献者** - Maciej Gruszka、Ankit Pandey
*   **最終更新者/日付** - Ankit Pandey、2023年3月