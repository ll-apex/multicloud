# 安裝 Verrazzano

## 簡介

這個實驗室會逐步引導您在 Oracle Cloud Infrastructure 的 Kubernetes 叢集上安裝 Verrazzano 的步驟。

預估時間：15 分鐘

### 關於產品 / 技術

Verrazzano 是一個端對端企業容器平台，用於在多雲端和混合環境中部署雲端原生和傳統應用程式。它是由一系列經過策劃的開放原始碼元件組成 - 許多您可能已經使用過與信任，有些是特別針對讓 Verrazzano 成為具有凝聚性且易於使用之平台的片段而撰寫的。

Verrazzano 包含下列功能：

*   混合與多叢集工作負載管理
*   WebLogic、Coherence 與 Helidon 應用程式的特殊處理
*   多叢集基礎架構管理
*   整合和預線應用程式監控
*   整合的安全
*   DevOps 和 GitOps 培訓

Verrazzano 支援下列安裝設定檔：開發 (`dev`)、生產 (`prod`) 以及受管理的叢集 (`managed-cluster`)。

下圖說明 Verrazzano 安裝設定檔。 ![安裝設定檔](images/installprofile.png)

若要變更下列任何指令中的設定檔，請將 _VZ\_PROFILE_ 環境變數設定為要安裝的設定檔名稱。

如需 Verrazzano 組態選項的完整說明，請參閱 [Verrazzano Custom Resource Definition](https://verrazzano.io/docs/reference/api/verrazzano/verrazzano/) 。

在此實驗室中，我們將安裝 _Verrazzano 的開發設定檔_，其特性如下：

*   萬用字元 (nip.io) DNS
*   自行簽署的憑證
*   系統元件和所有應用程式使用的共用可觀察性堆疊
*   可觀察堆疊的暫時儲存 (如果 Pod 重新啟動，您將會遺失所有日誌和測量結果)
*   它具有輕量型安裝。
*   此為評估用途。
*   單節點 Opensearch 叢集拓樸。

Verrazzano 安裝經過策劃的一組開放原始碼元件。下表列出每個元件、其版本和簡要描述。

![Verrazzano 設定檔](images/verrazzano-profile.png " ") ![Verrazzano 設定檔](images/verrazzano-profiles.png " ")

根據 DNS 的選擇，我們可以使用 nip.io (萬用字元 DNS) 或 [Oracle OCI DNS](https://docs.cloud.oracle.com/en-us/iaas/Content/DNS/Concepts/dnszonemanagement.htm) 。在此實驗室中，我們將使用 nip.io (萬用字元 DNS) 進行安裝。

傳入控制器是協助將 Docker 容器存取外部世界 (藉由提供 IP 位址) 的項目。傳入會將 IP 位址路由至不同的叢集。

### 目標

在此實驗室中，您將：

*   設定 `kubectl` 以使用 Oracle Kubernetes Engine 叢集
*   安裝 Verrazzano vz CLI。
*   安裝 Verrazzano 的開發 (`dev`) 設定檔。

### 先決條件

Verrazzano 需要下列項目：

*   Kubernetes 叢集和相容的 `kubectl`。
*   Kubernetes 工作節點上至少有 2 個 CPU、100GB 磁碟儲存空間及 16GB RAM。這足以安裝 Verrazzano 的開發設定檔。視您部署之應用程式的資源需求而定，這不一定足以部署您的應用程式。
*   在實驗室 1 中，您在 Oracle Cloud Infrastructure 上建立了一個 Kubernetes 叢集。您將使用該 Kubernetes 叢集 _cluster1_ 來安裝 Verrazzano 的開發設定檔。

## 作業 1：設定 `kubectl` (Kubernetes Cluster CLI)

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
    

## 作業 2：安裝 Verrazzano CLI

1.  下載最新的 vz CLI。
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz</copy>
        
    
    執行結果應該和下列類似：
    
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
        

0 0 0 0 0 0 0 0 --:-- --:--:-- --:--:--- 0 100 39.7M 100 39.7M 0 0 23.6M 0 0:00:01 0:00:01 --:--:-- 32.7M \`\`\`

2.  下載檢查碼檔案。
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        

執行結果應該和下列類似：

    ```bash
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    

0 0 0 0 0 0 0 0 --:-- --:--:-- --:--:--:-- 0 100 102 100 102 0 0 218 0 --:--:-- --:--:-- --:--:-- 218 \`\`

3.  對照總和檢查檔案驗證二進位檔。
    
        <copy>sha256sum -c verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        
    
    執行結果應該和下列類似：
    
        verrazzano-1.6.2-linux-amd64.tar.gz: OK
        
4.  解壓縮並移至 vz 二進位目錄。
    
        <copy>tar xvf verrazzano-1.6.2-linux-amd64.tar.gz
        cd ~/verrazzano-1.6.2/bin/</copy>
        
5.  進行測試，以確定您安裝的版本是最新的。
    
        <copy>./vz version</copy>
        
    
    執行結果應該和下列類似：
    
        Version: v1.6.2
        BuildDate: 2023-07-21T12:06:02Z
        GitCommit: 5cd8a2f8670000d2ef0b02404e3169699d87f26b
        

## 作業 3：安裝 Verrazzano 開發設定檔

安裝設定檔是已知的 Verrazzano 設定組態，可由名稱參考，然後可依需要自訂。

Verrazzano 支援下列安裝設定檔：開發 (`dev`)、生產 (`prod`) 以及受管理的叢集 (`managed-cluster`)。

1.  使用 nip.io DNS 方法進行安裝。複製下列命令並將它貼到 _Cloud Shell_ 中以安裝 Verrazzano。
    
        <copy>./vz install -f - <<EOF
        apiVersion: install.verrazzano.io/v1beta1
        kind: Verrazzano
        metadata:
          name: example-verrazzano
        spec:
          profile: dev
        EOF
        </copy>
        
    
    執行結果應該和下列類似：
    
        Installing Verrazzano version v1.6.2
        Applying the file https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-platform-operator.yaml
        customresourcedefinition.apiextensions.k8s.io/verrazzanos.install.verrazzano.io created
        namespace/verrazzano-install created
        serviceaccount/verrazzano-platform-operator created
        clusterrole.rbac.authorization.k8s.io/verrazzano-managed-cluster created
        clusterrolebinding.rbac.authorization.k8s.io/verrazzano-platform-operator created
        service/verrazzano-platform-operator created
        service/verrazzano-platform-operator-webhook created
        deployment.apps/verrazzano-platform-operator created
        deployment.apps/verrazzano-platform-operator-webhook created
        mutatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-mysql-backup created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-operator-webhook created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-mysqlinstalloverrides created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-requirements-validator created
        Waiting for verrazzano-platform-operator to be ready before starting install - 23 seconds
        
    
    > 完成安裝需要大約 15 到 20 分鐘。此命令會安裝 Verrazzano 平台操作員並套用 Verrazzano 自訂資源。安裝記錄會串流至指令視窗，直到完成安裝或達到預設逾時 (30m)。
    
2.  讓 _Cloud Shell_ 保持開啟，並讓安裝執行。
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月