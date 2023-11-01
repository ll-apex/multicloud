# 環境の設定

## 概要

この演習では、ワークショップに必要な環境を設定するステップについて説明します。

[Lab1ウォークスルー](videohub:1_far2bboa)

見積時間: 10分

### コード・エディタについて

コード・エディタを使用すると、OCIコンソールから直接、様々なOCIサービスのコードを編集およびデプロイできます。コンソールとローカル開発環境を切り替えることなく、サービス・ワークフローおよびスクリプトを更新できるようになりました。これにより、クラウド・ソリューションの迅速なプロトタイプ作成、新しいサービスの探索、迅速なコーディング・タスクの実行が簡単になります。

コード・エディタをクラウド・シェルと直接統合することで、クラウド・シェルに事前にインストールされているGraalVM Enterprise Native ImageおよびJDK 17 (Java Development Kit)にアクセスできます。

### 目的

*   コード・エディタの設定
*   必要なMavenおよびJDKバージョンをダウンロードします
*   Helidonソース・コードのダウンロード

### 前提条件

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)が有効になっているアカウントが必要です。
*   コード・エディタを開くには、ブラウザとしてFirefoxが必要です。

## タスク1: コード・エディタの設定

1.  クラウド・コンソールで、次のようにコード・エディタ・アイコンをクリックします。 ![コード・エディタ](images/code-editor.png)
    
2.  コード・エディタで、「ターミナル」→「新規ターミナル」をクリックします。 ![ターミナルを開く](images/open-terminal.png)
    

## タスク2: 必要なMavenおよびJDKバージョンのダウンロード

1.  次のコマンドをコピーして、端末に貼り付けます。必要なバージョンのJDKおよびMavenをダウンロードします。
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/archive/jdk-19.0.2_linux-x64_bin.tar.gz
        tar -xzvf jdk-19.0.2_linux-x64_bin.tar.gz</copy>
        

## タスク3: Helidonソース・コードのダウンロード

1.  次のコマンドをコピーして端末に貼り付け、helidonアプリケーションのソースコードをダウンロードします。
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  次のコマンドをコピーして貼り付け、_helidon-levelup-2023-main.zip_を解凍します。
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

_演習2に進む_ことができます。

## 確認

*   **作成者** - Joe DiPol
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年2月