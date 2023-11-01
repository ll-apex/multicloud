# 簡介

## 關於此研討會

在此實驗室中，您將使用 Oracle Code Editor 來建立、執行及修改 Helidon 4 中提供的 Helidon Nima WebServer 微服務。Nima 從頭開始撰寫以運用 Java 19 的虛擬執行緒，並提供簡單的封鎖程式設計模型。

您也將瞭解相較於更複雜的反應式程式設計方式。

在此實驗室中，您將使用 Helidon Project Starter 來開發 Helidon Microprofile 應用程式。它提供高度客製化的各種選項，可讓使用者選取想要新增至專案的 Helidon 功能。接著，您將使用虛擬執行緒，在新 Helidon Nima WebServer 上執行的應用程式移轉至 Helidon 4。

本工作坊旨在盡量提供自我說明，但歡迎隨時向閣下查詢有關詳情或協助。

預估時間：90 分鐘

### 目標

*   探索 OCI 程式碼編輯器
*   建置及執行 Nima 應用程式
*   建置及執行反應式應用系統
*   分析 Nima 應用程式的簡易性
*   比較 Nima 與反應應用程式的堆疊追蹤
*   產生 Helidon MP 應用程式
*   將 Helidon 3 應用程式移轉至 Helidon 4

**關於 Helidon Nima**

Helidon 是 Oracle 引進的開放原始碼微服務架構，提供一系列專為建立輕量而快速的微服務型應用而設計的 Java 程式庫。

在舊版的 Helidon 開發人員可以從兩個程式設計模型中挑選出來。[Helidon MP](https://helidon.io/docs/v3/#/mp/introduction) 導入 Eclipse MicroProfile 標準，其中包含熟悉 Jakarta EE 開發人員的 API。而 [Helidon SE](https://helidon.io/docs/v3/#/se/introduction) 是一個精實、反應性的框架。

在 Helidon 4 中，Oracle 引進 Níma，這是從基礎撰寫的 Web 伺服器，使用 JEP 425 虛擬執行緒。Nima 提供了簡單易用的程式設計模型，效能卓越。使用虛擬執行緒時，執行緒不再是要小心集中和管理的稀疏資源。而是可以根據需要建立大量資源來處理幾乎不受限制的並行要求。因為每個要求都在其專用繫線中執行，所以您可以自由執行阻隔作業，例如呼叫資料庫或其他服務。而且還能以簡單的同步方式進行，無須擔心是否會封鎖平台執行緒，以及阻礙其他要求。您不再需要重新排序複雜的非同步程式碼，才能實行低負荷、高度並行的服務。

Helidon Níma Web 伺服器打算取代 Helidon 生態系統的 Netty。其他架構也可用作內嵌 Web 伺服器元件。

### 先決條件

此實驗室假設您具有：

*   Oracle Cloud 帳戶

## 進一步瞭解

*   [https://helidon.io](https://helidon.io)

## 確認

*   **作者** - Joe DiPol
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 2 月