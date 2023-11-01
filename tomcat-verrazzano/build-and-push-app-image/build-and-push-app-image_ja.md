# Tomcatアプリケーション・イメージをOracle Cloud Container Registryにプッシュ

## 概要

この演習では、tomcatアプリケーションでDockerイメージを構築し、そのイメージをOracle Cloud Container Registry内のリポジトリにプッシュします。

見積時間: 10分

### 目的

*   Dockerを使用してアプリケーションを構築およびパッケージ化します。
*   Oracle Cloud Container Registryにログインするための認証トークンを生成します。
*   TomcatアプリケーションDockerイメージをOracle Cloud Container Registryリポジトリにプッシュします。

### 事前設定

*   Docker
*   Oracle Cloudアカウント

## タスク1: アプリケーションのソース・コードのダウンロード

1.  次のコマンドをコピーして貼り付け、このワークショップのソース・コードをダウンロードします。
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/5DhoxZ22MseWaoSjG3EoBRf6DF5JbQ1QyR7tHa2qem-7IHb7nNEymF1ZSm2eA1ix/n/lrv4zdykjqrj/b/ankit-bucket/o/tomcat-examples.zip >~/tomcat-examples.zip</copy>
        
2.  次のコマンドをコピーして貼り付け、ソース・コードを解凍し、現在のディレクトリをアプリケーション・フォルダに変更します。
    
        <copy>unzip ~/tomcat-examples.zip
        cd ~/tomcat-examples/</copy>
        

## タスク2: Tomcat Application Dockerイメージの構築

まず、Verrazzanoにデプロイするために使用するDockerイメージを準備します。

OCIアカウントに属するOracle Cloud Container RegistryにアップロードするDockerイメージを作成しています。そのためには、レジストリの座標を反映するイメージ名を作成する必要があります。

必要な情報は次のとおりです。

*   テナント・ネームスペース
    
*   リージョンのエンドポイント
    
    > この情報をテキスト・ファイルにコピーして、演習全体を通して参照できるようにします。
    

1.  テナンシのネームスペースを検索するには、次に示すように_「ユーザー」_アイコン-> _「テナンシ」_をクリックします。**オブジェクト・ストレージ設定**には、ネームスペースがあります。後で使用するため、コピーしてテキスト・ファイルに保存します。
    
    ![テナンシネームスペースのコピー](images/copy-tenancynamespace.png " ")
    
2.  URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)を開き、リージョン名のエンドポイントを特定してテキスト・ファイルにコピーします。この例では、リージョン名はUK South (London)です。このタスクには、この情報が必要です。 ![エンド・ポイント](images/end-point.png)
    
3.  次のコマンドをコピーして、テキスト・エディタに貼り付けます。次に、_`ENDPOINT_OF_YOUR_REGION`_をリージョン名のエンドポイント、_`NAMESPACE_OF_YOUR_TENANCY`_をテナンシの名前空間、_`your_first_name`_を自分の名に置き換えます。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-your_first_name:v1 .</copy>
        
    
    コマンドの準備が整ったら、クラウド・シェルで`~/tomcat-examples/`ディレクトリから実行します。ビルドによって次の結果が生成されます。
    
        $ cd ~/tomcat-examples/
        $ $ docker build -t lhr.ocir.io/tenancy-namespace/tomcat-example-ankit:v1 .
        Sending build context to Docker daemon  1.097MB
        Step 1/10 : FROM tomcat:8.0-alpine
        Trying to pull repository docker.io/library/tomcat ... 
        8.0-alpine: Pulling from docker.io/library/tomcat
        4fe2ade4980c: Pull complete 
        6fc58a8d4ae4: Pull complete 
        7d9bd64c803b: Pull complete 
        a22aedc5ac11: Pull complete 
        5bde63ae3587: Pull complete 
        69cb0c9b940a: Pull complete 
        Digest: sha256:d02a16c0147fcae13d812fa670a4b3c9944f5328b10a5a463ad697d2aa5bb063
        Status: Downloaded newer image for tomcat:8.0-alpine
        ---> 624fb61775c3
        Step 2/10 : LABEL maintainer="ankit.x.pandey@oracle.com"
        ---> Running in 20cc23726499
        Removing intermediate container 20cc23726499
        ---> 50245c696fb6
        Step 3/10 : ADD sample-webapp.war /usr/local/tomcat/webapps/
        ---> 727c55f91bb5
        Step 4/10 : RUN mkdir /data
        ---> Running in f3129a859e11
        Removing intermediate container f3129a859e11
        ---> 9ce0f5674f51
        Step 5/10 : ADD jmx_prometheus_javaagent-0.17.0.jar /data/jmx_prometheus_javaagent-0.17.0.jar
        ---> f03cc9ee1bee
        Step 6/10 : ADD prometheus-jmx-config.yaml /data/prometheus-jmx-config.yaml
        ---> 50c51ae6a148
        Step 7/10 : ENV JAVA_OPTS="-javaagent:/data/jmx_prometheus_javaagent-0.17.0.jar=8088:/data/prometheus-jmx-config.yaml"
        ---> Running in 5e9effd5d494
        Removing intermediate container 5e9effd5d494
        ---> 85ca06fcd965
        Step 8/10 : EXPOSE 8088
        ---> Running in 795325f82526
        Removing intermediate container 795325f82526
        ---> 19dfc6fd903c
        Step 9/10 : EXPOSE 8080
        ---> Running in 43be96f20275
        Removing intermediate container 43be96f20275
        ---> 7d9bcaa7a271
        Step 10/10 : CMD ["catalina.sh", "run"]
        ---> Running in 3e25cd78ab88
        Removing intermediate container 3e25cd78ab88
        ---> 516065fe1bf5
        Successfully built 516065fe1bf5
        Successfully tagged lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        $
        
4.  これにより、ローカル・リポジトリをチェックインできるDockerイメージが作成されます。
    
        $ docker images
        REPOSITORY                           TAG IMAGE ID      CREATED       SIZE
        lhr.ocir.io/name/tomcat-example-ankit v1 516065fe1bf5 2 minutes ago  147MB
        tomcat                       8.0-alpine  624fb61775c3 4 years ago    147MB
        
    
    後で必要になるため、置換された完全なイメージ名`ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1`をテキスト・エディタにコピーします。
    

## タスク3: Oracle Cloud Container Registryにログインするための認証トークンの生成

このステップでは、Oracle Cloud Container Registryへのログインに使用する_認証トークン_を生成します。

1.  右上隅にある「ユーザー」アイコンを選択し、_「マイ・プロファイル」_を選択します。
    
    ![自分のプロファイル](images/my-profile.png " ")
    
2.  下にスクロールして_「認証トークン」_を選択します。
    
    ![認証トークン](images/auth-token.png " ")
    
3.  _「トークンの生成」_をクリックします。
    
    ![トークンの生成](images/generate-token.png " ")
    
4.  _`tomcat-example-your_first_name`_をコピーして_「説明」_ボックスに貼り付け、_「トークンの生成」_をクリックします。
    
    ![トークン作成](images/token-create.png " ")
    
5.  「生成済トークン」の下の_「コピー」_を選択し、テキスト・エディタに貼り付けます。後でコピーできません。次に、_「閉じる」_をクリックします。
    
    ![トークンのコピー](images/copy-token.png " ")
    

## タスク4: コンテナ・レジストリ・リポジトリへのtomcatアプリケーションDockerイメージのプッシュ

1.  この演習のタスク2で、URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)を開き、リージョン名のエンドポイントを特定してテキスト・ファイルにコピーしました。この例では、リージョン名はUK South (London)です。このタスクには、この情報が必要です。 ![エンドポイント](images/end-point.png)
    
2.  次のコマンドをコピーしてテキスト・エディタに貼り付け、`ENDPOINT_OF_REGION_NAME`をリージョンのエンドポイントに置き換えます。
    
    > この例では、リージョン名は_英国南部(ロンドン)_で、エンドポイントは_lhr.ocir.io_です。このタスクに固有の情報が必要になります。
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  前のステップでは、テナンシ・ネームスペースも決定しました。「ユーザー名」に`NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`と入力します。  
    
    *   `NAMESPACE_OF_YOUR_TENANCY`をテナンシの名前空間に置き換えます
    *   `YOUR_ORACLE_CLOUD_USERNAME`をOracle Cloudアカウントのユーザー名に置き換え、置換したユーザー名をテキスト・エディタからコピーして_クラウド・シェル_に貼り付けます。
    *   パスワードの場合は、テキスト・エディタ(または保存した場所)から認証トークンをコピーして貼り付けます。
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  コンテナ・レジストリに戻ります。コンソールで、ナビゲーション・メニューを開き、**「開発者サービス」**をクリックします。**「コンテナとアーティファクト」**で、**「コンテナ・レジストリ」**をクリックします。 ![コンテナ・レジストリ](images/container-registry.png)
    
5.  コンパートメントを選択し、**「リポジトリの作成」**をクリックします。 ![リポジトリの作成](images/repository-create.png)
    
6.  コンパートメントを選択し、「リポジトリ名」として_`tomcat-example-your_first_name`_と入力し、**「パブリック」**として「アクセス」を選択して**「作成」**をクリックします。
    
    ![リポジトリの説明](images/describe-repository.png)
    
7.  DockerイメージをOracle Cloud Container Registry内のリポジトリにプッシュするには、テキスト・エディタで次のコマンドをコピーして貼り付け、`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`tomcat-example-your_first_name`:1.0を、以前に保存したDockerイメージのフルネームに置き換えます。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1</copy>
        
    
    結果は次のようになります。
    
        $ docker push lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        The push refers to repository [lhr.ocir.io/tenancynamespace/tomcat-example-ankit]
        4b193f4c616d: Pushed 
        0469528628db: Pushed 
        cce8193c4190: Pushed 
        ca36c0db4673: Pushed 
        0136a6a85859: Pushed 
        98a0db77a14c: Pushed 
        9072514c7af0: Pushed 
        f6146a44a7d3: Pushed 
        0c3170905795: Pushed 
        df64d3292fd6: Pushed 
        v1: digest: sha256:65b562a7117870540f1807e0d796fe964e6428bda0ae290b8a6389bf637d1aba size: 2405
        $ 
        
8.  _docker push_コマンドが正常に実行された後、_`tomcat-example-ankit:v1`_リポジトリを展開すると、新しいイメージがこのリポジトリにアップロードされていることがわかります。
    

**次の演習に進む**ことができます。

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月