# オブジェクト・ストレージ・サポートを追加するためのHelidonアプリケーションの変更

## 概要

この演習の目的は、Helidonアプリケーションから**オブジェクト・ストレージ・アクセスを追加する方法**をデモンストレーションすることです。これを行うには、グリーティング・ワードを格納するために使用される変数を、新しいグリーティング・ワード・コンテナになり、**オブジェクト・ストレージ・バケットから取得**されて格納されるオブジェクトに置き換えます。オブジェクトは永続化されるため、最後のグリーティング・ワード値はアプリケーションの再起動後も存続します。この変更がなく、変数を介したメモリー内の挨拶ワードを使用すると、アプリケーションの再起動時に挨拶ワードがデフォルト値にリセットされます。

見積時間: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[オブジェクト・ストレージのサポート](videohub:1_p5v2wehm)

### 目的

この演習では、次のことを行います。

*   オブジェクト・ストレージなどのOCIサービスとの統合を表示するようにHelidonアプリケーションを変更します
*   成功したオブジェクト・ストレージ統合の確認

### 前提条件

*   Oracle Free Tier(トライアル)、有料またはLiveLabsクラウド・アカウント

## タスク1: オブジェクト・ストレージ統合のためのHelidonアプリケーションの変更

1.  **oci.bucket.name**が**~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties**で正しく構成されていることを確認します。これは、演習2/タスク3のステップ5ですでに設定されている必要があります。
    
2.  **コード・エディタ**で、**~/OCI-mp/server/**の下のファイル名_`pom.xml`_をクリックして開き、次に示すように**オブジェクト・ストレージOCI SDK**依存関係を**dependencies**句内に追加します。
    
        <copy><dependency>
                    <groupId>com.oracle.oci.sdk</groupId>
                    <artifactId>oci-java-sdk-objectstorage</artifactId>
        </dependency></copy>
        
    
    ![依存性の追加](images/add-dependency.png)
    
    > インデントが適切であることを確認してください。
    
3.  ファイル_~/oci-mp/server/src/main/java/ocw/hol/mp/oci/server/GreetingProvider.java_を変更して、**オブジェクト・ストレージ**・アクセスを追加します。時間に関しては、この変更のソースコードがあります。次のコードをコピーして貼り付け、このファイルを必要な変更で更新してください。このファイルで行った変更について説明している次の_ノート_を参照してください。
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/source/GreetingProvider.java server/src/main/java/ocw/hol/mp/oci/server/</copy>
        
    
    ![オブジェクト・ストレージが追加されました](images/os-added.png)
    
    > **続きを読む:-**
    
    *   コンストラクタの引数セクションから、_ObjectStorage objectStorageClient_パラメータを追加しました。これは_@Injected_注釈の一部であるため、パラメータは自動的に処理され、Helidonによって設定され、その目的のために**OCI SDK**コードの複数の行を追加することなく、オブジェクト・ストレージ・サービスとの通信に使用できるクライアントが含まれます。
    *   同じコンストラクタの引数セクションから、構成内の**oci.bucket.name**プロパティから値を抽出する**ConfigProperty**が追加されました。これまでは、**`devops_helidon_to_instance_ocw_hol`**リポジトリ・ディレクトリから**`update_config_values.sh`**というユーティリティ・スクリプトが実行されたときに、初期アプリケーション設定時に**microprofile-config.properties**に移入されていました。
    *   **getNamespace() Object Storage SDK**メソッドを使用して、後でオブジェクトの取得または格納に使用されるオブジェクト・ストレージのネームスペースを取得します:
    
        <copy>public GreetingProvider(@ConfigProperty(name = "app.greeting") String message,
                            ObjectStorage objectStorageClient,
                            @ConfigProperty(name = "oci.bucket.name") String bucketName) {
        try {
            this.bucketName = bucketName;
            GetNamespaceResponse namespaceResponse =
                    objectStorageClient.getNamespace(GetNamespaceRequest.builder().build());
            this.objectStorageClient = objectStorageClient;
            this.namespaceName = namespaceResponse.getValue();
            LOGGER.info("Object storage namespace: " + namespaceName);
            setMessage(message);
        } catch (Exception e) {
            LOGGER.warning("Error invoking getNamespace from Object Storage: " + e);
        }
        }     </copy>
        
    
    *   GreetingProviderクラスのすぐ下で、変数宣言を削除しました。
    
        <copy>private final AtomicReference<String> message = new AtomicReference<>();</copy>
        
    
    *   次のようなローカル・グローバル変数に置き換えます。_LOGGER_はロギングに使用され、_オブジェクト・ストレージSDK_コールでは_objectStorageClient_、_namespaceName_、_bucketName_および_objectName_が使用されます。
    
        <copy>private static final Logger LOGGER = Logger.getLogger(GreetingProvider.class.getName());
        private ObjectStorage objectStorageClient;
        private String namespaceName;
        private String bucketName;
        private final String objectName = "hello.txt";</copy>
        
    
    *   _getMessage()_の内容を次のコードに置き換えます。これにより、**getObject() SDK**メソッドを使用して、バケットからフェッチされた**hello.txt**オブジェクトからグリーティング・ワードを取得します。
    
        <copy>try {
        GetObjectResponse getResponse =
                objectStorageClient.getObject(
                        GetObjectRequest.builder()
                                .namespaceName(namespaceName)
                                .bucketName(bucketName)
                                .objectName(objectName)
                                .build());
        return new String(getResponse.getInputStream().readAllBytes());
        } catch (Exception e) {
            LOGGER.warning("Error invoking getObject from Object Storage: " + e);
            return "Hello-Error";
        }</copy>
        
    
    *   void **setMessage(String message)**メソッドの内容を次のコードに置き換えます。これにより、**putObject() SDK**メソッドを使用して、挨拶文を含む**hello.txt**オブジェクトをバケットに格納します。
    
        <copy>try {
        byte[] contents = message.getBytes();
        PutObjectRequest putObjectRequest =
                PutObjectRequest.builder()
                        .namespaceName(namespaceName)
                        .bucketName(bucketName)
                        .objectName(objectName)
                        .putObjectBody(new ByteArrayInputStream(message.getBytes()))
                        .contentLength(Long.valueOf(contents.length))
                        .build();
        objectStorageClient.putObject(putObjectRequest);
        } catch (Exception e) {
            LOGGER.warning("Error invoking putObject from Object Storage: " + e);
        }</copy>
        
    
    *   インポート(**インポートjava.util.concurrent.atomic.AtomicReference;**)を削除し、追加されたすべての新しいコードをサポートするために、これらの新しいインポートに置き換えました。
    
        <copy>import java.io.ByteArrayInputStream;
        import java.util.logging.Logger;
        
        import com.oracle.bmc.objectstorage.ObjectStorage;
        import com.oracle.bmc.objectstorage.requests.GetNamespaceRequest;
        import com.oracle.bmc.objectstorage.requests.GetObjectRequest;
        import com.oracle.bmc.objectstorage.requests.PutObjectRequest;
        import com.oracle.bmc.objectstorage.responses.GetNamespaceResponse;
        import com.oracle.bmc.objectstorage.responses.GetObjectResponse;</copy>
        

## タスク2: Helidonアプリケーション・コード変更のプッシュとDevOpsパイプラインのトリガー

1.  次のコマンドをコピーして端末に貼り付け、**変更をコミットしてプッシュ**します。
    
        <copy>git add .
        git status
        git commit -m "Add support Object Storage integration"
        git push</copy>
        
    
    > パイプラインはこのgitプッシュによってトリガーされます。
    
2.  ビルドおよびデプロイメント・パイプライン・ログを監視して、**DevOpsライフサイクル**が完了するまで待ちます。 ![デプロイメント実行](images/deployment-run.png)
    

## タスク3: 成功したオブジェクト・ストレージ統合の確認

curlを使用してテストし、新しい**hello.txt**オブジェクトが**バケット**に追加されていることを確認します。オブジェクトのサイズがグリーティング・ワードのサイズと同じであることを確認します。たとえば、挨拶の単語が_Hello_の場合、サイズは**5**になります。グリーティングワードが _Hola_の場合、サイズは **4**になります。

1.  デプロイメント・ノード**`PUBLIC_IP`**を環境変数として設定します。
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  次のコマンドをコピーして貼り付け、**GET**リクエストを配置します。次に示すような出力が表示されます。
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  **クラウド・コンソール**で、次に示すように、_「ハンバーガー・メニュー」_→_「ストレージ」_→_「バケット」_をクリックします。 ![バケット・メニュー](images/bucket-menu.png)
    
4.  正しいコンパートメントを選択し、**app-bucket-helidonocw-hol-string**をクリックして**バケット**を開きます。 ![コンパートメントの選択](images/select-compartment.png)
    
5.  バケットにオブジェクト**hello.txt**が含まれ、グリーティング・ワードが**Hello**であるため、サイズが**5バイト**になっていることを確認します。オブジェクトをダウンロードして、コンテンツが実際に_Hello_であることを確認することもできます。 ![サイズの検証](images/verify-size.png)
    
    > このページは開いたままにしておきます。このページをリフレッシュすると、次のステップで挨拶の単語が変更されます。
    
6.  次のコマンドをコピーして貼り付け、**Hello**グリーティングワードを **Hola**に置き換えます。
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
7.  バケットの **hello.txt**オブジェクトのサイズが **4バイト**になっていることを確認します。これは、グリーティングワードが **Hola**に置き換えられるためです。オブジェクトをダウンロードして、コンテンツが**「Hola」**に変更されていることを確認することもできます。 ![ホラサイズ](images/hola-size.png)
    
8.  **restart.sh**ツールを使用してアプリケーションを再起動し、グリーティング・ワードの値が**オブジェクト・ストレージ**に保持されるため存続することを示します。
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/restart.sh</copy>
        Created private.key and can be used to ssh to the deployment instance by running this command: "ssh -i private.key opc@xx.xx.xx.xx"
        FIPS mode initialized
        The authenticity of host 'xx.xx.xx.xx (xx.xx.xx.xx)' can't be established.
        ECDSA key fingerprint is SHA256:hJl8axCNhFcILDo+AwxMkodxhY+UxRD40d1ans83GTg.
        ECDSA key fingerprint is SHA1:IBUhyn05DaIs60GAQsruVXajhym.
        Are you sure you want to continue connecting (yes/no)? yes
        Warning: Permanently added 'xx.xx.xx.xx' (ECDSA) to the list of known hosts.
        Starting oci-mp-server.jar
        Helidon app is now running with pid 264792!
        Cleaning up ssh private.key
        
    
    ![アプリケーションの再起動](images/restart-application.png)
    
    > _「Are you want to continue connecting (yes/no)?」_を求められたら、**yes**を入力します。
    
9.  デフォルトのHello Worldリクエストをコールし、**グリーティング・ワードがまだHolaであることを確認します**。
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
10.  **クラウド・コンソール**で「バケット」ページをリフレッシュすると、そのサイズがまだ**4**になり、挨拶文がまだ**「Hola」**であることを確認できます。 ![永続性の検証](images/verify-persistence.png)
    

**おめでとうございます!**ワークショップを完了しました。**ラボ6**に進むと、このワークショップで作成された**すべてのリソースが削除**されます。

## さらに学ぶ

*   [Helidon OCI統合](https://helidon.io/docs/v3/#/mp/integrations/oci)

## 確認

*   **著者** - Keith Lustria
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月