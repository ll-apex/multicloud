# WebLogic OperatorをOracle Kubernetes Cluster (OKE)にデプロイ

## 概要

この演習では、ブラウザを使用してOCI CLIを認証し、_.OCI/config_ファイルを作成します。kubectlを使用して、_ローカル・アクセス_を使用してクラスタをリモートで管理します。_kubeconfig_ファイルが必要です。このkubeconfigファイルは、OCI CLIを使用して生成されます。次に、WebLogic Kubernetes Toolkit UIからKubernetesクラスタへの接続を確認します。最後に、WebLogic Kubernetes OperatorをKubernetesクラスタ(OKE)にインストールします。

見積時間: 10分

### 目的

この演習では、次のことを行います。

*   Kubernetesクラスタに接続するようにkubectl (KubernetesクラスタCLI)を構成します。
*   WebLogic Kubernetes Toolkit UIからKubernetesクラスタへの接続を確認します。
*   WebLogic Kubernetes OperatorをKubernetesクラスタにインストールします。

## タスク1: Oracle Kubernetesクラスタに接続するためのkubectl (KubernetesクラスタCLI)の構成

このタスクでは、_/home/opc_ディレクトリに構成ファイル _.oci/config_および _.kube/config_を作成します。この構成ファイルを使用すると、この仮想マシンからOracle Kubernetesクラスタ(OKE)にアクセスできます。

1.  Firefoxでクラウド・コンソールを開き、次に示すように**「ハンバーガー・メニュー」**→**「開発者サービス」**→**「Kubernetesクラスタ(OKE)」**を選択します。 ![OKEアイコン](images/oke-icon.png)
    
2.  演習3で作成したクラスタ名をクリックし、**「クラスタへのアクセス」**をクリックします。 ![クラスタへのアクセス](images/access-cluster.png)
    
3.  **「ローカル・アクセス」**を選択し、次に示すように**「コピー」**をクリックします。 ![ローカル・アクセス](images/local-access.png)
    
4.  **「アクティビティ」**をクリックし、**「ターミナル」**を選択します。 ![終了](images/click-terminal.png)
    
5.  コピーしたコマンドを端末に貼り付けます。**「Do you want to create a new config file?」**で、**y**と入力し、**\[Enter\]**を押します。**「ブラウザからログインして構成ファイルを作成しますか。」**で、**y**と入力し、**\[Enter\]**を押します。 ![OCI構成](images/oci-config.png)
    
6.  Firefoxブラウザで、アクティブなセッションをクリックします。
    
    > 次に示すように、_「認可が完了しました」_が表示されます。 ![承認完了](images/authorization-complete.png)
    
7.  **「秘密キーのパスフレーズを入力」**で、空のままにして**\[Enter\]**を押します。 ![空のパスフレーズ](images/empty-passphrase.png)
    
8.  上矢印キーを使用して**oce ce ...**コマンドを再度実行し、**Kubeconfigファイル/home/opc/.kube/configに書き込まれた新しい構成**が表示されるまで複数回再実行します。 ![KubeConfigの作成](images/create-kubeconfig.png)
    
9.  次のコマンドをコピーして貼り付け、**~/.kube/config**ファイルのアクセス権を変更します。
    
        <copy>chmod 600 ~/.kube/config</copy>
        

## タスク2: Oracle WebLogic Server for OKEスタックのクラウド・リソースの表示

1.  OCIコンソールで、**ナビゲーション・メニュー**をクリックし、**「開発者サービス」**を選択します。「リソース・マネージャ」グループで、**「スタック」**をクリックします。
    
2.  スタックを含む**コンパートメント**を選択します。
    
3.  スタックの名前をクリックします。
    
4.  **「アプリケーション情報」**タブにナビゲートすると、スタックによって作成されたリソースを表示できます。**要塞インスタンスのパブリックIP**をコピーし、テキスト・ファイルに貼り付けます。 ![要塞のコピー](images/copy-bastion.png)
    

## タスク3: 端末でのプロキシ構成の有効化

このタスクでは、リモート・デスクトップからOracle Kubernetesクラスタ・ノードへの**sshトンネリング**を有効にします。

1.  端末を開き、次のコマンドをコピーして貼り付けて、リモート・デスクトップに秘密キー・ファイルを作成します。
    
        <copy>vi ~/.ssh/opc.key</copy>
        
2.  **i**を押して挿入モードに入り、このファイルに秘密鍵の内容を貼り付けてエスケープを押してから、**:wq**と入力してこのファイルを保存します。
    
3.  次のコマンドをコピーして貼り付け、秘密キー・ファイルの権限を変更します。
    
        <copy>chmod 600 ~/.ssh/opc.key</copy>
        
4.  次のコマンドをコピーして端末に貼り付け、**bastion-public-ip**を最後のタスクでコピーしたpublic ipに置き換えます。
    
        <copy>ssh -D 1088 -fCqN -i ~/.ssh/opc.key opc@bastion-public-ip</copy>
        
5.  **~/.kube/config**ファイルを開き、次に示すようにこのファイルに次をコピーして貼り付けます。
    
        <copy>proxy-url: socks5://localhost:1088</copy>
        
    
    ![プロキシーの設定](images/proxy-config.png)
    

## タスク4: WebLogic Kubernetes Toolkit UIからOracle Kubernetesクラスタへの接続性の検証

このタスクでは、`WebLogic Kubernetes Toolkit UI`アプリケーションから_Oracle Kubernetes Cluster(OKE)_への接続を確認します。

1.  WebLogic Kubernetes Tool Kit UIに戻り、_「アクティビティ」_をクリックして、WebLogic Kubernetes Tool Kit UIウィンドウを選択します。
    
2.  _「Kubernetes」_→_「クライアント構成」_をクリックし、_「接続の検証」_をクリックします。 ![接続の検証](images/verify-connectivity.png)
    
3.  _「Kubernetesクライアント接続の成功の確認」_ウィンドウが表示されたら、_「OK」_をクリックします。 ![正常に接続されました](images/successfully-connected.png)
    

## タスク5: WebLogic KubernetesオペレータのOracle Kubernetesクラスタへの更新

この項では、ターゲットKubernetesクラスタにWebLogic Kubernetes Operator ("operator")をインストールするためのサポートを提供します。

1.  **WebLogic演算子**をクリックします。次の構成詳細を指定し、**「演算子の更新」**をクリックします。
    
    **Kubernetesネームスペース** - オペレータをインストールするKubernetesネームスペース。次のスクリーンショットに示すように、vale **wko-operator-ns**を入力します。
    
    **Kubernetesサービス・アカウント** - Kubernetes APIリクエストの作成時に使用するオペレータのKubernetesサービス・アカウント。次のスクリーンショットに示すように、vale **wko-operator-sa**を入力します。
    
    **オペレータ・インストールに使用するHelmリリース名** - このインストールの識別に使用するHelmリリース名。次のスクリーンショットに示すように、vale **wko-weblogic-operator**を入力します。
    
    ![WebLogic演算子](images/weblogic-operator.png)
    
    > **参考情報:**  
    > !デフォルトでは、演算子の_「使用するイメージ・タグ」_フィールドは、GitHubコンテナ・レジストリの最新のオペレータ・リリース・バージョンに対応するイメージ・タグに設定されます。  
    > オペレータは、管理するKubernetesクラスタ内のWebLogicドメインを認識する必要があります。これはKubernetesネームスペース・レベルで行われるため、オペレータが管理するように構成されているKubernetesネームスペース内のWebLogicドメインは、インストールされるオペレータ・インスタンスによって管理されます。  
    > _Kubernetesネームスペース選択戦略_フィールドでは、_「ラベル・セレクタ」_を選択します。これは、_weblogic-operator=enabled_ラベルを持つKubernetesネームスペースがこの演算子によって管理されることを意味します。  
    > ![オペレータ・イメージ](images/operator-image.png)
    
    > 「クラスタ・ロール・バインディングの有効化」を有効にすると、オペレータのインストールによって、オペレータがすべての管理対象ネームスペースに使用するKubernetes ClusterRoleおよびClusterRoleBindingが作成されます。  
    > デフォルトでは、オペレータのREST APIはKubernetesクラスタ外では公開されません。REST APIを公開できるようにするには、_「外部にREST APIを公開」_を有効にします。 ![ロール・バインディング](images/role-binding.png)  
    
    > このペインでは、オペレータのJavaロギング構成をオーバーライドできます。これは、オペレータの問題をデバッグするときに便利です。  
    > ![Javaロギング](images/java-logging.png)  
    
    > _WebLogic Kubernetes Operator Image_、_Kubernetesネームスペース選択戦略_、_WebLogic Kubernetesロール・バインディング_、_外部REST APIアクセス_、_サード・パーティ統合_および_Javaロギング_の詳細は、[WebLogic Kubernetes Operator](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/)のドキュメントを参照してください。
    
2.  _WebLogic Kubernetes Operator Installation Complete_が表示されたら、_「OK」_をクリックします。 ![オペレータ設置済み](images/operator-installed.png)
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年10月