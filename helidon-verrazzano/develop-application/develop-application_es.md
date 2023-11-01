# Desarrollo de aplicaciones de Helidon

## Introducción

En este laboratorio se muestran los pasos para crear una aplicación de MP de Helidon.

Tiempo estimado: 15 minutos

### Acerca de Producto/Tecnología

Helidon está diseñado para ser fácil de usar, con herramientas y ejemplos para ponerte en marcha rápidamente. Dado que Helidon es solo una colección de bibliotecas que se ejecutan en un núcleo de Netty rápido, no hay sobrecarga o hinchazón adicional. Helidon soporta MicroProfile y proporciona API conocidas como JAX-RS, CDI y JSON-P/B. Nuestra implantación MicroProfile se ejecuta en nuestro rápido Helidon Reactive WebServer. WebServer reactivo proporciona un modelo de programación moderno y funcional y se ejecuta sobre Netty. Ligero, flexible y reactivo, Helidon WebServer proporciona una base fácil de usar y rápida para sus microservicios.

Con soporte para comprobaciones del sistema, métricas, rastreo y tolerancia a fallos, Helidon tiene lo que necesita para escribir aplicaciones listas para la nube que se integren con Prometheus, Jaeger/Zipkin y Kubernetes.

### Acerca de la CLI de Helidon

La CLI de Helidon le permite crear fácilmente un proyecto de Helidon mediante la selección de un conjunto de arquetipos. También soporta un bucle de desarrollador que realiza la compilación continua y el reinicio de la aplicación, por lo que puede iterar fácilmente sobre los cambios de código fuente.

La CLI se distribuye como un ejecutable independiente (compilado con GraalVM) para facilitar la instalación. Actualmente está disponible como descarga para Linux, Mac y Windows. Simplemente descargue el binario, instálelo en una ubicación accesible desde su RUTA DE ACCESO y estará listo para continuar.

### Objetivos

*   Instalación de la CLI de Helidon
*   Cree un microservicio soportado por MicroProfile denominado Helidon Greeting.
*   Ejecutar y ejecutar la aplicación Helidon Greeting
*   Ver datos de estado y métricas
*   Agregar nueva funcionalidad a la aplicación

### Requisitos

*   Helidon necesita Java 11+
*   Maven 3.6.x

> **Precaución: No utilice la versión 3.8.x debido a un problema conocido con la creación de la aplicación.**

*   Java y `mvn` están en su ruta de acceso.
*   Los usuarios de Windows también necesitarán el tiempo de ejecución de Visual C++ Redistributable.  
    Consulte [Helidon on Windows](https://helidon.io/docs/v2/#/about/04_windows) para obtener más información.

## Tarea 1: Instalación de la CLI de Helidon

1.  Instalación de la CLI de Helidon
    
    Para macOS:
    
        <copy>
        curl -O https://helidon.io/cli/latest/darwin/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Para Linux:
    
        <copy>
        curl -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Para Windows:
    
        <copy>
        PowerShell -Command Invoke-WebRequest -Uri "https://helidon.io/cli/latest/windows/helidon.exe" -OutFile "C:\Windows\system32\helidon.exe"
        </copy>
        

## Tarea 2: Crear aplicación de saludo de Helidon

1.  En la consola, introduzca:
    
        <copy>helidon init --version 2.4.0 </copy>
        
    
    > Para evitar posibles problemas, defina la versión de Helidon específica que se probó para el entorno de este laboratorio.
    
2.  Para esta demostración, crearemos un microservicio soportado por MicroProfile, así que seleccione la opción **(2)** para **Helidon MP Flavor**:
    
        Helidon flavor
        (1) SE 
        (2) MP 
        Enter selection (Default: 1): 2
        
3.  Para obtener la mayor funcionalidad, seleccione la opción **(2) inicio rápido** y, a continuación, **Intro** para las respuestas por defecto. Tenga en cuenta que puede tener diferentes nombres de paquetes y grupos de proyectos por defecto porque utiliza el nombre de usuario del sistema operativo. Anote el nombre del paquete, tendrá que utilizarlo al crear una nueva clase Java.
    
        Select archetype
        (1) bare | Minimal Helidon MP project suitable to start from scratch 
        (2) quickstart | Sample Helidon MP project that includes multiple REST operations 
        (3) database | Helidon MP application that uses JPA with an in-memory H2 database 
        Enter selection (Default: 1): 2
        Project name (Default: quickstart-mp): 
        Project groupId (Default: me.user-helidon): 
        Project artifactId (Default: quickstart-mp): 
        Project version (Default: 1.0-SNAPSHOT): 
        Java package name (Default: me.user_name.mp.quickstart): 
        Switch directory to /home/user/quickstart-mp to use CLI
        
        Start development loop? (Default: n):
        $
        
    
    > Para el **bucle de desarrollo**, acepte el valor por defecto (**n**) por ahora. Iniciará el bucle de desarrollo más adelante en esta práctica.
    
    Ahora tiene un proyecto de Maven de microservicio totalmente funcional:
    

        quickstart-mp
        ├── Dockerfile
        ├── Dockerfile.jlink
        ├── Dockerfile.native
        ├── README.md
        ├── app.yaml
        ├── pom.xml
        └── src
            ├── main
            │   ├── java
            │   │   └── me
            │   │       └── buzz
            │   │           └── mp
            │   │               └── quickstart
            │   │                   ├── GreetResource.java
            │   │                   ├── GreetingProvider.java
            │   │                   └── package-info.java
            │   └── resources
            │       ├── META-INF
            │       │   ├── beans.xml
            │       │   ├── microprofile-config.properties
            │       │   └── native-image
            │       │       └── reflect-config.json
            │       └── logging.properties
            └── test
                └── java
                    └── me
                        └── buzz
                            └── mp
                                └── quickstart
                                    └── MainTest.java
    
    

## Tarea 3: Ejecución de la aplicación de saludo de Helidon

1.  Desde la misma consola/terminal, navegue hasta el directorio quickstart-mp y ejecute los siguientes comandos:
    
        <copy> cd quickstart-mp
        </copy>
        
2.  Con JDK11+
    
        <copy>
        mvn package
        java -jar target/quickstart-mp.jar
        </copy>
        

### Ejercicio de la aplicación

1.  Abra un terminal o una consola nuevos y ejecute los siguientes comandos para comprobar la aplicación:
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        

### Revisión de datos de métricas y estado

1.  En el mismo terminal/consola, ejecute los siguientes comandos para comprobar el estado y las métricas:
    
        <copy>
        curl -s -X GET http://localhost:8080/health
        </copy>
        {"outcome":"UP",...
        . . .
        
    
        # Prometheus Format
        <copy>
        curl -s -X GET http://localhost:8080/metrics
        </copy>
        # TYPE base:gc_g1_young_generation_count gauge
        . . .
        
    
        # JSON Format
        <copy>
        curl -H 'Accept: application/json' -X GET http://localhost:8080/metrics
        </copy>
        {"base":...
        . . .
        
2.  Para detener la aplicación _quickstart-mp_, introduzca `Ctrl + C` en el terminal donde se ejecuta el comando "java -jar target/quickstart-mp.jar".
    

## Tarea 4: Modificación de la aplicación

1.  Abra su IDE favorito y vaya al archivo **microprofile-config.properties**.
    
    ![Archivo de Configuración](images/config.jpg)
    
2.  En la consola/terminal, navegue hasta la carpeta del proyecto e introduzca:
    
        <copy>helidon dev</copy>
        
    
    > Esto iniciará el **bucle de desarrollo** mencionado en la tarea anterior.
    
3.  Cambie la propiedad _app.greeting_ a "Hola, Oracle" y guarde el archivo.
    
        <copy>app.greeting=Hello Oracle</copy>
        
    
    ![HelidonDev](images/properties.jpg)
    
    > Verá que cada vez que cambie un archivo, la **CLI de Helidon** reconoce que hay un cambio, recompila la aplicación y la vuelve a ejecutar. Dado que Helidon es pequeño, todo sucede rápidamente.
    
4.  En la consola o el terminal, introduzca lo siguiente:
    
        <copy>curl -X GET http://localhost:8080/greet</copy>
        
    
    Se espera que el resultado sea:
    
        {"message":"Hello Oracle World!"}
        
    
    > Asegúrese de parar el bucle de desarrollo con `CTRL+C`
    
5.  Vuelva a la carpeta del proyecto y abra el archivo **GreetResource.java**.
    
    > Puede ver que es puro código compatible con MicroProfile:
    
    ![ModifyJava](images/greet-resource.jpg)
    
6.  Cree un nuevo punto final que proporcione ayuda para diferentes saludos en diferentes idiomas. Para crear esta nueva funcionalidad, cree una nueva clase denominada **GreetHelpResource** con el siguiente código:
    
        <copy>
        package me.user_name.me.quickstart;
        import java.util.Arrays;
        import java.util.List;
        import java.util.logging.Logger;
        
        import javax.enterprise.context.ApplicationScoped;
        import javax.ws.rs.GET;
        import javax.ws.rs.Path;
        
        import org.eclipse.microprofile.metrics.annotation.Counted;
        
        @ApplicationScoped
        @Path("/help")
        public class GreetHelpResource {
        
            Logger LOGGER = Logger.getLogger(GreetHelpResource.class.getName());
        
            @GET
            @Path("/allGreetings")
            @Counted(name = "helpCalled", description = "How many time help was called")
            public String getAllGreetings(){
                LOGGER.info("Help requested!");
                return Arrays.toString(List.of("Hello","Привет","Hola","Hallo","Ciao","Nǐ hǎo", "Marhaba","Olá").toArray());
            }
        }
        </copy>
        
    
    > La clase solo tiene un método _getAllGreetings_ que devuelve una lista de saludos en diferentes idiomas. Al copiar el código, asegúrese de agregar el nombre de paquete necesario encima de la clase.
    
7.  Cree y ejecute la aplicación:
    
        <copy>
        mvn package -DskipTests
        java -jar target/quickstart-mp.jar
        </copy>
        
8.  Ejecute el siguiente comando y observe los resultados:
    
        <copy>curl http://localhost:8080/help/allGreetings</copy>
        
    
    El resultado esperado:
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
9.  Mira las métricas y verás que apareció un nuevo contador. Siempre que se llame a este punto final, el valor aumentará:
    
        curl http://localhost:8080/metrics
        
        ...
        # TYPE application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total counter
        # HELP application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total How many time help was called
        application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total 1
        ...
        
10.  En la consola, ahora verá la línea de log INFO sobre esta llamada:
    
        INFO me.buzz.mp.quickstart.GreetHelpResource Thread[helidon-4,5,server]: Help requested!
        
    
    Y se ha añadido el nuevo punto final.
    
    ![NewEndpoint](images/logs-output.jpg)
    
    > Trabajar con Helidon y sus herramientas es fácil y rápido.
    
11.  Deje su terminal/consola abierta y continúe con el laboratorio de instalación de Verrazzano.
    

## Reconocimientos

*   **Autor**: Dmitry Aleksandrov
*   **Contribuyentes**: Maciej Gruszka, Ankit Pandey
*   **Última actualización por/fecha**: Ankit Pandey, marzo de 2023