# Oracle Cloud Infrastructure (OCI)でOracle Container Engine for Kubernetes (OKE)と対話するようにKUBECTLを構成します

## 概要

このラボでは、Oracle Cloud Infrastructure上のKubernetes環境へのアクセスを許可する構成ファイルを作成するステップについて説明します。

### 製品・技術について

Oracle Cloud Infrastructure Container Engine for Kubernetesは、コンテナ・アプリケーションをクラウドにデプロイするために使用できる、完全に管理されたスケーラブルで可用性の高いサービスです。開発チームがクラウドネイティブ・アプリケーションを確実に構築、導入、管理したい場合は、Container Engine for Kubernetes(OKEと略されることもあります)を使用します。アプリケーションに必要なコンピュート・リソースを指定すると、OKEは既存のOCIテナンシでOracle Cloud Infrastructureにそれらをプロビジョニングします。

### 目的

この演習では、次のことを行います。

*   OCI Cloud Shellを開き、`kubectl`を構成してKubernetesクラスタと対話します。

### 前提条件

*   このワークショップでは、このワークショップがLiveLabsで予約されていることを前提としています。

## タスク1: `kubectl`の構成(KubernetesクラスタCLI)

Oracle Cloud Infrastructure (OCI)のCloud Shellは、Oracle CloudコンソールからアクセスできるWebブラウザベースのターミナルです。クラウド・シェルは、事前認証済のOracle Cloud Infrastructure CLIおよびその他の有用なツール(_Git、kubectl、helm、OCI CLI_)を使用してLinuxシェルにアクセスし、Verrazzanoチュートリアルを完了します。クラウド・シェルにはコンソールからアクセスできます。クラウド・シェルは、コンソールの永続フレームとしてOracle Cloudコンソールに表示され、コンソールの別のページに移動してもアクティブなままになります。

_クラウド・シェル_を使用して、このワークショップを完了します。

`kubectl`を使用して、クラウド・シェルを使用してクラスタをリモートで管理します。`kubeconfig`ファイルが必要です。これは事前認証済のOCI CLIを使用して生成されるため、使用を開始する前に行う設定はありません。

1.  コンソールで、次に示すように_「ハンバーガー・メニュー」→「開発者サービス」→「Kubernetesクラスタ(OKE)」_を選択します。
    
    ![ハンバーガ・メニュー](../setup-oke-ocishell/images/hamburgermenu.png " ")
    
2.  割り当てられているコンパートメントを選択します。次に、_cluster1_クラスタをクリックします。
    
3.  クラスタの詳細ページで_「クラスタへのアクセス」_をクリックします。
    
    ![クラスタへのアクセス](../setup-oke-ocishell/images/accesscluster.png " ")
    
    > Kubernetes構成ファイルを作成するために、クラウド・シェルを開くことができるダイアログが表示され、実行する必要があるカスタマイズされたOCIコマンドが含まれています。
    
4.  デフォルトの_クラウド・シェル・アクセス_のままにして、最初に_「コピー」_リンクを選択し、`oci ce...`コマンドをクラウド・シェルにコピーします。
    
    ![kubectl構成のコピー](../setup-oke-ocishell/images/copyconfig.png " ")
    
5.  ここで、_「クラウド・シェルの起動」_をクリックして、組込みコンソールを開きます。次に、コマンドを_クラウド・シェル_に貼り付ける前に構成ダイアログを閉じます。
    
    ![Cloud Shellの起動](../setup-oke-ocishell/images/launchcloudshell.png " ")
    
6.  クリップボードからコマンド(Ctrl+Vまたは右クリックしてコピー)をクラウド・シェルにコピーし、コマンドを実行します。
    
    たとえば、コマンドは次のようになります。
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![kubectl構成](../setup-oke-ocishell/images/kubeconfig.png " ")
    
7.  `get node`コマンドなどを使用して、`kubectl`が機能していることを確認します。次のような出力が表示されるまで、このコマンドを複数回実行する必要がある場合があります。
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.17    Ready    node    11m   v1.21.5
        10.0.10.184   Ready    node    11m   v1.21.5
        10.0.10.31    Ready    node    11m   v1.21.5
        
    
    > ノードの情報が表示された場合、構成は成功しました。
    
8.  クラウド・シェルの右上隅にあるコントロールを使用して、いつでも端末サイズを最小化およびリストアできます。
    
    ![クラウド・シェル](../setup-oke-ocishell/images/cloudshell.png " ")
    

この_クラウド・シェル_は開いたままにしておきます。以降の演習に使用します。

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年2月