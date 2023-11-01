# 演習環境の設定

## 概要

このラボでは、必要なすべてのネットワーク・リソースで構成された3ノードのKubernetesクラスタを作成します。また、Oracle Cloud Container Image Registry内にリポジトリを作成します。次に、認証トークンを生成します。さらに、Oracle Container RegistryのWebLogicサーバー・イメージのライセンス契約に同意します。

見積時間: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[ラボ環境の設定](videohub:1_zhvohpqq)

### 目的

この演習では、次のことを行います。

*   Oracle Kubernetesクラスタを作成します。
*   Oracle Cloud Container Image Registry内にリポジトリを作成します。
*   認証トークンを生成します。
*   Oracle Container RegistryのWebLogicサーバー・イメージのライセンスを受け入れます。

### 事前設定

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)が有効になっているアカウントが必要です。
*   Oracle Accountが必要です。
*   テキスト・エディタが必要です。

## タスク1: Oracle Kubernetesクラスタの作成

_クイック作成_機能では、デフォルト設定を使用して、必要に応じて新しいネットワーク・リソースを含む_クイック・クラスタ_を作成します。このアプローチは、新しいクラスタを作成する最速の方法です。すべてのデフォルト値を受け入れると、わずか数回のクリックで新しいクラスタを作成できます。クラスタの新しいネットワーク・リソースは、ノード・プールおよび3つのワーカー・ノードとともに自動的に作成されます。

1.  コンソールで、次に示すように_「ハンバーガー・メニュー」→「開発者サービス」→「Kubernetesクラスタ(OKE)」_を選択します。
    
    ![ハンバーガ・メニュー](images/hamburger-menu.png " ")
    
2.  「クラスタ・リスト」ページでコンパートメントを選択し、_「クラスタの作成」_をクリックします。
    
    > クラスタの作成が許可されているコンパートメントと、Oracle Container Registry内のリポジトリを選択する必要があります。
    
    ![コンパートメントの選択](images/select-compartment.png " ")
    
3.  「クラスタ・ソリューションの作成」ダイアログで、_「クイック作成」_を選択して_「送信」_をクリックします。
    
    ![ワークフローの起動](images/launch-workflow.png " ")
    
    _クイック作成_では、新しいクラスタの新しいネットワーク・リソースとともに、デフォルト設定を使用して新しいクラスタが作成されます。
    
    クラスタ作成ページで次の構成詳細を指定します(_「シェイプ」_フィールドに配置した値に注意してください)。
    
    *   **名前**: クラスタの名前。デフォルト値のままにします。
    *   **コンパートメント**: コンパートメントの名前。リソースの作成を許可するコンパートメントを選択します。
    *   **Kubernetesバージョン**: Kubernetesのバージョン。Kubernetesバージョンとして_1.26.2_を選択します。
    *   **Kubernetes APIエンドポイント**: クラスタ・マスター・ノードがルーティング可能かどうか。_「パブリック・エンドポイント」_値を選択します。
    *   **ノード・タイプ**: ノード・タイプとして_「管理対象」_を選択します。
    *   **Kubernetesワーカー・ノード**: クラスタ・ワーカー・ノードがルーティング可能かどうか。デフォルトの_「非公開就業者」_値のままにします。
    *   **シェイプ**: ノード・プールの各ノードに使用するシェイプ。シェイプによって、CPUの数および各ノードに割り当てられるメモリー量が決まります。リストには、OKEでサポートされているテナンシで使用可能なシェイプのみが表示されます。_VM.Standard.E4を選択します。フレックス_(通常はOracle Free Tierアカウントで使用できます)。メモリー量として1 OCPUおよび16 GBを選択します。
    *   **イメージ**: Kubernetesバージョン_1.26.2_のデフォルト・イメージを選択します。
    *   **ノード数**: 作成するワーカー・ノードの数。デフォルト値は_3_のままにします。
    
    ![クイック・クラスタ](images/quick-cluster1.png " ") ![入力データ](images/enter-data.png " ")
    
4.  _「次」_をクリックして、新しいクラスタ用に入力した詳細を確認します。
    
5.  _「確認」_ページで、_「基本クラスタの作成」_のチェック・ボックスを選択し、_「クラスタの作成」_をクリックして、新しいネットワーク・リソースおよび新しいクラスタを作成します。
    
    ![クラスタのレビュー](images/review-cluster.png " ")
    
    > 作成したネットワーク・リソースが表示されます。ノード・プールを作成するリクエストが開始されるまで待機してから、_「閉じる」_をクリックします。
    
    ![ネットワーク・リソース](images/network-resource.png " ")
    
    > 次に、新しいクラスタが_「クラスタの詳細」_ページに表示されます。マスター・ノードが作成されると、新しいクラスタのステータスは_「アクティブ」_になります(約7分かかります)。待機する必要はありません。次のタスクに進んでください。
    
    ![クラスタ・アクセス](images/cluster-access.png " ")
    

## タスク2: リポジトリの作成

このタスクでは、パブリック・リポジトリを作成します。演習5では、このリポジトリに補助イメージをプッシュします。

1.  コンソールで、次に示すように_「ハンバーガー・メニュー」_→_「開発者サービス」_→_「コンテナ・レジストリ」_を選択します。 ![コンテナ・レジストリ](images/container-registry.png)
    
2.  リポジトリの作成が許可されているコンパートメントを選択します。_「リポジトリの作成」_をクリックします。 ![リポジトリの作成](images/create-repository.png)
    
3.  リポジトリ名として_`test-model-your_firstname`_を入力し、_「パブリック」_として「アクセス」を入力し、_「作成」_をクリックします。 ![リポジトリの詳細](images/repository-details.png)
    
4.  リポジトリの準備が整ったら。テキスト・エディタ内のテキスト・ファイル内のテナンシ・ネームスペースに注意してください。 ![ノート・テナンシNameSpace](images/tenancy-namespace.png)
    

## タスク3: 認証トークンの生成

このタスクでは、_認証トークン_を生成します。演習5では、この認証トークンを使用して補助イメージをOracle Cloud Container Registryリポジトリにプッシュします。

1.  右上隅にある「ユーザー・アイコン」を選択し、_MyProfile_を選択します。
    
    ![自分のプロファイル](images/my-profile.png)
    
2.  下にスクロールして_「認証トークン」_を選択し、_「トークンの生成」_をクリックします。
    
    ![認証トークン](images/auth-token.png)
    
3.  _`test-model-your_first_name`_をコピーして_「説明」_ボックスに貼り付け、_「トークンの生成」_をクリックします。
    
    ![トークンの作成](images/create-token.png)
    
4.  「生成済トークン」の下の_「コピー」_を選択し、テキスト・エディタに貼り付けます。後でコピーできません。_「閉じる」_をクリックします。
    
    ![トークンのコピー](images/copy-token.png)
    

## タスク4: WebLogicサーバー・イメージのライセンスの受入れ

このタスクでは、Oracle Container Registryに存在するWebLogicサーバー・イメージのライセンス契約に同意します。演習3と同様に、WebLogic Server 12.2.1.3.0イメージをプライマリ・イメージとして使用します。そのため、WebLogicサーバー・イメージにアクセスするには、ライセンス契約に同意します。

1.  Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/)のリンクをクリックしてサインインします。そのためには、Oracle Accountが必要です。 ![コンテナ・レジストリのサインイン](images/container-registry-sign-in.png)
    
2.  「ユーザー名」および「パスワード」フィールドに_Oracle Account資格証明_を入力し、_「サインイン」_をクリックします。 ![ログイン・コンテナ・レジストリ](images/login-container-registry.png)
    
3.  Oracle Container Registryのホーム・ページで、_weblogic_を検索します。 ![検索 WebLogic](images/search-weblogic.png)
    
4.  次に示すように_「weblogic」_をクリックし、言語として_「English」_を選択し、_「Continue」_をクリックします。![WebLogicをクリックします。](images/click-weblogic.png) ![言語の選択](images/select-language.png)
    
5.  「_Accept_」をクリックしてライセンス契約書に同意します。 ![ライセンスに同意](images/accept-license.png)
    

## さらに学ぶ

_Oracle Cloud Infrastructure Container Engine for Kubernetesについて_

Oracle Cloud Infrastructure Container Engine for Kubernetesは、コンテナ・アプリケーションをクラウドにデプロイするために使用できる、完全に管理されたスケーラブルで可用性の高いサービスです。開発チームがクラウドネイティブ・アプリケーションを確実に構築、導入、管理したい場合は、Container Engine for Kubernetes(OKEと略されることもあります)を使用します。アプリケーションに必要なコンピュート・リソースを指定すると、OKEは既存のOCIテナンシでOracle Cloud Infrastructureにそれらをプロビジョニングします。

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月