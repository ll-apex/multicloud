# Crear imagen nativa de la aplicación MP de Helidon

## Introducción

En este laboratorio, aprenderá a crear una imagen nativa GraalVM para una aplicación Helidon MP.

Tiempo estimado: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Crear imagen nativa de la aplicación MP de Helidon](videohub:1_0hftfgfy)

### Acerca de la imagen nativa GraalVM

GraalVM es una distribución de JDK de alto rendimiento que puede acelerar cualquier carga de trabajo de Java que se ejecute en la JVM HotSpot.

GraalVM La compilación anticipada de Native Image permite crear aplicaciones Java ligeras que son más pequeñas, más rápidas y utilizan menos memoria y CPU. En el momento de la creación, GraalVM Native Image analiza una aplicación Java y sus dependencias para identificar qué clases, métodos y campos son necesarios y genera código de máquina optimizado para solo esos elementos.

GraalVM Enterprise Edition está disponible para su uso en Oracle Cloud Infrastructure (OCI) sin costo adicional.

### Objetivos

*   Crear un ejecutable nativo de la aplicación Helidon MP con una instalación local de GraanVM
*   Ejecutar y ejecutar la aplicación Helidon Greeting

### Requisitos

*   Debe tener una cuenta activada de [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Tarea 1: Crear la imagen nativa GraalVM de la aplicación Helidon MP

1.  Para crear una imagen nativa GraalVM con la misma aplicación, ejecute el siguiente comando en el terminal, donde ha configurado maven y JDK.
    
        <copy>export USE_NATIVE_IMAGE_JAVA_PLATFORM_MODULE_SYSTEM=false
        mvn clean package -Pnative-image</copy>
        
    
    Verá una salida similar a la siguiente.
    
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
        
        

## Tarea 2: Ejecutar la imagen nativa GraalVM de la aplicación Helidon MP

1.  Copie y pegue el siguiente comando en el mismo terminal para ejecutar esta aplicación
    
        <copy>./target/myproject</copy>
        
    
    ![ejecutar imagen nativa](images/run-native.png)
    
    > Puede ver que el tiempo de inicio de la aplicación es de 142 milisegundos, que es casi 35 veces más rápido.
    
2.  _Para detener la aplicación **myproject**, introduzca `Ctrl + C` en el terminal donde se ejecuta el comando "./target/myproject"_. ES MUY IMPORTANTE, DE OTRA MANERA USTED ENFRENTARÁ CUESTIONES EN LA LAB LATERAL.
    

## Reconocimientos

*   **Autor**: Dmitry Aleksandrov
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, abril de 2023