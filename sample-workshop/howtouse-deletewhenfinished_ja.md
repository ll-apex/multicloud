# 単一のラボ・セットによるワークショップ

## 手順- 終了したらこのファイルを削除します

1.  AtomまたはVisual Studio Codeでサンプル・ワークショップ・テンプレートを開く
2.  5フォルダを事前に作成しました。ワークショップは複数のラボから作成されます。
3.  次のようなコメントを削除します: _この演習の目標のリスト_
4.  スペースには小文字のフォルダとファイル名とダッシュを使用してください(setup-adb NOT Setup\_ADB)。
5.  イメージ名には説明的な名前が必要です。adb1、adb2、adb3のみではありません。ハンディキャップのアクセシビリティのために、画像がどのように見えるかを説明するために画像の説明が必要です。すべての小文字とダッシュを覚えておいてください。
6.  WMSからQAドキュメントをダウンロードしてください。ワークショップは、前もって本番に移行し、スケルトンを使用するために必要なものがわかれば、より迅速に本番環境に移行できます。

PS: Readme.mdは必要ありません。Readmeは、最上位のライブラリレベルにのみ存在します。GitHubでは使用状況を追跡できないため、すべてのトラフィックをLiveLabsに送信します。GitHubへの直接リンクを作成しないでください。あなたのワークショップは非常に人気があるかもしれませんが、誰も知らないように追跡することはできません。

## 「Oracle Cloudの絶対パス」メニューのナビゲーション

**ラボ1: インスタンスのプロビジョニング->ステップ0: Oracle Cloudナビゲーション用に標準化された次の図を使用する(プロビジョニング用に共通)** - Oracle Cloudメニューをナビゲートするための共通のスクリーンショットのリストが含まれています。このセクションを読んで、必要に応じて関連する絶対パス・イメージを使用してください。これにより、Oracle Cloudのユーザー・インタフェースが更新された場合のワークショップが将来的に実証されます。

## フォルダ構造

この例では、目標は、より長い親ワークショップから複数の「子」ワークショップを作成することです。子は親の部品で構成されています。

サンプル ワークショップ-- 個々の実験室

        provision/
        setup/
        dataload/
        query/
        introduction/
          introduction.md       -- description of the everything workshop, note that it is a "lab" since there is only one
    
    workshops/
       freetier/                -- freetier version of the workshop
        index.html
        manifest.json
       livelabs/                -- livelabs version of the workshop
        index.html
        manifest.json
    

### FreeTierとLiveLabs

*   "FreeTier" - 無料トライアル、有料アカウント、および一部のワークショップでは、Always Freeアカウント(ブラウン・ボタン)が含まれます
*   "LiveLabs" - これらは、Oracle提供のテナンシを使用するワークショップです(緑色のボタン)
*   「デスクトップ」- これは、ワークショップがコンピュート・インスタンスで実行されているNoVNC環境にカプセル化される新しいデプロイメントです

### ワークショップについて

ワークショップには、個々のラボの6つすべてが1つのシーケンスで含まれています。

フォルダ構造には、6つのラボの完全なセットとしてワークショップを説明する紹介ラボが含まれています。ノート: ワークショップの親バージョンと子バージョンごとに異なる概要を用意する必要はありません。これは説明のみです。

product-name-workshop/freetierフォルダを参照し、manifest.jsonファイルを参照して構造を確認してください。

> **ノート:**タイトルでの「Lab n: 」の使用はオプションです。

前提条件の「lab」は、oracle-livelabs/common repoの共通フォルダの最初の演習です。この演習はすでに存在するため、かわりにRAW/absolute URLを使用できます。

    "filename": "https://oracle-livelabs.github.io/common/labs/cloud-login/cloud-login-livelabs2.md"        },
    

manifest.jsonファイルは、階層内の各ラボの場所を相対的に認識する必要があります。この構造では、演習は次の2つのレベル上に配置されます。

    "filename": "../../provision/provision.md"
    

### 次に例を示します。

この[APEXワークショップ](https://oracle-livelabs.github.io/apex/spreadsheet/workshops/freetier/)は、単一のラボ・セット([https://github.com/oracle-livelabs/APEX/tree/main/spreadsheet](https://github.com/oracle-livelabs/apex/tree/main/spreadsheet))を使用したワークショップのよい例です。