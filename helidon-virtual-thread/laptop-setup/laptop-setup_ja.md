# Helidon MPアプリケーションのネイティブ・イメージの構築

## 概要

この演習では、ローカルlinuxマシンに環境を設定する方法を学習します。

見積時間: 10分

### 目的

*   必要なJDKおよびMavenを構成します。
*   アプリケーションのソース・コードをダウンロードします。

### 前提条件

*   プロジェクトを変更、構築、および実行するには、IDEが必要です。

## タスク1: 必要なMavenおよびJDKバージョンのダウンロード

1.  IDEで、端末/コンソールを開きます。
    
2.  次のコマンドをコピーして、端末に貼り付けます。必要なバージョンのJDKおよびMavenをダウンロードし、必要なMavenおよびJDKを使用するようにPATH変数を設定します。
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/latest/jdk-19_linux-x64_bin.tar.gz
        tar -xzvf jdk-19_linux-x64_bin.tar.gz</copy>
        

## タスク2: Helidonソース・コードのダウンロード

1.  次のコマンドをコピーして端末に貼り付け、helidonアプリケーションのソースコードをダウンロードします。
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  次のコマンドをコピーして貼り付け、_helidon-levelup-2023-main.zip_を解凍します。
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

_次の演習に進む_ことができます。

## 確認

*   **作成者** - Joe DiPol
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年2月