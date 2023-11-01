# WKT UIプロジェクトの変更およびモデル・ファイルの作成

## 概要

この演習では、オンプレミスのWebLogicドメインを確認します。管理コンソールをナビゲートして、_test-domain_にデプロイされたアプリケーション、データソースおよびサーバーを表示します。また、事前に作成された_`base_project.wktproj`_も開きます。この_「プロジェクト設定」_セクションにはすでに値が入力されています。次に、オフラインのオンプレミス・ドメインをイントロスペクトしてモデル・ファイルを作成します。最後に、モデルを検証し、Oracle Kubernetesクラスタ(OKE)にデプロイするモデルを準備します。

見積時間: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[OCIでのOKEのモデルの作成](videohub:1_qdch3qqg)

### 目的

この演習では、次のことを行います。

*   オンプレミスのWebLogicドメイン_test-domain_を確認します。
*   ベースWKTプロジェクトを開きます。
*   オフライン・オンプレミス・ドメインのイントロスペクション。
*   モデルの検証と準備

### 事前設定

この演習を実行するには、次のことが必要です。

*   演習2で作成したnoVNCリモート・デスクトップにアクセスします。

## タスク1: オンプレミス・ドメインの確認

このタスクでは、WebLogic管理コンソールを使用して、オンプレミスの_test-domain_内のリソースをナビゲートします。

1.  左側の_「矢印アイコン」_をクリックします。 ![クリップボード](images/clipboard.png)

> **重要** - ホストマシンとリモートデスクトップ間のコピー&ペーストについては、_クリップボード_を参照してください。_クリップボード_を使用します。たとえば、ホストマシンからコピーしてリモートデスクトップに貼り付ける場合は、まずクリップボードに貼り付けてから、リモートデスクトップに貼り付ける必要があります。_「矢印アイコン」_を再度クリックして、_「設定」_オプションを非表示にします。

2.  _「Oracle WebLogic Server」_タブをクリックし、`Username/Password`として_weblogic/Welcome1%_と入力して_「ログイン」_をクリックします。WebLogic Serverバージョン_12.2.1.3.0_があります。  
    ![ログイン管理コンソール](images/login-admin-console.png)
    
3.  使用可能なサーバーを表示するには、_「環境」_を展開し、_「サーバー」_をクリックします。1つの動的クラスタに5つの管理対象サーバーがあることがわかります。 ![サーバーの表示](images/view-servers.png)
    
4.  データソースを表示するには、_「Services」_を展開して、_「Data Sources」_をクリックします。 ![リソースの表示](images/view-datasources.png)
    
5.  デプロイされたアプリケーションを表示するには、_「デプロイメント」_をクリックします。デプロイされたアプリケーションとして_opdemo_があります。 ![デプロイメントの表示](images/view-deployments.png)
    

## タスク2: ベースWKT UIプロジェクトのオープン

ラボをシンプルにするために、docker、Java、Oracle Home、Primary Image Tagの場所を事前設定した_`base_project.wktproj`_を作成しました。このタスクでは、_`base_project.wktproj`_プロジェクトを開きます。

1.  _「アクティビティ」_をクリックし、検索ボックスに**WebLogic**と入力します。_WebLogic Kubernetes Toolkit UI_のアイコンをクリックします。 ![WKTUIを開く](images/open-wktui.png)
    
2.  _base\_project.wktproj_プロジェクトを開くには、_「ファイル」_→_「プロジェクトのオープン」_をクリックします。 ![プロジェクトを開く](images/open-project.png)
    
3.  左側の_「ダウンロード」_をクリックし、_base\_project.wktproj_を選択して_「プロジェクトを開く」_をクリックします。 ![プロジェクトの場所](images/project-location.png)
    
    > **For your information only:**  
    > As _Credential Story Policy_, we select **Store in Native OS Credentials Store**. It means the credentials (like for WebLogic Server and datasources) are only stored on the local machine.  
    > For _Where would you like the target Oracle Fusion Middleware domain to live?_, we select **Created in the container from the model in the image**. In this case, the set of model-related files are added to the image. So when the WebLogic Kubernetes Operator domain object is deployed, its inspector process runs and creates the WebLogic Server domain inside a running container on-the-fly.  
    > ![プロジェクトの設定](images/project-settings.png) As _Kubernetes Environment Target Type_, we select **WebLogic Kubernetes Operator**. This means, you want this domain to be deployed in Kubernetes managed by the WebLogic Kubernetes Operator. This settings also determine what sections and their associated actions within the application, to display.  
    > we also specify the location for _JAVA HOME_ and _ORACLE\_HOME_. WebLogic Kubernetes Toolkit UI uses this directory when invoking the WebLogic Deployer Tooling and WebLogic Image Tool.  
    > To build new images, inspect images and interact with image repositories, the WKT UI application uses an image build tool, which defaults to docker.  
    > ![Kubernetesクラスタ・タイプ](images/kubernetes-cluster-type.png)
    
4.  _welcome1_に**「パスワード」**と入力し、_「ロック解除」_をクリックします。 ![開いた南京錠](images/unlock.png)
    

## タスク3: オフライン・オンプレミス・ドメインのイントロスペクション

このタスクでは、オンプレミス・ドメインのイントロスペクションを実行し、ドメイン構成で構成されるモデル・ファイルを作成します。

1.  WebLogic Kubernetes Toolkit UIで、_「モデル」_をクリックします。 ![モデル](images/click-model.png)
    
2.  _「ファイル」_→_「モデルの追加」_→_「Discoverモデル(オフライン)」_をクリックします。 ![Discoverモデル](images/discover-model.png)
    
3.  「フォルダのオープン」_アイコン_をクリックして、_「ドメイン・ホーム」_を開きます。 ![ドメインホームを開く](images/open-domain-home.png)
    
4.  「ホーム」フォルダで、_`/home/opc/Oracle/Middleware/Oracle_Home/user_projects/domains/`_ディレクトリに移動し、_test-domain_フォルダを選択して、「_選択_」をクリックします。_「OK」_をクリックします。![ナビゲート場所](images/navigate-location.png) ![場所の指定](images/specify-location.png)
    
    > コンソールを確認すると、WebLogicデプロイヤ・ツールが起動され、オフライン・モードでドメイン構成がイントロスペクトされます。
    
5.  次に示すように、ウィンドウが表示されます。最後に、モデルの準備ができています。 ![モデルの表示](images/view-model.png)
    
    > このWDTイントロスペクションの結果は、モデル(ドメイン構成のメタデータ表現)、プレースホルダで、アプリケーション・アーカイブ内の値(データソースのパスワードなど)およびアプリケーションを指定できます。
    

## タスク4: モデルの検証と準備

このタスクでは、モデルを検証し、Oracle Kubernetesクラスタ(OKE)にデプロイするモデルを準備します。

1.  _「コード・ビュー」_をクリックし、モデルを検証するには、_「モデルの検証」_をクリックします。 ![モデルの検証](images/validate-model.png)
    
    > **参考情報:**  
    > 検証モデルによってWDT[モデルの検証ツール](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/validate/)が起動され、モデルとその関連アーティファクトが整形式であることが検証され、特定のモデルの場所の有効な属性およびサブフォルダに関するヘルプが提供されます。
    
2.  _「モデルの検証完了」_ウィンドウが表示されたら、_「OK」_をクリックします。 ![検証完了](images/validate-complete.png)
    
3.  Kubernetesクラスタにデプロイするモデルを準備するには、_「モデルの準備」_をクリックします ![モデルの準備](images/prepare-model.png)
    
    > **参考情報:**  
    > 準備モデルは、WDT[モデルの準備ツール](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/prepare/)を起動し、WebLogic Kubernetes OperatorまたはVerrazzanoがインストールされているKubernetesクラスタで動作するようにモデルを変更します。  
    > 「モデルの準備」では、次の処理が実行されます。
    
    *   ターゲット環境と互換性がないモデル・セクションおよびフィールドを削除します。
    *   エンドポイント値を、変数を参照するモデル・トークンに置き換えます。
    *   資格証明値を、Kubernetesシークレットまたは変数のフィールドを参照するモデル・トークンに置き換えます。
    *   アプリケーションの変数、変数のオーバーライドおよびシークレット・エディタに表示されるフィールドのデフォルト値を提供します。
    *   ドメインのデプロイに使用されるリソース・ファイルの生成に使用するアプリケーションにトポロジ情報を抽出します。
4.  _「モデルの準備完了」_ウィンドウが表示されたら、_「OK」_をクリックします。 ![準備完了](images/prepare-complete.png)
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月