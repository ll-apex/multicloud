# WebLogic OperatorをOracle Kubernetes Cluster (OKE)にデプロイ

## 概要

この演習では、ブラウザを使用してOCI CLIを認証し、_.OCI/config_ファイルを作成します。kubectlを使用して、_ローカル・アクセス_を使用してクラスタをリモートで管理します。_kubeconfig_ファイルが必要です。このkubeconfigファイルは、OCI CLIを使用して生成されます。次に、WebLogic Kubernetes Toolkit UIからKubernetesクラスタへの接続を確認します。最後に、WebLogic Kubernetes OperatorをKubernetesクラスタ(OKE)にインストールします。

見積時間: 10分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[OKEクラスタへのWebLogic演算子のデプロイ](videohub:1_0itbllhe)

### 目的

この演習では、次のことを行います。

*   Kubernetesクラスタに接続するようにkubectl (KubernetesクラスタCLI)を構成します。
*   WebLogic Kubernetes Toolkit UIからKubernetesクラスタへの接続を確認します。
*   WebLogic Kubernetes OperatorをKubernetesクラスタにインストールします。

## タスク1: Oracle Kubernetesクラスタに接続するためのkubectl (KubernetesクラスタCLI)の構成

このタスクでは、_/home/opc_ディレクトリに構成ファイル _.oci/config_および _.kube/config_を作成します。この構成ファイルを使用すると、この仮想マシンからOracle Kubernetesクラスタ(OKE)にアクセスできます。

1.  _「アクティビティ」_をクリックし、検索ボックスに_Firefox_と入力します。_Firefox_のアイコンをクリックします。 ![firefoxを開く](images/open-firefox.png)
    
2.  URL [https://cloud.oracle.com](https://cloud.oracle.com)を開きます。_クラウド・アカウント名_、Oracle Cloudアカウントの資格証明を入力し、_「サインイン」_をクリックします。
    
3.  コンソールで、次に示すように_「ハンバーガー・メニュー」_→_「開発者サービス」_→_「Kubernetesクラスタ(OKE)」_を選択します。 ![OKEアイコン](images/oke-icon.png)
    
4.  演習3で作成したクラスタ名をクリックし、_「クラスタへのアクセス」_をクリックします。 ![クラスタへのアクセス](images/access-cluster.png)
    
5.  _「ローカル・アクセス」_を選択し、次に示すように_「コピー」_をクリックします。 ![ローカル・アクセス](images/local-access.png)
    
6.  _「アクティビティ」_をクリックし、_「ターミナル」_を選択します。 ![終了](images/click-terminal.png)
    
7.  コピーしたコマンドを端末に貼り付けます。_「Do you want to create a new config file?」_で、_y_と入力し、_\[Enter\]_を押します。_「ブラウザからログインして構成ファイルを作成しますか。」_で、_y_と入力し、_\[Enter\]_を押します。 ![OCI構成](images/oci-config.png)
    
8.  Firefoxブラウザで、アクティブなセッションをクリックします。
    
    > 次に示すように、_「認可が完了しました」_が表示されます。 ![承認完了](images/authorization-complete.png)
    
9.  _「秘密キーのパスフレーズを入力」_で、空のままにして_\[Enter\]_を押します。 ![空のパスフレーズ](images/empty-passphrase.png)
    
10.  上矢印キーを使用して_oce ce ..._コマンドを再度実行し、_Kubeconfigファイル/home/opc/.kube/configに書き込まれた新しい構成_が表示されるまで複数回再実行します。 ![KubeConfigの作成](images/create-kubeconfig.png)
    

## タスク2: WebLogic Kubernetes Toolkit UIからOracle Kubernetesクラスタへの接続性の検証

このタスクでは、`WebLogic Kubernetes Toolkit UI`アプリケーションから_Oracle Kubernetes Cluster(OKE)_への接続を確認します。

1.  WebLogic Kubernetes Tool Kit UIに戻り、_「アクティビティ」_をクリックして、WebLogic Kubernetes Tool Kit UIウィンドウを選択します。
    
2.  _「Kubernetes」_→_「クライアント構成」_をクリックし、_「接続の検証」_をクリックします。 ![接続の検証](images/verify-connectivity.png)
    
3.  _「Kubernetesクライアント接続の成功の確認」_ウィンドウが表示されたら、_「OK」_をクリックします。 ![正常に接続されました](images/successfully-connected.png)
    

## タスク3: WebLogic Kubernetes OperatorのOracle Kubernetesクラスタへのインストール

この項では、ターゲットKubernetesクラスタにWebLogic Kubernetes Operator ("operator")をインストールするためのサポートを提供します。

1.  _WebLogic演算子_をクリックします。次の構成詳細を指定し、_「Install Operator」_をクリックします。
    
    **Kubernetesネームスペース** - オペレータをインストールするKubernetesネームスペース。デフォルト値のままにします。  
    **Kubernetesサービス・アカウント** - オペレータがKubernetes APIリクエストを行うときに使用するKubernetesサービス・アカウント。デフォルト値のままにします。  
    **オペレータ・インストールに使用するHelmリリース名** - このインストールの識別に使用するHelmリリース名。デフォルト値のままにします。  
    
    ![WebLogic演算子](images/weblogic-operator.png)
    
    > **参考情報:**  
    > !デフォルトでは、演算子の_「使用するイメージ・タグ」_フィールドは、GitHubコンテナ・レジストリの最新のオペレータ・リリース・バージョンに対応するイメージ・タグに設定されます。  
    > オペレータは、管理するKubernetesクラスタ内のWebLogicドメインを認識する必要があります。これはKubernetesネームスペース・レベルで行われるため、オペレータが管理するように構成されているKubernetesネームスペース内のWebLogicドメインは、インストールされるオペレータ・インスタンスによって管理されます。  
    > _Kubernetesネームスペース選択戦略_フィールドでは、_「ラベル・セレクタ」_を選択します。これは、_weblogic-operator=enabled_ラベルを持つKubernetesネームスペースがこの演算子によって管理されることを意味します。  
    > ![オペレータ・イメージ](images/operator-image.png)
    
    > 「クラスタ・ロール・バインディングの有効化」を有効にすると、オペレータのインストールによって、オペレータがすべての管理対象ネームスペースに使用するKubernetes ClusterRoleおよびClusterRoleBindingが作成されます。  
    > デフォルトでは、オペレータのREST APIはKubernetesクラスタ外では公開されません。REST APIを公開できるようにするには、_「外部にREST APIを公開」_を有効にします。[ロール・バインディング](images/role-binding.png)  
    
    > このペインでは、オペレータのJavaロギング構成をオーバーライドできます。これは、オペレータの問題をデバッグするときに便利です。  
    > ![Javaロギング](images/java-logging.png)  
    
    > _WebLogic Kubernetes Operator Image_、_Kubernetesネームスペース選択戦略_、_WebLogic Kubernetesロール・バインディング_、_外部REST APIアクセス_、_サード・パーティ統合_および_Javaロギング_の詳細は、[WebLogic Kubernetes Operator](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/)のドキュメントを参照してください。
    
2.  _WebLogic Kubernetes Operator Installation Complete_が表示されたら、_「OK」_をクリックします。 ![オペレータ設置済み](images/operator-installed.png)
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月