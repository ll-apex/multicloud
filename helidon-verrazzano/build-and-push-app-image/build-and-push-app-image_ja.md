# Oracle Cloud Container RegistryへのHelidonアプリケーション・イメージのプッシュ

## 概要

この演習では、Helidonアプリケーションを使用してDockerイメージを構築し、そのイメージをOracle Cloud Container Registry内のリポジトリにプッシュします。

見積時間: 10分

### 目的

*   Dockerを使用してアプリケーションを構築およびパッケージ化します。
*   Oracle Cloud Container Registryにログインするための認証トークンを生成します。
*   HelidonアプリケーションDockerイメージをOracle Cloud Container Registryリポジトリにプッシュします。

### 事前設定

*   前の演習で作成したHelidonアプリケーション
*   Docker
*   Oracle Cloudアカウント

## タスク1: HelidonアプリケーションDockerイメージの構築

まず、Verrazzanoにデプロイするために使用するDockerイメージを準備します。

OCIアカウントに属するOracle Cloud Container RegistryにアップロードするDockerイメージを作成しています。そのためには、レジストリの座標を反映するイメージ名を作成する必要があります。

必要な情報は次のとおりです。

*   テナント・ネームスペース
*   リージョンのエンドポイント

1.  テナンシのネームスペースを検索するには、次に示すように_「ユーザー」_アイコン-> _「テナンシ」_をクリックします。**オブジェクト・ストレージ設定**には、ネームスペースがあります。後で使用するため、コピーしてテキスト・ファイルに保存します。
    
    ![テナンシネームスペースのコピー](images/copy-tenancynamespace.png " ")
    
2.  _「Endpoint for Your Region」_を見つけます。このURL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)に記載されている表を参照してください。この例では、リージョンのエンドポイントは_UK South (London)_ (リージョン名)で、そのエンドポイントは_lhr.ocir.io_です。自分の_リージョン名_のエンドポイントを見つけ、テキスト・ファイルに保存します。次の研究室にも必要です。
    
    ![エンド・ポイント](images/end-point.png " ")
    
    > これで、リージョンのテナンシ・ネームスペースとエンドポイントの両方が用意されました。
    
3.  次のコマンドをコピーして、テキスト・エディタに貼り付けます。次に、_`ENDPOINT_OF_YOUR_REGION`_をリージョン名のエンドポイント、_`NAMESPACE_OF_YOUR_TENANCY`_をテナンシの名前空間、_`your_first_name`_を自分の名に置き換えます。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0 .</copy>
        
    
    コマンドの準備が整ったら、_`~/quickstart-mp/`_ディレクトリからクラウド・シェルで実行します。ビルドによって次の結果が生成されます。
    
        $ cd ~/quickstart-mp/
        $ docker build lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0 .
        > docker pull lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        [+] Building 107.5s (19/19) FINISHED                                                                                                            
        => [internal] load build definition from Dockerfile                                                                                       0.1s
        => => transferring dockerfile: 785B                                                                                                       0.1s
        => [internal] load .dockerignore                                                                                                          0.1s
        => => transferring context: 48B                                                                                                           0.0s
        => [internal] load metadata for docker.io/library/openjdk:11-jre-slim                                                                     3.7s
        => [internal] load metadata for docker.io/library/maven:3.6-jdk-11                                                                        2.8s
        => [auth] library/openjdk:pull token for registry-1.docker.io                                                                             0.0s
        => [auth] library/maven:pull token for registry-1.docker.io                                                                               0.0s
        => [stage-1 1/4] FROM docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527      33.3s
        => => resolve docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527               0.0s
        => => sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527 549B / 549B                                                 0.0s
        => => sha256:f3cdb8fd164057f4ef3e60674fca986f3cd7b3081d55875c7ce75b7a214fca6d 1.16kB / 1.16kB                                             0.0s
        => => sha256:9c9e40a31d4fa290f933d76f3b0a4183ba02a7298a309f6bfa892d618e7196ef 7.56kB / 7.56kB                                             0.0s
        => => sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717 31.36MB / 31.36MB                                          18.6s
        => => sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251 1.58MB / 1.58MB                                             1.6s
        => => sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c 211B / 211B                                                 0.7s
        => => sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358 47.13MB / 47.13MB                                          24.4s
        => => extracting sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717                                                  7.7s
        => => extracting sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251                                                  0.3s
        => => extracting sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c                                                  0.0s
        => => extracting sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358                                                  5.7s
        => [build 1/7] FROM docker.io/library/maven:3.6-jdk-11@sha256:1d29ccf46ef2a5e64f7de3d79a63f9bcffb4dc56be0ae3daed5ca5542b38aa2d            0.0s
        => [internal] load build context                                                                                                          0.1s
        => => transferring context: 13.99kB                                                                                                       0.1s
        => CACHED [build 2/7] WORKDIR /helidon                                                                                                    0.0s
        => [build 3/7] ADD pom.xml .                                                                                                              0.1s
        => [build 4/7] RUN mvn package -Dmaven.test.skip -Declipselink.weave.skip                                                                91.7s
        => [stage-1 2/4] WORKDIR /helidon                                                                                                         0.8s
        => [build 5/7] ADD src src                                                                                                                0.1s
        => [build 6/7] RUN mvn package -DskipTests                                                                                               10.8s
        => [build 7/7] RUN echo "done!"                                                                                                           0.5s
        => [stage-1 3/4] COPY --from=build /helidon/target/quickstart-mp-ankit.jar ./                                                                   0.1s
        => [stage-1 4/4] COPY --from=build /helidon/target/libs ./libs                                                                            0.1s
        => exporting to image                                                                                                                     0.1s
        => => exporting layers                                                                                                                    0.1s
        => => writing image sha256:587a079ad854fc79e768acda11fc05dd87d37013261249e778e80749798c2837                                               0.0s
        => => naming to lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0                                                                           0.0s
        
4.  これにより、ローカル・リポジトリをチェックインできるDockerイメージが作成されます。
    
        $ docker images
        
        REPOSITORY                                                                           TAG                               IMAGE ID       CREATED         SIZE
        lhr.ocir.io/tenancynamespace/quickstart-mp-ankit                                               1.0                               587a079ad854   5 minutes ago   243MB
        
    
    後で必要になるため、置換された完全なイメージ名`ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-ankit:1.0`をテキスト・ファイルにコピーします。
    

## タスク2: Oracle Cloud Container Registryにログインするための認証トークンの生成

このステップでは、Oracle Cloud Container Registryへのログインに使用する_認証トークン_を生成します。

1.  右上隅にある「ユーザー」アイコンを選択し、_「マイ・プロファイル」_を選択します。
    
    ![自分のプロファイル](images/my-profile.png)
    
2.  下にスクロールして_「認証トークン」_を選択します。
    
    ![認証トークン](images/auth-token.png)
    
3.  _「トークンの生成」_をクリックします。
    
    ![トークンの生成](images/generate-token.png)
    
4.  _`quickstart-mp-your_first_name`_をコピーして「説明」ボックスに貼り付け、_「トークンの生成」_をクリックします。
    
    ![トークンの作成](images/create-token.png)
    
5.  「生成済トークン」の下の_「コピー」_をクリックし、テキスト・エディタに貼り付けます。後でコピーできないため、このトークンのコピーが保存されていることを確認してください。次に、_「閉じる」_をクリックします。
    
    ![トークンのコピー](images/copy-token.png)
    

## タスク3: コンテナ・レジストリ・リポジトリへのHelidonアプリケーション(quickstart-mp) Dockerイメージのプッシュ

1.  この演習のタスク1で、URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)を開いて、リージョン名のエンドポイントを特定し、テキスト・エディタにコピーしました。この例では、リージョン名はUK South (London)です。このタスクには、この情報が必要です。 ![エンド・ポイント](images/end-point.png)
    
2.  次のコマンドをコピーしてテキスト・エディタに貼り付け、`ENDPOINT_OF_REGION_NAME`をリージョンのエンドポイントに置き換えます。
    
    > この例では、リージョン名は_英国南部(ロンドン)_で、エンドポイントは_lhr.ocir.io_です。このタスクに固有の情報が必要になります。
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  前のステップでは、テナンシ・ネームスペースも決定しました。「ユーザー名」に`NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`と入力します。  
    
    *   _`NAMESPACE_OF_YOUR_TENANCY`_をテナンシの名前空間に置き換えます
    *   _`YOUR_ORACLE_CLOUD_USERNAME`_をOracle Cloudアカウントのユーザー名に置き換え、置換したユーザー名をテキスト・エディタからコピーして、_クラウド・シェル_に貼り付けます。
    *   パスワードの場合は、テキスト・エディタ(または保存した場所)から認証トークンをコピーして貼り付けます。
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  コンテナ・レジストリに戻ります。コンソールで、ナビゲーション・メニューを開き、**「開発者サービス」**をクリックします。**「コンテナとアーティファクト」**で、**「コンテナ・レジストリ」**をクリックします。 ![コンテナ・レジストリ](images/container-registry.png)
    
5.  コンパートメントを選択し、**「作成」**をクリックします。 ![リポジトリの作成](images/repository-create.png)
    
6.  コンパートメントを選択し、「リポジトリ名」として_`quickstart-mp-your_first_name`_と入力してから、「アクセス」に**「パブリック」**を選択し、**「リポジトリの作成」**をクリックします。
    
    ![リポジトリの説明](images/describe-repository.png)
    
7.  リポジトリ_`quickstart-mp-your_first_name`_の作成後、リポジトリ・リストとその設定で確認できます。
    
    ![ネームスペースの検証](images/verify-namespace.png)
    
8.  DockerイメージをOracle Cloud Container Registry内のリポジトリにプッシュするには、テキスト・エディタで次のコマンドをコピーして貼り付け、`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`quickstar-mp-your_first_name`:1.0を、以前に保存したDockerイメージのフルネームに置き換えます。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0</copy>
        
    
    結果は次のようになります。
    
        $ docker push lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        The push refers to a repository [lhr.ocir.io/tenancynamespace/quickstart-mp-ankit]
        0795b8384c47: Pushed
        131452972f9d: Pushed
        93c53f2e9519: Pushed
        3b78b65a4be9: Pushed
        e1434e7d0308: Pushed
        17679d5f39bd: Pushed
        300b011056d9: Pushed
        1.0: digest: sha256:355fa56eab185535a58c5038186381b6d02fd8e0bcb534872107fc249f98256a size: 1786
        
9.  _docker push_コマンドが正常に実行された後、_`quickstart-mp-your_first_name`_リポジトリを展開すると、新しいイメージがこのリポジトリにアップロードされていることがわかります。
    
10.  _クラウド・シェル_およびコンテナ・レジストリのリポジトリ・ページは開いたままにしておきます。次の演習で必要になります。
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月