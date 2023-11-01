# Criar imagem nativa do Aplicativo MP Helidon

## Introdução

Neste laboratório, você aprenderá a criar uma imagem nativa GraalVM para um aplicativo MP Helidon.

Tempo Estimado: 15 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Criar imagem nativa do Aplicativo Helidon MP](videohub:1_0hftfgfy)

### Sobre a imagem nativa GraalVM

GraalVM é uma distribuição JDK de alto desempenho que pode acelerar qualquer carga de trabalho Java em execução na JVM HotSpot.

A compilação GraalVM Native Image antecipadamente permite que você crie aplicativos Java leves que são menores, mais rápidos e usam menos memória e CPU. No momento da criação, o GraalVM Native Image analisa um aplicativo Java e suas dependências para identificar exatamente quais classes, métodos e campos são necessários e gera código de máquina otimizado apenas para esses elementos.

GraalVM A Enterprise Edition está disponível para uso no OCI (Oracle Cloud Infrastructure) sem custo adicional.

### Objetivos

*   Crie um executável nativo do aplicativo MP Helidon com uma instalação local de GraanVM
*   Executar e exercer o aplicativo Helidon Greeting

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Tarefa 1: Criar a imagem nativa do aplicativo Helidon MP GraalVM

1.  Para criar uma imagem Nativa GraalVM com o mesmo aplicativo, execute o seguinte comando no terminal, onde você configurou o maven e o JDK.
    
        <copy>export USE_NATIVE_IMAGE_JAVA_PLATFORM_MODULE_SYSTEM=false
        mvn clean package -Pnative-image</copy>
        
    
    você verá uma saída semelhante à abaixo.
    
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
        
        

## Tarefa 2: Executar a imagem nativa GraalVM do aplicativo Helidon MP

1.  Copie e cole o comando a seguir no mesmo terminal para executar este aplicativo
    
        <copy>./target/myproject</copy>
        
    
    ![executar imagem nativa](images/run-native.png)
    
    > Você pode ver que o tempo de inicialização do aplicativo é de 142 milissegundos, o que é quase 35 vezes mais rápido.
    
2.  _Interrompa o aplicativo **myproject** digitando `Ctrl + C` no terminal em que o comando "./target/myproject" está sendo executado_. É MUITO IMPORTANTE, OUTRAS QUAISQUER QUARIAS NA LABEÇA.
    

## Confirmação

*   **Autor** - Dmitry Aleksandrov
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, abril de 2023