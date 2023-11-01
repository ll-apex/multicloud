# 概要

## このワークショップについて

このラボでは、Oracle Kubernetes Engine、VerrazzanoおよびHelidonとのイントロレベルのハンズオン・セッションを参加者に提供します。Verrazzanoを使用してOKEにサンプルhello helidonアプリケーションをデプロイする方法を学習します。

このラボでは、Oracle Cloud Free Tierアカウントを使用して、Oracle Cloud Infrastructure (OCI)にOracle Kubernetes Engine (OKE)を設定します。Free Tierアカウントは、エンタープライズ・レベルでマイクロサービス・アプリケーションを実行および操作する方法を調べ、学習するのに十分です。

このワークショップの目標は、OKEとVerrazzanoの使用の基本を学び、彼らがあなたのエンターピーズでどのようにあなたを助けることができるかを理解することです。これらのプロジェクトの機能についてさらに学習するには、Oracle Free Tierクラウド・アカウントおよびOracle Cloud Infrastructureの使用について引き続き検討してください。

このワークショップは、できるだけわかりやすいように設計されていますが、途中で明確化や支援を依頼してください。

所要時間: 60分

### 目的

*   Oracle Cloud Free Tierアカウントを設定します(まだ設定していない場合)。
*   Oracle Cloud InfrastructureでOracle Kubernetes Engineインスタンスを設定します。
*   Verrazzanoプラットフォームをインストールします。
*   _Simple Hello Helidon_アプリケーションをデプロイします。
*   Rancher、Opensearch、Grafana、PrometheusなどのVerrazzanoツールを使用して、_Simple Hello Helidon_アプリケーションを監視します。

### 前提条件

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)が有効になっているアカウントが必要です。

## Helidonについて

Helidonは、軽量で高速なマイクロサービスベースのアプリケーションを作成するために設計されたJavaライブラリのコレクションを提供する、Oracleによって導入されたオープンソースのマイクロサービス・フレームワークです。このフレームワークは、マイクロサービスを作成するための2つのプログラミング・モデル(Helidon SEおよびHelidon MP)をサポートしています。

Helidon SEはリアクティブ・プログラミング・モデルをサポートするマイクロフレームワークとして設計されていますが、Helidon MPはMicroProfile仕様の実装です。MicroProfileにはJava EEのルートがあるため、MicroProfile APIは、注釈を大量に使用して、使い慣れた宣言的アプローチに従います。これは、Java EE開発者にとって適切な選択です。

MicroProfile機能は、マイクロサービスの実装を目的としています。RESTクライアントの定義、アプリケーションの監視、技術統計および機能統計の読取り、アプリケーションの構成を行うためのAPIがあります。Helidonでは、Microprofile APIのコア・セットにAPIが追加され、最新のクラウドネイティブ・アプリケーションを記述するために必要なすべての機能が提供されます。

> [MicroProfile](https://microprofile.io/)標準はJakarta EE上に構築されています。Jakarta EEと同様に、MicroProfileはオープン・ソースであり、Eclipse Foundationによって開発されています。MicroProfileを使用した実装は、Jakarta EEと同様に、標準を実装するライブラリまたはアプリケーション・サーバーで実行されます。

## Verrazzanoについて

Verrazzanoは、クラウドネイティブ・アプリケーションと従来のアプリケーションをマルチクラウド環境とハイブリッド環境にデプロイするためのエンドツーエンドのエンタープライズ・コンテナ・プラットフォームです。厳選されたオープン・ソース・コンポーネントのセットで構成されており、その多くはすでに使用され、信頼されているものです。一部は、すべての要素をプルして、Verrazzanoを一貫性のある使いやすいプラットフォームにするために特別に作成されています。

Verrazzanoには次の機能が含まれています:

*   ハイブリッドおよびマルチクラスタのワークロード管理
*   WebLogic、CoherenceおよびHelidonアプリケーションの特別な処理
*   マルチクラスタ・インフラストラクチャ管理
*   統合された、事前準備済みのアプリケーション・モニタリング
*   統合セキュリティ
*   DevOpsおよびGitOpsの有効化

![Verrazzano](images/verrazzano.png)

## さらに学ぶ

*   [https://helidon.io](https://helidon.io)
*   [https://verrazzano.io/](https://verrazzano.io/)

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年3月