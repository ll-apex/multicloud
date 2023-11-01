# インフラストラクチャのプロビジョニング

## 概要

この演習では、**コンパートメント**、**動的グループ**、**ユーザー・グループおよびポリシー**を作成します。次に、**OCIコード・エディタ**のTerraformサービスを使用して、**DevOpsプロジェクト**とその関連リソースを作成します。

所要時間: 10分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[インフラストラクチャのプロビジョニング](videohub:1_tnhjvsxr)

### 目的

この演習では、次のことを行います。

*   コード・エディタを開き、Terraformスクリプトをダウンロードします。
*   コンパートメント、動的グループ、ユーザー・グループおよびポリシーをプロビジョニングします。
*   DevOpsプロジェクトに必要なリソースをプロビジョニングします。

### 事前設定

*   Oracle Free Tier(トライアル)、有料またはLiveLabsクラウド・アカウント
*   コンパートメント、動的グループ、ユーザー・グループおよびポリシーに精通している

## タスク1: コード・エディタを開き、ソース・コードをダウンロードします。

1.  クラウド・コンソールで、次に示すように_「開発者ツール」_アイコンをクリックし、_「コード・エディタ」_をクリックします。 ![コード・エディタを開く](images/open-codeeditor.png)
    
2.  _「端末」_→_「新規端末」_をクリックして、端末を開きます。ワークショップでは、新しいターミナルを開くように求められます。このようにして、コード・エディタで新しい端末を開くことができます。 ![ターミナルを開く](images/open-terminal.png)
    
3.  次のコマンドをコピーして端末に貼り付け、ソース・コードをダウンロードします。このソース・コードには、このワークショップに必要なOCIリソースを作成するterraformスクリプトが含まれています。
    
        <copy>curl -O https://objectstorage.uk-london-1.oraclecloud.com/p/IxV3cO7Uf5VnbniLEmx8zOBhr6liix40QWNTnTy0TTBcGdrLaRNSt2IJYxBPHqdw/n/lrv4zdykjqrj/b/ankit-bucket/o/devops_helidon_to_instance_ocw_hol.zip
        unzip ~/devops_helidon_to_instance_ocw_hol.zip</copy>
        

![ソースコードをダウンロード](images/download-sourcecode.png)

4.  _コード・エディタ_でソース・コード**`devops_helidon_to_instance_ocw_hol`**を開くには、_「ファイル」_→_「開く」_をクリックします。 ![オープンソースコード](images/open-sourcecode.png)
    
5.  ホーム・ディレクトリで_`devops_helidon_to_instance_ocw_hol`_を選択し、_「開く」_をクリックします。 ![オープンなDevOps](images/open-devops.png)
    
6.  次に示すように、_`devops_helidon_to_instance_ocw_hol`_フォルダ内のファイル名 _terraform.tfvars_をクリックします。トライアル・クラウド・アカウント用にカスタマイズする変数**`tenancy_ocid`**、**region**、**`compartment_ocid`**、**`user_ocid`**があります。 ![開いたtfvars](images/open-tfvars.png)
    

> プロジェクトが見えなければ。コード・エディタで_「エクスプローラ」_アイコンをクリックする必要がある場合があります。 ![駆逐艦](images/explorer.png)

7.  ブラウザで、**クラウド・コンソール**の[新しいタブ](https://cloud.oracle.com/)を開きます。このタブを使用して、前述の変数の値を取得します。
    
8.  **`tenancy_ocid`**を取得するには、_「ユーザー」アイコン_をクリックし、次に示すように_「テナンシ」_をクリックします。 ![テナントOCIDの取得](images/get-tenancyocid.png)
    
9.  _「コピー」_をクリックしてテナンシの**OCID**をコピーし、_`tenancy_ocid`_の値として_terraform.tfvars_ファイルに貼り付けます。 ![テナンシocidのコピー](images/copy-tenancyocid.png)
    
10.  **`user_ocid`**を取得するには、_「ユーザー」アイコン_をクリックし、次に示すように_「自分のプロファイル」_をクリックします。 ![自分のプロファイル](images/my-profile.png)
    
11.  _「コピー」_をクリックしてユーザーの**OCID**をコピーし、_`user_ocid`_の値として_terraform.tfvars_ファイルに貼り付けます。 ![ユーザーOCIDのコピー](images/copy-userocid.png)
    
12.  リージョン名を検索するには、次に示すように**「リージョンの管理」**をクリックします。次に、ホーム・リージョンの**リージョン識別子**をコピーし、_terraform.tfvars_ファイルに_region_の値として貼り付けます。![リージョンの管理](images/manage-region.png) ![リージョン名](images/region-name.png)
    
13.  最後に、_terraform.tfvars_は次のようになります。_`compartment_ocid`_の値はそのままにします。タスク2の一部としてコンパートメントが作成されると、値を置き換えます。 ![init tfvars](images/init-tfvars.png)
    

## タスク2: コンパートメント、動的グループ、ユーザー・グループおよびポリシーの作成

このタスクの目標は、コンパートメント、動的グループ、ユーザー・グループおよびポリシーを作成して、DevOps設定の環境を準備することです。このセクションには、管理者権限を持つユーザーが必要です。持っていない場合は、このような権限を持つ別のユーザーにこの実行を要求してください。

1.  端末を開き、次のコマンドをコピーして貼り付けて、_init_フォルダに移動します。
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/init/</copy>
        
2.  コードエディタでは、_init_フォルダ内のさまざまなファイルを表示できます。これらは、コンパートメント、動的グループ、ユーザー・グループおよびポリシーを作成するterraformスクリプトです。 ![initファイル](images/init-files.png)
    
3.  次のコマンドをコピーして貼り付け、コンパートメント、動的グループ、ユーザー・グループおよびポリシーをプロビジョニングします。
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    次のような出力が表示されます。terraformスクリプトが作成する内容を確認するには、**出力を確認してください**。また、コードを参照して実装を確認できます。 ![initが作成されました](images/init-created.png)
    
    > エラーがある場合は、**terraform.tfvars**ファイルで値を正しく設定していることを確認してください。
    

## タスク3: DevOpsプロジェクトとそのリソースの作成

1.  端末で、次のコマンドをコピーして貼り付け、_main_フォルダに移動します。
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main/</copy>
        
2.  次のコマンドをコピーして貼り付け、_`devops_helidon_to_instance_ocw_hol`_フォルダ内の_terraform.tfvars_に新しく作成されたコンパートメントの**compartment\_ocid**を更新します。
    
        <copy>../utils/update_compartment.sh</copy>
        
3.  次のコマンドをコピーして端末に貼り付け、すべてのDevOpsリソースをプロビジョニングします。
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    > **お読みください:-**これは、DevOpsに必要な次のリソースを提供します:
    
    *   **OCI DevOpsサービス**
        *   このプロジェクトに必要なすべてのDevOpsコンポーネントを含む**OCI DevOpsプロジェクト**。
        *   アプリケーション・ソース・コード・プロジェクトをホストする**OCIコード・リポジトリ**。
        *   **DevOpsビルド・パイプライン**。次のステージがあります:
            *   **ビルドの管理** - JDK20、mavenをダウンロードしてHelidonアプリケーションを構築するステップを実行します
            *   **アーティファクトの配信** - ビルドされたHelidonアプリケーションおよびデプロイメントをアーティファクト・リポジトリにアップロードします
            *   **デプロイメントのトリガー** - デプロイメント・パイプラインをトリガーします
        *   ターゲット環境で次を実行する**DevOpsデプロイメント・パイプライン**:
            *   ダウンロード JDK20
            *   OCI CLIをインストールし、それを使用してアプリケーション成果物をダウンロードします
            *   アプリケーションの実行
        *   作成されたOCIコンピュート・インスタンスをデプロイメント・ターゲットとして識別するためにデプロイメント・パイプラインで使用される**DevOpsインスタンス・グループ環境**。
        *   OCIコード・リポジトリでプッシュ・イベントが発生したときに、パイプライン・ライフサイクルを最初から最後まで呼び出す**DevOpsトリガー**。
    *   **OCIアーティファクト・レジストリ**
        *   ビルドされたHelidonアプリケーション・バイナリおよびデプロイメント・マニフェストをバージョン管理されたアーティファクトとしてホストする**OCIアーティファクト・リポジトリ**。
    *   **OCIプラットフォーム**
        *   ファイアウォールからポート8080を開く**OCIコンピュート・インスタンス**。ここで、アプリケーションは最終的にデプロイされます。
    *   ポート8080を開くイングレスを含む**セキュリティ・リスト付きのOCI Virtual Cloud Network (VCN)**。ポート8080は、Helidonアプリケーションへのアクセス元です。OCI VCNは、ネットワークのニーズに応じてOCIコンピュート・インスタンスによって使用されます。
4.  次の図は、DevOps設定の動作を示しています。 ![devopsのダイアグラム](images/devops-diagram.png)
    
5.  次のような出力が表示されます。 ![tf出力](images/tf-output.png)
    

**次の演習に進みます。**

## 確認

*   **著者** - Keith Lustria
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月