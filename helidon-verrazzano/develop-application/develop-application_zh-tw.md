# 開發 Helidon 應用程式

## 簡介

本實驗室會逐步介紹建立 Helidon MP 應用程式的步驟。

預估時間：15 分鐘

### 關於產品 / 技術

Helidon 的設計可簡單易用，提供工具和範例讓您快速上手。由於 Helidon 只是一個在快速的 Netty 核心上執行的程式庫集合，因此沒有額外的負荷或 bloat。Helidon 支援 MicroProfile，並提供熟悉的 API，例如 JAX-RS、CDI 和 JSON-P/B。我們的 MicroProfile 導入作業是在快速的 Helidon Reactive WebServer 上執行。反應 WebServer 提供現代化的功能程式設計模型，並在 Netty 上執行。Helidon WebServer 輕量、彈性和反應性，為您的微服務提供簡單易用的快速基礎。

Helidon 支援狀況檢查、度量、追蹤和容錯功能，可撰寫與 Prometheus、Jaeger/Zipkin 及 Kubernetes 整合的雲端就緒應用程式。

### 關於 Helidon CLI

Helidon CLI 可讓您從一組架構中挑選，輕鬆建立 Helidon 專案。它也支援執行持續編譯和重新啟動應用程式的開發人員迴圈，因此您可以輕鬆地在程式碼變更時重複原始程式碼。

CLI 會以獨立執行檔 (使用 GraalVM 編譯) 的方式散佈，以方便安裝。目前可用作 Linux、Mac 及 Windows 的下載。只要下載二進位檔案，即可在可從 PATH 存取的位置安裝該二進位檔案。

### 目標

*   安裝 Helidon CLI
*   建立一個名為 Helidon Greeting 的 MicroProfile 支援的微服務
*   執行與執行 Helidon Greeting 應用程式
*   檢視狀況和度量資料
*   新增功能至應用程式

### 先決條件

*   Helidon 需要 Java 11+
*   Maven 3.6.x 版

> **注意：由於應用程式建置發生已知問題，請勿使用 3.8.x 版本！**

*   路徑中包含 Java 和 `mvn`。
*   Windows 使用者也需要 Visual C++ 可重新分配的程式實際執行。  
    如需更多資訊，請參閱 [Windows 上的 Helidon](https://helidon.io/docs/v2/#/about/04_windows) 。

## 作業 1：安裝 Helidon CLI

1.  安裝 Helidon CLI
    
    若為 macOS：
    
        <copy>
        curl -O https://helidon.io/cli/latest/darwin/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    適用於 Linux：
    
        <copy>
        curl -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Windows:
    
        <copy>
        PowerShell -Command Invoke-WebRequest -Uri "https://helidon.io/cli/latest/windows/helidon.exe" -OutFile "C:\Windows\system32\helidon.exe"
        </copy>
        

## 作業 2：建立 Helidon 問候語應用程式

1.  在主控台中輸入：
    
        <copy>helidon init --version 2.4.0 </copy>
        
    
    > 若要避免任何潛在問題，請定義針對此實驗室環境所測試的特定 Helidon 版本。
    
2.  在這個示範中，我們將建立支援 MicroProfile 的微服務，因此請為 **Helidon MP Flavor** 選擇 **(2)** 選項：
    
        Helidon flavor
        (1) SE 
        (2) MP 
        Enter selection (Default: 1): 2
        
3.  針對大部分功能，請依序選擇 **(2) quickstart** 和 **Enter** 作為預設答案。請注意，您可以有不同的預設套件與專案群組名稱，因為它使用作業系統使用者名稱。請注意套裝程式名稱之後，您必須在建立新的 Java 類別時使用它。
    
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
        
    
    > 若要讓**開發迴圈**立即接受預設值 ( **n**)。您稍後將會在本實驗室中開始開發迴圈。
    
    您現在已經有一個功能完整的微服務 Maven 專案：
    

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
    
    

## 作業 3：執行 Helidon 問候語應用程式

1.  從同一個主控台 / 終端機，瀏覽到 quickstart-mp 目錄，然後執行下列命令：
    
        <copy> cd quickstart-mp
        </copy>
        
2.  使用 JDK11+
    
        <copy>
        mvn package
        java -jar target/quickstart-mp.jar
        </copy>
        

### 執行應用程式

1.  開啟新的終端機 / 主控台並執行下列命令來檢查應用程式：
    
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
        

### 複查健康與評量標準資料

1.  在相同的終端機 / 主控台中，執行下列命令以檢查狀況與度量：
    
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
        
2.  在執行 "java -jar target/quickstart-mp.jar" 命令的終端機中輸入 `Ctrl + C`，即可停止 _quickstart-mp_ 應用程式。
    

## 作業 4：修改應用程式

1.  開啟您最常用的 IDE，瀏覽至 **microprofile-config.properties** 檔案。
    
    ![組態檔](images/config.jpg)
    
2.  在主控台 / 終端機中，瀏覽至專案資料夾並輸入：
    
        <copy>helidon dev</copy>
        
    
    > 這會啟動上一個作業中提到的**開發迴圈**。
    
3.  將特性 _app.greeting_ 變更為 "Hello Oracle"，然後儲存檔案。
    
        <copy>app.greeting=Hello Oracle</copy>
        
    
    ![HelidonDev](images/properties.jpg)
    
    > 您會看到每當您變更檔案時，**Helidon CLI** 就會辨識出有變更、重新編譯應用程式並重新執行。由於 Helidon 很小，因此一切都很快就會發生。
    
4.  在主控台 / 終端機中輸入下列內容：
    
        <copy>curl -X GET http://localhost:8080/greet</copy>
        
    
    結果應為：
    
        {"message":"Hello Oracle World!"}
        
    
    > 請務必使用 `CTRL+C` 停止開發迴圈
    
5.  返回您的專案資料夾，然後開啟 **GreetResource.java** 檔案。
    
    > 您可以看到它純 MicroProfile 相容的程式碼：
    
    ![ModifyJava](images/greet-resource.jpg)
    
6.  建立一個新的端點，提供不同語言的不同問候。若要建立此新功能，請使用以下程式碼建立一個名為 **GreetHelpResource** 的新類別：
    
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
        
    
    > 這個類別只有一個方法 _getAllGreetings_ ，會傳回不同語言的問候清單。複製程式碼時，請務必在類別頂端新增必要的套裝程式名稱。
    
7.  建置和執行應用程式：
    
        <copy>
        mvn package -DskipTests
        java -jar target/quickstart-mp.jar
        </copy>
        
8.  執行下列命令並注意結果：
    
        <copy>curl http://localhost:8080/help/allGreetings</copy>
        
    
    預期的結果：
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
9.  查看測量結果，您會看到出現一個新的計數器。每次呼叫此端點時，值都會遞增：
    
        curl http://localhost:8080/metrics
        
        ...
        # TYPE application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total counter
        # HELP application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total How many time help was called
        application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total 1
        ...
        
10.  您現在可以在主控台中看到有關此呼叫的 INFO 日誌行：
    
        INFO me.buzz.mp.quickstart.GreetHelpResource Thread[helidon-4,5,server]: Help requested!
        
    
    且已新增端點。
    
    ![NewEndpoint](images/logs-output.jpg)
    
    > 輕鬆快速地使用 Helidon 及其工具！
    
11.  讓您的終端機 / 主控台保持開啟，並繼續參加 Verrazzano 安裝實驗室。
    

## 確認

*   **作者** - Dmitry Aleksandrov
*   **貢獻者** - Maciej Gruszka，Ankit Pandey
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 3 月