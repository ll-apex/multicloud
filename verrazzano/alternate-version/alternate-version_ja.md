# シンプルなソリューションでバージョンを変更

## 概要

この演習では、変更されたbobbys-helidon-stock-applicationイメージを使用します。このイメージは、Oracle Cloud Container Registry内のパブリック・リポジトリに格納されます。また、変更されたbobs-books-comp-mod.yamlファイルも使用します。これは、変更されたbobbys-helidon-stock-applicationイメージを指します。

### 目的

この演習では、次のことを行います。

*   変更されたbobs-books-comp-mod.yamlファイルをダウンロードします。
*   kubectlを使用した変更の適用

### 事前設定

*   演習1を実行します。これにより、Oracle Cloud InfrastructureにOKEクラスタが作成されます。
*   Lab 2を実行します。これにより、VerrazzanoがKubernetesクラスタにインストールされます。
*   Bobs-BooksアプリケーションをデプロイするLab 3を実行します。
*   コマンドとURLを貼り付け、環境に応じて変更できるテキスト・エディタが必要です。その後、変更したコマンドをコピーして貼り付け、_クラウド・シェル_で実行できます。

## タスク1: 変更したbobs-books-comp-mod.yamlファイルのダウンロード

1.  次のコマンドを実行して、変更されたbobs-books-comp-mod.yamlファイルをダウンロードします。
    
        <copy>curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/alternate-version/bobs-books-comp-mod.yaml >~/bobs-books-comp-mod.yaml</copy>
        
    
    ![ファイルのダウンロード](images/downloadfile.png " ")
    

## タスク2: kubectlを使用した変更の適用

1.  変更を適用するには、_クラウド・シェル_で次のコマンドをコピーして貼り付けます。変更を適用すると、新しいコンポーネントのリクエストを処理するために新しいポッドが初期化され、古いコンポーネントに関連付けられたポッドは引き続きリクエストを処理します。その後、新しいポッドが_「実行中」_状態になると、古いポッドは_「終了済」_になり始めます。最終的には、新しいポッドのみが_「実行中」_状態になります。
    
        <copy>kubectl apply -f ~/bobs-books-comp-mod.yaml -n bobs-books</copy>
        
    
    ![変更の適用](images/applychanges.png " ")
    
    出力で確認できます。_component.core.oam.dev/bobby-helidon_のみが構成され、その他のコンポーネントは変更されません。
    
2.  新しいポッドがどのように初期化され、古いポッドが_「終了中」_状態になるかを表示するには、_クラウド・シェル_で次のコマンドをコピーして貼り付けます。
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    次のような出力が表示されます。
    
        vera_zano@cloudshell:~ (us-ashburn-1)$ kubectl get pods -n bobs-books
        NAME                                               READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                                 2/2    Running  0         130m
        bobbys-front-end-adminserver                       4/4    Running  0         127m
        bobbys-front-end-managed-server1                   4/4    Running  0         126m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  0/2    PodInitializing  0         10s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Running  0         130m
        bobs-bookstore-adminserver                         4/4    Running  0         127m
        bobs-bookstore-managed-server1                     4/4    Running  0         126m
        mysql-65d864bf8c-xf64p                             2/2    Running  0         130m
        robert-helidon-bfdfb58b8-58qfs                     2/2    Running  0         130m
        robert-helidon-bfdfb58b8-lkw8m                     2/2    Running  0         130m
        roberts-coherence-0                                2/2    Running  0         130m
        roberts-coherence-1                                2/2    Running  0         130m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  1/2    Running  0         28s
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  2/2    Running  0         34s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        vera_zano@cloudshell:~ (us-ashburn-1)$
        
    
    すべてのポッドが_「実行中」_ステータスであることを確認したら、_\[CTRL\]を押しながら\[C\]_を押してこのコマンドを強制終了します。
    

最後のラボにも必要なので、_クラウド・シェル_を開いたままにします。

## 確認

*   **著者** - Ankit Pandey
*   **貢献者** - Sid Joshi、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年2月