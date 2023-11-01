# 簡介

## 關於此研討會

這個實驗室示範如何使用 Verrazzano 概念在單一 Kubernetes 叢集上安裝 Verrazzano 平台，並使用 Verrazzano 概念部署範例應用程式。

[Bobby 的 Books](https://verrazzano.io/docs/samples/bobs-books/) 範例應用程式由三個主要部分組成：

*   後端「訂單處理」應用程式，這是 REST 服務的 Java EE 應用程式，也是非常簡單的 JSP UI，可將資料儲存在 MySQL 資料庫中。此應用程式會在 WebLogic 伺服器上執行。
*   前端網路商店「Robert's Books」是一般書籍賣家。這是以 Helidon 微服務 (從 Coherence 取得工作簿資料) 的方式實行，此微服務使用 Coherence 快取存放區來保存訂單管理程式的資料，並具有 React Web UI。
*   前端網路商店「Bobby's Books」是專門兒童書店。這是導入為 Helidon 微服務，它會從 (不同) Coherence 快取存放區取得工作簿資料、直接與訂單管理程式連接，並在 WebLogic 伺服器上執行 JSF Web UI。

### 關於產品 / 技術

Verrazzano 是一個端對端企業容器平台，用於在多雲端和混合環境中部署雲端原生和傳統應用程式。它是由成策的開放原始碼元件所組成，許多您可能已經使用過與信任，有些是特別為了讓 Verrazzano 成為整合性且易於使用的平台而撰寫的。

Verrazzano 包含下列功能：

*   混合與多叢集工作負載管理
*   WebLogic、Coherence 與 Helidon 應用程式的特殊處理
*   多叢集基礎架構管理
*   整合和預線應用程式監控
*   整合的安全
*   DevOps 和 GitOps 培訓

![Verrazzano，韋拉茲諾](images/verrazzano.png)

### 目標

1\. 在 Oracle Cloud Infrastructure 上設定 Oracle Kubernetes Engine 執行處理。 1\. 設定 \`kubectl\` 以與 Oracle Cloud Infrastructure 上的 Oracle Kubernetes Engine 執行處理互動。 2. 安裝並瞭解 Verrazzano 平台。3. 部署 \*Bobby 的 Books\* 範例應用程式。4. 修改並重新部署 Helidon 型 \*Stock\* 應用程式元件。

## 進一步瞭解

*   [Verrazzano，韋拉茲諾](https://verrazzano.io/)

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月