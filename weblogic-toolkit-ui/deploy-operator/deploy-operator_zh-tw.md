# 將 WebLogic 運算子部署至 Oracle Kubernetes 叢集 (OKE)

## 簡介

在此實驗室中，我們使用瀏覽器認證 OCI CLI，這會建立 _.oci/config_ 檔案。我們將使用 kubectl 從遠端使用_本機存取_來管理叢集。它需要 _kubeconfig_ 檔案。將使用 OCI CLI 產生此 kubeconfig 檔案。然後，我們就會從 WebLogic Kubernetes Toolkit UI 驗證與 Kubernetes 叢集的連線。最後，我們會將 WebLogic Kubernetes Operator 安裝至 Kubernetes 叢集 (OKE)。

預估時間：10 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[將 WebLogic 運算子部署到 OKE 叢集](videohub:1_0itbllhe)

### 目標

在此實驗室中，您將：

*   設定 kubectl (Kubernetes Cluster CLI) 以連線至 Kubernetes 叢集。
*   驗證 WebLogic Kubernetes Toolkit UI 與 Kubernetes 叢集的連線。
*   將 WebLogic Kubernetes 運算子安裝至 Kubernetes 叢集。

## 作業 1：設定 kubectl (Kubernetes Cluster CLI) 以連線至 Oracle Kubernetes 叢集

在此作業中，我們在 _/home/opc_ 目錄中建立配置檔案 _.oci/config_ 和 _.kube/config_ 。此組態檔可讓我們從此虛擬機器存取 Oracle Kubernetes Cluster (OKE)。

1.  在搜尋框中點擊_活動_，然後輸入 _Firefox_ 。點擊 _Firefox_ 的圖示。 ![開放式防火牆](images/open-firefox.png)
    
2.  開啟 URL：[https://cloud.oracle.com](https://cloud.oracle.com) 。輸入您的_雲端帳戶名稱_，然後您的 Oracle Cloud 帳戶證明資料，然後按一下_登入_。
    
3.  在主控台中，選取 \[ _漢堡功能表_ \] -> \[ _開發人員服務_ \] -> \[_Kubernetes 叢集 (OKE)_ \]，如下所示。 ![OKE 圖示](images/oke-icon.png)
    
4.  按一下您在實驗室 3 中建立的叢集名稱，然後按一下_存取叢集_。 ![存取叢集](images/access-cluster.png)
    
5.  選取_本機存取_，然後按一下_複製_ (如圖所示)。 ![本機存取權](images/local-access.png)
    
6.  按一下_活動_，然後選取_終端機_。 ![最終](images/click-terminal.png)
    
7.  在終端機中貼上複製的指令。若為 _您要建立新配置檔案嗎？_，請鍵入 _y_ ，然後按 _Enter_ 鍵。若要_您想透過瀏覽器登入來建立配置檔案嗎？_，請鍵入 _y_ ，然後按 _Enter_ 鍵。 ![OCI 組態](images/oci-config.png)
    
8.  在 Firefox 瀏覽器中，點擊您目前的瀏覽階段。
    
    > 您會看到_完成的授權_，如下所示。 ![授權完成](images/authorization-complete.png)
    
9.  在_輸入私密金鑰的密碼詞組_中，將其留白並按 _Enter_ 鍵。 ![空的密碼詞組](images/empty-passphrase.png)
    
10.  使用上箭頭鍵來再次執行 _oce ce ..._ 命令，然後多次重新執行，直到您看到 _New config for the Kubeconfig file /home/opc/.kube/config_ 為止。 ![建立 KubeConfig](images/create-kubeconfig.png)
    

## 工作 2：驗證 WebLogic Kubernetes Toolkit UI 與 Oracle Kubernetes 叢集的連線

在這項任務中，我們會確認從 `WebLogic Kubernetes Toolkit UI` 應用程式連線至 _Oracle Kubernetes Cluster (OKE)_ 。

1.  回到 WebLogic Kubernetes 工具套件 UI，按一下_活動_，然後選取 WebLogic Kubernetes 工具套件 UI 視窗。
    
2.  按一下 \[_Kubernetes_ \] -> \[ _用戶端組態_ \]，然後按一下_驗證連線_。 ![檢查連線](images/verify-connectivity.png)
    
3.  看到_驗證 Kubernetes 從屬端連線成功 (Client Connectivity Success)_ 視窗之後，按一下_確定 (Ok)_ 。 ![已順利連線](images/successfully-connected.png)
    

## 作業 3：將 WebLogic Kubernetes 運算子安裝至 Oracle Kubernetes 叢集

本節提供在目標 Kubernetes 叢集安裝 WebLogic Kubernetes Operator (「操作員」) 的支援。

1.  按一下 _WebLogic 運算子_。指定下列組態詳細資訊，然後按一下_安裝運算子_。
    
    **Kubernetes 命名空間** - 要安裝運算子的 Kubernetes 命名空間。保留預設值。  
    **Kubernetes 服務帳戶** - 讓操作員在發出 Kubernetes API 要求時使用的 Kubernetes 服務帳戶。保留預設值。  
    **用於安裝 Operator 的 Helm 版本名稱** - 用來識別此安裝的 Helm 發行版本名稱。維持預設值。  
    
    ![WebLogic 操作員](images/weblogic-operator.png)
    
    > **僅供參考：**  
    > ！運算子的_要使用的影像標記_欄位預設會設為對應至 GitHub Container Registry 上最新操作員版本的影像標記。  
    > 運算子必須知道將管理的 Kubernetes 叢集中的哪個 WebLogic 網域。它會在 Kubernetes 命名空間層次執行，因此將運算子設定為管理的 Kubernetes 命名空間中任何 WebLogic 網域，將由安裝的操作員執行處理進行管理。  
    > 針對 _Kubernetes 命名空間選擇策略_欄位，我們選取_標籤選取器_，這表示此運算子將會管理具有 _weblogic-operator=enabled_ 標籤的任何 Kubernetes 命名空間。  
    > ![操作員影像](images/operator-image.png)
    
    > 藉由啟用「啟用叢集角色連結」，操作員安裝將會建立一個操作員將用於所有受管理命名空間的 Kubernetes ClusterRole 和 ClusterRoleBinding.  
    > 操作員的 REST API 預設不會在 Kubernetes 叢集之外公開。若要啟用要公開的 REST API，您可以啟用_外部擴充 REST API_ 。[角色連結](images/role-binding.png)  
    
    > 此窗格可讓您覆寫運算子的 Java 記錄組態，這對以運算子除錯問題時非常有用。  
    > ![Java 日誌記錄](images/java-logging.png)  
    
    > 如需有關 _WebLogic Kubernetes Operator 映像檔_、_Kubernetes 命名空間選擇策略_、_WebLogic Kubernetes 角色連結_、_外部 REST API 存取_、_第三方整合_及 _Java 記錄日誌_的詳細資訊，請參閱 [WebLogic Kubernetes Operator](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/) 文件。
    
2.  看到 _WebLogic Kubernetes Operator 安裝完成_之後，請按一下_確定_。 ![已安裝操作員](images/operator-installed.png)
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月