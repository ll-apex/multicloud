# Verrazzanoのインストール

## 概要

このラボでは、Oracle Cloud InfrastructureのKubernetesクラスタにVerrazzanoをインストールするステップについて説明します。

見積時間: 15分

### 製品・技術について

Verrazzanoは、クラウドネイティブ・アプリケーションと従来のアプリケーションをマルチクラウド環境とハイブリッド環境にデプロイするためのエンドツーエンドのエンタープライズ・コンテナ・プラットフォームです。厳選されたオープン・ソース・コンポーネントのセットで構成されており、その多くはすでに使用され、信頼されているものです。一部は、すべての要素をプルして、Verrazzanoを一貫性のある使いやすいプラットフォームにするために特別に作成されています。

Verrazzanoには次の機能が含まれています:

*   ハイブリッドおよびマルチクラスタのワークロード管理
*   WebLogic、CoherenceおよびHelidonアプリケーションの特別な処理
*   マルチクラスタ・インフラストラクチャ管理
*   統合された、事前準備済みのアプリケーション・モニタリング
*   統合セキュリティ
*   DevOpsおよびGitOpsの有効化

Verrazzanoでは、開発(`dev`)、本番(`prod`)および管理対象クラスタ(`managed-cluster`)といったインストール・プロファイルがサポートされています。

次のイメージは、Verrazzanoのインストール・プロファイルを示しています。 ![インストール・プロファイル](images/installprofile.png)

次のいずれかのコマンドでプロファイルを変更するには、_VZ\_PROFILE_環境変数をインストールするプロファイルの名前に設定します。

Verrazzano構成オプションの詳細は、[Verrazzanoカスタム・リソース定義](https://verrazzano.io/docs/reference/api/verrazzano/verrazzano/)を参照してください。

このラボでは、次の特性を持つ_Verrazzanoの開発プロファイル_をインストールします:

*   ワイルドカード(nip.io)DNS
*   自己署名証明書
*   システム・コンポーネントとすべてのアプリケーションで使用される共有監視スタック
*   可観測性スタック用のエフェメラル・ストレージ(ポッドが再起動されると、すべてのログおよびメトリックが失われます)
*   軽量な設置です。
*   評価目的です。
*   単一ノードのOpensearchクラスタ・トポロジ。

Verrazzanoは、厳選された一連のオープン・ソース・コンポーネントをインストールします。次の表に、各コンポーネント、バージョンおよび簡単な説明を示します。

![Verrazzanoプロファイル](images/verrazzano-profile.png " ") ![Verrazzanoプロファイル](images/verrazzano-profiles.png " ")

DNSの選択によると、nip.io (ワイルドカードDNS)または[Oracle OCI DNS](https://docs.cloud.oracle.com/en-us/iaas/Content/DNS/Concepts/dnszonemanagement.htm)を使用できます。この演習では、nip.io (ワイルドカードDNS)を使用してインストールします。

イングレス・コントローラは、(IPアドレスを提供することで)外部へのDockerコンテナへのアクセスを提供するのに役立つものです。イングレスは、IPアドレスを別のクラスタにルーティングします。

### 目的

この演習では、次のことを行います。

*   Oracle Kubernetes Engineクラスタを使用するように`kubectl`を設定します
*   Verrazzano vz CLIをインストールします。
*   Verrazzanoの開発(`dev`)プロファイルをインストールします。

### 事前設定

Verrazzanoには次が必要です:

*   Kubernetesクラスタおよび互換性のある`kubectl`。
*   Kubernetesワーカー・ノードでは、少なくとも2つのCPU、100GBのディスク・ストレージおよび16GBのRAMを使用できます。これは、Verrazzanoの開発プロファイルをインストールするのに十分です。デプロイするアプリケーションのリソース要件によっては、アプリケーションのデプロイにこれで十分でない場合もあります。
*   演習1で、Oracle Cloud InfrastructureにKubernetesクラスタを作成しました。Kubernetesクラスタ_cluster1_を使用して、Verrazzanoの開発プロファイルをインストールします。

## タスク1: `kubectl`の構成(KubernetesクラスタCLI)

Oracle Cloud Infrastructure (OCI)のCloud Shellは、Oracle CloudコンソールからアクセスできるWebブラウザベースのターミナルです。クラウド・シェルは、事前認証済のOracle Cloud Infrastructure CLIおよびその他の有用なツール(_Git、kubectl、helm、OCI CLI_)を使用してLinuxシェルにアクセスし、Verrazzanoチュートリアルを完了します。クラウド・シェルにはコンソールからアクセスできます。クラウド・シェルは、コンソールの永続フレームとしてOracle Cloudコンソールに表示され、コンソールの別のページに移動してもアクティブなままになります。

_クラウド・シェル_を使用して、このワークショップを完了します。

`kubectl`を使用して、クラウド・シェルを使用してクラスタをリモートで管理します。`kubeconfig`ファイルが必要です。これは事前認証済のOCI CLIを使用して生成されるため、使用を開始する前に行う設定はありません。

1.  クラスタの詳細ページで_「クラスタへのアクセス」_をクリックします。
    
    > そのページから移動した場合は、ナビゲーション・メニューを開き、_「開発者サービス」_で_「Kubernetesクラスタ(OKE)」_を選択します。クラスタを選択し、詳細ページに移動します。
    
    ![クラスタへのアクセス](images/access-cluster.png " ")
    
    > Kubernetes構成ファイルを作成するために、クラウド・シェルを開くことができるダイアログが表示され、実行する必要があるカスタマイズされたOCIコマンドが含まれています。
    
2.  デフォルトの_クラウド・シェル・アクセス_のままにして、最初に_「コピー」_リンクを選択し、`oci ce...`コマンドをクラウド・シェルにコピーします。
    
    ![kubectl構成のコピー](images/copy-config.png " ")
    
3.  ここで、_「クラウド・シェルの起動」_をクリックして、組込みコンソールを開きます。次に、コマンドを_クラウド・シェル_に貼り付ける前に構成ダイアログを閉じます。
    
    ![Cloud Shellの起動](images/launch-cloudshell.png " ")
    
4.  クリップボードからコマンド(Ctrl+Vまたは右クリックしてコピー)をクラウド・シェルにコピーし、コマンドを実行します。
    
    たとえば、コマンドは次のようになります。
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![kubectl構成](images/kube-config.png " ")
    
5.  `get node`コマンドなどを使用して、`kubectl`が機能していることを確認します。次のような出力が表示されるまで、このコマンドを複数回実行する必要がある場合があります。
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.101   Ready    node    11m   v1.26.2
        10.0.10.29    Ready    node    11m   v1.26.2
        10.0.10.37    Ready    node    11m   v1.26.2
        
    
    > ノードの情報が表示された場合、構成は成功しました。
    

## タスク2: Verrazzano CLIのインストール

1.  最新のvz CLIをダウンロードします。
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz</copy>
        
    
    結果は次のようになります。
    
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
        

0 0 0 0 0 0 0 --:--:-- --:--:--:-- --:--:--:-- 0 100 39.7M 100 39.7M 0 0 23.6M 0 0:00:01 0:00:01 --:--:-- 32.7M \`\`\`

2.  チェックサム・ファイルをダウンロードします。
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        

結果は次のようになります。

    ```bash
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    

0 0 0 0 0 0 0 --:--:-- --:--:--:-- --:--:-- 0 100 102 100 102 0 218 0 --:--:--:--:--:--:-- --:--:--:--:--:--:--:--:--:--:-- 218 \`\`\`

3.  バイナリをチェックサム・ファイルに対して検証します。
    
        <copy>sha256sum -c verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        
    
    結果は次のようになります。
    
        verrazzano-1.6.2-linux-amd64.tar.gz: OK
        
4.  解凍してvzバイナリ・ディレクトリに移動します。
    
        <copy>tar xvf verrazzano-1.6.2-linux-amd64.tar.gz
        cd ~/verrazzano-1.6.2/bin/</copy>
        
5.  インストールしたバージョンが最新であることを確認するテスト。
    
        <copy>./vz version</copy>
        
    
    結果は次のようになります。
    
        Version: v1.6.2
        BuildDate: 2023-07-21T12:06:02Z
        GitCommit: 5cd8a2f8670000d2ef0b02404e3169699d87f26b
        

## タスク3: Verrazzano開発プロファイルのインストール

インストール・プロファイルは、名前で参照できるVerrazzano設定の既知の構成であり、必要に応じてカスタマイズできます。

Verrazzanoでは、開発(`dev`)、本番(`prod`)および管理対象クラスタ(`managed-cluster`)といったインストール・プロファイルがサポートされています。

1.  nip.io DNSメソッドを使用してインストールします。次のコマンドをコピーし、_クラウド・シェル_に貼り付けてVerrazzanoをインストールします。
    
        <copy>./vz install -f - <<EOF
        apiVersion: install.verrazzano.io/v1beta1
        kind: Verrazzano
        metadata:
          name: example-verrazzano
        spec:
          profile: dev
        EOF
        </copy>
        
    
    結果は次のようになります。
    
        Installing Verrazzano version v1.6.2
        Applying the file https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-platform-operator.yaml
        customresourcedefinition.apiextensions.k8s.io/verrazzanos.install.verrazzano.io created
        namespace/verrazzano-install created
        serviceaccount/verrazzano-platform-operator created
        clusterrole.rbac.authorization.k8s.io/verrazzano-managed-cluster created
        clusterrolebinding.rbac.authorization.k8s.io/verrazzano-platform-operator created
        service/verrazzano-platform-operator created
        service/verrazzano-platform-operator-webhook created
        deployment.apps/verrazzano-platform-operator created
        deployment.apps/verrazzano-platform-operator-webhook created
        mutatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-mysql-backup created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-operator-webhook created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-mysqlinstalloverrides created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-requirements-validator created
        Waiting for verrazzano-platform-operator to be ready before starting install - 23 seconds
        
    
    > インストールの完了には約15分から20分かかります。このコマンドは、Verrazzanoプラットフォーム・オペレータをインストールし、Verrazzanoカスタム・リソースを適用します。インストールが完了するか、デフォルトのタイムアウト(30分)に達するまで、インストール・ログがコマンド・ウィンドウにストリーミングされます。
    
2.  _クラウド・シェル_を開いたままにして、インストールを実行します。
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月