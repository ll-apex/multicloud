# 準備設定

## 簡介

這個實驗室將會示範如何下載設定執行此研討會所需資源所需的 Oracle Resource Manager (ORM) 堆疊壓縮檔。這個研討會需要一個運算執行處理和虛擬雲端網路 (VCN)。

預估時間：10 分鐘

### 目標

*   下載 ORM 堆疊
*   設定現有的虛擬雲端網路 (VCN)

### 先決條件

此實驗室假設您具有：

*   Oracle Free Tier 或 Paid Cloud 帳戶

## 作業 1：下載 Oracle Resource Manager (ORM) 堆疊壓縮檔

1.  按一下下方連結以下載建立環境所需的資源管理程式壓縮檔：
    
    _注意事項 1：_如果提供研討會的單一堆疊下載，請使用此簡單表示式。
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  儲存至您的下載項目資料夾。
    

強烈建議使用此堆疊在您的執行處理建立獨立 / 專用的 VCN。請跳至_任務 3_ 以追蹤我們的建議。如果想要使用現有的 VCN，請依照下方指示繼續進行下一個步驟，使用所需的輸出規則更新現有的 VCN。

## 作業 2：將安全規則新增至現有的 VCN

此研討會需要使用特定數量的連接埠，並要求使用建立專用 VCN 的預設 ORM 堆疊執行來滿足此要求。若要使用現有的 VCN，必須將下列連接埠新增至傳入規則。

| 連接埠 | 描述 |
| :-- | :-- |
| 22 日 | SSH |
| 80 歲 | noVNC 遠端桌面 (NGINX 代理) |
| 6080 年 | noVNC 遠端桌面 |

1.  前往_網路 >> 虛擬雲端網路_。
2.  選擇您的網路。
3.  在「資源」下，選取「安全性清單」。
4.  按一下「建立安全清單」按鈕底下的「預設安全清單」
5.  按一下「新增傳入規則」按鈕
6.  輸入下列資訊：
    *   來源 CIDR：0.0.0.0/0
    *   目的地連接埠範圍：_請參閱上面表格_
7.  按一下「新增輸入規則」按鈕。

## 任務 3：設定運算

使用上述兩項工作的詳細資訊，前往_環境設定_實驗室，使用 Oracle Resource Manager (ORM) 和下列其中一個選項設定您的研討會環境：

*   建立堆疊：_運算 + 網路_
*   建立堆疊：使用現有 VCN 進行_僅限運算_，根據上述_任務 2_ 的安全清單已更新

您現在可以繼續下一個實驗室。

## 確認

*   **作者** - 北美科技公司 LiveLabs 平台主管 Rene Fontcha
*   **貢獻者** - Meghana Banka
*   **上次更新者 / 日期** - 北美技術 2022 年 2 月 LiveLabs 平台主管 Rene Fontcha