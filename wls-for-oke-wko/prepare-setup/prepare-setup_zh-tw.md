# 準備設定

## 簡介

這個實驗室將會示範如何下載設定執行此研討會所需資源所需的 Oracle Resource Manager (ORM) 堆疊壓縮檔。接著建立一個運算執行處理和一個虛擬雲端網路 (VCN)，讓您能夠存取遠端桌面。

預估時間：10 分鐘

### 目標

*   下載 ORM 堆疊
*   使用資源管理程式堆疊建立運算 + 網路

### 先決條件

此實驗室假設您具有：

*   Oracle Free Tier 或 Paid Cloud 帳戶

## 作業 1：下載 Oracle Resource Manager (ORM) 堆疊壓縮檔

1.  按一下下方連結以下載建立環境所需的資源管理程式壓縮檔：
    
    _注意事項 1：_如果提供研討會的單一堆疊下載，請使用此簡單表示式。
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  儲存至您的下載項目資料夾。
    

## 作業 2：建立堆疊：運算 + 網路

1.  識別在**作業 1：下載 Oracle Resource Manager (ORM) 堆疊壓縮檔**中下載的 ORM 堆疊壓縮檔。
    
2.  開啟左上角的漢堡選單。按一下**開發人員服務 (Developer Services)** ，然後選擇**資源管理程式 (Resource Manager)** \> **堆疊 (Stacks)** 。選擇要安裝此堆疊的區間。按一下**建立堆疊**。![選項單堆疊](images/menu-stack.png) ![選取區間](images/select-compartment.png)
    
3.  選取**我的組態**，選擇**。Zip** 檔案按鈕，按一下**瀏覽**連結，然後選取您為檔案總管下載或拖放的 zip 檔案。按一下**下一步 (Next)** 。 ![瀏覽 zip](images/browse-zip.png)
    
4.  輸入或選取下列項目，然後按一下**下一步**。
    
    **執行處理計數：**接受預設值 1。
    
    **選取可用性網域：**從下拉式清單中選取可用性網域。
    
    **需要透過 SSH 進行遠端存取？**「不勾選遠端桌面存取」-「預設」。
    
    **使用可調整 OCPU 數目彈性執行處理資源配置嗎？：**將預設值維持勾選狀態 (除非計畫使用固定資源配置)。
    
    **執行處理資源配置：** 保留預設值，或從下拉式功能表中的 Flex 資源配置清單中選取 (例如 VM.Standard.E4) . 彈性)。
    
    **選取每一執行處理的 OCPU 數目：**接受顯示的預設值。例如 (1) 將佈建 1 個 OCPU 和 16GB 的記憶體。
    
    **使用現有的 VCN？：**若未勾選，請接受預設值。這會建立新的 VCN。![主組態](images/main-config.png) ![實例形狀](images/instance-shape.png)
    
5.  選取**執行套用 (Run Apply)** ，然後按一下**建立 (Create)** 。 ![執行套用](images/run-apply.png)
    

您現在可以繼續下一個實驗室。

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 10 月