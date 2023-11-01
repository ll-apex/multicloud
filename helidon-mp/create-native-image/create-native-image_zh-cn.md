# 构建 Helidon MP 应用原生映像

## 简介

在本练习中，您将学习如何为 Helidon MP 应用程序构建 GraalVM 本机映像。

估计时间：15 分钟

观看下面的视频，快速浏览实验室。[构建 Helidon MP 应用原生映像](videohub:1_0hftfgfy)

### 关于 GraalVM 本机映像

GraalVM 是一个高性能 JDK 分发，可以加快在 HotSpot JVM 上运行的任何 Java 工作负载。

GraalVM 利用原生映像提前编译，您可以构建较小、更快、更少内存和 CPU 的轻量级 Java 应用程序。在构建时，GraalVM 本机映像将分析 Java 应用程序及其依赖项，从而确定所需的类、方法和字段，并仅为这些元素生成优化的计算机代码。

GraalVM Enterprise Edition 可在 Oracle Cloud Infrastructure (OCI) 上使用，无需额外付费。

### 目标

*   使用本地安装 GraanVM 构建 Helidon MP 应用程序原生可执行文件
*   运行并练习 Helidon 问候应用程序

### 先备条件

*   您必须有启用 [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 的账户。

## 任务 1：创建 Helidon MP 应用程序 GraalVM 本机映像

1.  要使用同一应用程序创建 GraalVM 本机映像，请在终端中运行以下命令，在终端中配置 maven 和 JDK。
    
        <copy>export USE_NATIVE_IMAGE_JAVA_PLATFORM_MODULE_SYSTEM=false
        mvn clean package -Pnative-image</copy>
        
    
    输出如下所示。
    
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
        
        

## 任务 2：运行 Helidon MP 应用程序 GraalVM 本机映像

1.  在同一终端中复制并粘贴以下命令以运行此应用程序
    
        <copy>./target/myproject</copy>
        
    
    ![运行本机图像](images/run-native.png)
    
    > 您可以看到应用程序启动时间为 142 毫秒，速度快了 35 倍。
    
2.  _通过在运行 "./target/myproject" 命令的终端中输入 `Ctrl + C` 来停止 **myproject** 应用程序_。很重要，你将在实验室里面对问题。
    

## 确认

*   **作者** - Dmitry Aleksandrov
*   **贡献者** - Ankit Pandey、Maciej Gruszka
*   **上次更新者/日期** - Ankit Pandey，2023 年 4 月