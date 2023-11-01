# 概要

## このワークショップについて

この演習では、Helidon Project Starterを使用してHTTPマイクロサービス・アプリケーションを開発します。高度にカスタマイズ可能で、ユーザーがプロジェクトに追加するHelidon機能を選択できる様々なオプションを提供します。

次に、OCIコード・エディタを確認し、その中でHelidonアプリケーションを開きます。Oracle Cloud Infrastructure(OCI)クラウド・シェルでのGraalVM Enterprise Editionの使用方法についても学習します。

mavenを使用して、Helidon MPアプリケーションのGraalVMネイティブ・イメージを構築します。次に、既存のJavaクラスにカスタム・エンドポイントを追加して、アプリケーションを変更します。後で、このアプリケーションのネイティブdockerイメージをビルドし、Oracle Cloud Container Image Registryにプッシュします。また、コンピュート・インスタンスを作成し、ここでdockerイメージをプルし、このアプリケーションのdockerコンテナを実行します。

このワークショップは、できるだけわかりやすいように設計されていますが、途中で明確化や支援を依頼してください。

所要時間: 90分

### 目的

*   Helidon MPアプリケーションの生成
*   OCIコード・エディタの詳細
*   Helidon MPアプリケーションのネイティブ・イメージの作成
*   HelidonアプリケーションのGraalVMネイティブdockerイメージを作成します
*   仮想マシンの作成
*   Helidon MPアプリケーションのdockerコンテナを実行します

### 前提条件

この演習では、次を想定しています。

*   Oracle Cloudアカウント

## Helidonについて

Helidonは、軽量で高速なマイクロサービスベースのアプリケーションを作成するために設計されたJavaライブラリのコレクションを提供する、Oracleによって導入されたオープンソースのマイクロサービス・フレームワークです。このフレームワークは、マイクロサービスを作成するための2つのプログラミング・モデル(Helidon SEおよびHelidon MP)をサポートしています。

Helidon SEはリアクティブ・プログラミング・モデルをサポートするマイクロフレームワークとして設計されていますが、Helidon MPはMicroProfile仕様の実装です。MicroProfileにはJava EEのルートがあるため、MicroProfile APIは、注釈を大量に使用して、使い慣れた宣言的アプローチに従います。これは、Java EE開発者にとって適切な選択です。

MicroProfile機能は、マイクロサービスの実装を目的としています。RESTクライアントの定義、アプリケーションの監視、技術統計および機能統計の読取り、アプリケーションの構成を行うためのAPIがあります。Helidonでは、Microprofile APIのコア・セットにAPIが追加され、最新のクラウドネイティブ・アプリケーションを記述するために必要なすべての機能が提供されます。

> [MicroProfile](https://microprofile.io/)標準はJakarta EE上に構築されています。Jakarta EEと同様に、MicroProfileはオープン・ソースであり、Eclipse Foundationによって開発されています。MicroProfileを使用した実装は、Jakarta EEと同様に、標準を実装するライブラリまたはアプリケーション・サーバーで実行されます。

## さらに学ぶ

*   [https://helidon.io](https://helidon.io)

## 確認

*   **作成者** - Dmitry Aleksandrov
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年4月