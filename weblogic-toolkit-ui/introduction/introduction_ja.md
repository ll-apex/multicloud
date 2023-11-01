# 概要

## このワークショップについて

このワークショップでは、オンプレミスのWebLogicサーバー・ドメインのコンテナへのエンドツーエンドの移行を示し、Oracle Container Engine for Kubernetes (OKE)を使用してOCIで実行できるようにします。WebLogic Kubernetes Toolkit UIのグラフィカル・インタフェースと、WebLogic Deployer ToolおよびWeblogic Kubernetes Operatorを示します。DevOps指向のツール・セットを使用して、移行プロセスを簡略化および高速化する方法を示します。

![ラボ・フロー](images/lab-flow.png)

所要時間: 120分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[完全なワークショップ](videohub:1_q1mmkimy)

### 製品・技術について

WebLogic Kubernetes Toolkit (WKT)は、Kubernetesクラスタ上のLinuxコンテナで実行するためにWebLogicベースのアプリケーションをプロビジョニングするのに役立つオープン・ソース・ツールのコレクションです。WKTには、次のツールが含まれています。  

*   [WebLogicデプロイ・ツール(WDT)](https://github.com/oracle/weblogic-deploy-tooling) - WebLogicドメインの単一のメタデータ・モデル表現で動作する、単一目的のライフサイクル・ツールのセット。
*   [WebLogic Image Tool (WIT)](https://github.com/oracle/weblogic-image-tool) - WebLogicドメインを実行するためのLinuxコンテナ・イメージを作成するためのツール。
*   [WebLogic Kubernetes Operator (WKO)](https://github.com/oracle/weblogic-kubernetes-operator) - WebLogicドメインをKubernetesクラスタでネイティブに実行できるようにするKubernetesオペレータ。

WKT UIは、WKTツール、Docker、HelmおよびKubernetesクライアント(kubectl)をラップするグラフィカル・ユーザー・インタフェースを提供し、モデルの作成および変更プロセスをガイドしますWebLogicドメイン、ドメインの実行に使用するLinuxコンテナ・イメージの作成、およびKubernetesクラスタ内のドメインのデプロイとアクセスに必要なソフトウェアおよび構成の設定とデプロイ。

### 目的

*   マーケットプレイス・イメージからの仮想マシンの作成
*   Oracle Cloud InfrastructureでのOracle Kubernetesエンジン・インスタンスの設定
*   WKT UIプロジェクトの変更とモデル・ファイルの作成
*   Oracle Container Image Registryでの補助イメージの作成およびプッシュ
*   WebLogic OperatorをOracle Kubernetes Cluster (OKE)にデプロイ
*   WebLogicドメインをOracle Kubernetesクラスタ(OKE)にデプロイ
*   Oracle Kubernetesクラスタ(OKE)へのイングレス・コントローラのデプロイ
*   管理対象サーバー・ポッド間のロード・バランシングの表示およびWebLogic-Remote-Consoleの確認
*   WebLogicクラスタのスケーリング
*   新しいイメージのローリング再起動によるデプロイ済アプリケーションの更新

## さらに学ぶ

*   [WebLogic Kubernetes Toolkit UI](https://oracle.github.io/weblogic-toolkit-ui/)

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月