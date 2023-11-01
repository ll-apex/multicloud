# Helidon MPアプリケーションの生成

## 概要

このラボでは、**Helidon CLI**を使用して**Helidon MP**アプリケーションを作成するステップについて説明します。また、Helidonアプリケーションを変更して、**OCIロギングおよびモニタリング**・サービスとの統合を表示します。

見積時間: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[Helidon MPアプリケーションの生成](videohub:1_i4j33ed4)

### 目的

この演習では、次のことを行います。

*   Helidon CLIのダウンロード
*   Helidon CLIを使用したHelidon MPアプリケーションの生成
*   OCIロギングおよびメトリック・サービスとの統合の実行
*   アプリケーション・コードをOCIコード・リポジトリにプッシュします
*   DevOpsパイプラインのトリガー

### 事前設定

*   Oracle Free Tier(トライアル)、有料またはLiveLabsクラウド・アカウント
*   gitコマンドの理解

## タスク1: Helidon CLIのダウンロード

1.  **OCIコード・エディタ**で新しいターミナルを開き、次のコマンドを貼り付けてホーム・フォルダに移動します。
    
        <copy>cd ~</copy>
        
2.  次のコマンドをコピーして貼り付けて**Helidon CLI**をダウンロードし、**権限**を変更して実行可能にします。
    
        <copy>curl -L -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon</copy>
        

## タスク2: Helidon CLIを使用したHelidon Microprofileアプリケーションの生成

1.  CLIを実行して、**Helidon Microprofileアプリケーション**・プロジェクトを生成します。
    
        <copy>./helidon init</copy>
        
2.  _Helidonバージョン_の入力を求められたら、_4_と入力してすべてのバージョンを表示し、_35_と入力して、次に示すように_4.0.0-ALPHA6_バージョンを選択します。
    
        Helidon versions
        (1) 3.2.2 
        (2) 2.6.2 
        (3) 4.0.0-M1 
        (4) Show all versions 
        Enter selection (default: 1): 4
        -----------------------------
        -----------------------------
        (33) 2.0.1 
        (34) 2.0.0 
        (35) 4.0.0-ALPHA6 
        (36) 4.0.0-M1 
        Enter selection (default: 1): 35 
        
3.  _フレーバの選択_を求められたら、次の値をコピーして端末に貼り付けます。
    
            | Helidon Flavor
        
            Select a Flavor
            (1) se   | Helidon SE
            (2) mp   | Helidon MP
            (3) nima | Helidon Níma
            Enter selection (default: 1): <copy>2</copy>
        
4.  _「アプリケーション・タイプの選択」_を求められたら、次の値をコピーして端末に貼り付けます。
    
            Select an Application Type
        (1) quickstart | Quickstart
        (2) database   | Database
        (3) custom     | Custom
        (4) oci        | OCI
        Enter selection (default:1):<copy>4</copy>
        
5.  _「プロジェクトgroupId」_、_「プロジェクトartifactId」_および_「プロジェクト・バージョン」_の入力を求められたら、**デフォルト値を受け入れます**。
    
6.  _Javaパッケージ名_の入力を求められたら、次の値をコピーして端末に貼り付けます。
    
        Java package name (default: me.username.mp.oci): <copy>ocw.hol.mp.oci</copy>
        
7.  「_Start development loop? (default: n):_」のプロンプトが表示されたら、「_Enter_」を押してデフォルト値を選択します。
    
    > 完了すると、**oci-mp**プロジェクトが生成されます。
    

## タスク3: ロギングおよびメトリック・エクスプローラ用のHelidonアプリケーションの変更

1.  _コード・エディタ_で**oci-mp**プロジェクトを開くには、_「ファイル」_→_「開く」_をクリックします。 ![プロジェクトを開く](images/open-project.png)
    
2.  _上矢印_をクリックして親フォルダに移動し、_oci-mp_フォルダを選択して_「開く」_をクリックします。 ![オープンOCI MP](images/open-ocimp.png)
    
    > これにより、エクスプローラで _oci-mp_アプリケーションが開きます。
    
3.  新しいターミナルを開き、次のコマンドをコピーして貼り付けて、**`devops_helidon_to_instance_ocw_hol`**フォルダから_ビルドおよびデプロイメント・パイプライン仕様をコピー_します。
    
        <copy>cd ~/oci-mp/
        cp ~/devops_helidon_to_instance_ocw_hol/pipeline_specs/* .</copy>
        
4.  **.gitignore**を追加すると、リポジトリの一部にする必要のないファイルとディレクトリはgitによって無視されます。
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/.gitignore .</copy>
        
5.  次のコマンドをコピーして貼り付け、ユーティリティ・スクリプトをメイン・フォルダ_`devops_helidon_to_instance_ocw_hol`_から実行し、**config**パラメータを更新します。**Helidon MPプロジェクトのルート・ディレクトリの入力**を求められたら、\[Enter\]を押して**デフォルト**値を選択します。
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/update_config_values.sh</copy>
        
    
    次のような出力が表示されます。 ![構成の更新](images/update-config.png)
    
    > **続きを読む:-**
    
    *   このスクリプトを起動すると、次が実行されます。
    *   _~/OCI-mp/server/src/main/resources/application.yaml_構成ファイルで更新し、Helidon生成メトリックをOCIモニタリング・サービスに送信するHelidon機能を設定します。
        *   **compartmentId** - このデモに使用されるコンパートメントocid
        *   **namespace** - これは任意の文字列にできますが、このデモではhelidon\_metricsに設定されます。
    *   _~/OCI-mp/server/src/main/resources/META-INF/microprofile-config.properties_構成ファイルで更新し、OCIロギングおよびメトリック・サービスとの統合を実行するためにHelidonアプリケーション・コードで使用される構成パラメータを設定します。
        *   **oci.monitoring.compartmentId** - このデモに使用されるコンパートメントocid
        *   **oci.monitoring.namespace** - これは任意の文字列にできますが、このデモではhelidon\_applicationに設定されます。
        *   **oci.logging.id** - Terraformスクリプトによってプロビジョニングされたアプリケーション・ログID。
    *   _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_構成ファイルで更新し、_oci.bucket.name_プロパティを設定して、terraformスクリプトによってプロビジョニングされたオブジェクト・ストレージ・バケット名を含めます。このバケット名は、後の演習でHelidonアプリケーションからのオブジェクト・ストレージのサポートを示すために使用されます。
6.  コード・エディタでファイル_~/oci-mp/server/src/main/resources/application.yaml_を開き、**compartmentId**および**namespace**の値が更新されていることを確認します。 ![アプリケーション ヤムル](images/application-yaml.png)
    
7.  コード・エディタでファイル_~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_を開き、**oci.monitoring.compartmentId**、**oci.monitoring.namespace**、**oci.logging.id**および**oci.bucket.name**の値が更新されていることを確認します。_oci.bucket.name_は、後で演習5で使用されます。
    

## タスク4: OCIコード・リポジトリにコードをプッシュする認証トークンの生成

このステップでは、Helidonアプリケーション・コードを_OCIコード・リポジトリ_にプッシュするために使用する_認証トークン_を生成します。

1.  右上隅にある_「ユーザー・アイコン」_を選択し、_「マイ・プロファイル」_を選択します。 ![ユーザー・アイコン](images/user-icon.png)
    
2.  下にスクロールして_「認証トークン」_を選択します。 ![認証トークン](images/auth-tokens.png)
    
3.  _「トークンの生成」_をクリックします。 ![トークンの生成](images/generate-token.png)
    
4.  _oci-mp_をコピーして「説明」ボックスに貼り付け、_「トークンの生成」_をクリックします。 ![トークン摘要](images/token-description.png)
    
5.  「生成済トークン」の下の**「コピー」**を選択し、任意のエディタを使用してテキスト・ファイルに貼り付け/保存します。トークンは後で取得できないため、このコピーを今すぐ保持することが重要です。完了したら、**「閉じる」**をクリックします。 ![トークンのコピー](images/copy-token.png)
    

## タスク5: Helidonアプリケーション・プロジェクトをDevOpsプロジェクトのOCIコード・リポジトリに同期します

1.  新しい端末を開き、次のコマンドをコピーして貼り付けて、_oci-mp_ディレクトリに移動します。
    
        <copy>cd ~/oci-mp</copy>
        
2.  _oci-mp_プロジェクトディレクトリを初期化して、**gitリポジトリ**になります。
    
        <copy>git init</copy>
        
3.  対応するリモートブランチと一致するようにブランチを **main**に設定します。
    
        <copy>git checkout -b main</copy>
        
4.  **リモートリポジトリ**を設定します。最後のterraform出力から表示されたOCIコード・リポジトリのhttps URLを使用するか、`devops_helidon_to_instance_ocw_hol`のget.shツールを使用してその値を取得します。
    
        <copy>git remote add origin $(~/devops_helidon_to_instance_ocw_hol/main/get.sh code_repo_https_url)
        git remote -v</copy>
        
    
    > **git remote -v**は、起点が設定されていることを確認します。
    
5.  OCIリポジトリのユーザー名とパスワードが必要なgitコマンドで1回のみ入力されるように、**資格証明ヘルパー**・ストアを使用するようにgitを構成します。また、gitコミットに必要な**user.name**および**user.email**を設定します。
    
        <copy>git config credential.helper store
        git config --global user.email "my.name@example.com"
        git config --global user.name "FIRST_NAME LAST_NAME"</copy>
        
6.  **git pull**を使用して、ociリポジトリのgitログをローカル・リポジトリと同期します。
    
        <copy>git pull origin main</copy>
        
7.  これにより、ユーザー名とパスワードの入力が求められます。ユーザー名には**テナンシ名**/**ユーザー名**を使用し、パスワードには**タスク4**で生成されたOCIユーザー**認証トークン**を使用します。![gitユーザー名](images/git-username.png) ![gitパスワード](images/git-password.png)
    
8.  次のような出力が表示されます。そうでない場合は、すべてのgitコマンドが正しく実行されていることを確認してください。 ![gitプル同期](images/git-pull-sync.png)
    

## タスク6: Helidonアプリケーション・コードのプッシュとDevOpsパイプラインのトリガー

1.  gitコミットのすべてのファイルを**ステージング**します。
    
        <copy>git add .
        git status</copy>
        
    
    > git statusは、リポジトリ内のすべてのファイルを出力します。
    
2.  最初の **commit**を実行します。
    
        <copy>git commit -m "Helidon oci-mp first commit"</copy>
        
3.  リモート・リポジトリに変更を**プッシュ**します。
    
        <copy>git push -u origin main</copy>
        
    
    > これにより、DevOpsがトリガーされ、ビルド・パイプラインが開始されます。
    
4.  新しいタブで[クラウド・コンソール](https://cloud.oracle.com/)を開き、_「ハンバーガー・メニュー」_→_「開発者サービス」_→_「プロジェクト」_(**DevOps**の下)をクリックします。 ![devopsプロジェクト](images/devops-project.png)
    
5.  **ラボ1**で作成したコンパートメントを選択し、_「devops-project-helidon-ocw-hol-string」_をクリックして**DevOpsプロジェクト**を開きます。 ![コンパートメントの選択](images/select-compartment.png)
    
    > 新しいコンパートメントが表示されない場合は、ブラウザをリフレッシュします。
    
6.  _「最新のビルド履歴」_に、_「実行」_および「ステータス」が_「受入れ済/進行中」_と表示されます。次に示すように、最新の実行をクリックします。 ![ビルド履歴](images/build-history.png)
    
7.  ビルド・パイプラインが3つのステージすべてを完了すると、次に示すように出力が表示されます。ステージの直前の矢印をクリックして、実行しているアクションを表示できます。このアクションでは、_oci-mp_フォルダの _`build_spec.yaml`_ファイルに定義があります。 ![最初にビルド実行](images/build-run-first.png)
    
8.  ビルド実行の進行中、第3ステージで**「3つのドット」**をクリックし、次に示すように**「デプロイメントの表示」**をクリックします。これにより、**デプロイメント・パイプライン**が開きます。 ![ビュー・デプロイメント](images/view-deployment.png)
    
9.  _「デプロイメントの進行状況」_を参照してください。デプロイメント・パイプラインが完了すると、次に示すように出力が表示されます。ステージの直前の矢印をクリックして、実行しているアクションを表示できます。このアクションは、_oci-mp_フォルダの _`deployment_spec.yaml`_ファイルに定義されています。 ![デプロイメント実行](images/deployment-run.png)
    
    > これにより、HelidonアプリケーションがOCIの**コンピュート・インスタンス**に正常にデプロイされます。
    
10.  デプロイメント・パイプラインのログを表示するには、デプロイメント・ステージの近くにある**3つのドット**をクリックし、次に示すように**「詳細の表示」**をクリックします。 ![ログの表示](images/view-logs.png)
    
11.  ログを下にスクロールし、JDKフレーバが**Open JDK**であることを確認します。ログからも、次に示すように、HelidonアプリケーションがJavaの新しい**仮想スレッド**機能を利用していることに注意してください。 ![jdkを開く](images/open-jdk.png)
    
    > **ラボ4**の一部として、**Open JDK**を**Oracle JDK**に置き換えます。
    

**次の演習に進みます。**

## さらに学ぶ

*   [Helidon CLI](https://helidon.io/docs/v3/#/about/cli)
*   [Helidon MPクイックスタートガイド](https://helidon.io/docs/v3/#/mp/guides/quickstart)
*   [Helidon MP構成ソース](https://helidon.io/docs/v3/#/mp/config/advanced-configuration)
*   [Helidon MP構成ソース](https://helidon.io/docs/v3/#/mp/guides/config)

## 確認

*   **著者** - Keith Lustria
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月