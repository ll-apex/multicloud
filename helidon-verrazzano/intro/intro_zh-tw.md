# 簡介

## 關於此研討會

這個實驗室會為參與者提供 Helidon 和 Verrazzano 的入門級實作課程。您將會瞭解如何建立及組合服務，以及在企業容器平台上部署這些服務。

這是 BYOL (內含筆記型電腦) 作業階段，讓您的 Windows、OSX 或 Linux 筆記型電腦輕鬆上手。開始之前，您必須先安裝 JDK 11+、Apache Maven (3.6) 和 Docker。

在此實驗室中，您將安裝 Helidon CLI 工具並開發 HTTP 微服務應用程式。針對 Verrazzano，您將使用 Oracle Cloud Free Tier 帳戶在 Oracle Cloud Infrastructure 上設定 Oracle Kubernetes Engine (OKE)。Free Tier 帳戶已足以探索和了解如何在企業層級上執行和操作微服務應用程式。

此研討會的目標是您瞭解使用 Helidon 和 Verrazzano 的基本知識，並瞭解它們如何協助您進行專案。如果您想要深入瞭解這些專案的功能，請繼續探索使用 Oracle Free Tier 雲端帳戶和 Oracle Cloud Infrastructure 的功能。

本工作坊旨在盡量提供自我說明，但歡迎隨時向閣下查詢有關詳情或協助。

> 需要數分鐘來佈建 Oracle Kubernetes Engine (OKE) 和安裝 Verrazzano。為了節省時間，系統會要求您平行執行開發和環境設定。遵循指示並在必要時切換任務。

預估時間：90 分鐘

### 目標

*   設定您的 Oracle Cloud Free Tier 帳戶 (如果您尚未註冊)。
*   在 Oracle Cloud Infrastructure 上設定 Oracle Kubernetes Engine 執行處理。
*   安裝 Verrazzano 平台。
*   部署 _Helidon MP_ 應用程式。
*   使用 Verrazzano 工具監控 _Helidon MP_ 應用程式，包括 OpenSearch、Grafana 和 Prometheus。

### 先決條件

此實驗室假設您的機器已安裝下列項目：

*   JDK 11+
*   Apache Maven (3.6)
*   Docker

## 關於 Helidon

Helidon 是 Oracle 引進的開放原始碼微服務架構，它提供了專為建立輕量而快速的微服務型應用軟體而設計的 Java 程式庫集合。此架構支援兩種編寫微服務的程式設計模型：Helidon SE 與 Helidon MP。

雖然 Helidon SE 是設計為支援反應式程式設計模型的微架構，但 Helidon MP 是 MicroProfile 規格的導入。由於 MicroProfile 的根源在 Java EE 中，因此 MicroProfile API 採用熟悉的宣告式方法，使用大量註解。這對 Java EE 開發人員而言是個不錯的選擇。

MicroProfile 的特色旨在執行微服務。您可以尋找用來定義 REST 用戶端、監控應用程式、讀取技術與功能統計資料的 API，以及設定應用程式。Helidon 也將額外的 API 加入核心 Microprofile API 組，提供您撰寫現代化雲端原生應用程式所需的所有功能。

> Jakarta EE 上建置 [MicroProfile](https://microprofile.io/) 標準版。就像 Jakarta EE 一樣，MicroProfile 是開放原始碼，由 Eclipse Foundation 開發。使用 MicroProfile 進行建置時，會使用實作標準的程式庫或應用程式伺服器，就像 Jakarta EE 一樣。

## 關於 Verrazzano

Verrazzano 是一個端對端企業容器平台，用於在多雲端和混合環境中部署雲端原生和傳統應用程式。它是由一系列經過策劃的開放原始碼元件組成 - 許多您可能已經使用過與信任，有些是特別針對讓 Verrazzano 成為具有凝聚性且易於使用之平台的片段而撰寫的。

Verrazzano 包含下列功能：

*   混合與多叢集工作負載管理
*   WebLogic、Coherence 與 Helidon 應用程式的特殊處理
*   多叢集基礎架構管理
*   整合和預線應用程式監控
*   整合的安全
*   DevOps 和 GitOps 培訓

![Verrazzano，韋拉茲諾](images/verrazzano.png)

## 進一步瞭解

*   [https://helidon.io](https://helidon.io)
*   [https://verrazzano.io/](https://verrazzano.io/)

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月