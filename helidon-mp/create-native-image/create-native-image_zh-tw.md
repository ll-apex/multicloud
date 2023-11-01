# 建立 Helidon MP 應用程式原生影像

## 簡介

在此實驗室中，您將瞭解如何為 Helidon MP 應用程式建立 GraalVM 原生影像。

預估時間：15 分鐘

請觀看下方影片，快速瞭解實驗室的逐步解說。[建置 Helidon MP 應用程式原生影像](videohub:1_0hftfgfy)

### 關於 GraalVM 原生影像

GraalVM 是一種高效能 JDK 發行版，可加速在 HotSpot JVM 上執行的所有 Java 工作負載。

GraalVM 原生映像檔會預先編譯，可讓您建置較小、更快且記憶體與 CPU 較少的輕量型 Java 應用程式。「GraalVM 原生影像」會在建立時分析 Java 應用程式及其相依性，以便只識別必要的類別、方法及欄位，並為這些元素產生最佳化的機器程式碼。

GraalVM Enterprise Edition 可用於 Oracle Cloud Infrastructure (OCI)，無須額外付費。

### 目標

*   在本機安裝 GraanVM 時建置 Helidon MP 應用程式原生執行檔
*   執行與執行 Helidon Greeting 應用程式

### 先決條件

*   必須要有已啟用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的帳戶。

## 作業 1：建立 Helidon MP 應用程式 GraalVM 原生影像

1.  若要使用相同的應用程式建立 GraalVM 原生影像，請在終端機中執行下列命令，並在其中設定 maven 和 JDK。
    
        <copy>export USE_NATIVE_IMAGE_JAVA_PLATFORM_MODULE_SYSTEM=false
        mvn clean package -Pnative-image</copy>
        
    
    您會看到類似下方的執行結果。
    
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
        
        

## 作業 2：執行 Helidon MP 應用程式 GraalVM 原生影像

1.  在相同的終端機中複製並貼上下列命令以執行此應用程式
    
        <copy>./target/myproject</copy>
        
    
    ![執行原生影像](images/run-native.png)
    
    > 您會看到應用程式啟動時間是 142 毫秒，速度將近 35 倍。
    
2.  _在執行 "./target/myproject" 指令的終端機中輸入 `Ctrl + C` 來停止 **myproject** 應用程式_。太多，您還會發放「標籤」中的元素。
    

## 確認

*   **作者** - Dmitry Aleksandrov
*   **貢獻者** - Ankit Pandey，Maciej Gruszka
*   **上次更新者 / 日期** - Ankit Pandey，2023 年 4 月