# 建立輔助映像檔並將其推送至 Oracle Container Image Registry

## 簡介

**主要映像檔** - 包含 Oracle Fusion Middleware 軟體的映像檔。它被用來作為所有執行網域 WebLogic 伺服器的容器的基礎。

**輔助影像** - 提供 WebLogic Deploy Tooling 軟體和模型檔案的影像。在執行階段，輔助影像的內容會與主要影像的內容合併。![影像結構](images/image-structure.png)

在此實驗室中，我們使用 WebLogic server 12.2.1.3.0-ol8 影像作為主要影像。此外，我們也會建立輔助映像檔，然後使用產生的認證權杖將它推送至 Oracle Container Image Registry 儲存區域。

預估時間：10 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[建立 OCI 上 OKE 的映像檔](videohub:1_y5o56oe5)

### 目標

在此實驗室中，您將：

*   建立輔助映像檔並將映像檔推送至 Oracle Cloud Container Image Registry。

## 作業 1：準備輔助映像檔並推送輔助映像檔

在這項任務中，我們正在建立一個輔助映像檔，系統會推送至 Oracle Cloud Container Registry。

1.  按一下_影像_。對於主要映像檔，我們將使用下方的 _weblogic_ Image.So，將預設值保留在_主要映像檔_區段下方，如
    
        <copy>container-registry.oracle.com/middleware/weblogic:12.2.1.3-ol8</copy>
        
    
    ![主要影像](images/primary-image.png)
    
    > **僅供參考：**  
    > 主要影像是用於執行網域的影像。數百個網域可以重複使用一個主要映像檔。主映像檔包含作業系統、JDK 和 FMW 軟體安裝。
    
2.  若要建立輔助映像檔標記，需要以下資訊：
    
    *   區域的端點
    *   租用戶命名空間
3.  尋找_您區域的端點_。請參閱位於此 URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) 的表格。在顯示範例中，區域的端點是 _UK South (London)_ (作為區域名稱)，而其端點是 _lhr.ocir.io_ 。尋找您自己的_區域名稱_端點，然後將它儲存在文字檔中。您也需要這個實驗室才能上手。
    
    ![端點](images/end-point.png " ")
    
    > 您現在已同時具有區域的租用戶命名空間和端點。
    
4.  在實驗室 3 中，您已經注意到文字檔中的租用戶命名空間。如果不是，則若要尋找租用戶的命名空間，請選取 \[ _漢堡功能表_ \] -> \[ _開發人員服務_ \] -> \[ _容器登錄_ \]，如下所示。選取您建立的儲存區域，您將會找到顯示的命名空間。 ![租用戶命名空間](images/tenancy-namespace.png)
    
5.  您現在已同時具有區域的租用戶命名空間和端點。複製下列命令並將它貼到您的文字檔中。然後將 `END_POINT_OF_YOUR_REGION` 取代為您區域名稱的端點，也就是使用您租用戶的命名空間來取代 `NAMESPACE_OF_YOUR_TENANCY`。按一下_輔助影像_頁籤，如下所示。 ![輔助頁標](images/auxiliary-tab.png)
    
        <copy>END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/test-model-your_first_name:v1</copy>
        

> 例如，在我的案例中，輔助影像標記為 `lhr.ocir.io/tenancynamespace/test-model-ankit:v1`。

6.  在步驟 4 中，您也決定了租用戶命名空間。輸入「輔助影像登錄推送使用者名稱」，如下所示：`NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`。  
    

*   將 `NAMESPACE_OF_YOUR_TENANCY` 取代為租用戶的命名空間
*   將 `YOUR_ORACLE_CLOUD_USERNAME` 取代為您的 Oracle Cloud 帳戶使用者名稱，然後從您的文字檔複製取代的使用者名稱，然後將其貼到_輔助映像檔登錄推送使用者名稱_中。

> 例如，在我的情況下，**輔助影像登錄推送使用者名稱**為 `tenancynamespace/lab.user@oracle.com`。

*   若為密碼，請從您的文字檔 (或任何您儲存的位置) 複製並貼上認證權杖，然後將其貼到**輔助映像檔登錄推送使用者名稱**中。 ![輔助影像詳細資訊](images/auxiliary-image-details.png)

7.  按一下_建立輔助影像_。 ![建立輔助映像檔](images/create-auxiliary-image.png)
    
8.  隨著我們已在實驗室 2 中準備模型，請按一下_否_。 ![準備模型](images/prepare-model.png)
    
9.  選取要儲存 _WebLogic 建置程式_的_下載_資料夾，然後按一下_選取_ (如圖所示)。 ![WDT 地點](images/wdt-location.png)
    
10.  成功建立輔助影像之後，請在_建立輔助影像完成_視窗上，按一下_確定_。 ![已建立輔助](images/auxiliary-created.png)
    
    > **僅供參考：**  
    > 輔助影像是網域特定。輔助映像檔包含定義網域的資料。
    
11.  按一下_推送輔助映像檔_，將映像檔推送至您 Oracle Cloud Container Image Registry 的儲存區域內。 ![推送輔助](images/push-auxiliary.png)
    
12.  順利推送影像之後，請在_推送影像完成_視窗上，按一下_確定_。 ![輔助推動](images/auxiliary-pushed.png)
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月