# 修改 WKT UI 專案並建立模型檔案

## 簡介

在此實驗室中，我們將探索企業內部部署的 WebLogic 網域。我們會瀏覽整個管理主控台以檢視 _test-domain_ 中部署的應用程式、資料來源和伺服器。我們也會開啟預先建立的 _`base_project.wktproj`_ ，此預先建立的值已預先填入 _Project Settings_ 區段。接著，我們會介紹離線企業內部部署網域，建立模型檔案。最後，我們會驗證模型，並準備要部署在 Oracle Kubernetes 叢集 (OKE) 上的模型。

預估時間：15 分鐘

### 目標

在此實驗室中，您將：

*   探索內部部署 WebLogic 網域 _test-domain_ 。
*   開啟基本 WKT 專案。
*   自我檢查離線內部部署網域。
*   驗證並準備模型。

### 先決條件

若要執行此實驗室，您必須具備：

*   存取實驗室 2 中建立的 noVNC 遠端桌面。

## 作業 1：探索企業內部部署網域

在這項任務中，我們使用 WebLogic Administration 主控台來瀏覽內部部署 _test-domain_ 中的資源。

1.  在左側，按一下_箭頭圖示_。 ![剪貼簿](images/clipboard.png)

> **重要事項** - 您可以參閱_剪貼簿_，在主機機器和遠端桌面之間複製和貼上，使用_剪貼簿_。例如，如果您要從主機機器複製並要將它貼到遠端桌面，則必須先在剪貼簿中貼上，然後再將它貼到遠端桌面。再次按一下_箭頭圖示_以隱藏_設定值_選項。

2.  按一下 _Oracle WebLogic Server_ 頁籤並輸入 _weblogic/Welcome1%_ 作為 `Username/Password`，然後按一下_登入_。請參閱我們的 WebLogic 伺服器版本 _12.2.1.3.0_ 。  
    ![登入管理主控台](images/login-admin-console.png)
    
3.  若要檢視可用的伺服器，請展開_環境_，然後按一下_伺服器_。您可以看到一個含有 5 個受管理伺服器的動態叢集。 ![檢視伺服器](images/view-servers.png)
    
4.  若要檢視資料來源，請展開_服務_，然後按一下_資料來源_。 ![檢視資料來源](images/view-datasources.png)
    
5.  若要檢視已部署的應用程式，請按一下_部署_。您可以看到 _opdemo_ 作為已部署的應用程式。 ![檢視部署](images/view-deployments.png)
    

## 作業 2：開啟基本 WKT UI 專案

為了簡化實驗室，我們建立了 _`base_project.wktproj`_ ，並預先設定了 docker、Java、Oracle 本位目錄、主要映像檔標記的位置。在此任務中，我們開啟了 _`base_project.wktproj`_ 專案。

1.  按一下_活動_，然後在搜尋方塊中輸入 **WebLogic** 。按一下 _WebLogic Kubernetes Toolkit UI_ 的圖示。 ![開啟 WKTUI](images/open-wktui.png)
    
2.  若要開啟 _base\_project.wktproj_ 專案，請按一下 \[ _檔案_ \] -> \[ _開啟專案_ \]。 ![開啟專案](images/open-project.png)
    
3.  按一下左側的_下載_，然後選擇 _base\_project.wktproj_ ，然後按一下_開啟專案_。 ![專案位置](images/project-location.png)
    
    > **For your information only:**  
    > As _Credential Story Policy_, we select **Store in Native OS Credentials Store**. It means the credentials (like for WebLogic Server and datasources) are only stored on the local machine.  
    > For _Where would you like the target Oracle Fusion Middleware domain to live?_, we select **Created in the container from the model in the image**. In this case, the set of model-related files are added to the image. So when the WebLogic Kubernetes Operator domain object is deployed, its inspector process runs and creates the WebLogic Server domain inside a running container on-the-fly.  
    > ![專案設定值](images/project-settings.png) As _Kubernetes Environment Target Type_, we select **WebLogic Kubernetes Operator**. This means, you want this domain to be deployed in Kubernetes managed by the WebLogic Kubernetes Operator. This settings also determine what sections and their associated actions within the application, to display.  
    > we also specify the location for _JAVA HOME_ and _ORACLE\_HOME_. WebLogic Kubernetes Toolkit UI uses this directory when invoking the WebLogic Deployer Tooling and WebLogic Image Tool.  
    > To build new images, inspect images and interact with image repositories, the WKT UI application uses an image build tool, which defaults to docker.  
    > ![Kubernetes 叢集類型](images/kubernetes-cluster-type.png)
    
4.  以 _Password_ 身分輸入 **welcome1** ，然後按一下_解除鎖定_。 ![解除鎖定](images/unlock.png)
    

## 作業 3：檢查離線內部部署網域

在這項任務中，我們會執行內部部署網域的自我檢查，這會建立一個由網域組態組成的模型檔案。

1.  在 WebLogic Kubernetes Toolkit UI 中，按一下_模型_。 ![模型](images/click-model.png)
    
2.  按一下 \[ _檔案_ \] -> \[ _新增模型_ \] -> \[ _尋找模型 (離線)_ \]。 ![尋找模型](images/discover-model.png)
    
3.  按一下「開啟」資料夾_圖示_即可開啟_網域本位目錄_。 ![開啟網域首頁](images/open-domain-home.png)
    
4.  在 \[Home\] (個人) 資料夾中，瀏覽至 _`/home/opc/Oracle/Middleware/Oracle_Home/user_projects/domains/`_ 目錄並選取 _test-domain_ 資料夾，然後按一下 \[ _選取_ \]。按一下_確定_。![導覽位置](images/navigate-location.png) ![指定位置](images/specify-location.png)
    
    > 如果您在主控台中查看，將會看到這將會呼叫 WebLogic Deployer Tool 以離線模式檢查網域組態。
    
5.  您可以看到視窗，如下所示，最後，您的模型已就緒。 ![檢視模型](images/view-model.png)
    
    > 此 WDT 自我檢查的結果是模型 (網域組態的描述資料表示法)、預留位置，您可以在其中指定應用程式存檔中的值 (例如資料來源的密碼) 和應用程式。
    

## 作業 4：驗證與準備模型

在這項任務中，我們會驗證模型並準備要部署在 Oracle Kubernetes 叢集 (OKE) 上的模型。

1.  按一下_程式碼檢視_並驗證模型，按一下_驗證模型_。 ![驗證模型](images/validate-model.png)
    
    > **僅供參考：**  
    > 驗證模型會呼叫 WDT [驗證模型工具](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/validate/)，驗證模型及其相關構件的格式是否正確，並提供特定模型位置之有效屬性與子資料夾的說明。
    
2.  看到_驗證模型完成_視窗之後，按一下_確定_。 ![驗證完成](images/validate-complete.png)
    
3.  若要準備要部署在 Kubernetes 叢集上的模型，請按一下_準備模型_ ![準備模型](images/prepare-model.png)
    
    > **僅供參考：**  
    > 準備模型會呼叫 WDT [準備模型工具](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/prepare/)，以透過安裝 WebLogic Kubernetes Operator 或 Verrazzano 來修改模型以在 Kubernetes 叢集中運作。  
    > 「準備模型」會執行下列作業：
    
    *   移除與目標環境不相容的模型區段與欄位。
    *   以參照變數的模型記號取代端點值。
    *   以參照 Kubernetes 密碼或變數中的欄位的模型記號取代證明資料值。
    *   提供應用程式變數、變數覆寫和秘密編輯程式中顯示之欄位的預設值。
    *   將拓樸資訊擷取至其用來產生用來部署網域之資源檔案的應用程式。
4.  看到_準備模型完成_視窗之後，按一下_確定_。 ![準備完成](images/prepare-complete.png)
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 10 月