# 在 Oracle Cloud Infrastructure 上設定 Oracle Kubernetes Engine 執行處理

## 簡介

這個實驗室會逐步引導您完成在 Oracle Cloud Infrastructure 上建立託管 Kubernetes 環境的步驟。

預估時間：15 分鐘

### 關於產品 / 技術

Oracle Cloud Infrastructure Container Engine for Kubernetes 是一項完全託管、可擴展且高可用性的服務，可用來將容器應用系統部署到雲端。當您的開發團隊想要可靠地建置、部署及管理雲端原生應用程式時，請使用 Kubernetes 容器引擎 (有時縮寫為 OKE)。您可以指定應用系統所需的運算資源，然後 OKE 在現有 OCI 租用戶的 Oracle Cloud Infrastructure 上佈建運算資源。

### 目標

在此實驗室中，您將：

*   建立 OKE (Oracle Kubernetes Engine) 執行處理。
*   開啟 OCI Cloud Shell 並將 `kubectl` 設定為與 Kubernetes 叢集互動。

### 先決條件

*   必須要有已啟用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的帳戶。

若要建立 Kubernetes (OKE) 的容器引擎，請完成下列步驟：

*   建立網路資源 (VCN、子網路、安全清單等)。
*   建立叢集。
*   建立 `NodePool`。

這個實驗室示範 _快速入門_功能如何建立及設定 3 節點 Kubernetes 叢集的所有必要資源。所有節點都將部署在不同的可用性網域中，以確保高可用性。

如需 OKE 和自訂叢集部署的詳細資訊，請參閱 [Oracle Container Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm) 文件。

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
    

## 作業 2：設定 `kubectl` (Kubernetes Cluster CLI)

Oracle Cloud Infrastructure (OCI) Cloud Shell 是一個 Web 瀏覽器型終端機，可從 Oracle Cloud 主控台存取。Cloud Shell 提供對 Linux Shell 的存取，並搭配預先認證的 Oracle Cloud Infrastructure CLI 和其他有用的工具 ( _Git、kubectl、helm、OCI CLI_)，以完成 Verrazzano 教學課程。可從主控台存取 Cloud Shell. 您的 Cloud Shell 將會在 Oracle Cloud 主控台中顯示為主控台的永久性框架，而且在您瀏覽至主控台的不同頁面時仍會保持作用中。

您將使用 _Cloud Shell_ 完成此研討會。

我們將使用 `kubectl` 從遠端使用 Cloud Shell 管理叢集。需要 `kubeconfig` 檔案。這項作業將會使用預先認證的 OCI CLI 產生，因此開始使用它之前，沒有任何設定可進行。

1.  按一下叢集詳細資訊頁面上的_存取叢集_。
    
    > 如果您離開該頁面，請開啟導覽功能表，然後在_開發人員服務_底下選取 _Kubernetes 叢集 (OKE)_ 。選取您的叢集並前往詳細資訊頁面。
    
    ![存取叢集](images/access-cluster.png " ")
    
    > 此時會顯示一個對話方塊，您可以從其中開啟 Cloud Shell，其中包含需要執行的自訂 OCI 命令，以建立 Kubernetes 組態檔。
    
2.  請保留預設的 _Cloud Shell 存取_，然後先選取_複製_連結，將 `oci ce...` 命令複製到 Cloud Shell。
    
    ![複製 kubectl 組態](images/copy-config.png " ")
    
3.  現在，按一下_啟動 Cloud Shell_ 以開啟內建主控台。然後關閉組態對話方塊，再將命令貼到 _Cloud Shell_ 中。
    
    ![啟動 Cloud Shell](images/launch-cloudshell.png " ")
    
4.  從剪貼簿複製命令 (Ctrl+V 或按一下滑鼠右鍵並複製) 至 Cloud Shell，然後執行該命令。
    
    例如，此指令如下所示：
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![kubectl 組態](images/kube-config.png " ")
    
5.  現在，請確認 `kubectl` 是否正常運作，例如使用 `get node` 指令。您可能需要數次執行此指令，直到您看到類似以下的輸出為止。
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.101   Ready    node    11m   v1.26.2
        10.0.10.29    Ready    node    11m   v1.26.2
        10.0.10.37    Ready    node    11m   v1.26.2
        
    
    > 如果您看到節點的資訊，則組態已成功。
    
6.  您可以使用 Cloud Shell 右上角的控制項，隨時最小化與回復終端機大小。
    
    ![cloud shell，雲端殼層](images/cloudshell.png " ")
    

將此 _Cloud Shell_ 開放；我們會將它用於進一步的實驗室。

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 3 月