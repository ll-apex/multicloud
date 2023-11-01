# 在 Oracle Cloud Infrastructure 上設定 Oracle Kubernetes Engine 執行處理

## 簡介

在此實驗室中，您將建立設定了所有必要網路資源的 3 節點 Kubernetes 叢集。這些節點將會部署在不同的容錯域，以確保高可用性。

如需有關 OKE 和自訂叢集部署的詳細資訊，請參閱 [Oracle Kubernetes Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm) 文件。

預估時間：15 分鐘

### 關於產品 / 技術

Oracle Cloud Infrastructure Container Engine for Kubernetes 是一項完全託管、可擴展且高可用性的服務，可用來將容器應用系統部署到雲端。當您的開發團隊想要可靠地建置、部署及管理雲端原生應用程式時，請使用 Kubernetes 容器引擎 (有時縮寫為 OKE)。您可以指定應用系統所需的運算資源，然後 OKE 在現有 OCI 租用戶的 Oracle Cloud Infrastructure 上佈建運算資源。

### 目標

請使用_快速建立_叢集功能，建立具備必要網路資源、節點集區及三個工作節點的 OKE (Oracle Kubernetes Engine) 執行處理。_快速建立_方法是最快建立新叢集的方式。如果您接受所有預設值，只需按幾下滑鼠，即可建立新叢集。

### 先決條件

*   必須要有已啟用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的帳戶。

## 作業 1：建立 OKE 叢集

_快速建立_功能使用預設設定值，視需要使用新的網路資源來建立_快速叢集_。此方式是建立新叢集的最快速方式。如果您接受所有預設值，只需按幾下滑鼠，即可建立新叢集。叢集的新網路資源會自動建立，以及節點集區和三個工作節點。

1.  在主控台中，選取_漢堡功能表 -> 開發人員服務 -> Kubernetes 叢集 (OKE)_ ，如圖所示。
    
    ![漢堡功能表](images/hamburger-menu.png " ")
    
2.  在「叢集清單」頁面中，選取您選擇的區間，您可以在此處建立叢集，然後按一下_建立叢集_。
    
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
    *   **節點類型**：選取_受管理_作為「節點」類型。
    *   **Kubernetes 工作節點**：叢集工作節點是否可遞送。保留預設的 _Private Workers_ 值。
    *   **資源配置**：用於節點集區中每個節點的資源配置。資源配置決定了 CPU 的數目，以及配置給每個節點的記憶體大小。此清單只顯示您租用戶中 OKE 支援的資源配置。選取 _VM.Standard.E4。Flex_ (通常可在 Oracle 免費層帳戶中使用)。選取 2 個 OCPU 和 32 GB 作為記憶體大小。
    *   **映像檔**：選取 Kubernetes 版本為 _1.26.2_ 的預設映像檔。
    *   **節點數目**：要建立的工作節點數目。保留預設值 _3_ 。
    
    > _請小心不要離開預設元件；預設元件太小，以符合所有 VERRAZZANO 元件_
    
    ![快速叢集](images/quick-cluster.png " ") ![節點編號](images/node-number.png " ")
    
4.  按一下_下一步_，複查為新叢集輸入的詳細資料。
    
5.  在_複查_頁面中，選取_建立基本叢集_的核取方塊，然後按一下_建立叢集_以建立新的網路資源和新的叢集。
    
    ![複查叢集](images/review-cluster.png " ")
    
    > 您會看到已經為您建立的網路資源。等到起始建立節點集區的要求，然後按一下_關閉_。
    
    ![網路資源](images/network-resource.png " ")
    
    > 接著，新的叢集會顯示在_叢集詳細資訊_頁面上。建立主要節點時，新的叢集會取得_作用中_狀態 (大約需要 7 分鐘)。然後，您可以繼續實驗室。
    
    ![叢集佈建](images/cluster-provision.png " ")
    
    ![叢集存取](images/cluster-access.png " ")
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月