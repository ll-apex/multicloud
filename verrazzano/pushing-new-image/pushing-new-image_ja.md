# bobbys-helidon-stock-applicationのDockerイメージをOracle Cloud Container Registryにプッシュ

## 概要

演習5では、bobbys-helidon-stock-applicationを変更し、新しいDockerイメージを構築しました。このラボでは、そのイメージをOracle Cloud Container Registry内のリポジトリにプッシュします。

所要時間: 10分

### 目的

この演習では、次のことを行います。

*   Oracle Cloud Container Registryにログインするための認証トークンを生成します。
*   bobbys-helidon-stock-application DockerイメージをOracle Cloud Container Registryリポジトリにプッシュします。

### 前提条件

コマンドとURLを貼り付け、環境に応じて変更できるテキスト・エディタが必要です。その後、変更したコマンドをコピーして貼り付け、_クラウド・シェル_で実行できます。

## タスク1: Oracle Cloud Container Registryにログインするための認証トークンの生成

このステップでは、Oracle Cloud Container Registryへのログインに使用する_認証トークン_を生成します。

1.  右上隅にある「ユーザー」アイコンを選択し、_「マイ・プロファイル」_を選択します。
    
    ![ユーザー設定](images/user-settings.png " ")
    
2.  下にスクロールして_「認証トークン」_を選択します。
    
    ![認証トークン](images/auth-token.png " ")
    
3.  _「トークンの生成」_をクリックします。
    
    ![トークンの生成](images/generate-token.png " ")
    
4.  _`helidon-stock-application-your_first_name`_をコピーして_「説明」_ボックスに貼り付け、_「トークンの生成」_をクリックします。
    
    ![トークン作成](images/token-create.png " ")
    
5.  「生成済トークン」の下の_「コピー」_を選択し、テキスト・ファイルに貼り付けます。後でコピーできません。
    
    ![トークンのコピー](images/copy-token.png " ")
    

## タスク2: bobbys-helidon-stock-application DockerイメージのOracle Cloud Container Registryリポジトリへのプッシュ

1.  この演習のタスク1で、URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)を開いて、リージョン名のエンドポイントを特定し、テキスト・エディタにコピーしました。この例では、リージョン名はUK South (London)です。このタスクには、この情報が必要です。 ![エンド・ポイント](images/end-point.png)
    
2.  次のコマンドをコピーしてテキスト・エディタに貼り付け、**`END_POINT_OF_REGION_NAME`**をリージョンのエンドポイントに置き換えます。
    
        <copy> docker login `END_POINT_OF_REGION_NAME`</copy>
        

3\. 前の演習で、テナンシ・ネームスペースを決定しました。ユーザー名は次のようにします: \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`/oracleidentitycloudservice/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\*。ここで、\*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`\*\*をテナンシの名前空間に置き換え、\*\*\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\*をOracle Cloudアカウントのユーザー名に置き換え、置換したユーザー名をテキスト・ファイルからコピーして\*クラウド・シェル\*に貼り付けます。「パスワード」に、テキスト・エディタまたは保存した場所に認証トークンを貼り付けます。!\[ログイン・レジストリ\](images/login-registry.png " ") 3.前の演習で、テナンシ・ネームスペースを決定しました。ユーザー名は次のようにします: \`NAMESPACE\_OF\_YOUR\_TENANCY\`/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`。dockerログインの実行中に、テナンシ・ネームスペースおよびユーザー名を小文字で使用する必要があります。ここで、\`NAMESPACE\_OF\_YOUR\_TENANCY\`をテナンシの名前空間に置き換え、\`YOUR\_ORACLE\_CLOUD\_USERNAME\`をOracle Cloudアカウントのユーザー名に置き換え、置換したユーザー名をテキスト・エディタからコピーして\*Cloud Shell\*に貼り付けます。\[パスワード\]で、テキスト エディタまたは保存した場所に認証トークンを貼り付けます。

4.  コンテナ・レジストリに戻り、_「ハンバーガー・メニュー」→「開発者サービス」→「コンテナ・レジストリ」_を選択します。
    
    ![コンテナ・レジストリ](images/container-registry.png " ")
    
5.  コンパートメントを選択し、_「リポジトリの作成」_をクリックします。
    
    ![リポジトリの作成](images/repository-create.png " ")
    
6.  コンパートメントを選択し、「リポジトリ名」として_`helidon-stock-application-your_first_name`_と入力してから、「アクセス」に_「パブリック」_を選択し、_「作成」_をクリックします。
    
    ![リポジトリ・データ](images/repository-data.png " ")
    
    リポジトリ_`helidon-stock-application-your_first_name`_の作成後、ネームスペースを検証でき、テナンシのネームスペースと同じである必要があります。
    
    !\[ネームスペースの検証\](images/verify-namespace.png" ")
    
7.  ラボ5の一部として、Dockerイメージのフルネームをテキスト・エディタにコピーしました。DockerイメージをOracle Cloud Container Registry内のリポジトリにプッシュするには、テキスト・エディタで次のコマンドをコピーして貼り付け、_`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`helidon-stock-application-your_first_name`:1.0_をテキスト・エディタで保存したDockerイメージのフルネームに置き換えます。
    
        <copy>docker push `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/helidon-stock-application-your_first_name:1.0</copy>
        
    
    ![Dockerプッシュ](images/docker-push.png " ")
    
    _docker push_コマンドが正常に実行された後、_`helidon-stock-application-your_first_name`_リポジトリを展開すると、新しいイメージがこのリポジトリにアップロードされていることがわかります。
    
    _クラウド・シェル_およびコンテナ・レジストリのリポジトリ・ページは開いたままにしておきます。次の演習で必要になります。
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年3月