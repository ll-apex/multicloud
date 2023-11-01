# 修改 Bobby 的書本應用程式，並建立新的應用程式元件影像

## 簡介

在此實驗室中，我們將透過 _Cloud Shell_ 修改 bobbys-helidon-stock-application。之後，我們將為 bobbys-helidon-stock-application 建立新的 Docker 映像檔。此 bobbys-helidon-stock-application 影像是 Bobby 之 Books 應用程式的元件。

預估時間：10 分鐘

### 目標

在此實驗室中，您將：

*   修改 bobbys-helidon-stock-application。
*   為 bobbys-helidon-stock-application 建立新的 Docker 映像檔。

### 先決條件

您應該有一個文字編輯器，您可以在其中根據您的環境貼上指令和 URL 並加以修改。接著您可以複製並貼上修改的指令，以便在 _Cloud Shell_ 中執行這些指令。

## 工作 1：修改 bobbys-helidon-stock-application

1.  選取 Bobby 的 \[Book\] (書本) 頁籤，按一下 \[ _書籍_ \]，然後按一下 _Hobbit_ 手冊的影像，如下所示：
    
    ![點擊書籍](images/clickbooks.png " ")
    
    它會以 _Hobbit_ 格式顯示工作簿名稱，如影像所示。
    
    ![霍比特](images/thehobbit.png " ")
    
2.  我們希望將書本名稱轉換成大寫字母 (THE HOBBIT)。我們需要下載 Bobby 之 Books 應用程式的原始碼。請確認您位於家目錄。複製下列命令並將其貼到 _Cloud Shell_ 中。
    
        <copy>cd ~
        git clone -b v0.16.0 https://github.com/verrazzano/examples.git</copy>
        
    
    ![複製儲存區域](images/clonerepository.png " ")
    
3.  若要檢視 Bobby 之 Book 應用程式內的檔案，請複製下列指令並將其貼到 _Cloud Shell_ 中。
    
        <copy>ls -la ~/examples/bobs-books/</copy>
        
    
    執行結果應該和下列類似：`bash $ ls -la ~/examples/bobs-books/ total 16 drwxr-xr-x. 5 ankit_x_pa oci 100 Mar 21 12:14 . drwxr-xr-x. 7 ankit_x_pa oci 4096 Mar 21 12:14 .. drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobbys-books drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobs-bookstore-order-manager -rw-r--r--. 1 ankit_x_pa oci 942 Mar 21 12:14 README.md drwxr-xr-x. 4 ankit_x_pa oci 72 Mar 21 12:14 roberts-books $`
    
4.  現在，我們將變更相關的 JAVA\_FILE。若要開啟此檔案，請複製下列命令並將它貼到 _Cloud Shell_ 中。
    
        <copy>vi ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/src/main/java/org/books/bobby/BookResource.java</copy>
        
    
    ![開啟檔案](images/openfile.png " ")
    
5.  按 _i_ ，即可修改程式碼。如果要在行號 84 新增行，請複製下列行並將其貼到行號 84，如圖所示：
    
        <copy>optional.get().setTitle(optional.get().getTitle().toUpperCase());</copy>
        
    
    ![插入明細行](images/insertline.png " ")
    
6.  按 _Esc_ ，然後按 _：wq_ 以儲存變更。
    
    ![儲存變更](images/savechanges.png " ")
    

## 作業 2：為 bobbys-helidon-stock-application 建立新的 Docker 映像檔

1.  由於我們將為 _bobbys-helidon-stock-application_ 建立一個新的 Docker 映像檔，此映像檔相依於 _bobbys-coherence-application_ ，因此我們會執行 _Maven_ 命令來清除現有的 bobbys-coherence-application 存檔並編譯、建置、封裝以及在本機 Maven 儲存區域中安裝新的 bobby-coherence-application 存檔。若要變更至 _bobbys-coherence_ 目錄資料夾，並編譯、建置以及安裝 _bobbys-coherence_ 應用程式存檔至本機 Maven 儲存區域，請複製下列命令並將它貼到 _Cloud Shell_ 中。
    
        <copy>
        cd ~/examples/bobs-books/bobbys-books/bobbys-coherence/
        mvn clean install
        </copy>
        
    
    產生的輸出與下列類似 (縮寫為僅顯示輸出的開始與結束)：`bash $ mvn clean install [INFO] Scanning for projects... [INFO] [INFO] ------------< io.verrazzano.example.books:bobbys-coherence >------------ [INFO] Building bobbys-coherence 1.0-SNAPSHOT [INFO] --------------------------------[ jar ]--------------------------------- [INFO] [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ bobbys-coherence --- [[INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/target/bobbys-coherence.jar to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.jar [INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/pom.xml to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.pom [INFO] ------------------------------------------------------------------------ [INFO] BUILD SUCCESS [INFO] ------------------------------------------------------------------------ [INFO] Total time: 8.887 s [INFO] Finished at: 2023-03-22T04:09:11Z [INFO] ------------------------------------------------------------------------ $`
    
2.  由於我們修改了 _bobbys-helidon-stock-application_ ，因此需要編譯、建置以及封裝此應用程式。若要變更為 _bobbys-helidon-stock-application_ 目錄，並將 _bobbys-helidon-stock-application_ 封裝成 JAR 檔案，請複製下列命令並將其貼到 _Cloud Shell_ 中。在第二個影像中，您可以在 `~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target` 看到 `bobbys-helidon-stock-application.jar` 檔案的建立。
    
        <copy>cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/
        mvn clean package
        </copy>
        
    
    The resulting output is similar to the following (abbreviated to show only the start and end of output): \`\`\`bash $ cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/ $ mvn clean package \[INFO\] Scanning for projects... \[INFO\] ------------------------------------------------------------------------ \[INFO\] Detecting the operating system and CPU architecture \[INFO\] ------------------------------------------------------------------------
    
         [INFO] --- maven-jar-plugin:2.5:jar (default-jar) @ bobbys-helidon-stock-application ---
         [INFO] Building jar: /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target/bobbys-helidon-stock-application.jar
         [INFO] ------------------------------------------------------------------------
         [INFO] BUILD SUCCESS
         [INFO] ------------------------------------------------------------------------
         [INFO] Total time:  10.106 s
         [INFO] Finished at: 2023-03-22T04:11:38Z
         [INFO] ------------------------------------------------------------------------
         $
         ```
        
3.  我們將建立一個用於 bobby-stock-helidon-application 的 Docker 映像檔，但這個應用程式會使用特定版本的 JDK，而且我們不想變更建置新映像檔的 Docker 檔案。所以，我們會下載必要的 JDK。若要下載所需的 JDK 版本，請複製下方命令，然後貼到 _Cloud Shell_ 中。
    
        <copy>wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2_linux-x64_bin.tar.gz</copy>
        
    
    輸出應與下列類似：\`\`\`bash $ wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz --2023-03-22 04:20:16-- https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz 解析 download.java.net (download.java.net) ... 2.23.220.73 連線至 download.java.net (download.java.net) |2.23.220.73|：443... 連線。已傳送 HTTP 要求，等待回應 ... 200 OK 長度：198606200 (189M) \[application/x-gzip\] 儲存至：'openjdk-14.0.2\_linux-x64\_bin.tar.gz'
    
         100%[==============================================================================================================================>] 198,606,200  194MB/s   in 1.0s   
        
         2023-03-22 04:20:17 (194 MB/s) - ‘openjdk-14.0.2_linux-x64_bin.tar.gz’ saved [198606200/198606200]
         $
         ```
        
4.  我們正在建立一個 Docker 映像檔，上傳至實驗室 6 中的 Oracle Cloud Container Registry。若要建立檔案名稱，需要以下資訊：
    
    *   租用戶命名空間
    *   區域的端點
5.  若要尋找租用戶的命名空間，請按一下_使用者_圖示 -> _租用戶_ (如圖所示) . 在**物件儲存設定值**中，您會發現命名空間。複製並儲存到文字檔中的某處，因為我們還會在實驗室 6 中使用它。
    
    ![複製租用戶命名空間](images/copy-tenancynamespace.png " ")
    
6.  尋找_您區域的端點_。請參閱位於此 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 的表格。在顯示範例中，區域的端點是 _UK South (London)_ (作為區域名稱)，而其端點是 _lhr.ocir.io_ 。尋找您自己的_區域名稱_端點，然後將它儲存在文字檔中。您也需要這個實驗室才能上手。
    
    ![端點](images/end-point.png " ")
    
    > 您現在已同時具有區域的租用戶命名空間和端點。
    
7.  您現在已同時具有區域的租用戶命名空間和端點。複製下列命令並將它貼到您的文字編輯器中。然後將 **`END_POINT_OF_YOUR_REGION`** 取代為您區域名稱的端點，將 **`NAMESPACE_OF_YOUR_TENANCY`** 取代為租用戶的命名空間，並以小寫字母為您的名字 **`your_first_name`** 。
    
        <copy>docker build --force-rm=true -f Dockerfile -t END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0 .</copy>
        
    
    ![Docker 組建](images/docker-build.png " ")
    
    ![已建立影像](images/image-created.png " ")
    
    > 例如，在我的情況下，命令是 `docker build --force-rm=true -f Dockerfile -t lhr.ocir.io/tenancynamespace/helidon-stock-application-ankit`。
    
    這會建立 Docker 映像檔，我們將推送至實驗室 6 的 Oracle Cloud Container Registry 儲存區域。您必須在文字 editor.In Lab 6 中複製取代的完整影像名稱 **`END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0`** ，當您需要建立儲存庫時，必須提供名稱為 **`helidon-stock-application-your_first_name`** 。
    
    讓 _Cloud Shell_ 保持開放，我們需要下個實驗室才能使用。
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月