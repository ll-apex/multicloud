# 演習環境の設定

## 概要

この演習では、Oracle Cloud Container Image Registry内にリポジトリを作成します。さらに、Oracle Container RegistryのWebLogicサーバー・イメージのライセンス契約に同意します。

見積時間: 15分

### 目的

この演習では、次のことを行います。

*   Oracle Cloud Container Image Registry内にリポジトリを作成します。
*   Oracle Container RegistryのWebLogicサーバー・イメージのライセンスを受け入れます。

### 事前設定

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)が有効になっているアカウントが必要です。
*   Oracle Accountが必要です。
*   テキスト・エディタが必要です。

## タスク1: リポジトリの作成

このタスクでは、パブリック・リポジトリを作成します。演習5では、このリポジトリに補助イメージをプッシュします。

1.  リモートデスクトップで、Firefoxを開くには、「**Activities**」をクリックし、検索ボックスに「**Fire**」と入力して、次に示すように「**Firefox**」をクリックします。 ![firefoxを開く](images/open-firefox.png)
    
2.  資格証明を使用して[Oracle Cloudコンソール](https://cloud.oracle.com)にログインします。
    
3.  コンソールで、次に示すように_「ハンバーガー・メニュー」_→_「開発者サービス」_→_「コンテナ・レジストリ」_を選択します。 ![コンテナ・レジストリ](images/container-registry.png)
    
4.  リポジトリの作成が許可されているコンパートメントを選択します。_「リポジトリの作成」_をクリックします。 ![リポジトリの作成](images/create-repository.png)
    
5.  リポジトリ名として_`test-model-your_firstname`_を入力し、_「パブリック」_として「アクセス」を入力し、_「作成」_をクリックします。 ![リポジトリの詳細](images/repository-details.png)
    
6.  リポジトリの準備が整ったら。テキスト・ファイルのテナンシ・ネームスペースを書き留めておいてください。 ![ノート・テナンシNameSpace](images/tenancy-namespace.png)
    

## タスク2: WebLogicサーバー・イメージのライセンスの受入れ

このタスクでは、Oracle Container Registryに存在するWebLogicサーバー・イメージのライセンス契約に同意します。演習5と同様に、WebLogic Server 12.2.1.3.0イメージをプライマリ・イメージとして使用します。そのため、WebLogicサーバー・イメージにアクセスするには、ライセンス契約に同意します。

1.  Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/)のリンクをコピーしてブラウザに貼り付け、サインインします。そのためには、Oracle Accountが必要です。 ![コンテナ・レジストリのサインイン](images/container-registry-sign-in.png)
    
2.  「ユーザー名」および「パスワード」フィールドに_Oracle Account資格証明_を入力し、_「サインイン」_をクリックします。 ![ログイン・コンテナ・レジストリ](images/login-container-registry.png)
    
3.  Oracle Container Registryのホーム・ページで、_weblogic_を検索します。 ![検索 WebLogic](images/search-weblogic.png)
    
4.  次に示すように_「weblogic」_をクリックし、言語として_「English」_を選択し、_「Continue」_をクリックします。![WebLogicをクリックします。](images/click-weblogic.png) ![言語の選択](images/select-language.png)
    
5.  「_Accept_」をクリックしてライセンス契約書に同意します。 ![ライセンスに同意](images/accept-license.png)
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年10月