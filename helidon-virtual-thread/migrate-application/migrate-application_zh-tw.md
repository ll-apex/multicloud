# 建立 Helidon 3 應用程式，然後將其移轉至 Helidon 4

## 簡介

在此實驗室中，您將從以 Netty 為基礎的原始反應 Web 伺服器上執行的 Helidon 3 應用程式開始。然後，您就可以使用虛擬執行緒，將應用程式移轉至在新的 Helidon Nima WebServer 上執行的 Helidon 4。

[Lab4 逐步解說](videohub:1_zr1m00ba)

預估時間：20 分鐘

### 目標

*   使用 Helidon Starter 產生、建立及執行 Helidon MicroProfile 應用程式。
*   將 Helidon 3 Microprofile 應用程式移轉至 Helidon 4

### 先決條件

*   Oracle Cloud 帳戶

## 任務 1：建立 Helidon 3 應用程式並建置應用程式

1.  複製下方 URL 並將其貼至瀏覽器，以開啟「Helidon 專案」頁面。
    
        <copy>https://helidon.io/starter/</copy>
        
2.  在「產生您的專案」下方，選取 _Helidon MP_ 作為 Helidon Flavor，然後按_下一步_。
    
3.  針對「應用程式類型」，選取_快速啟動_，然後按_下一步_。
    
4.  若為媒體支援，請選取 _Jackson_ ，然後按一下_下一步_。
    
5.  針對「自訂專案」，選取預設值並按一下_下載_。這將會彈出在視窗中，將此 _myproject.zip_ 儲存到您選擇的位置。在本研討會的其餘部分，將使用 _myproject_ 名稱。如果您選擇其他名稱，請個別變更。
    
6.  返回程式碼編輯器，在 HELIDON-LEVELUP-2023-MAIN 中，按一下_文件_。 ![選擇文件](images/select-docs.png)
    
7.  按一下 \[ _檔案_ \] -> \[ _上傳檔案_ \]，從您先前儲存此檔案的位置選取 _myproject.zip_ ，然後按一下 \[ _開啟_ \]。 ![上傳檔案](images/upload-files.png)
    
8.  您會在 _docs_ 資料夾下的 _myproject.zip_ 檔案。 ![Zip 檔案](images/zip-file.png)
    
9.  複製並貼上下列指令以解壓縮檔案。
    
        <copy>cd ~/helidon-levelup-2023-main/docs/
        unzip myproject.zip</copy>
        
10.  從 myproject 資料夾執行下列命令來建立專案。請使用終端機來設定 PATH 與 JAVA\_HOME 變數。
    
        <copy>cd myproject
        mvn clean package</copy>
        
    
    > 您應該會在執行此命令的結束時看到 _BUILD SUCCESS_ 。
    
11.  將下列指令複製並貼到終端機中，以執行此應用程式。您會看到和以下螢幕擷取畫面類似的執行結果。
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    您會看到類似以下的執行結果：
    
        $ java -jar target/myproject.jar
        2023.02.28 03:48:36 INFO io.helidon.common.LogConfig Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:48:39 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:48:40 INFO io.helidon.webserver.NettyWebServer Thread[#31,nioEventLoopGroup-2-1,10,main]: Channel '@default' started: [id: 0x5b66bd2a, L:/0.0.0.0:8080]
        2023.02.28 03:48:41 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 4816 milliseconds (since JVM startup).
        2023.02.28 03:48:41 INFO io.helidon.common.HelidonFeatures Thread[#32,features-thread,5,main]: Helidon MP 3.1.2 features: [CDI, Config, Health, JAX-RS, Metrics, Open API, Server]
        
12.  返回終端機，從此處執行 curl 命令，然後執行下列命令來檢查應用程式：
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
13.  在執行 "java -jar target/myproject.jar" 命令的終端機中輸入 `Ctrl + C`，即可停止 _myproject_ 應用程式。
    
14.  編輯 _src/main/java/com/example/myproject/GreetResource.java_ 檔案，找出 _createResponse (String his)_ 方法，然後新增下面這行，如螢幕擷取畫面所示。
    
        <copy>System.out.println("Running on thread " + Thread.currentThread());</copy>
        
    
    ![修改應用程式](images/modify-application.png)
    
15.  如步驟 10、11 與 12 所述，重建、執行與執行應用程式。
    
16.  在您啟動伺服器的終端機中查看伺服器輸出。請注意，執行緒的名稱是 helidon-server-n。這是 Helidon 建立之執行緒集區中的傳統平台執行緒，用來處理 JAX-RS 要求。您的伺服器輸出將與下列類似：
    
        Running on thread Thread[#25,helidon-server-1,5,server]
        
17.  在執行 "java -jar target/myproject.jar" 命令的終端機中輸入 `Ctrl + C`，即可停止 _myproject_ 應用程式。
    

## 作業 2：將 Helidom MicroProfile 應用程式移轉至 Helidon 4

1.  若為 myproject，請開啟 _pom.xml_ 檔案，並將父系 pom 從 _3.1.1_ 變更為 _4.0.0-ALPHA5_ 。
    
        <parent>
            <groupId>io.helidon.applications</groupId>
            <artifactId>helidon-mp</artifactId>
            <version>4.0.0-ALPHA5</version>
            <relativePath/>
        </parent>
        
    
    ![POM 版本](images/pom-version.png)
    
2.  編輯 src/main/resources/logging.properties，並將 _io.helidon.common.HelidonConsoleHandler_ 變更為 _io.helidon.logging.jul.HelidonConsoleHandler_ 。 ![編輯紀錄](images/edit-logging.png)
    
3.  您的應用程式現在已經移轉至 Helidon 4！複製並貼上下列命令以建立應用程式。
    
        <copy>mvn clean package -DskipTests</copy>
        
4.  複製並貼上下列命令以執行應用程式。
    
        <copy>java --enable-preview  -jar target/myproject.jar</copy>
        
    
    您會有類似以下的執行結果。
    
        $ java --enable-preview  -jar target/myproject.jar
        2023.02.28 03:56:17 INFO io.helidon.logging.jul.JulProvider Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:56:21 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] http://0.0.0.0:8080 bound for socket '@default'
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] direct writes
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Started all channels in 19 milliseconds. 5131 milliseconds since JVM startup. Java 19.0.2+7-44
        2023.02.28 03:56:22 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 5144 milliseconds (since JVM startup).
        2023.02.28 03:56:22 INFO io.helidon.common.features.HelidonFeatures Thread[#28,features-thread,5,main]: Helidon MP 4.0.0-ALPHA5 features: [CDI, Config, Health, Metrics, Open API, Server, WebServer]
        
5.  查看伺服器輸出內容，請注意執行緒現在是 VirtualThread。
    
6.  在執行 "java --enable-preview -jar target/myproject.jar" 命令的終端機中輸入 `Ctrl + C`，停止 _myproject_ 應用程式。
    

恭喜，您已完成 Helidon 虛擬討論串工作坊。

## 確認

*   **作者** - Joe DiPol
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 2 月