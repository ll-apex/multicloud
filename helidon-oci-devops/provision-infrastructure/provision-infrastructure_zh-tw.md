# 佈建基礎架構

## 簡介

在此實驗室中，您將建立**區間**、**動態群組**、**使用者群組和原則**。接著，您將使用 **OCI Code Editor** 中的 Terraform 服務來建立 **DevOps 專案**及其相關資源。

預估時間：10 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[佈建基礎架構](videohub:1_tnhjvsxr)

### 目標

在此實驗室中，您將：

*   開啟「程式碼編輯器」以下載 Terraform 命令檔。
*   佈建區間、動態群組、使用者群組及原則。
*   佈建 DevOps 專案所需的資源

### 先決條件

*   一個 Oracle Free Tier (Trial)、Paid 或 LiveLabs 雲端帳戶
*   熟悉區間、動態群組、使用者群組和原則

## 工作 1：開啟程式碼編輯器並下載原始程式碼

1.  在雲端主控台中，按一下顯示的_開發人員工具_圖示，然後按一下_程式碼編輯器_。 ![開碼編輯器](images/open-codeeditor.png)
    
2.  按一下_終端機_ -> _新終端機_來開啟終端機。在研討會中，系統會要求您開啟新的終端機。透過這種方式，您可以在「程式碼編輯器」中開啟新的終端機。 ![開放終端](images/open-terminal.png)
    
3.  將下列指令複製並貼上至終端機以下載原始碼。此原始碼包含地形指令碼，可建立此研討會所需的 OCI 資源。
    
        <copy>curl -O https://objectstorage.uk-london-1.oraclecloud.com/p/IxV3cO7Uf5VnbniLEmx8zOBhr6liix40QWNTnTy0TTBcGdrLaRNSt2IJYxBPHqdw/n/lrv4zdykjqrj/b/ankit-bucket/o/devops_helidon_to_instance_ocw_hol.zip
        unzip ~/devops_helidon_to_instance_ocw_hol.zip</copy>
        

![下載原始碼](images/download-sourcecode.png)

4.  若要在_程式碼編輯器_中開啟原始碼 **`devops_helidon_to_instance_ocw_hol`** ，請按一下_檔案_ \-> _開啟_。 ![開放原始碼](images/open-sourcecode.png)
    
5.  選取本位目錄中的 _`devops_helidon_to_instance_ocw_hol`_ ，然後按一下_開啟_。 ![開放式裝置](images/open-devops.png)
    
6.  按一下 _`devops_helidon_to_instance_ocw_hol`_ 資料夾內的檔案名稱 _terraform.tfvars_ ，如下所示。我們提供 **`tenancy_ocid`** 、 **region** 、**`compartment_ocid`** 及 **`user_ocid`** 等變數，可供您自訂試用雲端帳戶。 ![開放式電視機](images/open-tfvars.png)
    

> 如果您看不到專案。您可能需要按一下「程式碼編輯器」中的_總管_圖示。 ![艾勒](images/explorer.png)

7.  在您的瀏覽器中，開啟**雲端主控台**的[新分頁](https://cloud.oracle.com/)。我們將使用此頁籤來取得上述變數的值。
    
8.  若要取得 **`tenancy_ocid`** ，請按一下_使用者圖示_，然後按一下_租用戶_，如下所示。 ![取得租用戶 OCID](images/get-tenancyocid.png)
    
9.  按一下_複製_以複製租用戶的 **OCID** ，並將它貼到 _terraform.tfvars_ 檔案中作為 _`tenancy_ocid`_ 的值。 ![複製租用戶 OCID](images/copy-tenancyocid.png)
    
10.  若要取得 **`user_ocid`** ，請按一下_使用者圖示_，然後按一下_我的設定檔_，如下所示。 ![我的設定檔](images/my-profile.png)
    
11.  按一下_複製_以複製使用者的 **OCID** ，並將它貼到 _terraform.tfvars_ 檔案中作為 _`user_ocid`_ 的值。 ![複製使用者 OCID](images/copy-userocid.png)
    
12.  若要尋找區域名稱，請按一下**管理區域**，如下所示。然後複製您主要區域的**區域識別碼**，並將它貼到 _terraform.tfvars_ 檔案中作為 _region_ 的值。![管理區域](images/manage-region.png) ![區域名稱](images/region-name.png)
    
13.  最後，您的 _terraform.tfvars_ 應該顯示如下。保留 _`compartment_ocid`_ 的值。區間建立成為作業 2 的一部分之後，系統就會取代這個值。 ![init tfvars](images/init-tfvars.png)
    

## 作業 2：建立區間、動態群組、使用者群組和原則

此任務的目標是透過建立區間、動態群組、使用者群組及原則來準備 DevOps 設定的環境。此區段需要具備管理員權限的使用者。如果沒有，請務必要求另一位具有此類權限的使用者為您執行此作業。

1.  開啟終端機，複製並貼上下列指令以瀏覽至 _init_ 資料夾。
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/init/</copy>
        
2.  在「程式碼編輯器」中，您可以檢視 _init_ 資料夾中的各種檔案。這些是建立區間、動態群組、使用者群組和原則的地形命令檔。 ![init 檔案](images/init-files.png)
    
3.  複製並貼上下列命令，以佈建區間、動態群組、使用者群組以及原則。
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    您會看到類似下方的執行結果。請**觀察輸出**以了解 terraform 指令碼的建立內容。另外，您可以參照程式碼來查看它的實行。 ![已建立初始](images/init-created.png)
    
    > 如果有任何錯誤，請檢查以確定您已在 **terraform.tfvars** 檔案中正確設定這些值。
    

## 任務 3：建立 DevOps 專案及其資源

1.  在終端機中，複製並貼上下列指令以瀏覽至 _main_ 資料夾。
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main/</copy>
        
2.  複製並貼上下列命令，以更新 **`devops_helidon_to_instance_ocw_hol`** 資料夾中 _terraform.tfvars_ 中新建立之區間的 _compartment\_ocid_ 。
    
        <copy>../utils/update_compartment.sh</copy>
        
3.  在終端機中複製並貼上下列命令，以佈建所有 DevOps 資源。
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    > **請閱讀：-** 這會提供下列 DevOps 所需的資源：
    
    *   **OCI DevOps 服務**
        *   **OCI DevOps 專案**包含此專案所需的所有 DevOps 元件。
        *   代管應用程式原始程式碼專案的 **OCI 程式碼儲存區域**。
        *   **DevOps 組建管線**具有下列階段：
            *   **管理組建** - 執行下載 JDK20、maven 以及建置 Helidon 應用程式的步驟
            *   **傳遞使用者自建物件** - 將建立的 Helidon App 和部署上傳至使用者自建物件儲存區域
            *   **觸發部署** - 觸發部署業務進程
        *   將在目標環境執行下列作業的 **DevOps 部署管線**：
            *   下載 JDK20
            *   安裝 OCI CLI 並用於下載應用程式交付項目
            *   執行應用程式
        *   部署管線將使用的 **DevOps 執行處理群組環境**，以識別建立的 OCI Compute 執行處理作為部署目標。
        *   **DevOps 觸發程式**：當 OCI 程式碼儲存區域發生推送事件時，將會從頭到尾呼叫管線週期。
    *   **OCI 物件登錄**
        *   **OCI 使用者自建物件儲存區域**，會將建立的 Helidon 應用程式二進位檔案和部署資訊清單代管為啟動多版本功能的使用者自建物件。
    *   **OCI 平台**
        *   **OCI Compute 執行處理**，可從防火牆開啟連接埠 8080。這是最終部署應用程式的位置。
    *   **具備安全清單的 OCI 虛擬雲端網路 (VCN)** 包含開啟連接埠 8080 的傳入。連接埠 8080 是將從其中存取 Helidon 應用程式的位置。OCI Compute 執行處理將使用 OCI VCN 來滿足其網路需求。
4.  下圖說明 DevOps 設定如何運作： ![裝置圖](images/devops-diagram.png)
    
5.  您會得到如下所示的類似輸出。 ![tf 輸出](images/tf-output.png)
    

您現在可以**繼續進行下一個實驗室。**

## 確認

*   **作者** - Keith Lustria
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月