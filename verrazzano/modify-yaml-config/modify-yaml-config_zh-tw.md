# 修改 Bobby 的書籍組態 YAML 檔案

## 簡介

在實驗室 6 中，我們為 _bobby-helidon-stock-application_ 推出更新後的 Docker 映像檔。現在，我們希望 Verrazzano 重新部署更新的應用程式與元件，而不會影響服務。因此，我們需要設定 YAML 檔案，讓 Verrazzano 選擇新影像並為其啟動 Pod。Pod 處於_執行中_狀態之後，就會終止與先前應用程式和元件關聯的 Pod。

預估時間：10 分鐘

### 目標

在此實驗室中，您將：

*   修改 `bobs-books-comp.yaml` 檔案。
*   使用 `kubectl` 套用變更。

### 先決條件

您應該有一個文字編輯器，您可以在其中根據您的環境貼上指令和 URL 並加以修改。接著您可以複製並貼上修改的指令，以便在 _Cloud Shell_ 中執行這些指令。

## 工作 1：修改 bobs-books-comp.yaml 檔案

1.  我們有一個應用程式組態檔 _bobs-books-comp.yaml_ 。在 Lab 2 中，我們下載了應用程式 yaml 檔案。若要變更為包含 yaml 檔案的本位目錄，請複製下列命令並將它貼到 _Cloud Shell_ 中。
    
        <copy>cd ~</copy>
        
2.  在此位置中，我們有 Bobs-books 應用程式的組態檔。在實驗室 5 的過程中，我們修改了 bobbys-helidon-stock-application，並為該元件建置新的 Docker 映像檔。在實驗室 6 中，我們會將該 Docker 映像檔推送至 Oracle Cloud Container Registry 儲存區域。現在，在此實驗室中，我們將修改 _bobs-books-comp.yaml_ 檔案，以便從 Oracle Cloud Container Registry 儲存區域取得新的更新 Docker 映像檔。若要修改 _bobs-books-comp.yaml_ 檔案，請複製下列指令，然後貼到 _Cloud Shell_ 中。
    
        <copy>vi bobs-books-comp.yaml</copy>
        
    
    ![開啟檔案](images/openfile.png " ")
    
3.  在實驗室 5 中，您已儲存 Docker 映像檔全名。您需要複製下列行並將它貼到文字編輯器中。接著，您必須將 `docker image full name` 取代為您的 Docker 映像檔名稱。然後複製修改過的行，然後按 _i_ 將文字插入 `*bobs-books-comp.yaml*` 檔案中。在行號 148 貼上輸出 (確定您保持縮排)，並註解結束行號 147 與 _#_ (如下圖所示)，然後按 _Esc_ ，然後鍵入 _：wq_ 儲存檔案。
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![插入明細行](images/insert-line.png " ")
    

## 作業 2：使用 `kubectl` 套用變更

1.  若要套用變更，請在 _Cloud Shell_ 複製並貼上下列命令。當您套用變更時，新的 Pod 將會起始服務新元件要求時，而與舊元件關聯的 Pod 將會繼續提供服務要求。之後，新 Pod 將會達到_執行中_ 狀態，而舊的 Pod 將會開始_終止_。最後，只有新的 Pod 會處於_執行中_狀態。
    
        <copy>kubectl apply -f bobs-books-comp.yaml -n bobs-books</copy>
        
    
    執行結果應該和下列類似：
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh unchanged
        component.core.oam.dev/robert-helidon unchanged
        component.core.oam.dev/bobby-coh unchanged
        component.core.oam.dev/bobby-helidon configured
        component.core.oam.dev/bobby-wls unchanged
        component.core.oam.dev/bobs-mysql-configmap unchanged
        component.core.oam.dev/bobs-mysql-service unchanged
        component.core.oam.dev/bobs-mysql-deployment unchanged
        component.core.oam.dev/bobs-orders-configmap unchanged
        component.core.oam.dev/bobs-orders-wls unchanged
        $
        
    
    您只能在輸出中監看；只設定了 _component.core.oam.dev/bobby-helidon_ ，其他的元件則維持不變。
    
2.  若要檢視新的 Pod 如何起始，而舊的 Pod 會處於_終止_狀態，請在 _Cloud Shell_ 中複製並貼上下列命令。
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    您會看到類似以下的執行結果：
    
        $ kubectl get pods -n bobs-books
        NAME                                         READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                           2/2    Running      0         130m
        bobbys-front-end-adminserver                 4/4    Running      0         127m
        bobbys-front-end-managed-server1             4/4    Running      0         126m
        bobbys-helidon-stock-application-64fb55-zzp  0/2    PodInitializing  0     10s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Running      0         130m
        bobs-bookstore-adminserver                   4/4    Running      0         127m
        bobs-bookstore-managed-server1               4/4    Running      0         126m
        mysql-65d864bf8c-xf64p                       2/2    Running      0         130m
        robert-helidon-bfdfb58b8-58qfs               2/2    Running      0         130m
        robert-helidon-bfdfb58b8-lkw8m               2/2    Running      0         130m
        roberts-coherence-0                          2/2    Running      0         130m
        roberts-coherence-1                          2/2    Running      0         130m
        bobbys-helidon-stock-application-64fb55-zzp  1/2    Running      0         28s
        bobbys-helidon-stock-application-64fb55-zzp  2/2    Running      0         34s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        $
        
    
    看到所有的 Pod 都處於_執行中_ 狀態之後，請按 _CTRL + C_ 終止此命令。
    
    歡迎加入 _Cloud Shell_ ，因為我們也在最後一個實驗室中也需要此功能。
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月