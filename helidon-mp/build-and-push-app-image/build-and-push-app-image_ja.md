# Oracle Cloud Container RegistryへのHelidonアプリケーション・イメージの構築およびプッシュ

## 概要

このラボでは、Helidonアプリケーションで_ネイティブ_Dockerイメージを構築し、そのイメージをOracle Cloud Container Registry内のリポジトリにプッシュします。

見積時間: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[Oracle Cloud Container RegistryへのHelidonアプリケーション・イメージの構築およびプッシュ](videohub:1_mh1brw5t)

### 目的

*   Dockerを使用してアプリケーションを構築およびパッケージ化します。
*   Oracle Cloud Container Registryにログインするための認証トークンを生成します。
*   HelidonアプリケーションDockerイメージをOracle Cloud Container Registryリポジトリにプッシュします。

### 事前設定

*   前の演習で作成したHelidonアプリケーション
*   Docker
*   Oracle Cloudアカウント

## タスク1: HelidonアプリケーションDockerイメージの構築

OCIアカウントに属するOracle Cloud Container RegistryにアップロードするDockerイメージを作成しています。そのためには、レジストリの座標を反映するイメージ名を作成する必要があります。

必要な情報は次のとおりです。

*   リージョン名
*   テナント・ネームスペース
*   リージョンのエンドポイント
    
    > この情報をテキスト・ファイルにコピーして、演習全体を通して参照できるようにします。
    

1.  _「リージョン名」_を見つけます。  
    _リージョン名_は、Oracle Cloudコンソールの右上隅にあります。この例では、_英国南部(ロンドン)_と表示されています。あなたのことは違うかもしれません。
    
    ![コンテナ・レジストリ](images/region-name.png)
    
2.  _テナンシ・ネームスペース_を見つけます。  
    コンソールで、ナビゲーション・メニューを開き、**「開発者サービス」**をクリックします。**「コンテナとアーティファクト」**で、**「コンテナ・レジストリ」**をクリックします。
    
    ![テナント・ネームスペース](images/container-registry.png)
    
    > テナンシ・ネームスペースがコンパートメントにリストされます。コピーしてテキスト・ファイルに保存します。この情報は、次の演習でも使用します。 ![テナント・ネームスペース](images/name-space.png)
    
3.  _「Endpoint for Your Region」_を見つけます。  
    このURL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)に記載されている表を参照してください。この例では、リージョンのエンドポイントは_UK South (London)_ (リージョン名)で、そのエンドポイントは_lhr.ocir.io_です。自分の_リージョン名_のエンドポイントを見つけ、テキスト・ファイルに保存します。次の研究室にも必要です。
    
    ![エンド・ポイント](images/end-points.png)
    
    > これで、リージョンのテナンシ・ネームスペースとエンドポイントの両方が用意されました。
    
4.  次のコマンドをコピーして、テキスト・ファイルに貼り付けます。次に、_`ENDPOINT_OF_YOUR_REGION`_をリージョン名のエンドポイント、_`NAMESPACE_OF_YOUR_TENANCY`_をテナンシの名前空間、_`your_first_name`_を自分の名に置き換えます。
    
    > これは、Dockerコンテナ内で完全なビルドを実行します。最初に実行すると、すべてのMaven依存関係をダウンロードしてDockerレイヤーにキャッシュするため、時間がかかります。pom.xmlファイルを変更しないかぎり、後続のビルドははるかに高速になります。ポムが変更されると、依存関係が再ダウンロードされます。
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0 -f Dockerfile.native .</copy>
        
    
    コマンドの準備が整ったら、_`~/helidon-project/myproject/myproject`_ディレクトリのコード・エディタ内のターミナルで実行します。ビルドによって、最後に次の結果が生成されます。
    
        $ docker build -t lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 -f Dockerfile.native .
        
        [1/7] Initializing...                                                                                   (15.7s @ 0.14GB)
        Version info: 'GraalVM 22.3.0 Java 17 CE'
        Java version info: '17.0.5+8-jvmci-22.3-b08'
        C compiler: gcc (redhat, x86_64, 11.3.1)
        Garbage collector: Serial GC
        2 user-specific feature(s)
        - io.helidon.integrations.graal.mp.nativeimage.extension.WeldFeature
        - io.helidon.integrations.graal.nativeimage.extension.HelidonReflectionFeature
        2023.04.06 05:41:01 INFO io.helidon.common.LogConfig Thread[main,5,main]: Logging at initialization configured using classpath: /logging.properties
        [2/7] Performing analysis...  [**********]                                                             (202.8s @ 1.92GB)
        18,812 (92.77%) of 20,278 classes reachable
        27,564 (63.52%) of 43,392 fields reachable
        87,900 (62.22%) of 141,268 methods reachable
        1,068 classes,   565 fields, and 6,864 methods registered for reflection
            65 classes,    70 fields, and    58 methods registered for JNI access
            6 native libraries: dl, m, pthread, rt, stdc++, z
        [3/7] Building universe...                                                                              (27.6s @ 3.15GB)
        [4/7] Parsing methods...      [*****]                                                                   (22.5s @ 3.00GB)
        [5/7] Inlining methods...     [***]                                                                     (11.9s @ 1.84GB)
        [6/7] Compiling methods...    [************]                                                           (156.5s @ 3.05GB)
        [7/7] Creating image...                                                                                 (15.6s @ 2.44GB)
        35.03MB (45.80%) for code area:    57,947 compilation units
        39.02MB (51.01%) for image heap:  477,987 objects and 128 resources
        2.44MB ( 3.19%) for other data
        76.49MB in total
        ------------------------------------------------------------------------------------------------------------------------
        Top 10 packages in code area:                               Top 10 object types in image heap:
        1.63MB sun.security.ssl                                     7.71MB byte[] for code metadata
        1.20MB com.sun.media.sound                                  4.60MB java.lang.Class
        1.17MB java.util                                            3.93MB java.lang.String
        822.87KB java.lang.invoke                                     3.41MB byte[] for java.lang.String
        717.54KB com.sun.crypto.provider                              3.22MB byte[] for general heap data
        517.57KB io.helidon.config                                    1.58MB com.oracle.svm.core.hub.DynamicHubCompanion
        510.02KB java.util.concurrent                                 1.13MB byte[] for reflection metadata
        481.49KB jdk.proxy4                                           1.03MB byte[] for embedded resources
        474.98KB java.lang                                          915.61KB java.util.HashMap$Node
        468.42KB com.sun.org.apache.xerces.internal.impl            781.21KB java.lang.String[]
        26.70MB for 671 more packages                                9.83MB for 4584 more object types
        ------------------------------------------------------------------------------------------------------------------------
                                31.0s (6.6% of total time) in 59 GCs | Peak RSS: 4.80GB | CPU load: 1.60
        ------------------------------------------------------------------------------------------------------------------------
        Produced artifacts:
        /helidon/target/myproject (executable)
        /helidon/target/myproject.build_artifacts.txt (txt)
        ========================================================================================================================
        Finished generating 'myproject' in 7m 48s.
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  08:04 min
        [INFO] Finished at: 2023-04-06T05:48:33Z
        [INFO] ------------------------------------------------------------------------
        Removing intermediate container e400c5c6897b
        ---> 20099e4619d6
        Step 10/15 : RUN echo "done!"
        ---> Running in a8eddd448e48
        done!
        Removing intermediate container a8eddd448e48
        ---> ebfd3064dc68
        Step 11/15 : FROM scratch
        ---> 
        Step 12/15 : WORKDIR /helidon
        ---> Running in 46be56a98462
        Removing intermediate container 46be56a98462
        ---> eaf15b746a1c
        Step 13/15 : COPY --from=build /helidon/target/myproject .
        ---> a69ac5933048
        Step 14/15 : ENTRYPOINT ["./myproject"]
        ---> Running in 71633a601e7f
        Removing intermediate container 71633a601e7f
        ---> cd9f22bfa4b3
        Step 15/15 : EXPOSE 8080
        ---> Running in 4b9763eb49fa
        Removing intermediate container 4b9763eb49fa
        ---> aa8b6e7b04c0
        Successfully built aa8b6e7b04c0
        Successfully tagged lhr.ocir.io/lrv4zdykjqrj/myproject-ankit:1.0
        
5.  これにより、ローカル・リポジトリをチェックインできるDockerイメージが作成されます。
    
        $ docker images
        REPOSITORY                     TAG IMAGE ID        CREATED           SIZE
        lhr.ocir.io/tn/myproject-ankit 1.0 aa8b6e7b04c0 About a minute ago   80.2MB
        
    
    後で必要になるため、置換された完全なイメージ名`ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0`をテキスト・エディタにコピーします。
    
6.  コード・エディタのクラウド・シェルでdockerイメージを実行するには、ターミナルで次のコマンドをコピーして貼り付けます。
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    ![ドッカーラン](images/docker-run.png)
    
7.  新しい端末/コンソールを開き、次のコマンドを実行してアプリケーションを確認します。
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        

## タスク2: Oracle Cloud Container Registryにログインするための認証トークンの生成

このステップでは、Oracle Cloud Container Registryへのログインに使用する_認証トークン_を生成します。

1.  右上隅にある「ユーザー」アイコンを選択し、_「マイ・プロファイル」_を選択します。
    
    ![自分のプロファイル](images/my-profile.png " ")
    
2.  下にスクロールして_「認証トークン」_を選択します。
    
    ![認証トークン](images/auth-token.png " ")
    
3.  _「トークンの生成」_をクリックします。
    
    ![トークンの生成](images/generate-token.png " ")
    
4.  _`myproject-your_first_name`_をコピーして_「説明」_ボックスに貼り付け、_「トークンの生成」_をクリックします。
    
    ![トークン作成](images/token-create.png " ")
    
5.  「生成済トークン」の下の_「コピー」_を選択し、テキスト・エディタに貼り付けます。後でコピーできません。次に、_「閉じる」_をクリックします。
    
    ![トークンのコピー](images/copy-token.png " ")
    

## タスク3: コンテナ・レジストリ・リポジトリへのHelidonアプリケーション(myproject) Dockerイメージのプッシュ

1.  この演習のタスク1で、URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)を開き、リージョン名のエンドポイントを特定してテキスト・ファイルにコピーしました。この例では、リージョン名はUK South (London)です。このタスクには、この情報が必要です。 ![エンドポイント](images/end-points.png)
    
2.  次のコマンドをコピーしてテキスト・ファイルに貼り付け、`ENDPOINT_OF_REGION_NAME`をリージョンのエンドポイントに置き換えます。
    
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
    
5.  コンパートメントを選択し、**「リポジトリの作成」**をクリックします。 ![リポジトリの作成](images/repository-create.png)
    
6.  コンパートメントを選択し、「リポジトリ名」として_`myproject-your_first_name`_と入力してから、「アクセス」に**「パブリック」**を選択し、**「リポジトリの作成」**をクリックします。
    
    ![リポジトリの説明](images/describe-repository.png)
    
7.  リポジトリ_`myproject-your_first_name`_の作成後、リポジトリ・リストとその設定で確認できます。
    
    ![ネームスペースの検証](images/verify-namespace.png)
    
8.  DockerイメージをOracle Cloud Container Registry内のリポジトリにプッシュするには、テキスト・ファイルに次のコマンドをコピーして貼り付け、`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/myproject-your\_first\_name:1.0を、以前に保存したDockerイメージのフルネームで置き換えます。
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    結果は次のようになります。
    
        $ docker push lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 The push refers to repository [lhr.ocir.io/tenancynamespace/myproject-ankit]
        0bf9e88b7284: Pushed 
        0acc08535a89: Pushed 
        1.0: digest: sha256:3e8cc0ff23d256499dff8d907150b639925687aed0c41008cbe1794bc02433e2 size: 735
        $
        
9.  _docker push_コマンドが正常に実行された後、_`myproject-your_first_name`_リポジトリを展開すると、新しいイメージがこのリポジトリにアップロードされていることがわかります。
    
    ![イメージがアップロードされました](images/verify-push.png)
    

## 確認

*   **作成者** - Dmitry Aleksandrov
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年4月