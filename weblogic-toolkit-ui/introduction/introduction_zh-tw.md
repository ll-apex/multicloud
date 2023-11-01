# 簡介

## 關於此研討會

此研討會示範將內部部署 WebLogic 伺服器網域端對端移轉至容器，並透過 Oracle Container Engine for Kubernetes (OKE) 在 OCI 中執行。我們示範了 WebLogic Kubernetes Toolkit UI，以及 WebLogic Deployer Tool 和 Weblogic Kubernetes Operator 的圖形介面。我們示範如何使用一組 DevOps 導向的工具來簡化移轉流程並加速移轉。

![實驗室流程](images/lab-flow.png)

預估時間：120 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[完整研討會](videohub:1_q1mmkimy)

### 關於產品 / 技術

WebLogic Kubernetes Toolkit (WKT) 是一組開源工具，可協助您在 Kubernetes 叢集上的 Linux 容器中佈建以 WebLogic 為基礎的應用程式。WKT 包含下列工具：  

*   [WebLogic 部署工具 (WDT)](https://github.com/oracle/weblogic-deploy-tooling) \- 一組在 WebLogic 網域的單一描述資料模型表示法下運作的單一用途生命週期工具。
*   [WebLogic 影像工具 (WIT)](https://github.com/oracle/weblogic-image-tool) \- 用於建立 Linux 容器映像檔以執行 WebLogic 網域的工具。
*   [WebLogic Kubernetes Operator (WKO)](https://github.com/oracle/weblogic-kubernetes-operator) \- 一種 Kubernetes 運算子，可讓 WebLogic 網域在 Kubernetes 叢集中以原生方式執行。

WKT UI 提供一個圖形化使用者介面，可包裝 WKT 工具、Docker、Helm 和 Kubernetes 用戶端 (kubectl)，並協助您完成建立和修改模型的程序的 WebLogic 網域、建立用來執行網域的 Linux 容器映像檔，以及設定和部署在 Kubernetes 叢集中部署和存取網域所需的軟體和組態。

### 目標

*   從市集映像檔建立虛擬機器
*   在 Oracle Cloud Infrastructure 上設定 Oracle Kubernetes 引擎執行處理
*   修改 WKT UI 專案與建立模型檔案
*   在 Oracle Container Image Registry 中建立輔助映像檔並推送
*   將 WebLogic 運算子部署至 Oracle Kubernetes 叢集 (OKE)
*   將 WebLogic 網域部署至 Oracle Kubernetes 叢集 (OKE)
*   將傳入控制器部署到 Oracle Kubernetes 叢集 (OKE)
*   顯示受管理伺服器 Pod 與探索 WebLogic-Remote-Console 之間的負載平衡
*   調整 WebLogic 叢集
*   透過新映像檔的輪流重新啟動來更新部署的應用程式

## 進一步瞭解

*   [WebLogic Kubernetes 工具套件 UI](https://oracle.github.io/weblogic-toolkit-ui/)

## 確認

*   **作者** - Ankit Pandey
*   **貢獻者** - Maciej Gruszka、Sid Joshi
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 8 月