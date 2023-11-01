# Helidon MPアプリケーションのネイティブ・イメージの構築

## 概要

この演習では、Helidon MPアプリケーションのGraalVMネイティブ・イメージを構築する方法を学習します。

見積時間: 15分

ラボのクイック・ウォークスルーについては、次のビデオをご覧ください。[Helidon MPアプリケーションのネイティブ・イメージの構築](videohub:1_0hftfgfy)

### GraalVMネイティブ・イメージについて

GraalVMは、HotSpot JVMで実行されているJavaワークロードを高速化できる高パフォーマンスのJDKディストリビューションです。

GraalVMネイティブ・イメージの事前コンパイルにより、小型で高速な軽量Javaアプリケーションを構築でき、メモリーとCPUの使用量を削減できます。ビルド時に、GraalVMネイティブ・イメージは、Javaアプリケーションとその依存関係を分析して、必要なクラス、メソッドおよびフィールドのみを識別し、それらの要素のみに対して最適化されたマシン・コードを生成します。

GraalVM Enterprise Editionは、追加コストなしでOracle Cloud Infrastructure (OCI)上で使用できます。

### 目的

*   GraanVMのローカル・インストールを使用して、Helidon MPアプリケーション・ネイティブ実行可能ファイルをビルドします
*   Helidon Greetingアプリの実行と実行

### 事前設定

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)が有効になっているアカウントが必要です。

## タスク1: Helidon MPアプリケーションGraalVMネイティブ・イメージの作成

1.  同じアプリケーションでGraalVMネイティブ・イメージを作成するには、mavenとJDKを構成したターミナルで次のコマンドを実行します。
    
        <copy>export USE_NATIVE_IMAGE_JAVA_PLATFORM_MODULE_SYSTEM=false
        mvn clean package -Pnative-image</copy>
        
    
    次のような出力が表示されます。
    
        [1/7] Initializing...                                                                                  (18.4s @ 0.19GB)
        Version info: 'GraalVM 22.3.1 Java 17 EE'
        Java version info: '17.0.6+9-LTS-jvmci-22.3-b11'
        C compiler: gcc (redhat, x86_64, 12.1.1)
        Garbage collector: Serial GC
        2 user-specific feature(s)
        - io.helidon.integrations.graal.mp.nativeimage.extension.WeldFeature
        - io.helidon.integrations.graal.nativeimage.extension.HelidonReflectionFeature
        2023.04.06 04:22:45 INFO io.helidon.common.LogConfig Thread[main,5,main]: Logging at initialization configured using classpath: /logging.properties
        [2/7] Performing analysis...  [*********]                                                             (208.1s @ 2.13GB)
        18,899 (92.48%) of 20,436 classes reachable
        27,626 (63.50%) of 43,504 fields reachable
        97,938 (64.48%) of 151,894 methods reachable
        1,068 classes,   565 fields, and 6,861 methods registered for reflection
            65 classes,    70 fields, and    58 methods registered for JNI access
            6 native libraries: dl, m, pthread, rt, stdc++, z
        [3/7] Building universe...                                                                             (25.3s @ 3.10GB)
        [4/7] Parsing methods...      [*****]                                                                  (23.1s @ 3.36GB)
        [5/7] Inlining methods...     [***]                                                                     (9.1s @ 1.66GB)
        [6/7] Compiling methods...    [[6/7] Compiling methods...    [********************]                                                  (436.2s @ 2.94GB)
        [7/7] Creating image...                                                                                (20.5s @ 2.37GB)
        50.11MB (58.19%) for code area:    56,410 compilation units
        35.15MB (40.82%) for image heap:  506,032 objects and 128 resources
        867.91KB ( 0.98%) for other data
        86.11MB in total
        -----------------------------------------------------------------------------------------------------------------------
        Top 10 packages in code area:                              Top 10 object types in image heap:
        4.03MB com.oracle.svm.core.code                           10.20MB byte[] for code metadata
        1.67MB sun.security.ssl                                    4.17MB byte[] for java.lang.String
        1.53MB java.util                                           3.47MB java.lang.Class
        1.48MB org.jboss.weld.logging                              3.20MB java.lang.String
        1.45MB com.sun.media.sound                                 2.86MB byte[] for general heap data
        1.01MB io.netty.buffer                                     1.22MB byte[] for reflection metadata
        919.44KB java.lang.invoke                                    1.03MB byte[] for embedded resources
        906.25KB com.sun.crypto.provider                           885.89KB com.oracle.svm.core.hub.DynamicHubCompanion
        812.44KB java.util.concurrent                              594.09KB java.util.HashMap$Node
        752.97KB io.helidon.config                                 539.66KB c.o.svm.core.hub.DynamicHub$ReflectionMetadata
        35.21MB for 674 more packages                               6.36MB for 4412 more object types
        -----------------------------------------------------------------------------------------------------------------------
                            40.7s (5.3% of total time) in 111 GCs | Peak RSS: 5.12GB | CPU load: 1.63
        -----------------------------------------------------------------------------------------------------------------------
        Produced artifacts:
        /home/ankit_x_pa/helidon-project/myproject/myproject/target/myproject (executable)
        /home/ankit_x_pa/helidon-project/myproject/myproject/target/myproject.build_artifacts.txt (txt)
        =======================================================================================================================
        Finished generating 'myproject' in 12m 40s.
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  13:00 min
        [INFO] Finished at: 2023-04-06T04:35:07Z
        [INFO] ------------------------------------------------------------------------
        
        

## タスク2: Helidon MPアプリケーションGraalVMネイティブ・イメージの実行

1.  次のコマンドを同じ端末にコピーして貼り付け、このアプリケーションを実行します
    
        <copy>./target/myproject</copy>
        
    
    ![ネイティブ・イメージの実行](images/run-native.png)
    
    > アプリケーションの起動時間は142ミリ秒で、これはほぼ35倍高速です。
    
2.  _"./target/myproject"コマンドが実行されている端末に`Ctrl + C`と入力して、**myproject**アプリケーションを停止します_。これは非常に重要であり、それ以外はラボの後半で問題に直面します。
    

## 確認

*   **作成者** - Dmitry Aleksandrov
*   **貢献者** - Ankit Pandey、Maciej Gruszka
*   **最終更新者/日付** - Ankit Pandey、2023年4月