# Bobbyのブック構成YAMLファイルの変更

## 概要

演習6では、_bobby-helidon-stock-application_の更新されたDockerイメージをプッシュしました。ここでは、Verrazzanoがサービスに影響を与えずに、更新されたアプリケーションおよびコンポーネントを再デプロイします。そのためには、Verrazzanoが新しいイメージを取得し、そのポッドを開始するようにYAMLファイルを構成する必要があります。ポッドが_「実行中」_状態になると、前のアプリケーションおよびコンポーネントに関連付けられたポッドが終了します。

所要時間: 10分

### 目的

この演習では、次のことを行います。

*   `bobs-books-comp.yaml`ファイルを変更します。
*   `kubectl`を使用して変更を適用します。

### 事前設定

コマンドとURLを貼り付け、環境に応じて変更できるテキスト・エディタが必要です。その後、変更したコマンドをコピーして貼り付け、_クラウド・シェル_で実行できます。

## タスク1: bobs-books-comp.yamlファイルの変更

1.  アプリケーション構成ファイル_bobs-books-comp.yaml_があります。演習2では、アプリケーションyamlファイルをダウンロードしました。yamlファイルを含むホーム・ディレクトリに変更するには、次のコマンドをコピーして_クラウド・シェル_に貼り付けます。
    
        <copy>cd ~</copy>
        
2.  この場所には、bobs-booksアプリケーションの構成ファイルがあります。ラボ5の一部として、bobbys-helidon-stock-applicationを変更し、そのコンポーネントの新しいDockerイメージを構築しました。演習6では、そのDockerイメージをOracle Cloud Container Registryリポジトリにプッシュしました。この演習では、Oracle Cloud Container Registryリポジトリから新しい更新済Dockerイメージを取得するように、_bobs-books-comp.yaml_ファイルを変更します。_bobs-books-comp.yaml_ファイルを変更するには、次のコマンドをコピーして_クラウド・シェル_に貼り付けます。
    
        <copy>vi bobs-books-comp.yaml</copy>
        
    
    ![ファイルを開く](images/openfile.png " ")
    
3.  Lab 5の一部として、Dockerイメージのフルネームを保存しました。次の行をコピーして、テキスト・エディタに貼り付ける必要があります。次に、`docker image full name`をDockerイメージ名に置き換える必要があります。次に、変更した行をコピーし、_i_を押して`*bobs-books-comp.yaml*`ファイルにテキストを挿入します。次の図に示すように、出力を行番号148で貼り付け(必ずインデントを保持してください)、終了行番号147を _#_でコメントアウトしてから、_Esc_キーを押してから、_:wq_と入力してファイルを保存します。
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![行挿入](images/insert-line.png " ")
    

## タスク2: `kubectl`を使用した変更の適用

1.  変更を適用するには、_クラウド・シェル_で次のコマンドをコピーして貼り付けます。変更を適用すると、新しいコンポーネントのリクエストを処理するために新しいポッドが初期化され、古いコンポーネントに関連付けられたポッドは引き続きリクエストを処理します。その後、新しいポッドが_「実行中」_状態になると、古いポッドは_「終了済」_になり始めます。最終的には、新しいポッドのみが_「実行中」_状態になります。
    
        <copy>kubectl apply -f bobs-books-comp.yaml -n bobs-books</copy>
        
    
    結果は次のようになります。
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh unchanged
        component.core.oam.dev/robert-helidon unchanged
        component.core.oam.dev/bobby-coh unchanged
        component.core.oam.dev/bobby-helidon configured
        component.core.oam.dev/bobby-wls unchanged
        component.core.oam.dev/bobs-mysql-configmap unchanged
        component.core.oam.dev/bobs-mysql-service unchanged
        component.core.oam.dev/bobs-mysql-deployment unchanged
        component.core.oam.dev/bobs-orders-configmap unchanged
        component.core.oam.dev/bobs-orders-wls unchanged
        $
        
    
    出力で確認できます。_component.core.oam.dev/bobby-helidon_のみが構成され、その他のコンポーネントは変更されません。
    
2.  新しいポッドがどのように初期化され、古いポッドが_「終了中」_状態になるかを表示するには、_クラウド・シェル_で次のコマンドをコピーして貼り付けます。
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    次のような出力が表示されます。
    
        $ kubectl get pods -n bobs-books
        NAME                                         READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                           2/2    Running      0         130m
        bobbys-front-end-adminserver                 4/4    Running      0         127m
        bobbys-front-end-managed-server1             4/4    Running      0         126m
        bobbys-helidon-stock-application-64fb55-zzp  0/2    PodInitializing  0     10s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Running      0         130m
        bobs-bookstore-adminserver                   4/4    Running      0         127m
        bobs-bookstore-managed-server1               4/4    Running      0         126m
        mysql-65d864bf8c-xf64p                       2/2    Running      0         130m
        robert-helidon-bfdfb58b8-58qfs               2/2    Running      0         130m
        robert-helidon-bfdfb58b8-lkw8m               2/2    Running      0         130m
        roberts-coherence-0                          2/2    Running      0         130m
        roberts-coherence-1                          2/2    Running      0         130m
        bobbys-helidon-stock-application-64fb55-zzp  1/2    Running      0         28s
        bobbys-helidon-stock-application-64fb55-zzp  2/2    Running      0         34s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        $
        
    
    すべてのポッドが_「実行中」_ステータスであることを確認したら、_\[CTRL\]を押しながら\[C\]_を押してこのコマンドを強制終了します。
    
    最後のラボにも必要なので、_クラウド・シェル_を開いたままにします。
    

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年8月