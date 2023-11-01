# 簡介

## 關於此研討會

在 Kubernetes 叢集上部署 Tomcat 應用程式是一個複雜的程序，涉及管理多個容器、設定網路，以及確保高可用性和擴展性。為了簡化此流程，許多組織紛紛採用 Docker 和 Kubernetes 等容器化技術。

Docker 允許建立可部署在任何平台上的可攜式容器映像檔，而 Kubernetes 提供了強大的協調引擎，用於管理容器的部署和擴展。

Oracle Verrazzano 是一個雲端原生平台，為在 Kubernetes 叢集上部署和操作容器化應用程式提供統一的管理體驗。它提供一組一致的工具和 API，用來管理和監控多個叢集間的應用程式，簡化複雜應用程式的部署。

在本研討會中，我們將探索使用 Verrazzano 將 Tomcat 應用程式 Docker 映像檔部署至 Oracle Kubernetes 叢集的程序。我們將涵蓋建立 Docker 映像檔、設定 Verrazzano、部署應用程式及監控效能等相關步驟。

在此研討會結束時，您將清楚瞭解如何使用 Verrazzano 在 Oracle Kubernetes 叢集上部署容器化應用程式。

本工作坊旨在盡量提供自我說明，但歡迎隨時向閣下查詢有關詳情或協助。

預估時間：90 分鐘

### 目標

*   設定您的 Oracle Cloud Free Tier 帳戶 (如果您尚未註冊)。
*   在 Oracle Cloud Infrastructure 上設定 Oracle Kubernetes Engine 執行處理。
*   安裝 Verrazzano 生產環境設定檔。
*   建立範例 Tomcat 應用程式嵌入器影像。
*   使用 Verrazzano 將 Tomcat 應用程式部署至 OKE。
*   探索 Grafana、Prometheus 和 OpenSearch 儀表板主控台。

### 先決條件

*   必須要有已啟用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的帳戶。

## 進一步瞭解

**關於 Tomcat**

Apache Tomcat 是廣泛用於部署 Java Web 應用程式的開放原始碼 Web 伺服器和 Servlet 容器。Tomcat 的設計不僅輕量級、高效且容易使用，而且具備管理和部署 Web 應用程式的豐富功能。

**關於 Verrazzano**

Verrazzano 是一個端對端企業容器平台，用於在多雲端和混合環境中部署雲端原生和傳統應用程式。它是由一系列經過策劃的開放原始碼元件組成 - 許多您可能已經使用過與信任，有些是特別針對讓 Verrazzano 成為具有凝聚性且易於使用之平台的片段而撰寫的。

Verrazzano 包含下列功能：

*   混合與多叢集工作負載管理
*   WebLogic、Coherence 與 Helidon 應用程式的特殊處理
*   多叢集基礎架構管理
*   整合和預線應用程式監控
*   整合的安全
*   DevOps 和 GitOps 培訓

![Verrazzano，韋拉茲諾](images/verrazzano.png)

*   [https://tomcat.apache.org/](https://tomcat.apache.org/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月