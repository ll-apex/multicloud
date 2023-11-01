# 設定實驗室環境

## 簡介

在此實驗室中，您將建立設定了所有必要網路資源的 3 節點 Kubernetes 叢集。此外，您也可以在 Oracle Cloud Container Image Registry 中建立儲存區域。接著，您將會產生認證權杖。此外，您將接受 Oracle Container Registry 中的 WebLogic 伺服器映像檔授權合約。

預估時間：15 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[設定實驗室環境](videohub:1_zhvohpqq)

### 目標

在此實驗室中，您將：

*   建立 Oracle Kubernetes 叢集。
*   在 Oracle Cloud Container Image Registry 中建立儲存區域。
*   產生認證權杖。
*   接受 Oracle Container Registry 中 WebLogic 伺服器映像檔的授權。

### 先決條件

*   必須要有已啟用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的帳戶。
*   您必須擁有 Oracle 帳戶。
*   您應該使用文字編輯器。

## 工作 1：建立 Oracle Kubernetes 叢集

_快速建立_功能使用預設設定值，視需要使用新的網路資源來建立_快速叢集_。此方式是建立新叢集的最快速方式。如果您接受所有預設值，只需按幾下滑鼠，即可建立新叢集。叢集的新網路資源會自動建立，以及節點集區和三個工作節點。

1.  在主控台中，選取_漢堡功能表 -> 開發人員服務 -> Kubernetes 叢集 (OKE)_ ，如圖所示。
    
    ![漢堡功能表](images/hamburger-menu.png " ")
    
2.  在「叢集清單」頁面中，選取「區間」，然後按一下_建立叢集_。
    
    > 您必須選取允許您建立叢集的區間，以及 Oracle Container Registry 中的儲存區域。
    
    ![選取區間](images/select-compartment.png " ")
    
3.  在「建立叢集解決方案」對話方塊中，選取_快速建立_，然後按一下_提交_。
    
    ![啟動工作流程](images/launch-workflow.png " ")
    
    _快速建立_將會使用預設設定值建立新的叢集，以及新叢集的新網路資源。
    
    在「叢集建立」頁面上指定下列組態詳細資訊 (請特別注意您在_資源配置_欄位中放置的值)：
    
    *   **名稱**：叢集的名稱。保留預設值。
    *   **區間**：區間的名稱。選取允許您建立資源的區間。
    *   **Kubernetes 版本**：Kubernetes 的版本。選取 _1.26.2_ 作為 Kubernetes 版本。
    *   **Kubernetes API 端點**：叢集主要節點是否可路由。選取_公用端點_值。
    *   **節點類型**：選取_受管理_作為節點類型。
    *   **Kubernetes 工作節點**：叢集工作節點是否可遞送。保留預設的 _Private Workers_ 值。
    *   **資源配置**：用於節點集區中每個節點的資源配置。資源配置決定了 CPU 的數目，以及配置給每個節點的記憶體大小。此清單只顯示您租用戶中 OKE 支援的資源配置。選取 _VM.Standard.E4。Flex_ (通常可在 Oracle 免費層帳戶中使用)。選取 1 個 OCPU 和 16 GB 作為記憶體大小。
    *   **映像檔**：選取 Kubernetes 版本為 _1.26.2_ 的預設映像檔。
    *   **節點數目**：要建立的工作節點數目。保留預設值 _3_ 。
    
    ![快速叢集](images/quick-cluster1.png " ") ![輸入資料](images/enter-data.png " ")
    
4.  按一下_下一步_，複查為新叢集輸入的詳細資料。
    
5.  在_複查_頁面中，選取_建立基本叢集_的核取方塊，然後按一下_建立叢集_以建立新的網路資源和新的叢集。
    
    ![複查叢集](images/review-cluster.png " ")
    
    > 您會看到已經為您建立的網路資源。等到起始建立節點集區的要求，然後按一下_關閉_。
    
    ![網路資源](images/network-resource.png " ")
    
    > 接著，新的叢集會顯示在_叢集詳細資訊_頁面上。建立主要節點時，新的叢集會取得_作用中_狀態 (大約需要 7 分鐘)。您不需要等待，請繼續下一個任務。
    
    ![叢集存取](images/cluster-access.png " ")
    

## 作業 2：建立儲存區域

在這個任務中，您會建立公用儲存庫。在實驗室 5 中，我們將推送輔助映像檔至此儲存區域。

1.  在主控台中，選取 \[ _漢堡功能表_ \] -> \[ _開發人員服務_ \] -> \[ _容器登錄_ \]，如下所示。 ![容器登錄](images/container-registry.png)
    
2.  選取允許您建立儲存區域的區間。按一下_建立儲存區域_。 ![建立儲存區域](images/create-repository.png)
    
3.  輸入 _`test-model-your_firstname`_ 作為「儲存區域」名稱和「以_公用_身分存取」，然後按一下_建立_。 ![儲存區域詳細資訊](images/repository-details.png)
    
4.  儲存區域準備就緒之後。請注意文字編輯器內文字檔中的租用戶命名空間。 ![注意：租用戶 NameSpace](images/tenancy-namespace.png)
    

## 作業 3：產生認證記號

在此作業中，將會產生_認證權杖_。在實驗室 5 中，我們將使用此認證權杖將輔助映像檔推送至 Oracle Cloud Container Registry 儲存區域。

1.  選取右上角的「使用者圖示」，然後選取 _MyProfile_ 。
    
    ![我的設定檔](images/my-profile.png)
    
2.  向下捲動並選取_認證權杖_，然後按一下_產生權杖_。
    
    ![認證權杖](images/auth-token.png)
    
3.  複製 _`test-model-your_first_name`_ 並將其貼到_描述_方塊中，然後按一下_產生記號_。
    
    ![建立權杖](images/create-token.png)
    
4.  在「產生的記號」下選取_複製_，然後將其貼到您的文字編輯器中。我們無法稍後再複製。按一下_關閉 (Close)_ 。
    
    ![複製權杖](images/copy-token.png)
    

## 作業 4：接受 WebLogic 伺服器映像檔的授權

在這項任務中，我們接受 WebLogic 伺服器映像檔位於 Oracle Container Registry 中的授權合約。在實驗室 3 中，我們將使用 WebLogic Server 12.2.1.3.0 映像檔作為我們的主要映像檔。因此，若要存取 WebLogic 伺服器映像檔，我們接受授權合約。

1.  按一下 Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) 的連結並登入。您需要有 Oracle 帳戶。 ![容器登錄登入](images/container-registry-sign-in.png)
    
2.  在「使用者名稱 (Username)」與「密碼 (Password)」欄位中輸入您的 _Oracle Account Credentials_ ，然後按一下_登入 (Sign In)_ 。 ![登入容器登錄](images/login-container-registry.png)
    
3.  在 Oracle Container Registry 的首頁中，搜尋 _weblogic_ 。 ![搜尋 WebLogic](images/search-weblogic.png)
    
4.  按一下顯示的 _weblogic_ ，然後選取_英文_作為語言，然後按一下_繼續_。![按一下 WebLogic](images/click-weblogic.png) ![選取語言](images/select-language.png)
    
5.  按一下_接受_接受授權合約。 ![接受授權](images/accept-license.png)
    

## 進一步瞭解

_關於 Oracle Cloud Infrastructure Container Engine for Kubernetes_

Oracle Cloud Infrastructure Container Engine for Kubernetes 是一項完全託管、可擴展且高可用性的服務，可用來將容器應用系統部署到雲端。當您的開發團隊想要可靠地建置、部署及管理雲端原生應用程式時，請使用 Kubernetes 容器引擎 (有時縮寫為 OKE)。您可以指定應用系統所需的運算資源，然後 OKE 在現有 OCI 租用戶的 Oracle Cloud Infrastructure 上佈建運算資源。

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月