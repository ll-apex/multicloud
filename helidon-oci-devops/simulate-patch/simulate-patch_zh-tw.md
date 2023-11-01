# 模擬修補程式情境

## 簡介

此實驗室將嘗試模擬修補程式案例。在初始環境使用_開放式 JDK_ 來進行測試時，最後將它移到 _Oracle JDK_ 上供生產環境使用。以上組建與部署管線已經將 _Open JDK 20_ 設定為 Java 較適合使用。在本課堂練習中，將會取代為 _Oracle JDK 20_ 。

預估時間：10 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[模擬修補程式案例](videohub:1_4pecvhmf)

### 目標

在此實驗室中，您將：

*   修改 JDK 的我的最愛
*   驗證部署管線中的 JDK 新產品

### 先決條件

*   一個 Oracle Free Tier (Trial)、Paid 或 LiveLabs 雲端帳戶

## 作業 1：變更 JDK 安裝程式

1.  如**實驗室 2** 所見，在先前完成的**部署管線日誌**中，並觀察到接近使用**開啟 JDK** 的日誌底端。 ![開啟 jdk](images/open-jdk.png)
    
2.  在**程式碼編輯器**中，按一下檔案名稱 **`build_spec.yaml`** 以開啟檔案。將環境變數 **`JDK20_TAR_GZ_INSTALLER`** 標示為 **Open JDK** 安裝程式的 URL 路徑第 15 行，並註釋 **`JDK20_TAR_GZ_INSTALLER`** ，此變數會將 URL 路徑設定為第 #16 行的 **Oracle JDK** 安裝程式，如下所示。 ![修改 jdk](images/modify-jdk.png)
    

## 作業 2：推送變更並觸發 DevOps 管線

1.  在終端機中複製並貼上下列命令**以確認和推送**變更。
    
        <copy>git add .
        git status
        git commit -m "Replace OpenJDK 20 with OracleJDK 20"
        git push</copy>
        
    
    > 此 Git 推送將會觸發管線。
    

## 作業 3：確認部署管線中的 JDK 新口味

1.  在新頁籤中開啟[雲端主控台](https://cloud.oracle.com/)，按一下 _漢堡功能表_ -> _開發人員服務_ -> _專案_ (在 **DevOps** 下)。 ![devops 專案](images/devops-project.png)
    
2.  選取您在**實驗室 1** 中建立的區間，然後按一下 _devops-project-helidon-ocw-hol-string_ 以開啟 **DevOps 專案**。 ![選取區間](images/select-compartment.png)
    
3.  在**最新的組建歷史記錄**底下，您將看到**執行**和狀態為**已接受 / 進行中**。按一下下面所示的最新執行。 ![組建歷史記錄](images/build-history.png)
    
4.  組建管線**全部完成三個階段**之後，您將會見到輸出，如下所示。 ![組建執行優先](images/build-run-first.png)
    
5.  在「組建」執行進度中，在第三個階段按一下**三個點**，然後按一下**檢視部署**，如下所示。這會開啟部署管線。 ![檢視部署](images/view-deployment.png)
    
6.  您可以在此處查看**部署進度**。部署管線完成之後，您將會見到輸出，如下所示。 ![部署運行](images/deployment-run.png)
    
    > 這會順利將 Helidon 應用程式部署到 OCI 中的運算執行處理。
    
7.  若要檢視部署管線的日誌，請按一下接近部署階段的**三個點**，然後按一下**檢視詳細資訊**，如下所示。 ![檢視日誌](images/view-logs.png)
    
8.  向下捲動日誌並確認 JDK 的風格，應該是 **Oracle JDK** ，如下所示。 ![oracle jdk](images/oracle-jdk.png)
    

您現在可以**繼續進行下一個實驗室。**

## 確認

*   **作者** - Keith Lustria
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月