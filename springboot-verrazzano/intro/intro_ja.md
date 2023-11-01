# 概要

## このワークショップについて

このワークショップでは、Verrazzanoを使用してSpring BootアプリケーションをOracle Kubernetes Engine (OKE)クラスタにデプロイする方法について説明します。

今日のデジタル時代、企業はより迅速かつ効率的にアプリケーションを開発およびデプロイする方法を求めています。Kubernetesはコンテナ・オーケストレーションの事実上の標準となり、組織はコンテナ化されたアプリケーションをデプロイ、管理およびスケーリングできます。Oracle Kubernetes Engine(OKE)は、Oracle Cloud Infrastructure(OCI)上のコンテナ化されたアプリケーションのデプロイメントと管理を簡素化するマネージドKubernetesサービスを提供します。

Spring Bootは、Webアプリケーションとマイクロサービスの構築に使用される一般的なJavaベースのフレームワークです。これは、コンベンションオーバー構成アプローチによる合理化された開発エクスペリエンスを提供し、コンテナ化されたアプリケーションをKubernetesに構築およびデプロイするための優れた選択肢となります。

Verrazzanoは、クラウドネイティブ・アプリケーションをデプロイおよび管理するための完全なエンドツーエンドのソリューションを提供するオープンソースのKubernetesネイティブ・プラットフォームです。アプリケーション・トポロジの統合ビューと、デプロイメント、監視および管理のための統合ツールのセットを提供することで、複雑なマルチコンポーネント・アプリケーションのデプロイメントを簡素化します。

このワークショップは、できるだけわかりやすいように設計されていますが、途中で明確化や支援を依頼してください。

所要時間: 90分

### 目的

*   Oracle Cloud Free Tierアカウントを設定します(まだ設定していない場合)。
*   OCIでのOKEクラスタの設定
*   クラスタへのVerrazzanoのインストールおよび構成
*   Spring Bootアプリケーションのコンテナ・イメージの作成
*   Verrazzanoを使用したOKEクラスタへのアプリケーションのデプロイ
*   Verrazzanoの統合ツールを使用したアプリケーションの監視および管理

### 事前設定

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)が有効になっているアカウントが必要です。

## さらに学ぶ

**Springbootについて**

Spring Bootは、Webアプリケーションとマイクロサービスの構築に使用される一般的なJavaベースのフレームワークです。これは、コンベンションオーバー構成アプローチによる合理化された開発エクスペリエンスを提供し、コンテナ化されたアプリケーションをKubernetesに構築およびデプロイするための優れた選択肢となります。

Spring Bootには、組込みサーバー、自動構成、Webアプリケーション、メッセージング・システム、データドリブン・アプリケーションを構築するための幅広いライブラリとツールなど、アプリケーションを迅速に構築およびデプロイできる様々な機能が用意されています。また、テストおよびデバッグ用の豊富なツール・セットも提供しているため、堅牢で信頼性の高いコードを簡単に記述できます。

Spring Bootの主な利点の1つは、データベース・システム、メッセージ・キュー、キャッシュ・システムなど、他のテクノロジやフレームワークと簡単に統合できることです。これにより、開発者は、大規模なワークロードを処理し、最新のクラウドベースのアプリケーションの要求を満たすことができる、複雑な分散システムを構築できます。

Spring Bootは、シンプルさと使いやすさに重点を置いており、最新のクラウドネイティブ・アプリケーションの構築を目指す開発者や組織で人気のある選択肢となっています。貢献者とユーザーの活気あるコミュニティを持ち、変化する業界のニーズを満たすために継続的に進化しています。

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

*   [https://spring.io/](https://spring.io/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年4月