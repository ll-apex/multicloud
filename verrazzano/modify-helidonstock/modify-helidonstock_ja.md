# BobbyのBookアプリケーションを変更し、新しいアプリケーション・コンポーネント・イメージを作成する

## 概要

このラボでは、_クラウド・シェル_を介してbobbys-helidon-stock-applicationを変更します。後で、bobbys-helidon-stock-applicationの新しいDockerイメージを作成します。このbobbys-helidon-stock-applicationイメージは、BobbyのBooksアプリケーションのコンポーネントです。

所要時間: 10分

### 目的

この演習では、次のことを行います。

*   bobbys-helidon-stock-applicationを変更します。
*   bobbys-helidon-stock-applicationの新しいDockerイメージを作成します。

### 前提条件

コマンドとURLを貼り付け、環境に応じて変更できるテキスト・エディタが必要です。その後、変更したコマンドをコピーして貼り付け、_クラウド・シェル_で実行できます。

## タスク1: bobbys-helidon-stock-applicationの変更

1.  Bobbyの「ブック」タブを選択し、_「ブック」_をクリックして、次に示すように_「The Hobbit」_ブックのイメージをクリックします。
    
    ![「Books」をクリックします。](images/clickbooks.png " ")
    
    図に示すように、ブック名が_The Hobbit_の形式で表示されます。
    
    ![ホビット](images/thehobbit.png " ")
    
2.  ブック名を大文字(THE HOBBIT)に変換します。Bobby's Booksアプリケーションのソース・コードをダウンロードする必要があります。ホーム・フォルダにいることを確認します。次のコマンドをコピーして、_クラウド・シェル_に貼り付けます。
    
        <copy>cd ~
        git clone -b v0.16.0 https://github.com/verrazzano/examples.git</copy>
        
    
    ![リポジトリをクローニングします。](images/clonerepository.png " ")
    
3.  BobbyのBookアプリケーション内のファイルを表示するには、次のコマンドをコピーして_クラウド・シェル_に貼り付けます。
    
        <copy>ls -la ~/examples/bobs-books/</copy>
        
    
    出力は次のようになります。`bash $ ls -la ~/examples/bobs-books/ total 16 drwxr-xr-x. 5 ankit_x_pa oci 100 Mar 21 12:14 . drwxr-xr-x. 7 ankit_x_pa oci 4096 Mar 21 12:14 .. drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobbys-books drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobs-bookstore-order-manager -rw-r--r--. 1 ankit_x_pa oci 942 Mar 21 12:14 README.md drwxr-xr-x. 4 ankit_x_pa oci 72 Mar 21 12:14 roberts-books $`
    
4.  次に、関連するJAVA\_FILEを変更します。ファイルを開くには、次のコマンドをコピーして_クラウド・シェル_に貼り付けてください。
    
        <copy>vi ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/src/main/java/org/books/bobby/BookResource.java</copy>
        
    
    ![ファイルを開く](images/openfile.png " ")
    
5.  コードを変更できるように、_i_を押します。行番号84に新しい行を追加するには、次の行をコピーして行番号84に貼り付けます。
    
        <copy>optional.get().setTitle(optional.get().getTitle().toUpperCase());</copy>
        
    
    ![行挿入](images/insertline.png " ")
    
6.  「_Esc_」、「_:wq_」の順に押して変更を保存します。
    
    ![変更の保存](images/savechanges.png " ")
    

## タスク2: bobbys-helidon-stock-applicationの新しいDockerイメージの作成

1.  _bobbys-coherence-application_に依存する_bobbys-helidon-stock-application_の新しいDockerイメージをビルドするため、_Maven_コマンドを実行して既存のbobbys-coherence-applicationアーカイブをクリーンアップし、ローカルのMavenリポジトリに新しいbobby-coherence-applicationアーカイブをコンパイル、ビルド、パッケージ化およびインストールします。_bobbys-coherence_ディレクトリ・フォルダに変更し、_bobbys-coherence_アプリケーション・アーカイブをローカルMavenリポジトリにコンパイル、ビルドおよびインストールするには、次のコマンドをコピーして_クラウド・シェル_に貼り付けます。
    
        <copy>
        cd ~/examples/bobs-books/bobbys-books/bobbys-coherence/
        mvn clean install
        </copy>
        
    
    結果の出力は次のようになります(出力の開始と終了のみを示すように省略されます): `bash $ mvn clean install [INFO] Scanning for projects... [INFO] [INFO] ------------< io.verrazzano.example.books:bobbys-coherence >------------ [INFO] Building bobbys-coherence 1.0-SNAPSHOT [INFO] --------------------------------[ jar ]--------------------------------- [INFO] [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ bobbys-coherence --- [[INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/target/bobbys-coherence.jar to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.jar [INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/pom.xml to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.pom [INFO] ------------------------------------------------------------------------ [INFO] BUILD SUCCESS [INFO] ------------------------------------------------------------------------ [INFO] Total time: 8.887 s [INFO] Finished at: 2023-03-22T04:09:11Z [INFO] ------------------------------------------------------------------------ $`
    
2.  _bobbys-helidon-stock-application_を変更したため、このアプリケーションをコンパイル、ビルドおよびパッケージ化する必要があります。_bobbys-helidon-stock-application_ディレクトリに変更し、_bobbys-helidon-stock-application_をJARファイルにパッケージ化するには、次のコマンドをコピーして_クラウド・シェル_に貼り付けます。2番目のイメージでは、`bobbys-helidon-stock-application.JAR`ファイルの作成が`~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target`で確認できます。
    
        <copy>cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/
        mvn clean package
        </copy>
        
    
    The resulting output is similar to the following (abbreviated to show only the start and end of output): \`\`\`bash $ cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/ $ mvn clean package \[INFO\] Scanning for projects... \[INFO\] ------------------------------------------------------------------------ \[INFO\] Detecting the operating system and CPU architecture \[INFO\] ------------------------------------------------------------------------
    
         [INFO] --- maven-jar-plugin:2.5:jar (default-jar) @ bobbys-helidon-stock-application ---
         [INFO] Building jar: /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target/bobbys-helidon-stock-application.jar
         [INFO] ------------------------------------------------------------------------
         [INFO] BUILD SUCCESS
         [INFO] ------------------------------------------------------------------------
         [INFO] Total time:  10.106 s
         [INFO] Finished at: 2023-03-22T04:11:38Z
         [INFO] ------------------------------------------------------------------------
         $
         ```
        
3.  bobby-stock-helidon-applicationのDockerイメージを作成しますが、このアプリケーションでは特定のバージョンのJDKが使用されるため、新しいイメージを構築するDockerファイルは変更しません。そのため、必要なJDKをダウンロードします。必要なJDKバージョンをダウンロードするには、次のコマンドをコピーして_クラウド・シェル_に貼り付けます。
    
        <copy>wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2_linux-x64_bin.tar.gz</copy>
        
    
    出力は次のようになります: \`\`bash $ wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz --2023-03-22 04:20:16-- https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz Resolving download.java.net (download.java.net)... 2.23.220.73 download.java.net (download.java.net)|2.23.220.73|:443... connected。送信されたHTTPリクエスト、応答待ち... 200 OK長さ: 198606200 (189M) \[application/x-gzip\]保存先: 'openjdk-14.0.2\_linux-x64\_bin.tar.gz'
    
         100%[==============================================================================================================================>] 198,606,200  194MB/s   in 1.0s   
        
         2023-03-22 04:20:17 (194 MB/s) - ‘openjdk-14.0.2_linux-x64_bin.tar.gz’ saved [198606200/198606200]
         $
         ```
        
4.  演習6でOracle Cloud Container RegistryにアップロードするDockerイメージを作成しています。ファイル名を作成するには、次の情報が必要です。
    
    *   テナント・ネームスペース
    *   リージョンのエンドポイント
5.  テナンシのネームスペースを検索するには、次に示すように_「ユーザー」_アイコン-> _「テナンシ」_をクリックします。**オブジェクト・ストレージ設定**には、ネームスペースがあります。テキスト・ファイルのどこかにコピーして保存します。これは、演習6でも使用するためです。
    
    ![テナンシネームスペースのコピー](images/copy-tenancynamespace.png " ")
    
6.  _「Endpoint for Your Region」_を見つけます。このURL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)に記載されている表を参照してください。この例では、リージョンのエンドポイントは_UK South (London)_ (リージョン名)で、そのエンドポイントは_lhr.ocir.io_です。自分の_リージョン名_のエンドポイントを見つけ、テキスト・ファイルに保存します。次の研究室にも必要です。
    
    ![エンド・ポイント](images/end-point.png " ")
    
    > これで、リージョンのテナンシ・ネームスペースとエンドポイントの両方が用意されました。
    
7.  これで、リージョンのテナンシ・ネームスペースとエンドポイントの両方があります。次のコマンドをコピーして、テキスト・エディタに貼り付けます。次に、**`END_POINT_OF_YOUR_REGION`**をリージョン名のエンドポイント、**`NAMESPACE_OF_YOUR_TENANCY`**をテナンシの名前空間、**`your_first_name`**を小文字の名に置き換えます。
    
        <copy>docker build --force-rm=true -f Dockerfile -t END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0 .</copy>
        
    
    ![Dockerビルド](images/docker-build.png " ")
    
    ![イメージが作成されました](images/image-created.png " ")
    
    > たとえば、私の場合、コマンドは`docker build --force-rm=true -f Dockerfile -t lhr.ocir.io/tenancynamespace/helidon-stock-application-ankit`です。
    
    これによりDockerイメージが作成され、演習6でOracle Cloud Container Registryリポジトリにプッシュされます。置換されたフル・イメージ名**`END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0`**をテキストeditor.In Lab 6にコピーする必要があります。リポジトリを作成する必要がある場合は、**`helidon-stock-application-your_first_name`**という名前を付ける必要があります。
    
    _クラウド・シェル_は開いたままにしておきます。次の演習に必要です。
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月