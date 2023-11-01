# 設定實驗室環境

## 簡介

在此實驗室中，您將在 Oracle Cloud Container Image Registry 中建立儲存區域。此外，您將接受 Oracle Container Registry 中的 WebLogic 伺服器映像檔授權合約。

預估時間：15 分鐘

### 目標

在此實驗室中，您將：

*   在 Oracle Cloud Container Image Registry 中建立儲存區域。
*   接受 Oracle Container Registry 中 WebLogic 伺服器映像檔的授權。

### 先決條件

*   必須要有已啟用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的帳戶。
*   您必須擁有 Oracle 帳戶。
*   您應該使用文字編輯器。

## 作業 1：建立儲存區域

在這個任務中，您會建立公用儲存庫。在實驗室 5 中，我們將推送輔助映像檔至此儲存區域。

1.  在遠端桌面上，若要開啟 Firefox，請點擊**活動**，在搜尋框中輸入 **Fire** ，然後點擊 **Firefox** 。 ![開放式防火牆](images/open-firefox.png)
    
2.  使用您的證明資料登入 [Oracle Cloud 主控台](https://cloud.oracle.com)。
    
3.  在主控台中，選取 \[ _漢堡功能表_ \] -> \[ _開發人員服務_ \] -> \[ _容器登錄_ \]，如下所示。 ![容器登錄](images/container-registry.png)
    
4.  選取允許您建立儲存區域的區間。按一下_建立儲存區域_。 ![建立儲存區域](images/create-repository.png)
    
5.  輸入 _`test-model-your_firstname`_ 作為「儲存區域」名稱和「以_公用_身分存取」，然後按一下_建立_。 ![儲存區域詳細資訊](images/repository-details.png)
    
6.  儲存區域準備就緒之後。請注意您的文字檔中的租用戶命名空間。 ![注意：租用戶 NameSpace](images/tenancy-namespace.png)
    

## 作業 2：接受 WebLogic 伺服器映像檔的授權

在這項任務中，我們接受 WebLogic 伺服器映像檔位於 Oracle Container Registry 中的授權合約。如同實驗室 5，我們將使用 WebLogic Server 12.2.1.3.0 映像檔作為我們的主要映像檔。因此，若要存取 WebLogic 伺服器映像檔，我們接受授權合約。

1.  在瀏覽器中複製並貼上 Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) 的連結，然後登入。您需要有 Oracle 帳戶。 ![容器登錄登入](images/container-registry-sign-in.png)
    
2.  在「使用者名稱 (Username)」與「密碼 (Password)」欄位中輸入您的 _Oracle Account Credentials_ ，然後按一下_登入 (Sign In)_ 。 ![登入容器登錄](images/login-container-registry.png)
    
3.  在 Oracle Container Registry 的首頁中，搜尋 _weblogic_ 。 ![搜尋 WebLogic](images/search-weblogic.png)
    
4.  按一下顯示的 _weblogic_ ，然後選取_英文_作為語言，然後按一下_繼續_。![按一下 WebLogic](images/click-weblogic.png) ![選取語言](images/select-language.png)
    
5.  按一下_接受_接受授權合約。 ![接受授權](images/accept-license.png)
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 10 月