# 透過應用程式存取和曲線堆疊驗證變更

## 簡介

在實驗室 7 中，我們套用了 bobbys-helidon-stock-application 的變更，以及處於_執行中_狀態的 Pod。在此實驗室中，我們將驗證應用程式、Verrazzano 主控台及 Grafana 主控台的變更。

估計時間：05 分鐘

### 目標

在此實驗室中，您將：

*   驗證 Bobby 之 Books 應用程式中的變更。
*   驗證 Grafana 主控台中的變更。

### 先決條件

*   您應該有一個文字編輯器，您可以在其中根據您的環境貼上指令和 URL 並加以修改。接著您可以複製並貼上修改的指令，以便在 _Cloud Shell_ 中執行這些指令。

## 作業 1：驗證 Bobby 之報表簿應用程式的變更

1.  開啟 Bobby 的「報表簿」頁籤，然後選取「重新整理」。如果您已關閉該頁標，請在文字編輯器中複製並貼上下列指令，然後以應用程式的 EXTERNAL\_IP 取代 XX.XX.XX.XX。您會注意到「工作簿名稱」全都是大寫字母。
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![Bobby 的書](images/bobbysbooks.png " ")
    

## 作業 2：檢查 Grafana 主控台中的變更

1.  返回 Verrazzano 首頁。
    
    ![Verrazzano 首頁](images/verrazzao-home.png " ")
    
2.  選取 Grafana 的連結，以開啟 _Grafana 主控台_。
    
    ![Grafana 連結](images/grafana-link.png " ")
    
3.  選取_首頁_，輸入 _Helidon_ ，然後選取 _Helidon 監控儀表板_。
    
    ![搜尋 Helidon](images/search-helidon.png " ")
    
4.  在 ServiceID 中，選取 _bobs-books\_default\_bobs-books\_bobby-helidon_ ，然後在執行處理中選取新建立的執行處理。在此情況下，您將會取得修改後的 _bobby-helidon-stock-application_ 資訊。
    
    ![新元件](images/new-component.png " ")
    
    恭喜！您已順利完成實驗室。
    

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月