# 設定運算執行處理

## 簡介

這個實驗室將為您展示如何設定 **Oracle WebLogic Suite for OKE BYOL** 堆疊，以產生進行研討會所需的 Oracle Cloud 物件。

預估時間：15 分鐘

### 目標

在此實驗室中，您將：

*   產生認證權杖。
*   在保存庫中建立加密密碼
*   建立堆疊：適用於 OKE BYOL 的 Oracle WebLogic Suite
*   連線至運算執行處理

### 先決條件

此實驗室假設您具有：

*   一個 Oracle Cloud 帳戶
*   您已產生 SSH 金鑰組。
*   您已完成：**實驗室：準備設定**

## 作業 1：產生認證權杖

在此作業中，將會產生_認證權杖_。在實驗室 5 中，我們將使用此認證權杖將輔助映像檔推送至 Oracle Cloud Container Registry 儲存區域。此外，我們也會使用此認證權杖將 WebLogic 碼頭映像檔提取至 Oracle Cloud Infrastructure Registry (亦稱為容器登錄)。

1.  選取右上角的「使用者圖示」，然後選取 _MyProfile_ 。
    
    ![我的設定檔](images/my-profile.png)
    
2.  向下捲動並選取_認證權杖_，然後按一下_產生權杖_。
    
    ![認證權杖](images/auth-token.png)
    
3.  複製 _`test-model-your_first_name`_ 並將其貼到_描述_方塊中，然後按一下_產生記號_。
    
    ![建立權杖](images/create-token.png)
    
4.  在「產生的記號」下選取_複製_，然後將其貼到您的文字檔中。我們無法稍後再複製。按一下_關閉 (Close)_ 。
    
    ![複製權杖](images/copy-token.png)
    
    > 在下一個作業中，系統會將此認證記號儲存在**加密**中。
    

## 作業 2：在保存庫中建立加密密碼

加密密碼是您與 Oracle Cloud Infrastructure 服務搭配使用的密碼、憑證、SSH 金鑰或**認證權杖**等證明資料。

1.  在 OCI 主控台中，按一下**漢堡功能表** -> **身分識別與安全性** -> **保存庫**。 ![選單庫](images/menu-vault.png)
    
2.  選取**清單範圍**底下的區間，然後按一下**建立保存庫**。
    
3.  輸入 Vault 的名稱，然後按一下**建立 Vault** 。 ![建立保存庫](images/create-vault.png)
    
4.  看到您建立的保存庫處於**作用中**狀態後，請按一下保存庫名稱，如下所示。 ![保存庫名稱](images/vault-name.png)
    
5.  在保存庫內，按一下**主要加密金鑰**，然後按一下**建立金鑰**。 ![功能表曲目](images/menu-mek.png)
    
6.  輸入**金鑰**的名稱，然後按一下**建立金鑰**，如下所示。 ![建立 mek](images/create-mek.png)
    
7.  在您看到新的「主要加密」金鑰狀態變更為**已啟用**之後，請按一下**資源**底下的**加密密碼**，如下所示。 ![功能表加密密碼](images/menu-secret.png)
    
8.  按一下**建立加密密碼 (Create Secret)** 。
    
9.  輸入加密密碼的名稱並選擇新的加密金鑰，並將認證記號輸入為**加密內容**，如下所示。按一下**建立加密密碼 (Create Secret)** 。 ![建立加密密碼](images/create-secret.png)
    

## 作業 3：建立堆疊：適用於 OKE BYOL 的 Oracle WebLogic Suite

1.  在 OCI 主控台中，按一下**漢堡功能表** -> **市集** -> **所有應用程式**。 ![選項單市集](images/menu-marketplace.png)
    
2.  在搜尋方塊中輸入 **WebLogic OKE** ，然後按一下 **Oracle WebLogic Suite for OKE BYOL** 。 ![選項單堆疊](images/menu-stack.png)
    
3.  選取可用的最新版本，然後選擇區間，然後勾選方塊以接受條款與條件。按一下**啟動堆疊 (Launch Stack)** 。 ![發射堆疊](images/launch-stack.png)
    
4.  在「堆疊資訊 (Stack Information)」區段中，將所有內容保留為預設值，然後按一下**下一步 (Next)** 。 ![堆疊資訊](images/stack-info.png)
    
5.  輸入 **wko** 作為資源名稱前置碼，然後按一下「瀏覽」選擇 SSH 公開金鑰。 ![前置詞 SSH](images/prefix-ssh.png)
    
    > 請務必使用 **wko** 作為前綴，我們將在進一步的實驗室中使用本產品。
    
6.  在 Verrazzano 整合中，保留**啟用 Verrazzano** 的預設未勾選方塊。 ![不檢谷](images/uncheck-verrazzano.png)
    
7.  在網路中，選取**建立新 VCN** 作為虛擬雲端網路策略，並保留所有預設值。 ![網路](images/network.png)
    
8.  在容器叢集 (OKE) 組態中，選取下列項目。
    
    **Kubernetes 版本：** - 保留預設 **v1.26.2** 。
    
    **非 WebLogic 節點集區資源配置 (必要)** \- 選取 **VM.Standard.E4。彈性** 資源配置和將 **1** 視為 OCPU 數目，將 **16** 視為記憶體大小。
    
    **非 WebLogic Pod 的 NodePool 中的節點** - 保留預設的 **1** 。
    
    ![非網路邏輯](images/non-weblogic.png)
    
    **建立 WebLogic 節點集區資源配置** - 將預設方塊保留為已勾選。
    
    **WebLogic 節點集區資源配置 (必要)** \- 選取 **VM.Standard.E4。彈性** 資源配置和將 **1** 視為 OCPU 數目，將 **16** 視為記憶體大小。
    
    **NodePool 中用於 WebLogic Pod 的節點** - 保留預設的 **1** 。
    
    ![網路邏輯](images/weblogic-pool.png)
    
9.  在管理例項中，輸入或選取如下所示。**運算執行處理的可用性網域** - 從下拉式清單中選取可用性網域。
    
    **管理執行處理運算資源配置 (必要)** \- 選取 **VM.Standard.E4。彈性** 資源配置和將 **1** 視為 OCPU 數目，將 **16** 視為記憶體大小。
    
    ![管理執行處理](images/admin-instance.png)
    
    **堡壘主機執行處理資源配置 (必要)** \- 選取 **VM.Standard.E4. 彈性** 資源配置和將 **1** 視為 OCPU 數目，將 **16** 視為記憶體大小。
    
    ![堡壘主機](images/bastion.png)
    
10.  在「檔案系統 (File System)」區段中，選取如下所示。**檔案系統的可用性網域** - 從下拉式清單中選取可用性網域。 ![檔案 avd](images/file-avd.png)
    
11.  在「登錄 (OCIR)」區段中，輸入如下所示。
    
    **登錄使用者名稱** - Kubernetes 用來存取容器映像檔登錄的使用者名稱，其格式為 {identity domain name}/{username}. 您的租用戶若使用 Oracle Identity Cloud Service，請使用 oracleidentitycloudservice/{username} 格式。
    
    **OCIR 認證權杖區間** - 選取您擁有 OCIR 認證權杖的區間。
    
    **驗證的 OCIR 認證權杖加密密碼** - 包含您為使用者存取映像檔登錄所產生之 OCIR 認證權杖的加密密碼。
    
    ![ocir 密語](images/ocir-secret.png)
    
12.  在 OCI 政策中，勾選 **OCI 政策**方塊。您可以建立原則以從保存庫讀取加密密碼，以及管理 Autonomous Transaction Processing Database (若適用)。按一下**下一步 (Next)** 。
    
13.  在「複查」區段中，勾選**執行套用**的方塊，然後按一下**建立**。 ![執行套用](images/run-apply.png)
    
    > 這會建立一個工作，在堆疊中建立必要的資源。您不需要等待，請繼續下一個任務，
    

## 作業 4：存取圖形遠端桌面

為便於執行此研討會，您的 VM 執行處理已預先設定可使用筆記型電腦或工作站上任何現代瀏覽器的遠端圖形桌面。請依照下列詳細資訊繼續進行登入。

1.  開啟左上角的漢堡選單。按一下**開發人員服務 (Developer Services)** ，然後選擇**資源管理程式 (Resource Manager)** \> **堆疊 (Stacks)** 。
    
2.  按一下您在實驗室 1 中建立的堆疊名稱。 ![點擊堆疊](images/click-stack.png)
    
3.  瀏覽至**應用程式資訊**頁籤，然後複製**遠端桌面 URL** 並將其貼到新的瀏覽器頁籤中。 ![桌面 URL](images/desktop-url.png)
    
    > 現在您必須依照這個遠端桌面中的所有說明進行操作。
    

您現在可以繼續下一個實驗室。

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 10 月