# 簡介

## 關於此研討會

在此實驗室中，您將使用 Helidon Project Starter 來開發 HTTP 微服務應用程式。它提供高度客製化的各種選項，可讓使用者選取想要新增至專案的 Helidon 功能。

然後，您將探索 OCI 程式碼編輯器，並在其中開啟 Helidon 應用程式。此外，您也將瞭解如何在 Oracle Cloud Infrastructure (OCI) Cloud Shell 中使用 GraalVM Enterprise Edition。

您將使用 maven 建立 Helidon MP 應用程式的 GraalVM 原生影像。然後您將在現有的 Java 類別中新增自訂端點來修改應用程式。之後，您將建立此應用程式的原生 docker 映像檔，並推送至 Oracle Cloud Container Image Registry。您也將建立一個運算執行處理，然後在此處提取 docker 映像檔，然後執行此應用程式的 docker 容器。

本工作坊旨在盡量提供自我說明，但歡迎隨時向閣下查詢有關詳情或協助。

預估時間：90 分鐘

### 目標

*   產生 Helidon MP 應用程式
*   探索 OCI 程式碼編輯器
*   為 Helidon MP 應用程式建立原生影像
*   建立 Helidon 應用程式的 GraalVM 原生 docker 映像檔
*   建立虛擬機器
*   執行 Helidon MP 應用程式的 docker 容器

### 先決條件

此實驗室假設您具有：

*   Oracle Cloud 帳戶

## 關於 Helidon

Helidon 是 Oracle 引進的開放原始碼微服務架構，提供一系列專為建立輕量而快速的微服務型應用軟體而設計的 Java 程式庫。此架構支援兩種編寫微服務的程式設計模型：Helidon SE 與 Helidon MP。

雖然 Helidon SE 是設計為支援反應式程式設計模型的微架構，但 Helidon MP 是 MicroProfile 規格的導入。由於 MicroProfile 的根源在 Java EE 中，因此 MicroProfile API 採用熟悉的宣告式方法，使用大量註解。這對 Java EE 開發人員而言是個不錯的選擇。

MicroProfile 的特色旨在執行微服務。您可以尋找用來定義 REST 用戶端、監控應用程式、讀取技術與功能統計資料的 API，以及設定應用程式。Helidon 也將額外的 API 加入核心 Microprofile API 組，提供您撰寫現代化雲端原生應用程式所需的所有功能。

> Jakarta EE 上建置 [MicroProfile](https://microprofile.io/) 標準版。就像 Jakarta EE 一樣，MicroProfile 是開放原始碼，由 Eclipse Foundation 開發。使用 MicroProfile 進行建置時，會使用實作標準的程式庫或應用程式伺服器，就像 Jakarta EE 一樣。

## 進一步瞭解

*   [https://helidon.io](https://helidon.io)

## 確認

*   **作者** - Dmitry Aleksandrov
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 4 月