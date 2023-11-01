# Helidon MP 애플리케이션 네이티브 이미지 구축

## 소개

이 실습에서는 Helidon MP 애플리케이션용 GraalVM 네이티브 이미지를 구축하는 방법을 알아봅니다.

예상 시간: 15분

아래 비디오를 통해 랩을 빠르게 살펴보십시오. [Helidon MP 애플리케이션 네이티브 이미지 구축](videohub:1_0hftfgfy)

### GraalVM 네이티브 이미지 정보

GraalVM는 HotSpot JVM에서 실행되는 모든 Java 워크로드를 가속화할 수 있는 고성능 JDK 배포판입니다.

GraalVM Native Image AOT 컴파일을 사용하면 더 작고 빠르며 더 적은 메모리 및 CPU를 사용하는 경량 Java 애플리케이션을 구축할 수 있습니다. 빌드 시 GraalVM Native Image는 Java 응용 프로그램과 종속성을 분석하여 필요한 클래스, 메소드 및 필드만 식별하고 해당 요소에 대해서만 최적화된 시스템 코드를 생성합니다.

GraalVM Enterprise Edition은 추가 비용 없이 OCI(Oracle Cloud Infrastructure)에서 사용할 수 있습니다.

### 목표

*   로컬에서 GraanVM를 설치하여 Helidon MP 애플리케이션 전용 실행 파일을 빌드합니다.
*   Helidon 인사말 앱 실행 및 실행

### 필요 조건

*   [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure) 사용 계정이 있어야 합니다.

## 작업 1: Helidon MP 애플리케이션 GraalVM 네이티브 이미지 생성

1.  동일한 응용 프로그램으로 GraalVM Native 이미지를 만들려면 maven 및 JDK를 구성한 터미널에서 다음 명령을 실행합니다.
    
        <copy>export USE_NATIVE_IMAGE_JAVA_PLATFORM_MODULE_SYSTEM=false
        mvn clean package -Pnative-image</copy>
        
    
    아래와 유사한 출력이 표시됩니다.
    
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
        
        

## 작업 2: Helidon MP 애플리케이션 GraalVM 네이티브 이미지 실행

1.  동일한 터미널에서 다음 명령을 복사하여 붙여넣어 이 응용 프로그램을 실행합니다.
    
        <copy>./target/myproject</copy>
        
    
    ![고유 이미지 실행](images/run-native.png)
    
    > 응용 프로그램 시작 시간이 142밀리초인 것을 확인할 수 있습니다. 이 시간은 거의 35배 빠릅니다.
    
2.  _"./target/myproject" 명령이 실행 중인 터미널에 `Ctrl + C`를 입력하여 **myproject** 응용 프로그램을 중지합니다_. 그것은 매우 중요하다, 다른 경우 당신은 실험실에서 문제를 직면하게됩니다.
    

## 확인

*   **작성자** - Dmitry Aleksandrov
*   **기여자** - Ankit Pandey, Maciej Gruszka
*   **최종 업데이트 수행자/날짜** - Ankit Pandey, 2023년 4월