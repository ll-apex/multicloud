# 建置並執行 Helidon Nima 和 Reactive 應用程式

## 簡介

這個實驗室會逐步引導您完成在 Oracle Cloud Infrastructure 的 Oracle Code 編輯器中建置及執行 Nima 和 Reactive Helidon 應用系統的程序。

[Lab2 逐步解說](videohub:1_e88ydqwt)

預估時間：15 分鐘

### 目標

在此實驗室中，您將：

*   建置、執行及測試 Helidon Nima 應用程式
*   建立、執行和測試 Helidon Reactive 應用程式
*   分析與反應應用程式比較的 Nima 應用程式簡單性

### 先決條件

若要執行此實驗室，您必須具備：

*   Oracle Cloud 帳戶
*   完成的實驗室 1，安裝必要的 JDK 和 maven。

## 作業 1：建置及執行 Nima 應用程式

1.  在「程式碼編輯器 (Code Editor)」中按一下_檔案 (File)_ \-> _開啟 (Open)_ 。 ![開啟專案](images/open-project.png)
    
2.  選取 _helidon-levelup-2023-main_ 資料夾，然後按一下_開啟_。 ![Helidon 專案](images/helidon-project.png)
    
    > 請忽略警告 / 問題，開啟此資料夾時，「程式碼編輯器」中會通知您。
    
3.  開啟新的終端機，然後複製並貼上下列命令來設定 PATH 與 JAVA\_HOME 變數。
    
        <copy>PATH=~/jdk-19.0.2/bin:~/apache-maven-3.8.3/bin:$PATH
        JAVA_HOME=~/jdk-19.0.2</copy>
        
4.  複製以下命令並在終端機中執行，確認是否已正確設定必要的 JDK 和 Maven 版本。
    
        <copy>mvn -v</copy>
        
    
    執行結果的方式如下：
    
        $ mvn -v 
        Apache Maven 3.8.3 (ff8e977a158738155dc465c6a97ffaf31982d739)
        Maven home: /home/ankit_x_pa/apache-maven-3.8.3
        Java version: 19.0.2, vendor: Oracle Corporation, runtime: /home/ankit_x_pa/jdk-19.0.2
        Default locale: en_US, platform encoding: UTF-8
        OS name: "linux", version: "4.14.35-2047.520.3.1.el7uek.x86_64", arch: "amd64", family: "unix"
        $ 
        
    
    > _您將只使用此終端機來建置和執行應用程式，因為它具備必要的 JDK 和 Maven 版本。_
    
5.  複製並貼上下列命令以建立 nima 應用程式。
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    您會有類似以下的執行結果：
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-blocking ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/nima/target/example-nima-blocking.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  7.562 s
        [INFO] Finished at: 2023-02-27T11:42:52Z
        [INFO] ------------------------------------------------------------------------
        
6.  在終端機中複製並貼上下列命令，以執行阻隔的 nima 版本應用程式：
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    您會有類似以下的執行結果：
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.27 11:43:18.589 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:43:18.902 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:43:19.700 [0x314d2373] http://127.0.0.1:33193 bound for socket '@default'
        2023.02.27 11:43:19.703 [0x314d2373] direct writes
        2023.02.27 11:43:19.722 Helidon Níma 4.0.0-ALPHA5
        
7.  將伺服器執行的連接埠號碼記下來 (請參閱 @default 的日誌項目)。例如，在我們的輸出中，連接埠號碼為 33193。同樣地，瞭解您的伺服器連接埠號碼。
    
8.  若要開啟新終端機，請按一下 \[ _終端機_ \] -> \[ _新終端機_ \]。我們將使用此終端機執行 _curl_ 指令。 ![新終端機](images/new-terminal.png)
    
9.  將下列指令複製並貼到新終端機中。別忘記將 _`<port>`_ 取代為您的伺服器連接埠。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    您會有類似以下的執行結果：
    
        curl http://localhost:33193/one
        remote_1
        $
        
10.  將下列指令複製並貼到新終端機中。別忘記將伺服器連接埠取代為 _`<port>`_ 。
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    您會有類似以下的執行結果：
    
        $ curl http://localhost:33193/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > 請注意結果的順序。依名稱建議，此要求會依序呼叫遠端資源多次。
    
11.  將下列指令複製並貼到新終端機中。別忘記將伺服器連接埠取代為 _`<port>`_ 。
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    您會有類似以下的執行結果：
    
        $ curl http://localhost:33193/parallel
        Combined results: [remote_21, remote_18, remote_12, remote_13, remote_14, remote_15, remote_16, remote_17, remote_19, remote_20]
        $
        
    
    > 請注意結果的順序。依名稱建議，此要求會平行呼叫遠端資源多次。
    
12.  在執行 \*java -jar \* 命令的終端機中按 _Ctrl + C_ 以停止伺服器。
    

## 作業 2：建置和執行反應應用程式

1.  複製並貼上下列命令以建立 nima 應用程式。
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    您會有類似以下的執行結果：
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-reactive ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/reactive/target/example-nima-reactive.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  6.196 s
        [INFO] Finished at: 2023-02-27T11:51:17Z
        [INFO] ------------------------------------------------------------------------
        $
        
2.  在終端機中複製並貼上下列命令，以執行應用程式的反應版本：
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    您會有類似以下的執行結果：
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.27 11:51:26.227 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:51:26.723 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:51:27.286 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.27 11:51:27.618 Channel '@default' started: [id: 0x53fadd43, L:/0.0.0.0:45765]
        
3.  將伺服器執行的連接埠號碼記下來 (請參閱 @default 的日誌項目)。例如，在我們的輸出中，連接埠號碼為 45765。同樣地，瞭解您的伺服器連接埠號碼。
    
4.  回到終端機，我們會在 Task 1 中開啟以執行 curl 命令。
    
5.  將下列指令複製並貼到新終端機中。別忘記將伺服器連接埠取代為 _`<port>`_ 。
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    您會有類似以下的執行結果：
    
        $ curl http://localhost:45765/one
        remote_1
        $
        
6.  將下列指令複製並貼到新終端機中。別忘記將伺服器連接埠取代為 _`<port>`_ 。
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    您會有類似以下的執行結果：
    
        $ curl http://localhost:45765/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > 請注意結果的順序。依名稱建議，此要求會依序呼叫遠端資源多次。
    
7.  將下列指令複製並貼到新終端機中。別忘記將伺服器連接埠取代為 _`<port>`_ 。
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    您會有類似以下的執行結果：
    
        $ curl http://localhost:45765/parallel
        Combined results: [remote_17, remote_16, remote_13, remote_20, remote_12, remote_19, remote_18, remote_14, remote_21, remote_15]
        $
        
    
    > 請注意結果的順序。依名稱建議，此要求會平行呼叫遠端資源多次。
    
8.  在執行 \*java -jar \* 命令的終端機中按 _Ctrl + C_ 以停止伺服器。
    

## 任務 3：分析 Nima 應用程式的簡潔性

**封鎖 vs. 反應**

讓我們比較相同任務 Níma (阻隔) 與 Helidon SE (反應) 之間的實作。

*   這兩個實作都使用 Helidon WebClient 執行 REST 呼叫
*   封鎖實行很簡單，如下所示：
    *   執行流程會依程式碼中的敘述句順序反映
    *   沒有呼叫可增強或語意豐富的磁帶櫃呼叫
    *   除錯很簡單
*   反應式版本需要清楚瞭解反應式磁帶櫃
    *   包括 Multi、flatMap、錯誤處理等。
    *   由於使用反應式處理程式，控制流程不再明顯
    *   瞭解 FlatMap 的啟用 / 禁止並行功能是必要的
    *   除錯變得更困難

1.  開啟 _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ 檔案，瞭解如何在 nima 版本的應用程式中實行端點。 ![Nima Block](images/nima-block.png)
    
2.  開啟 _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ 檔案，查看如何在應用程式的反應版本中實行端點。 ![反應式服務](images/reactive-service.png)
    
3.  在 _OPEN EDITORS_ 區段中，在 _BlockingService.java_ 檔案上按一下滑鼠右鍵，然後選取_選取以進行比較_。 ![選取比較](images/select-compare.png)
    
4.  在 _ReactiveService.java_ 檔案上按一下滑鼠右鍵，然後選取_與選取的比較_。 ![比較選取的項目](images/compare-selected.png)
    
5.  看反應代碼比封鎖 (虛擬執行緒) 更複雜 ![比較代碼](images/compare-code.png)
    
    > 分別檢查 BlockingService 和 ReactiveService 中的方法 1、序列和平行。看看您是否了解他們如何運作！
    

您現在可以_進入下一個實驗室_。

## 確認

*   **作者** - Joe DiPol
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 2 月