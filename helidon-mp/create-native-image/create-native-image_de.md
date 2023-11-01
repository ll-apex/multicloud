# Natives Image der Helidon MP-Anwendung erstellen

## Einführung

In dieser Übung lernen Sie, wie Sie ein natives GraalVM-Image für eine Helidon-MP-Anwendung erstellen.

Geschätzte Zeit: 15 Minuten

Sehen Sie sich das Video unten an, um einen schnellen Durchgang des Labors zu erhalten. [Natives Image der Helidon MP-Anwendung erstellen](videohub:1_0hftfgfy)

### Informationen zum nativen Image GraalVM

GraalVM ist eine leistungsstarke JDK-Distribution, die jede Java-Workload, die auf der HotSpot JVM ausgeführt wird, beschleunigen kann.

Mit der GraalVM Native Image-Ahead-of-Time-Kompilierung können Sie leichte Java-Anwendungen erstellen, die kleiner, schneller sind und weniger Speicher und CPU benötigen. Zum Zeitpunkt der Erstellung analysiert GraalVM Native Image eine Java-Anwendung und ihre Abhängigkeiten, um genau zu identifizieren, welche Klassen, Methoden und Felder erforderlich sind, und generiert optimierten Maschinencode für diese Elemente.

GraalVM Enterprise Edition kann ohne zusätzliche Kosten in Oracle Cloud Infrastructure (OCI) verwendet werden.

### Ziele

*   Erstellen Sie eine native ausführbare Helidon MP-Anwendung mit einer lokalen Installation von GraanVM
*   Ausführen und Ausführen der Helidon-Gruß-App

### Voraussetzungen

*   Sie müssen über einen [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure)\-fähigen Account verfügen.

## Aufgabe 1: Natives Image der Helidon MP-Anwendung GraalVM erstellen

1.  Um ein natives GraalVM-Image mit derselben Anwendung zu erstellen, führen Sie den folgenden Befehl im Terminal aus, in dem Sie Maven und JDK konfiguriert haben.
    
        <copy>export USE_NATIVE_IMAGE_JAVA_PLATFORM_MODULE_SYSTEM=false
        mvn clean package -Pnative-image</copy>
        
    
    Sie sehen eine ähnliche Ausgabe wie unten.
    
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
        
        

## Aufgabe 2: Natives Image der Helidon MP-Anwendung GraalVM ausführen

1.  Kopieren Sie den folgenden Befehl, und fügen Sie ihn in dasselbe Terminal ein, um diese Anwendung auszuführen
    
        <copy>./target/myproject</copy>
        
    
    ![Native-Image ausführen](images/run-native.png)
    
    > Sie können sehen, dass die Startzeit der Anwendung 142 Millisekunden beträgt, was fast 35-mal schneller ist.
    
2.  _Stoppen Sie die Anwendung **myproject**, indem Sie im Terminal, IN dem der Befehl "./target/myproject" ausgeführt wird, `Ctrl + C` eingeben_. ES IST SEHR WICHTIG, SONSTIG WERDEN SIE IM LABELRECHT GESICHTEN.
    

## Danksagungen

*   **Autor** - Dmitry Aleksandrov
*   **Mitwirkende** - Ankit Pandey, Maciej Gruszka
*   **Zuletzt aktualisiert am/um** - Ankit Pandey, April 2023