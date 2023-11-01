# 概要

## このワークショップについて

このワークショップでは、**OCI DevOpsサービス**を使用して、**Helidonマイクロサービス・アプリケーション**を最初から最後まで構築、メンテナンスおよびアップグレードする方法を学習します。仮想スレッド・テクノロジでJDK20を利用して、ライフサイクル管理プロセス全体を加速および合理化する方法をデモンストレーションします。

予想時間: 5分

### 目的

このワークショップでは、次のことを行います。

*   コンパートメント、動的グループおよびポリシーの作成
*   Terraformを使用したDevOpsプロジェクトおよび関連リソースの作成
*   OCI DevOpsを使用して、Helidon MPアプリケーションをコンピュート・インスタンスに構築およびデプロイします
*   OCI Monitoring SDK統合の表示
*   OCI Logging SDK統合を検証して、メッセージをロギング・サービスにプッシュ
*   パッチ適用シナリオをシミュレートします。
*   Helidon MPアプリケーションからのオブジェクト・ストレージ・アクセスの追加

### 事前設定

*   Oracle Free Tier(トライアル)、有料またはLiveLabsクラウド・アカウント
*   [OCIコンソールに関する知識](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
*   [ネットワーキングの概要](https://docs.us-phoenix-1.oraclecloud.com/Content/Network/Concepts/overview.htm)
*   [コンパートメントについての理解](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/concepts.htm)
*   [OCI DevOpsサービス](https://docs.oracle.com/en-us/iaas/Content/devops/using/home.htm)

## Oracle DevOps

Oracle Cloud Infrastructure DevOpsサービスは、開発者にエンドツーエンドのCI/CDプラットフォームを提供します。OCI DevOpsサービスは、ソフトウェア・ライフサイクルに必要なすべてのニーズに幅広く対応しています。など

*   **OCIデプロイメント・パイプライン** - VMやベアメタル、Oracle Container Engine for Kubernetes(OKE)、OCI FunctionsなどのOCIプラットフォームへの宣言的なパイプライン・リリース戦略により、リリースを自動化します
*   **OCIアーティファクト・リポジトリ** - 不変のアーティファクトを含む、バージョン設定されたアーティファクトを格納する場所。
*   **OCIコード・リポジトリ** - OCIはスケーラブルなコード・リポジトリ・サービスを提供しました。
*   **OCIビルド・パイプライン** - ビルド、テスト、アーティファクトおよびデプロイメントの呼出しを自動化するサーバーレスでスケーラブルなサービス。

![Devopsアーキテクチャ](images/oci-devops.png)

## ロール・プレイ・アーキテクチャ

OCIのサービスと機能を使用して、次のアーキテクチャを構築およびデプロイします。

![Devopsダイアグラム](images/devops-diagram.png)

**次の演習に進みます。**

## さらに学ぶ

*   [リファレンス・アーキテクチャ: Oracle Cloud Infrastructure DevOpsによる最新のアプリケーション・デプロイメント戦略の理解](https://docs.oracle.com/en/solutions/mod-app-deploy-strategies-oci/index.html)
*   [https://helidon.io/](https://helidon.io/)
*   [https://medium.com/helidon](https://medium.com/helidon)
*   [https://github.com/helidon-io/helidon](https://github.com/helidon-io/helidon)
*   [https://www.youtube.com/Helidon\_Project](https://www.youtube.com/Helidon_Project)
*   [https://helidon.slack.com](https://helidon.slack.com)

## 確認

*   **著者** - Keith Lustria
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年5月