# 設定 KUBECTL 與 Oracle Cloud Infrastructure (OCI) 上的 Oracle Container Engine for Kubernetes (OKE) 互動

## 簡介

這個實驗室會逐步引導您建立組態檔，讓您可以存取 Oracle Cloud Infrastructure 上的 Kubernetes 環境。

### 關於產品 / 技術

Oracle Cloud Infrastructure Container Engine for Kubernetes 是一項完全託管、可擴展且高可用性的服務，可用來將容器應用系統部署到雲端。當您的開發團隊想要可靠地建置、部署及管理雲端原生應用程式時，請使用 Kubernetes 容器引擎 (有時縮寫為 OKE)。您可以指定應用系統所需的運算資源，然後 OKE 在現有 OCI 租用戶的 Oracle Cloud Infrastructure 上佈建運算資源。

### 目標

在此實驗室中，您將：

*   開啟 OCI Cloud Shell 並將 `kubectl` 設定為與 Kubernetes 叢集互動。

### 先決條件

*   此研討會假設您已於 LiveLabs 預訂此研討會。

## 作業 1：設定 `kubectl` (Kubernetes Cluster CLI)

Oracle Cloud Infrastructure (OCI) Cloud Shell 是一個 Web 瀏覽器型終端機，可從 Oracle Cloud 主控台存取。Cloud Shell 提供對 Linux Shell 的存取，並搭配預先認證的 Oracle Cloud Infrastructure CLI 和其他有用的工具 ( _Git、kubectl、helm、OCI CLI_)，以完成 Verrazzano 教學課程。可從主控台存取 Cloud Shell. 您的 Cloud Shell 將會在 Oracle Cloud 主控台中顯示為主控台的永久性框架，而且在您瀏覽至主控台的不同頁面時仍會保持作用中。

您將使用 _Cloud Shell_ 完成此研討會。

我們將使用 `kubectl` 從遠端使用 Cloud Shell 管理叢集。需要 `kubeconfig` 檔案。這項作業將會使用預先認證的 OCI CLI 產生，因此開始使用它之前，沒有任何設定可進行。

1.  在主控台中，選取_漢堡功能表 -> 開發人員服務 -> Kubernetes 叢集 (OKE)_ ，如圖所示。
    
    ![漢堡功能表](../setup-oke-ocishell/images/hamburgermenu.png " ")
    
2.  選取與您關聯的區間。然後按一下 _cluster1_ 叢集。
    
3.  按一下叢集詳細資訊頁面上的_存取叢集_。
    
    ![存取叢集](../setup-oke-ocishell/images/accesscluster.png " ")
    
    > 此時會顯示一個對話方塊，您可以從其中開啟 Cloud Shell，其中包含需要執行的自訂 OCI 命令，以建立 Kubernetes 組態檔。
    
4.  請保留預設的 _Cloud Shell 存取_，然後先選取_複製_連結，將 `oci ce...` 命令複製到 Cloud Shell。
    
    ![複製 kubectl 組態](../setup-oke-ocishell/images/copyconfig.png " ")
    
5.  現在，按一下_啟動 Cloud Shell_ 以開啟內建主控台。然後關閉組態對話方塊，再將命令貼到 _Cloud Shell_ 中。
    
    ![啟動 Cloud Shell](../setup-oke-ocishell/images/launchcloudshell.png " ")
    
6.  從剪貼簿複製命令 (Ctrl+V 或按一下滑鼠右鍵並複製) 至 Cloud Shell，然後執行該命令。
    
    例如，此指令如下所示：
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![kubectl 組態](../setup-oke-ocishell/images/kubeconfig.png " ")
    
7.  現在，請確認 `kubectl` 是否正常運作，例如使用 `get node` 指令。您可能需要數次執行此指令，直到您看到類似以下的輸出為止。
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.17    Ready    node    11m   v1.21.5
        10.0.10.184   Ready    node    11m   v1.21.5
        10.0.10.31    Ready    node    11m   v1.21.5
        
    
    > 如果您看到節點的資訊，則組態已成功。
    
8.  您可以使用 Cloud Shell 右上角的控制項，隨時最小化與回復終端機大小。
    
    ![cloud shell，雲端殼層](../setup-oke-ocishell/images/cloudshell.png " ")
    

將此 _Cloud Shell_ 開放；我們會將它用於進一步的實驗室。

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 2 月