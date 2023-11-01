# Oracle Cloud Container RegistryへのSpringbootアプリケーション・イメージのプッシュ

## 概要

この演習では、SpringbootアプリケーションでDockerイメージを構築し、そのイメージをOracle Cloud Container Registry内のリポジトリにプッシュします。

見積時間: 10分

### 目的

*   Dockerを使用してアプリケーションを構築およびパッケージ化します。
*   Oracle Cloud Container Registryにログインするための認証トークンを生成します。
*   SpringbootアプリケーションのDockerイメージをOracle Cloud Container Registryリポジトリにプッシュします。

### 事前設定

*   Docker
*   Oracle Cloudアカウント

## タスク1: アプリケーション・ソース・コードおよび必要なJDKのダウンロード

1.  次のコマンドをコピーして貼り付け、このワークショップのソース・コードをダウンロードします。
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/EeNNvCVLObhXjHIwu5M-J_zbSXJsbLop6wcsFFnDfneY3zqbAFfdKikZe-Q0GT3I/n/lrv4zdykjqrj/b/ankit-bucket/o/springboot-app.zip >~/springboot-app.zip</copy>
        
2.  次のコマンドをコピーして貼り付け、ソース・コードを解凍し、現在のディレクトリをアプリケーション・フォルダに変更します。
    
        <copy>unzip ~/springboot-app.zip
        cd ~/springboot-app/</copy>
        
3.  SpringbootアプリケーションのDockerイメージを作成しますが、このアプリケーションでは特定のバージョンのJDKが使用されるため、新しいイメージを構築するDockerファイルは変更しません。そのため、必要なJDKをダウンロードします。必要なJDKバージョンをダウンロードするには、次のコマンドをコピーしてクラウド・シェルに貼り付けます。
    
        <copy>wget https://download.java.net/java/ga/jdk11/openjdk-11_linux-x64_bin.tar.gz</copy>
        
4.  次のコマンドをコピーして貼り付け、Springbootアプリケーションをビルドします。
    
        <copy>mvn clean package</copy>
        

## タスク2: SpringbootアプリケーションのDockerイメージの構築

まず、Verrazzanoにデプロイするために使用するDockerイメージを準備します。

OCIアカウントに属するOracle Cloud Container RegistryにアップロードするDockerイメージを作成しています。そのためには、レジストリの座標を反映するイメージ名を作成する必要があります。

必要な情報は次のとおりです。

*   テナント・ネームスペース
    
*   リージョンのエンドポイント
    
    > この情報をテキスト・ファイルにコピーして、演習全体を通して参照できるようにします。
    

1.  テナンシのネームスペースを検索するには、次に示すように_「ユーザー」_アイコン-> _「テナンシ」_をクリックします。**オブジェクト・ストレージ設定**には、ネームスペースがあります。後で使用するため、コピーしてテキスト・ファイルに保存します。
    
    ![テナンシネームスペースのコピー](images/copy-tenancynamespace.png " ")
    
2.  _「Endpoint for Your Region」_を見つけます。  
    このURL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)に記載されている表を参照してください。この例では、リージョンのエンドポイントは_UK South (London)_ (リージョン名)で、そのエンドポイントは_lhr.ocir.io_です。自分の_リージョン名_のエンドポイントを見つけ、テキスト・ファイルに保存します。
    
    ![エンド・ポイント](images/end-points.png)
    
    > これで、リージョンのテナンシ・ネームスペースとエンドポイントの両方が用意されました。
    
3.  次のコマンドをコピーして、テキスト・ファイルに貼り付けます。次に、_`ENDPOINT_OF_YOUR_REGION`_をリージョン名のエンドポイント、_`NAMESPACE_OF_YOUR_TENANCY`_をテナンシの名前空間、_`your_first_name`_を自分の名に置き換えます。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-your_first_name:v1 .</copy>
        
    
    コマンドの準備が整ったら、クラウド・シェルで`~/springboot-app/`ディレクトリから実行します。ビルドによって次の結果が生成されます。
    
        $ cd ~/springboot-app/
        $ docker build -t lhr.ocir.io/tenancy-namespace/springboot-ankit:v1 .
        Sending build context to Docker daemon  206.7MB
        Step 1/14 : FROM ghcr.io/oracle/oraclelinux:7-slim AS build_base
        Trying to pull repository ghcr.io/oracle/oraclelinux ... 
        7-slim: Pulling from ghcr.io/oracle/oraclelinux
        6cb086706000: Pull complete 
        Digest: sha256:4353fdc8664c386c0a443eb40b10a7662b4eb8d6eb5d6dcefe218e9783132c71
        Status: Downloaded newer image for ghcr.io/oracle/oraclelinux:7-slim
        ---> 1d56b1a0fd84
        Step 2/14 : RUN yum update -y && yum-config-manager --save --setopt=ol7_ociyum_config.skip_if_unavailable=true     && yum install -y tar unzip gzip     && yum clean all; rm -rf /var/cache/yum     && mkdir -p /license
        ---> Running in 92c013a8e84f
        Loaded plugins: ovl
        No packages marked for update
        Loaded plugins: ovl
        ===================================== main =====================================
        [main]
        alwaysprompt = True
        assumeno = False
        assumeyes = False
        autocheck_running_kernel = True
        autosavets = True
        bandwidth = 0
        bugtracker_url = https://linux.oracle.com
        cache = 0
        cachedir = /var/cache/yum/x86_64/7Server
        check_config_file_age = True
        clean_requirements_on_remove = False
        ----------------------------------------------------------------------
        Step 3/14 : ENV JAVA_HOME=/usr/java
        ---> Running in 96b99fba7e50
        Removing intermediate container 96b99fba7e50
        ---> 1cc6c7a63b89
        Step 4/14 : ENV PATH $JAVA_HOME/bin:$PATH
        ---> Running in 4a88eb052547
        Removing intermediate container 4a88eb052547
        ---> 48e7fa9b7b0c
        Step 5/14 : ARG JDK_BINARY="${JDK_BINARY:-openjdk-11_linux-x64_bin.tar.gz}"
        ---> Running in e922e8b35bfd
        Removing intermediate container e922e8b35bfd
        ---> 6888b690a4b0
        Step 6/14 : COPY ${JDK_BINARY} jdk.tar.gz
        ---> e9c5ffd0f2a5
        Step 7/14 : ENV JDK_DOWNLOAD_SHA256=3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e
        ---> Running in a7a89b057f25
        Removing intermediate container a7a89b057f25
        ---> 12ecfaa002bf
        Step 8/14 : RUN set -eux     echo "Checking JDK hash";     echo "${JDK_DOWNLOAD_SHA256} *jdk.tar.gz" | sha256sum --check -;     echo "Installing JDK";     mkdir -p "$JAVA_HOME";     tar xzf jdk.tar.gz --directory "${JAVA_HOME}" --strip-components=1;     rm -f jdk.tar.gz;
        ---> Running in a6a590adeee6
        + echo '3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e *jdk.tar.gz'
        + sha256sum --check -
        jdk.tar.gz: OK
        + echo 'Installing JDK'
        + mkdir -p /usr/java
        Installing JDK
        + tar xzf jdk.tar.gz --directory /usr/java --strip-components=1
        + rm -f jdk.tar.gz
        Removing intermediate container a6a590adeee6
        ---> 1f37a6cb044d
        Step 9/14 : COPY LICENSE.txt /license/
        ---> dc69f24f5be6
        Step 10/14 : COPY THIRD_PARTY_LICENSES.txt /license/
        ---> 5ef683dbda22
        Step 11/14 : ARG JAR_FILE=target/*.jar
        ---> Running in 3b80032f8310
        Removing intermediate container 3b80032f8310
        ---> 8eca16289bd7
        Step 12/14 : COPY ${JAR_FILE} app.jar
        ---> dcb7e3ed0871
        Step 13/14 : ENTRYPOINT ["java","-jar","/app.jar"]
        ---> Running in 2191623bf524
        Removing intermediate container 2191623bf524
        ---> 11e59e19cfb4
        Step 14/14 : USER 1000
        ---> Running in 16446779b92b
        Removing intermediate container 16446779b92b
        ---> a701fa912f2e
        Successfully built a701fa912f2e
        Successfully tagged lhr.ocir.io/tenancynamespace/springboot-ankit:v1
        $
        
4.  これにより、ローカル・リポジトリをチェックインできるDockerイメージが作成されます。
    
        $ docker images
        REPOSITORY                            TAG     IMAGE ID    CREATED      SIZE
        lhr.ocir.io/namespace/springboot-ankit v1    a701fa912f2 3 minutes ago 664MB
        ghcr.io/oracle/oraclelinux            7-slim 1d56b1a0fd8 3 weeks ago   138MB
        $ 
        
    
    後で必要になるため、置換された完全なイメージ名`ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1`をテキスト・ファイルにコピーします。
    

## タスク3: Oracle Cloud Container Registryにログインするための認証トークンの生成

このステップでは、Oracle Cloud Container Registryへのログインに使用する_認証トークン_を生成します。

1.  右上隅にある「ユーザー」アイコンを選択し、_「マイ・プロファイル」_を選択します。
    
    ![自分のプロファイル](images/my-profile.png " ")
    
2.  下にスクロールして_「認証トークン」_を選択します。
    
    ![認証トークン](images/auth-token.png " ")
    
3.  _「トークンの生成」_をクリックします。
    
    ![トークンの生成](images/generate-token.png " ")
    
4.  _`springboot-your_first_name`_をコピーして_「説明」_ボックスに貼り付け、_「トークンの生成」_をクリックします。
    
    ![トークン作成](images/token-create.png " ")
    
5.  「生成済トークン」の下の_「コピー」_を選択し、テキスト・ファイルに貼り付けます。後でコピーできません。次に、_「閉じる」_をクリックします。
    
    ![トークンのコピー](images/copy-token.png " ")
    

## タスク4: SpringbootアプリケーションDockerイメージのコンテナ・レジストリ・リポジトリへのプッシュ

1.  この演習のタスク1で、URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)を開き、リージョン名のエンドポイントを特定してテキスト・ファイルにコピーしました。この例では、リージョン名はUK South (London)です。このタスクには、この情報が必要です。 ![エンドポイント](images/end-points.png)
    
2.  次のコマンドをコピーしてテキスト・エディタに貼り付け、`ENDPOINT_OF_REGION_NAME`をリージョンのエンドポイントに置き換えます。
    
    > この例では、リージョン名は_英国南部(ロンドン)_で、エンドポイントは_lhr.ocir.io_です。このタスクに固有の情報が必要になります。
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  前のステップでは、テナンシ・ネームスペースも決定しました。「ユーザー名」に`NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`と入力します。  
    
    *   `NAMESPACE_OF_YOUR_TENANCY`をテナンシの名前空間に置き換えます
    *   `YOUR_ORACLE_CLOUD_USERNAME`をOracle Cloudアカウントのユーザー名に置き換え、置換したユーザー名をテキスト・ファイルからコピーして_クラウド・シェル_に貼り付けます。
    *   「パスワード」で、テキスト・ファイル(または保存した場所)から認証トークンをコピーして貼り付けます。
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  コンテナ・レジストリに戻ります。コンソールで、ナビゲーション・メニューを開き、**「開発者サービス」**をクリックします。**「コンテナとアーティファクト」**で、**「コンテナ・レジストリ」**をクリックします。 ![コンテナ・レジストリ](images/container-registry.png)
    
5.  コンパートメントを選択し、**「作成」**をクリックします。 ![リポジトリの作成](images/repository-create.png)
    
6.  コンパートメントを選択し、「リポジトリ名」として_`springboot-your_first_name`_と入力してから、「アクセス」に**「パブリック」**を選択し、**「リポジトリの作成」**をクリックします。
    
    ![リポジトリの説明](images/describe-repository.png)
    
7.  DockerイメージをOracle Cloud Container Registry内のリポジトリにプッシュするには、テキスト・ファイルに次のコマンドをコピーして貼り付け、_`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/springboot-your\_firstname:1.0_をDockerイメージのフルネームで置き換えます(この名前は以前に保存しました)。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1</copy>
        
    
    結果は次のようになります。
    
        $ docker push lhr.ocir.io/namespace/springboot-ankit:v1
        The push refers to repository [lhr.ocir.io/namespace/springboot-ankit]
        31118271414e: Pushed 
        e6144652ec48: Pushed 
        a5ac4d4576aa: Pushed 
        2f93ab3a0c42: Pushed 
        3ed60ad88e51: Pushed 
        f47db30f116a: Pushed 
        f50ba2e0b2f9: Pushed 
        v1: digest: sha256:96aacff31cb255ea815213aba837f16f40d73b14d67449d4744ed811c7a864c8 size: 1795
        $ 
        
8.  _docker push_コマンドが正常に実行された後、_`springboot-ankit:v1`_リポジトリを展開すると、新しいイメージがこのリポジトリにアップロードされていることがわかります。
    

**次の演習に進む**ことができます。

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月