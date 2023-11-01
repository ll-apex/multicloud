# 設定環境

## 簡介

本實驗室會逐步介紹設定研討會所需環境的步驟。

[Lab1 逐步解說](videohub:1_far2bboa)

預估時間：10 分鐘

### 關於程式碼編輯器

「程式碼編輯器」可讓您直接從 OCI 主控台編輯及部署各種 OCI 服務的程式碼。您現在可以更新服務工作流程和程序檔，而不需要在主控台和本機開發環境之間切換。這樣一來，您便可以輕鬆快速地建立雲端解決方案的原型、探索新的服務，以及完成快速的編碼作業。

程式碼編輯器與 Cloud Shell 的直接整合可讓您存取已預先安裝在 Cloud Shell 中的 GraalVM Enterprise Native Image 和 JDK 17 (Java Development Kit)。

### 目標

*   設定程式碼編輯器
*   下載必要的 Maven 和 JDK 版本
*   下載 Helidon 原始程式碼

### 先決條件

*   必須要有已啟用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的帳戶。
*   您必須使用 Firefox 作為瀏覽器，才能開啟程式碼編輯器。

## 作業 1：設定程式碼編輯器

1.  在雲端主控台中，按一下顯示的「程式碼編輯器」圖示。 ![程式碼編輯器](images/code-editor.png)
    
2.  在「程式碼編輯器」中，按一下「終端機」→「新終端機」。 ![開啟終端機](images/open-terminal.png)
    

## 作業 2：下載必要的 Maven 和 JDK 版本

1.  複製下列指令並將其貼入終端機。它會下載必要的 JDK 和 Maven 版本。
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/archive/jdk-19.0.2_linux-x64_bin.tar.gz
        tar -xzvf jdk-19.0.2_linux-x64_bin.tar.gz</copy>
        

## 任務 3：下載 Helidon 原始碼

1.  複製下列指令並貼到終端機以下載 helidon 應用程式的原始碼。
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  複製並貼上下列命令來解壓縮 _helidon-levelup-2023-main.zip_ 。
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

您現在可以_進入實驗室 2_ 。

## 確認

*   **作者** - Joe DiPol
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 2 月