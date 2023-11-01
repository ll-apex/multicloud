# 安裝 Verrazzano

## 簡介

這個實驗室會逐步引導您在 Oracle Cloud Infrastructure 的 Kubernetes 叢集上安裝 Verrazzano 的步驟。

估計時間：20 分鐘

### 關於產品 / 技術

Verrazzano 是一個端對端企業容器平台，用於在多雲端和混合環境中部署雲端原生和傳統應用程式。它是由成策的開放原始碼元件所組成，許多您可能已經使用過與信任，有些是特別為了讓 Verrazzano 成為整合性且易於使用的平台而撰寫的。

Verrazzano 包含下列功能：

*   混合與多叢集工作負載管理
*   WebLogic、Coherence 與 Helidon 應用程式的特殊處理
*   多叢集基礎架構管理
*   整合和預線應用程式監控
*   整合的安全
*   DevOps 和 GitOps 培訓

### 目標

在此實驗室中，您將：

*   安裝 Verrazzano 指令行工具。
*   安裝 Verrazzano 的開發 (`dev`) 設定檔。
*   驗證成功的 Verrazzano 安裝。

### 先決條件

Verrazzano 需要下列項目：

*   Kubernetes 叢集和相容的 `kubectl`。
*   Kubernetes 工作節點上至少有 2 個 CPU、100GB 磁碟儲存空間及 16GB RAM。這足以安裝 Verrazzano 的開發設定檔。視您部署之應用程式的資源需求而定，這不一定足以部署您的應用程式。

在實驗室 1 中，您在 Oracle Cloud Infrastructure 上建立了一個 Kubernetes 叢集。您將使用該 Kubernetes 叢集 \*cluster1\* 來安裝 Verrazzano 的開發設定檔。 在實驗室 1 中，您建立了可存取 Oracle Cloud Infrastructure 上 Kubernetes 叢集的組態檔。您將使用該 Kubernetes 叢集 \*cluster1\* 來安裝 Verrazzano 的開發設定檔。

## 作業 1：安裝 vz CLI

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
        

## 作業 2：安裝 Verrazzano 開發設定檔

安裝設定檔是已知的 Verrazzano 設定組態，可由名稱參考，然後可依需要自訂。

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
        \> 完成安裝需要大約 15 至 20 分鐘。此命令會安裝 Verrazzano 平台運算子並套用 Verrazzano 自訂資源。 \> 需要大約 8 到 10 分鐘才能完成安裝。此命令會安裝 Verrazzano 平台操作員並套用 Verrazzano 自訂資源。
2.  等待安裝完成。安裝記錄會串流至指令視窗，直到完成安裝或達到預設逾時 (30m)。
    

## 工作 3：驗證成功的 Verrazzano 安裝

Verrazzano 在多個命名空間中安裝多個物件。Verrazzano 元件已安裝在命名空間 _verrazzano-system_ 中。

1.  請確認與多個物件關聯的所有 Pod 都處於_執行中_狀態。所有 Pod 的狀態將會是_執行中_。
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    執行結果應該和下列類似：
    
        $   kubectl get pods -n verrazzano-system
        NAME                                    READY   STATUS  RESTARTS AGE
        coherence-operator-b5dc669c6-rk2sm      1/1     Running   1       23m
        fluentd-54f5x                           2/2     Running   1       11m
        fluentd-h7mgh                           2/2     Running   1       11m
        fluentd-xcdfz                           2/2     Running   0       11m
        oam-kubernetes-runtime-5b48f944b-cx7b9  1/1     Running   0       24m
        verrazzano-application-operator-665c5   1/1     Running   0       22m
        verrazzano-application-operator-webhook 1/1     Running   0       22m
        verrazzano-authproxy-67776ff58b-8l      3/3     Running   0       21m
        verrazzano-cluster-operator-67dc5695    1/1     Running   0       14m
        verrazzano-cluster-operator-webhook     1/1     Running   0       14m
        verrazzano-console-7d95c98cb9-9ql2      2/2     Running   0       20m
        verrazzano-monitoring-operator-59f      2/2     Running   0       21m
        vmi-system-es-master-0                  2/2     Running   0       19m
        vmi-system-grafana-7fd956b585-2tbgl     3/3     Running   0       19m
        vmi-system-kiali-dd87546d6-ddxss        2/2     Running   0       22m
        vmi-system-osd-7687d6fccf-nm7kt         2/2     Running   0       14m
        weblogic-operator-54979449f4-njgrq      2/2     Running   0       22m
        weblogic-operator-webhook-f7ff8c8cf     1/1     Running   0       22m
        
    
    只要開啟 _Cloud Shell_ ，即可使用實驗室 3。
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月