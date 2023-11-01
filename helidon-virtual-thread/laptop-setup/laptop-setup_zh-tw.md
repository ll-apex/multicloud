# 建立 Helidon MP 應用程式原生影像

## 簡介

在本實驗室中，您將瞭解如何在本機 linux 機器上設定環境。

預估時間：10 分鐘

### 目標

*   設定必要的 JDK 和 Maven。
*   下載應用程式來源代碼。

### 先決條件

*   您必須具有 IDE，才能修改、建置及執行專案。

## 作業 1：下載必要的 Maven 和 JDK 版本

1.  在 IDE 中，開啟終端機 / 主控台。
    
2.  複製下列指令並將其貼入終端機。它會下載必要的 JDK 和 Maven 版本，並設定 PATH 變數來使用必要的 Maven 和 JDK。
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/latest/jdk-19_linux-x64_bin.tar.gz
        tar -xzvf jdk-19_linux-x64_bin.tar.gz</copy>
        

## 任務 2：下載 Helidon 原始碼

1.  複製下列指令並貼到終端機以下載 helidon 應用程式的原始碼。
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  複製並貼上下列命令來解壓縮 _helidon-levelup-2023-main.zip_ 。
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

您現在可以_進入下一個實驗室_。

## 確認

*   **作者** - Joe DiPol
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 2 月