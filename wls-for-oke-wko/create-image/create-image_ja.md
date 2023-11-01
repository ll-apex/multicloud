# 補助イメージの作成およびOracle Container Image Registryへのプッシュ

## 概要

**プライマリ・イメージ** - Oracle Fusion Middlewareソフトウェアを含むイメージ。ドメインに対してWebLogicサーバーを実行するすべてのコンテナの基礎として使用されます。

**補助イメージ** - WebLogicデプロイ・ツール・ソフトウェアおよびモデル・ファイルを提供するイメージ。実行時に、補助イメージのコンテンツがプライマリ・イメージのコンテンツとマージされます。![イメージ構造](images/image-structure.png)

この演習では、WebLogicサーバーの12.2.1.3.0-ol8イメージをプライマリ・イメージとして使用します。また、補助イメージを作成し、生成された認証トークンを使用してOracle Container Image Registryリポジトリにプッシュします。

見積時間: 10分

### 目的

この演習では、次のことを行います。

*   補助イメージを作成し、イメージをOracle Cloud Container Image Registryにプッシュします。

## タスク1: 補助イメージの準備と補助イメージのプッシュ

このタスクでは、Oracle Cloud Container Registryにプッシュする補助イメージを作成します。

1.  _「イメージ」_をクリックします。プライマリ・イメージの場合、次に示すように、次の_weblogic_ Image.Soのデフォルト値を_「プライマリ・イメージ」_セクションの下で使用します
    
        <copy>container-registry.oracle.com/middleware/weblogic:12.2.1.3-ol8</copy>
        
    
    ![プライマリ・イメージ](images/primary-image.png)
    
    > **参考情報:**  
    > プライマリ・イメージは、ドメインの実行に使用されるイメージです。1つのプライマリイメージを何百ものドメインで再利用できます。プライマリ・イメージには、OS、JDKおよびFMWソフトウェアのインストールが含まれています。
    
2.  補助イメージ・タグを作成するには、次の情報が必要です。
    
    *   リージョンのエンドポイント
    *   テナント・ネームスペース
3.  _「Endpoint for Your Region」_を見つけます。このURL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab)に記載されている表を参照してください。この例では、リージョンのエンドポイントは_UK South (London)_ (リージョン名)で、そのエンドポイントは_lhr.ocir.io_です。自分の_リージョン名_のエンドポイントを見つけ、テキスト・ファイルに保存します。次の研究室にも必要です。
    
    ![エンド・ポイント](images/end-point.png " ")
    
    > これで、リージョンのテナンシ・ネームスペースとエンドポイントの両方が用意されました。
    
4.  演習3では、テキスト・ファイルにテナンシ・ネームスペースがすでに記載されています。そうでない場合は、テナンシのネームスペースを検索するために、次に示すように、_「ハンバーガー・メニュー」_→_「開発者サービス」_→_「コンテナ・レジストリ」_を選択します。作成したリポジトリを選択すると、次のようにネームスペースが表示されます。 ![テナント・ネームスペース](images/tenancy-namespace.png)
    
5.  これで、リージョンのテナンシ・ネームスペースとエンドポイントの両方があります。次のコマンドをコピーして、テキスト・ファイルに貼り付けます。次に、`END_POINT_OF_YOUR_REGION`をリージョン名のエンドポイント、`NAMESPACE_OF_YOUR_TENANCY`をテナンシのネームスペースに置き換えます。次に示すように、_「補助イメージ」_タブをクリックします。 ![「補助」タブ](images/auxiliary-tab.png)
    
        <copy>END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/test-model-your_first_name:v1</copy>
        

> たとえば、この場合、補助イメージ・タグは`lhr.ocir.io/tenancynamespace/test-model-ankit:v1`です。

6.  ステップ4では、テナンシ・ネームスペースも決定しました。補助イメージ・レジストリのプッシュ・ユーザー名を次のように入力します: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`。  
    

*   `NAMESPACE_OF_YOUR_TENANCY`をテナンシの名前空間に置き換えます
*   `YOUR_ORACLE_CLOUD_USERNAME`をOracle Cloudアカウントのユーザー名に置き換え、置換したユーザー名をテキスト・ファイルからコピーして、_補助イメージ・レジストリのプッシュ・ユーザー名_に貼り付けます。

> たとえば、この場合、**補助イメージ・レジストリのプッシュ・ユーザー名**は`tenancynamespace/lab.user@oracle.com`です。

*   「パスワード」で、テキスト・ファイル(または保存した場所)から認証トークンをコピーして貼り付け、**補助イメージ・レジストリのプッシュ・ユーザー名**に貼り付けます。 ![補助イメージの詳細](images/auxiliary-image-details.png)

7.  _「補助イメージの作成」_をクリックします。 ![補助イメージの作成](images/create-auxiliary-image.png)
    
8.  演習2でモデルをすでに準備しているため、_「いいえ」_をクリックします。 ![モデルの準備](images/prepare-model.png)
    
9.  _WebLogicデプロイヤ_を保存する_「ダウンロード」_フォルダを選択し、次に示すように_「選択」_をクリックします。 ![WDT場所](images/wdt-location.png)
    
10.  補助イメージが正常に作成されたら、_「補助イメージの作成の完了」_ウィンドウで_「OK」_をクリックします。 ![補助の作成](images/auxiliary-created.png)
    
    > **参考情報:**  
    > 補助イメージはドメイン固有です。補助イメージには、ドメインを定義するデータが含まれます。
    
11.  _「補助イメージのプッシュ」_をクリックして、Oracle Cloud Container Image Registry内のリポジトリ内のイメージをプッシュします。 ![プッシュ補助](images/push-auxiliary.png)
    
12.  イメージが正常にプッシュされたら、_「イメージのプッシュ完了」_ウィンドウで_「OK」_をクリックします。 ![押された補助](images/auxiliary-pushed.png)
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年10月