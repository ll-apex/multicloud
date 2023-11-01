# 概要

## このワークショップについて

このラボでは、Verrazzanoプラットフォームを単一のKubernetesクラスタにインストールし、Verrazzanoの概念を使用してサンプル・アプリケーションをデプロイする方法を示します。

[Bobby's Books](https://verrazzano.io/docs/samples/bobs-books/)サンプル・アプリケーションは、次の3つの主要部分で構成されています。

*   バックエンドの注文処理アプリケーション。これは、RESTサービスと非常に単純なJSP UIを備えたJava EEアプリケーションで、MySQLデータベースにデータを格納します。このアプリケーションは、WebLogicサーバーで実行されます。
*   フロントエンドWebストア「Robert's Books」は、一般書籍販売者です。これは、Helidonマイクロサービスとして実装されます。Coherenceからブック・データを取得し、Coherenceキャッシュ・ストアを使用してオーダー・マネージャのデータを保持し、React Web UIを持ちます。
*   特製児童書店「ボビーの本」のフロントエンドWebストア。これは、Helidonマイクロサービスとして実装されます。(異なる)Coherenceキャッシュ・ストアからブック・データを取得し、オーダー・マネージャと直接インタフェースし、JSF Web UIをWebLogicサーバー上で実行します。

### 製品・技術について

Verrazzanoは、クラウドネイティブ・アプリケーションと従来のアプリケーションをマルチクラウド環境とハイブリッド環境にデプロイするためのエンドツーエンドのエンタープライズ・コンテナ・プラットフォームです。厳選されたオープン・ソース・コンポーネントのセットで構成されており、その多くはすでに使用され、信頼されているものです。一部は、すべての要素をプルして、Verrazzanoを結束性が高く、使いやすいプラットフォームにするために特別に作成されています。

Verrazzanoには次の機能が含まれています:

*   ハイブリッドおよびマルチクラスタのワークロード管理
*   WebLogic、CoherenceおよびHelidonアプリケーションの特別な処理
*   マルチクラスタ・インフラストラクチャ管理
*   統合された、事前準備済みのアプリケーション・モニタリング
*   統合セキュリティ
*   DevOpsおよびGitOpsの有効化

![Verrazzano](images/verrazzano.png)

### 目的

1\. Oracle Cloud InfrastructureでOracle Kubernetes Engineインスタンスを設定します。 1.Oracle Cloud InfrastructureでOracle Kubernetes Engineインスタンスと対話するように\`kubectl\`を構成します。 2.インストールして、Verrazzanoプラットフォームを確認します。3.\*Bobby's Books\*サンプル・アプリケーションをデプロイします。4.Helidonベースの\*Stock\*アプリケーション・コンポーネントを変更および再デプロイします。

## さらに学ぶ

*   [Verrazzano](https://verrazzano.io/)

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月