# 概要

## このワークショップについて

TomcatアプリケーションのKubernetesクラスタへのデプロイは、複数のコンテナの管理、ネットワーキングの構成、高可用性とスケーラビリティの確保を含む複雑なプロセスです。このプロセスを簡素化するために、多くの組織がDockerやKubernetesなどのコンテナ化テクノロジに注目しています。

Dockerを使用すると、任意のプラットフォームにデプロイできるポータブル・コンテナ・イメージを作成できますが、Kubernetesは、コンテナのデプロイメントとスケーリングを管理できる強力なオーケストレーション・エンジンを提供します。

Oracle Verrazzanoは、Kubernetesクラスタにコンテナ化されたアプリケーションをデプロイおよび操作するための統合管理エクスペリエンスを提供するクラウドネイティブ・プラットフォームです。複数のクラスタにわたるアプリケーションの管理および監視に使用できるツールとAPIの一貫性のあるセットを提供することで、複雑なアプリケーションのデプロイメントを簡略化します。

このワークショップでは、Verrazzanoを使用してTomcatアプリケーションDockerイメージをOracle Kubernetesクラスタにデプロイするプロセスについて説明します。Dockerイメージの作成、Verrazzanoの構成、アプリケーションのデプロイおよびそのパフォーマンスのモニタリングに関連するステップについて説明します。

このワークショップを終えるまでには、Verrazzanoを使用してコンテナ化されたアプリケーションをOracle Kubernetesクラスタにデプロイする方法を明確に理解できます。

このワークショップは、できるだけわかりやすいように設計されていますが、途中で明確化や支援を依頼してください。

所要時間: 90分

### 目的

*   Oracle Cloud Free Tierアカウントを設定します(まだ設定していない場合)。
*   Oracle Cloud InfrastructureでOracle Kubernetes Engineインスタンスを設定します。
*   Verrazzano本番プロファイルをインストールします。
*   サンプルTomcatアプリケーションdockerイメージを作成します。
*   Verrazzanoを使用してTomcatアプリケーションをOKEにデプロイします。
*   Grafana、Prometheus、OpenSearch Dashboardコンソールの詳細をご覧ください。

### 前提条件

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)が有効になっているアカウントが必要です。

## さらに学ぶ

**Tomcatについて**

Apache Tomcatは、JavaベースのWebアプリケーションのデプロイに広く使用されているオープンソースのWebサーバーおよびサーブレット・コンテナです。Tomcatは軽量で効率的で使いやすいように設計されており、Webアプリケーションを管理およびデプロイするための豊富な機能セットを提供しています。

**Verrazzanoについて**

Verrazzanoは、クラウドネイティブ・アプリケーションと従来のアプリケーションをマルチクラウド環境とハイブリッド環境にデプロイするためのエンドツーエンドのエンタープライズ・コンテナ・プラットフォームです。厳選されたオープン・ソース・コンポーネントのセットで構成されており、その多くはすでに使用され、信頼されているものです。一部は、すべての要素をプルして、Verrazzanoを一貫性のある使いやすいプラットフォームにするために特別に作成されています。

Verrazzanoには次の機能が含まれています:

*   ハイブリッドおよびマルチクラスタのワークロード管理
*   WebLogic、CoherenceおよびHelidonアプリケーションの特別な処理
*   マルチクラスタ・インフラストラクチャ管理
*   統合された、事前準備済みのアプリケーション・モニタリング
*   統合セキュリティ
*   DevOpsおよびGitOpsの有効化

![Verrazzano](images/verrazzano.png)

*   [https://tomcat.apache.org/](https://tomcat.apache.org/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月