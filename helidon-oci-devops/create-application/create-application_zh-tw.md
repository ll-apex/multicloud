# 產生 Helidon MP 應用程式

## 簡介

這個實驗室會逐步介紹使用 **Helidon CLI** 建立 **Helidon MP** 應用程式的步驟。此外，您將修改 Helidon 應用程式以顯示與 **OCI Logging and Monitoring** 服務的整合。

預估時間：15 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[產生 Helidon MP 應用程式](videohub:1_i4j33ed4)

### 目標

在此實驗室中，您將：

*   下載 Helidon CLI
*   使用 Helidon CLI 產生 Helidon MP 應用程式
*   執行 OCI 日誌記錄和度量服務的整合
*   將應用程式程式碼推送至 OCI 程式碼儲存區域
*   觸發 DevOps 管線

### 先決條件

*   一個 Oracle Free Tier (Trial)、Paid 或 LiveLabs 雲端帳戶
*   熟悉 git 命令

## 作業 1：下載 Helidon CLI

1.  在 **OCI Code Editor** 中開啟新的終端機，然後貼上下列命令以瀏覽至本位目錄資料夾。
    
        <copy>cd ~</copy>
        
2.  複製並貼上下列命令來下載 **Helidon CLI** ，然後變更**權限**使其可執行。
    
        <copy>curl -L -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon</copy>
        

## 作業 2：使用 Helidon CLI 產生 Helidon Microprofile 應用程式

1.  執行 CLI 以**產生 Helidon Microprofile 應用程式**專案。
    
        <copy>./helidon init</copy>
        
2.  當系統提示您輸入 _Helidon 版本_時，請輸入 _4_ 以查看所有版本，然後輸入 _35_ 以選取 _4.0.0-ALPHA6_ 版本，如下所示。
    
        Helidon versions
        (1) 3.2.2 
        (2) 2.6.2 
        (3) 4.0.0-M1 
        (4) Show all versions 
        Enter selection (default: 1): 4
        -----------------------------
        -----------------------------
        (33) 2.0.1 
        (34) 2.0.0 
        (35) 4.0.0-ALPHA6 
        (36) 4.0.0-M1 
        Enter selection (default: 1): 35 
        
3.  提示_選取風格_時，請複製下方值並將其貼至終端機。
    
            | Helidon Flavor
        
            Select a Flavor
            (1) se   | Helidon SE
            (2) mp   | Helidon MP
            (3) nima | Helidon Níma
            Enter selection (default: 1): <copy>2</copy>
        
4.  當系統提示_選取應用程式類型_時，請複製下方的值並將其貼到終端機。
    
            Select an Application Type
        (1) quickstart | Quickstart
        (2) database   | Database
        (3) custom     | Custom
        (4) oci        | OCI
        Enter selection (default:1):<copy>4</copy>
        
5.  當系統提示輸入_專案 groupId_ 、_專案 artifactId_ 及_專案版本_時，只要**接受預設值**即可。
    
6.  當系統提示您輸入 _Java 套裝軟體名稱_時，請複製下列值並將其貼到終端機中。
    
        Java package name (default: me.username.mp.oci): <copy>ocw.hol.mp.oci</copy>
        
7.  承諾_開始開發迴圈？(預設值：n)_ 時，請按 _Enter_ 以選取預設值。
    
    > 完成之後，就會產生 **oci-mp** 專案。
    

## 作業 3：修改 Helidon 應用程式以進行記錄和度量總管

1.  若要在_程式碼編輯器_中開啟 **oci-mp** 專案，請按一下 \[ _檔案 (File)_ \] -> \[ _開啟 (Open)_ \]。 ![開放專案](images/open-project.png)
    
2.  按一下_向上箭號_以瀏覽至上層資料夾，然後選取 _oci-mp_ 資料夾，然後按一下_開啟_。 ![開啟 oci mp](images/open-ocimp.png)
    
    > 這會在瀏覽器中開啟 _oci-mp_ 應用程式。
    
3.  開啟新的終端機，複製並貼上下列命令，以**複製 _`devops_helidon_to_instance_ocw_hol`_ 資料夾中的組建和部署管線規格**。
    
        <copy>cd ~/oci-mp/
        cp ~/devops_helidon_to_instance_ocw_hol/pipeline_specs/* .</copy>
        
4.  新增 **.gitignore** ，以致於不需要屬於儲存庫的檔案與目錄將被 git 忽略。
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/.gitignore .</copy>
        
5.  複製並貼上下列命令，從主要資料夾 _`devops_helidon_to_instance_ocw_hol`_ 執行公用程式命令檔來更新 **config** 參數。當它要求**輸入 Helidon MP 專案的根目錄**時，請按 Enter 以選取**預設**值。
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/update_config_values.sh</copy>
        
    
    您會有類似下方所示的輸出： ![更新組態](images/update-config.png)
    
    > **請閱讀：-**
    
    *   呼叫此程序檔會執行下列動作：
    *   在 _~/oci-mp/server/src/main/resources/application.yaml_ 組態檔中更新以設定 Helidon 功能，此功能會將 Helidon 產生的度量傳送至 OCI 監控服務。
        *   **compartmentId** - 此示範所使用的區間 ocid
        *   **命名空間** - 這可以是任何字串，但對於此示範，這將會設定為 helidon\_metrics。
    *   _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ 組態檔中的更新可設定 Helidon 應用程式程式碼所使用的組態參數，以執行與 OCI 日誌記錄和度量服務的整合。
        *   **oci.monitoring.compartmentId** - 此示範所使用的區間 ocid
        *   **oci.monitoring.namespace** - 這可以是任何字串，但對於此示範，這將會設定為 helidon\_application。
        *   **oci.logging.id** - Terraform 程序檔啟動設定的應用程式日誌 ID。
    *   在 _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ 組態檔中更新以設定 _oci.bucket.name_ 特性，以包含由 terraform 程序檔所佈建的 Object Storage 貯體名稱，該程序檔稍後將用於示範 Helidon 應用程式的 Object Storage 支援。
6.  在「程式碼」編輯器中開啟 _~/oci-mp/server/src/main/resources/application.yaml_ 檔案，以確認 **compartmentId** 和 **namespace** 的值已更新。 ![應用程式 yaml](images/application-yaml.png)
    
7.  在「程式碼」編輯器中開啟 _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ 檔案，以驗證 **oci.monitoring.compartmentId** 、**oci.monitoring.namespace** 、**oci.logging.id** 和 **oci.bucket.name** 的值是否更新。實驗室 5 之後將使用 _oci.bucket.name_ 。
    

## 作業 4：產生將程式碼推送至 OCI 程式碼儲存區域的認證記號

在此步驟中，我們將產生一個_認證記號_，用來將 Helidon 應用程式程式碼推送至 _OCI Code repository_ 。

1.  選取右上角的_使用者圖示_，然後選取_我的設定檔_。 ![使用者圖示](images/user-icon.png)
    
2.  向下捲動並選取_認證權杖_。 ![認證權杖](images/auth-tokens.png)
    
3.  按一下_產生憑證_。 ![產生符記](images/generate-token.png)
    
4.  複製 _oci-mp_ ，然後將它貼到「描述 (Description)」方塊中，然後按一下_產生權杖 (Generate Token)_ 。 ![符記描述](images/token-description.png)
    
5.  在「產生的記號」底下選取**複製**，然後使用您選擇的編輯器將它貼到文字檔中。請記住，稍後無法擷取記號，因此現在請務必保留此記號的副本。完成之後，請按一下**關閉**。 ![複製記號](images/copy-token.png)
    

## 作業 5：將 Helidon 應用程式專案同步至 DevOps 專案中的 OCI 程式碼儲存區域

1.  開啟新的終端機，複製並貼上下列命令以瀏覽至 _oci-mp_ 目錄。
    
        <copy>cd ~/oci-mp</copy>
        
2.  將 _oci-mp_ 專案目錄初始化，成為 **git repository** 。
    
        <copy>git init</copy>
        
3.  將分支設定為**維護**，以比對對應的遠端分支。
    
        <copy>git checkout -b main</copy>
        
4.  設定**遠端儲存區域**。使用從最後一個 terraform 輸出顯示的 OCI 程式碼儲存區域 https URL，或使用 `devops_helidon_to_instance_ocw_hol` 的 get.sh 工具擷取該值。
    
        <copy>git remote add origin $(~/devops_helidon_to_instance_ocw_hol/main/get.sh code_repo_https_url)
        git remote -v</copy>
        
    
    > **git remote -v** 是用來確認來源已經設定。
    
5.  將 git 設定為使用**證明資料協助程式**存放區，以便只在需要 OCI 儲存區域的使用者名稱和密碼輸入一次。此外，請設定 git 確認所需的 **user.name** 和 **user.email** 。
    
        <copy>git config credential.helper store
        git config --global user.email "my.name@example.com"
        git config --global user.name "FIRST_NAME LAST_NAME"</copy>
        
6.  使用 **git 拉引式**將 oci 儲存區域的 git 日誌與本機儲存區域同步。
    
        <copy>git pull origin main</copy>
        
7.  這將會提示您輸入使用者名稱和密碼。針對在 **task 4** 為密碼產生的使用者名稱和 OCI 使用者 **auth token** ，請使用 **tenancy name** / **username** 。![Git 使用者名稱](images/git-username.png) ![git 密碼](images/git-password.png)
    
8.  您會有類似以下的執行結果。如果不是，請檢查您是否已正確執行每個 git 指令。 ![git 提取同步](images/git-pull-sync.png)
    

## 作業 6：推送 Helidon 應用程式程式碼並觸發 DevOps 管線

1.  **暫存** git 確認的所有檔案。
    
        <copy>git add .
        git status</copy>
        
    
    > git 狀態將會輸出儲存區域中的所有檔案。
    
2.  執行第一個 **commit** 。
    
        <copy>git commit -m "Helidon oci-mp first commit"</copy>
        
3.  將變更**植入**至遠端儲存區域。
    
        <copy>git push -u origin main</copy>
        
    
    > 這會觸發 DevOps 來啟動組建管線。
    
4.  在新頁籤中開啟[雲端主控台](https://cloud.oracle.com/)，按一下 _漢堡功能表_ -> _開發人員服務_ -> _專案_ (在 **DevOps** 下)。 ![devops 專案](images/devops-project.png)
    
5.  選取您在**實驗室 1** 中建立的區間，然後按一下 _devops-project-helidon-ocw-hol-string_ 以開啟 **DevOps 專案**。 ![選取區間](images/select-compartment.png)
    
    > 若未見到新區間，請重新整理瀏覽器。
    
6.  在_最新的組建歷史記錄_底下，您將看到_執行_和狀態為_已接受 / 進行中_。按一下下面所示的最新執行。 ![組建歷史記錄](images/build-history.png)
    
7.  組建管線完成這三個階段之後，您就會看到輸出，如下所示。您可以按一下位於階段之前的箭頭來檢視正在執行的動作。此動作我們在 _oci-mp_ 資料夾的 _`build_spec.yaml`_ 檔案中有定義。 ![組建執行優先](images/build-run-first.png)
    
8.  在「組建」執行進度中，在第三個階段按一下**三個點**，然後按一下**檢視部署**，如下所示。這會開啟**部署管線**。 ![檢視部署](images/view-deployment.png)
    
9.  您可以在此處查看_部署進度_。部署管線完成之後，您將會見到輸出，如下所示。您可以按一下出現在階段之前的箭頭來檢視其執行的動作。此動作會在 _oci-mp_ 資料夾的 _`deployment_spec.yaml`_ 檔案中出現缺陷。 ![部署運行](images/deployment-run.png)
    
    > 這會將 Helidon 應用程式成功部署到 OCI 中的**運算執行處理**。
    
10.  若要檢視部署管線的日誌，請按一下接近部署階段的**三個點**，然後按一下**檢視詳細資訊**，如下所示。 ![檢視日誌](images/view-logs.png)
    
11.  向下捲動日誌，並確認 JDK 的風格是**開啟 JDK** 。另外，請注意 Helidon 應用程式在 Java 中運用新**虛擬執行緒**功能的日誌，如下所示。 ![開啟 jdk](images/open-jdk.png)
    
    > 在**實驗室 4** 中，我們將使用 **Oracle JDK** 取代 **Open JDK** 。
    

您現在可以**繼續進行下一個實驗室。**

## 進一步瞭解

*   [Helidon CLI](https://helidon.io/docs/v3/#/about/cli)
*   [Helidon MP 快速入門指南](https://helidon.io/docs/v3/#/mp/guides/quickstart)
*   [Helidon MP 組態來源](https://helidon.io/docs/v3/#/mp/config/advanced-configuration)
*   [Helidon MP 組態來源](https://helidon.io/docs/v3/#/mp/guides/config)

## 確認

*   **作者** - Keith Lustria
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月