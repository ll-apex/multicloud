# 在程式碼編輯器中修改 Helidon MP 應用程式

## 簡介

在此實驗室中，您可以將自訂端點新增至「程式碼編輯器」中的 JAVA 類別。

預估時間：10 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[在程式碼編輯器中修改 Helidon MP 應用程式](videohub:1_sv1iug41)

### 目標

在此實驗室中，您將：

*   在 Java 類別中新增自訂端點
*   組建及執行修改過的應用程式

### 先決條件

*   必須要有已啟用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的帳戶。

## 作業 1：新增自訂端點

1.  在「程式碼編輯器」中，按一下 _GreetResource.java_ 檔案以開啟檔案。 ![開啟檔案](images/open-file.png)
    
2.  正如您可能會在程式碼中看到的，它完全是以 MicroProfile 為基礎。這表示所有功能皆使用 POJO 和註解達成。這些註釋是「標準」的，因此可跨不同供應商攜手合作。這表示您可以輕鬆地在其他支援相同版本 MicroProfile 的平台上執行程式碼。如需詳細資訊，請參閱[這個網頁](https://microprofile.io/)。
    
3.  建立一個可從一組五個標題隨機取得標題的新端點。若要建立此端點，請複製以下程式碼並貼上第 80 行，如下所示。
    
        <copy>@Path("/title")
        @GET
        @Produces(MediaType.APPLICATION_JSON)
        public String getRandomTitle() {
            return new String[] {"Mr. ", "Mrs.", "Miss", "Dr.", "Dr. sc."} [(int)(Math.random()*5)];
        }</copy>
        
    
    ![加碼](images/add-code.png)
    
4.  若要儲存檔案的內容，請在「程式碼編輯器」中，按一下_檔案 (File)_ \-> _儲存 (Save)_ 。
    
    > 「程式碼編輯器」中預設會啟用 AutoSave。
    

## 作業 2：執行應用程式

1.  將下列指令複製並貼至終端機以執行應用程式。
    
        <copy>mvn clean package
        java -jar target/myproject.jar</copy>
        
2.  開啟新的終端機 / 主控台並執行下列命令來檢查應用程式：
    
        <copy>curl -X GET http://localhost:8080/greet/title</copy>
        Mr.
        
    
    > 它會隨機提供 5 個選項中的標題，您可以多次執行此命令。
    
3.  _在執行 "java -jar target/myproject.jar" 命令的終端機中輸入 `Ctrl + C`，以停止 **myproject** 應用程式_。太多，您還會發放「標籤」中的元素。
    

## 確認

*   **作者** - Dmitry Aleksandrov
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 4 月