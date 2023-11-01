# ALTERNATE VERSION 簡化解決方案

## 簡介

在此實驗室中，我們將使用修改過的 bobbys-helidon-stock-application 影像。此映像檔儲存在 Oracle Cloud Container Registry 中的公用儲存區域中。我們也使用修改過的 bobs-books-comp-mod.yaml 檔案，它指向修改過的 bobbys-helidon-stock-application 影像。

### 目標

在此實驗室中，您將：

*   下載修改過的 bobs-books-comp-mod.yaml 檔案。
*   使用 kubectl 套用變更

### 先決條件

*   執行實驗室 1，在 Oracle Cloud Infrastructure 上建立 OKE 叢集。
*   執行實驗室 2，在 Kubernetes 叢集上安裝 Verrazzano。
*   執行建置 Bobs-Books 應用程式的 Lab 3。
*   您應該有一個文字編輯器，您可以在其中根據您的環境貼上指令和 URL 並加以修改。接著您可以複製並貼上修改的指令，以便在 _Cloud Shell_ 中執行這些指令。

## 工作 1：下載修改過的 bobs-books-comp-mod.yaml 檔案

1.  執行下列命令以下載修改過的 bobs-books-comp-mod.yaml 檔案。
    
        <copy>curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/alternate-version/bobs-books-comp-mod.yaml >~/bobs-books-comp-mod.yaml</copy>
        
    
    ![下載檔案](images/downloadfile.png " ")
    

## 作業 2：使用 kubectl 套用變更

1.  若要套用變更，請在 _Cloud Shell_ 複製並貼上下列命令。當您套用變更時，新的 Pod 將會起始服務新元件要求時，而與舊元件關聯的 Pod 將會繼續提供服務要求。之後，新 Pod 將會達到_執行中_ 狀態，而舊的 Pod 將會開始_終止_。最後，只有新的 Pod 會處於_執行中_狀態。
    
        <copy>kubectl apply -f ~/bobs-books-comp-mod.yaml -n bobs-books</copy>
        
    
    ![套用變更](images/applychanges.png " ")
    
    您只能在輸出中監看；只設定了 _component.core.oam.dev/bobby-helidon_ ，其他的元件則維持不變。
    
2.  若要檢視新的 Pod 如何起始，而舊的 Pod 會處於_終止_狀態，請在 _Cloud Shell_ 中複製並貼上下列命令。
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    您會看到類似以下的執行結果：
    
        vera_zano@cloudshell:~ (us-ashburn-1)$ kubectl get pods -n bobs-books
        NAME                                               READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                                 2/2    Running  0         130m
        bobbys-front-end-adminserver                       4/4    Running  0         127m
        bobbys-front-end-managed-server1                   4/4    Running  0         126m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  0/2    PodInitializing  0         10s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Running  0         130m
        bobs-bookstore-adminserver                         4/4    Running  0         127m
        bobs-bookstore-managed-server1                     4/4    Running  0         126m
        mysql-65d864bf8c-xf64p                             2/2    Running  0         130m
        robert-helidon-bfdfb58b8-58qfs                     2/2    Running  0         130m
        robert-helidon-bfdfb58b8-lkw8m                     2/2    Running  0         130m
        roberts-coherence-0                                2/2    Running  0         130m
        roberts-coherence-1                                2/2    Running  0         130m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  1/2    Running  0         28s
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  2/2    Running  0         34s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        vera_zano@cloudshell:~ (us-ashburn-1)$
        
    
    看到所有的 Pod 都處於_執行中_ 狀態之後，請按 _CTRL + C_ 終止此命令。
    

歡迎加入 _Cloud Shell_ ，因為我們也在最後一個實驗室中也需要此功能。

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 2 月