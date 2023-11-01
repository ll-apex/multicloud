# 產生 Helidon MP 專案並在程式碼編輯器中執行專案

## 簡介

本實驗室會逐步介紹建立 Helidon MP 應用程式的步驟。

預估時間：15 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[在程式碼編輯器中產生 Helidon MP 專案並執行專案](videohub:1_22nv8v4q)

### 關於產品 / 技術

Helidon 的設計可簡單易用，提供工具和範例讓您快速上手。由於 Helidon 只是一個在快速的 Netty 核心上執行的程式庫集合，因此沒有額外的負荷或 bloat。Helidon 支援 MicroProfile，並提供熟悉的 API，例如 JAX-RS、CDI 和 JSON-P/B。我們的 MicroProfile 導入作業是在快速的 Helidon Reactive WebServer 上執行。反應 WebServer 提供現代化的功能程式設計模型，並在 Netty 上執行。Helidon WebServer 輕量、彈性和反應性，為您的微服務提供簡單易用的快速基礎。

Helidon 支援狀況檢查、度量、追蹤和容錯功能，可撰寫與 Prometheus、Jaeger/Zipkin 及 Kubernetes 整合的雲端就緒應用程式。

### 關於 Helidon Project Starter

Project Starter 是用於建立 Helidon 專案的新 Web UI。它提供高度客製化的各種選項，可讓使用者選取想要新增至專案的 Helidon 功能。一般使用者可根據其特定需求產生專案。如需詳細資訊，請按一下 [Helidon Starter](https://helidon.io/starter) 。

### 關於程式碼編輯器

「程式碼編輯器」可讓您直接從 OCI 主控台編輯及部署各種 OCI 服務的程式碼。您現在可以更新服務工作流程和程序檔，而不需要在主控台和本機開發環境之間切換。這樣一來，您便可以輕鬆快速地建立雲端解決方案的原型、探索新的服務，以及完成快速的編碼作業。

程式碼編輯器與 Cloud Shell 的直接整合可讓您存取已預先安裝在 Cloud Shell 中的 GraalVM Enterprise Native Image 和 JDK 17 (Java Development Kit)。

### 關於 OCI Cloud Shell

[OCI Cloud Shell](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm) 是以瀏覽器為基礎的終端機，可從 Oracle Cloud 主控台存取。它除了提供對 Linux Shell 的存取 (含預先認證的 OCI 命令行介面 (CLI) 和預先安裝的開發人員工具之外，還具備 5GB 儲存空間。

從版本 22.2.0 開始，GraalVM Enterprise JDK 17 和原生映像檔已預先安裝在 Cloud Shell 中。

> Oracle Cloud Infrastructure 提供 GraalVM 企業版，無須額外付費。

### 目標

*   建立名為 Helidon Greeting 應用程式的 MicroProfile 支援微服務
*   在程式碼編輯器中開啟 Helidon 應用程式
*   變更雲端 Shell 中的預設 JDK
*   設定必要的 Maven
*   執行與執行 Helidon Greeting 應用程式

### 先決條件

*   必須要有已啟用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的帳戶。

## 任務 1：使用專案起始者產生 Helidon 專案

1.  複製下方 URL 並將其貼至瀏覽器，以開啟「Helidon 專案」頁面。
    
        <copy>https://helidon.io/starter/</copy>
        
2.  在「產生您的專案」下方，選取 _Helidon MP_ 作為 Helidon Flavor，然後按_下一步_。
    
3.  針對「應用程式類型」，選取_快速啟動_，然後按_下一步_。
    
4.  若為「媒體支援」，請選取 _JSON-B_ ，然後按一下_下一步_。
    
5.  針對「自訂專案」，選取預設值並按一下_下載_。這將會彈出在視窗中，將此 _myproject.zip_ 儲存到您選擇的位置。在本研討會的其餘部分，將使用 myproject 名稱。如果您選擇其他名稱，請分別變更。
    

## 工作 2：在本地端建立並執行 helidon 專案

1.  在雲端主控台中，按一下_開發人員工具_圖示，然後按一下_程式碼編輯器_。 ![碼編輯器](images/code-editor.png)
    
2.  在「程式碼編輯器」中，按一下_終端機_ -> _新終端機_。 ![開放終端](images/open-terminal.png)
    
3.  在終端機中複製並貼上下列指令，以建立 _myproject_ 資料夾，我們將在其中下載預設的 myproject.zip 檔案。
    
        <copy>mkdir -p ~/helidon-project/myproject</copy>
        
4.  在「程式碼編輯器 (Code Editor)」中，按一下_檔案 (File)_ \-> _開啟 (Open)_ 。 ![開啟資料夾](images/open-folder.png)
    
5.  選取 _helidon-project_ 資料夾，然後按一下_開啟_。 ![開啟 Helidon](images/open-helidon.png)
    
6.  在「程式碼編輯器 (Code Editor)」的 EXPLORER 視窗中，您可以看到您的 _HELIDON-PROJECT_ 。您可以看到 _myproject_ 資料夾，請按一下此處。現在按一下_檔案_ -> _上傳檔案。。_，然後指定您下載專案的位置，並選取壓縮檔案，然後按一下_開啟_。![穿孔](images/myproject.png) ![上傳檔案](images/upload-files.png)
    
7.  複製並貼上下列指令以解壓縮 _myproject.zip_ 檔案。
    
        <copy>cd ~/helidon-project/myproject
        unzip myproject.zip
        </copy>
        
8.  展開 _myproject_ 資料夾，以檢視專案結構。 ![展開專案](images/expand-project.png)
    
9.  若要執行此專案，請使用 Maven 3.8+ 和 JDK 17+。在 Oracle 雲端，您提供了各種 JDK。我們會在這裡選取 GraalVM JDK。在終端機中複製並貼上下列命令，以瞭解您的預設 JDK。
    
        <copy>csruntimectl java list</copy>
        
    
    ![列出 JDK](images/list-jdk.png)
    
    > 開頭的 JDK 包含 \* _asterisk_ 是您的預設 JDK。如果您有其他 JDK，則可以使用下列命令來變更預設的 JDK 版本。請使用顯示版本的 graalvmeejdk，因為它可能會與指令中顯示的不同。
    
        <copy>csruntimectl java set graalvmeejdk-17</copy>
        
10.  若要設定必要的 maven，請在終端機中複製並貼上下列命令。
    
        <copy>cd ~/helidon-project/myproject/
        wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz
        tar -xzvf apache-maven-3.8.8-bin.tar.gz
        PATH=~/helidon-project/myproject/apache-maven-3.8.8/bin:$PATH</copy>
        
    
    ![設定 maven](images/configure-maven.png)
    
11.  若要確認您的 JDK 和 Maven 版本正確，如下所示，請在終端機中執行下列命令。
    
        <copy>mvn -v</copy>
        
    
    ![確認先決條件](images/verify-prerequisite.png)
    
12.  從 _myproject_ 資料夾執行下列命令來建立專案。
    
        <copy>cd ~/helidon-project/myproject/myproject
        mvn clean package</copy>
        
    
    ![建立專案](images/build-project.png)
    
    > 您應該會在執行此命令的結束時看到 _BUILD SUCCESS_ 。
    
13.  將下列指令複製並貼到終端機中，以執行此應用程式。您會看到和以下螢幕擷取畫面類似的執行結果。
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    ![運行專案](images/run-project.png)
    

> 請注意，開始時間為 4822 毫秒。系統會在稍後比較此時間與原生映像檔可執行檔。

14.  在「程式碼編輯器 (Code Editor)」中開啟新的終端機 / 主控台，然後執行下列命令來檢查應用程式：
    
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
        
15.  _在執行 "java -jar target/myproject.jar" 命令的終端機中輸入 `Ctrl + C`，以停止 **myproject** 應用程式_。太多，您還會發放「標籤」中的元素。
    

## 確認

*   **作者** - Dmitry Aleksandrov
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 4 月