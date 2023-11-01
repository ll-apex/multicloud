# Verrazzanoのインストール

## 概要

このラボでは、Oracle Cloud InfrastructureのKubernetesクラスタにVerrazzanoをインストールするステップについて説明します。

見積時間: 20分

### 製品・技術について

Verrazzanoは、クラウドネイティブ・アプリケーションと従来のアプリケーションをマルチクラウド環境とハイブリッド環境にデプロイするためのエンドツーエンドのエンタープライズ・コンテナ・プラットフォームです。厳選されたオープン・ソース・コンポーネントのセットで構成されており、その多くはすでに使用され、信頼されているものです。一部は、すべての要素をプルして、Verrazzanoを結束性が高く、使いやすいプラットフォームにするために特別に作成されています。

Verrazzanoには次の機能が含まれています:

*   ハイブリッドおよびマルチクラスタのワークロード管理
*   WebLogic、CoherenceおよびHelidonアプリケーションの特別な処理
*   マルチクラスタ・インフラストラクチャ管理
*   統合された、事前準備済みのアプリケーション・モニタリング
*   統合セキュリティ
*   DevOpsおよびGitOpsの有効化

### 目的

この演習では、次のことを行います。

*   Verrazzanoコマンドライン・ツールをインストールします。
*   Verrazzanoの開発(`dev`)プロファイルをインストールします。
*   Verrazzanoが正常にインストールされたことを確認します。

### 事前設定

Verrazzanoには次が必要です:

*   Kubernetesクラスタおよび互換性のある`kubectl`。
*   Kubernetesワーカー・ノードでは、少なくとも2つのCPU、100GBのディスク・ストレージおよび16GBのRAMを使用できます。これは、Verrazzanoの開発プロファイルをインストールするのに十分です。デプロイするアプリケーションのリソース要件によっては、アプリケーションのデプロイにこれで十分でない場合もあります。

演習1で、Oracle Cloud InfrastructureにKubernetesクラスタを作成しました。Kubernetesクラスタ\*cluster1\*を使用して、Verrazzanoの開発プロファイルをインストールします。 演習1で、Oracle Cloud Infrastructure上のKubernetesクラスタにアクセスするための構成ファイルを作成しました。Kubernetesクラスタ\*cluster1\*を使用して、Verrazzanoの開発プロファイルをインストールします。

## タスク1: vz CLIのインストール

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
        

## タスク2: Verrazzano開発プロファイルのインストール

インストール・プロファイルは、名前で参照できるVerrazzano設定の既知の構成であり、必要に応じてカスタマイズできます。

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
        \>取付けを完了するのに約15から20分かかります。このコマンドは、Verrazzanoプラットフォーム・オペレータをインストールし、Verrazzanoカスタム・リソースを適用します。 \>インストールの完了には約8分から10分かかります。このコマンドは、Verrazzanoプラットフォーム・オペレータをインストールし、Verrazzanoカスタム・リソースを適用します。
2.  インストールが完了するまで待ちます。インストールが完了するか、デフォルトのタイムアウト(30分)に達するまで、インストール・ログがコマンド・ウィンドウにストリーミングされます。
    

## タスク3: Verrazzanoのインストールの成功の確認

Verrazzanoは複数のオブジェクトを複数のネームスペースにインストールします。Verrazzanoコンポーネントは、ネームスペース_verrazzano-system_にインストールされます。

1.  複数のオブジェクトに関連付けられたすべてのポッドが_「実行中」_ステータスであることを確認してください。
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    結果は次のようになります。
    
        $   kubectl get pods -n verrazzano-system
        NAME                                    READY   STATUS  RESTARTS AGE
        coherence-operator-b5dc669c6-rk2sm      1/1     Running   1       23m
        fluentd-54f5x                           2/2     Running   1       11m
        fluentd-h7mgh                           2/2     Running   1       11m
        fluentd-xcdfz                           2/2     Running   0       11m
        oam-kubernetes-runtime-5b48f944b-cx7b9  1/1     Running   0       24m
        verrazzano-application-operator-665c5   1/1     Running   0       22m
        verrazzano-application-operator-webhook 1/1     Running   0       22m
        verrazzano-authproxy-67776ff58b-8l      3/3     Running   0       21m
        verrazzano-cluster-operator-67dc5695    1/1     Running   0       14m
        verrazzano-cluster-operator-webhook     1/1     Running   0       14m
        verrazzano-console-7d95c98cb9-9ql2      2/2     Running   0       20m
        verrazzano-monitoring-operator-59f      2/2     Running   0       21m
        vmi-system-es-master-0                  2/2     Running   0       19m
        vmi-system-grafana-7fd956b585-2tbgl     3/3     Running   0       19m
        vmi-system-kiali-dd87546d6-ddxss        2/2     Running   0       22m
        vmi-system-osd-7687d6fccf-nm7kt         2/2     Running   0       14m
        weblogic-operator-54979449f4-njgrq      2/2     Running   0       22m
        weblogic-operator-webhook-f7ff8c8cf     1/1     Running   0       22m
        
    
    _クラウド・シェル_は開いたままにしておきます。演習4で必要です。
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月