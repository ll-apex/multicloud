# Crear una aplicación de Helidon 3 y, a continuación, migrarla a Helidon 4

## Introducción

En este laboratorio, comenzará con una aplicación Helidon 3 que se ejecuta en nuestro servidor web reactivo original basado en Netty. A continuación, migrará la aplicación a Helidon 4 que se ejecuta en el nuevo Helidon Nima WebServer mediante threads virtuales.

[Recorrido virtual por Lab4](videohub:1_zr1m00ba)

Tiempo estimado: 20 minutos

### Objetivos

*   Genere, cree y ejecute una aplicación MicroProfile de Helidon mediante Helidon Starter.
*   Migre la aplicación de microperfil Helidon 3 a Helidon 4

### Requisitos

*   Cuenta de Oracle Cloud

## Tarea 1: Creación de una aplicación de Helidon 3 y creación de la aplicación

1.  Copie la siguiente URL y péguela en el explorador para abrir la página Proyecto de Helidon.
    
        <copy>https://helidon.io/starter/</copy>
        
2.  En Generar el proyecto, seleccione _Helidon MP_ como sabor de Helidon y, a continuación, haga clic en _Siguiente_.
    
3.  En Tipo de aplicación, seleccione _Inicio rápido_ y, a continuación, haga clic en _Siguiente_.
    
4.  Para Soporte de medios, seleccione _Jackson_ y, a continuación, haga clic en _Next_ (Siguiente).
    
5.  Para Personalizar proyecto, seleccione los valores por defecto y haga clic en _Descargas_. Esto aparecerá en una ventana y guarde _myproject.zip_ en la ubicación que desee. En el resto de este taller, se utilizará el nombre de _myproject_. Si selecciona un nombre diferente, cambie respectivamente.
    
6.  Vuelva al editor de códigos, en HELIDON-LEVELUP-2023-MAIN y haga clic en _docs_. ![Seleccionar documentos](images/select-docs.png)
    
7.  Haga clic en _Archivo_ -> _Cargar archivos_ y seleccione _myproject.zip_ en la ubicación donde ha guardado este archivo anteriormente y, a continuación, haga clic en _Abrir_. ![Carga de Archivos](images/upload-files.png)
    
8.  Verá el archivo _myproject.zip_ en la carpeta _docs_. ![Archivo zip](images/zip-file.png)
    
9.  copie y pegue el siguiente comando para descomprimir el archivo.
    
        <copy>cd ~/helidon-levelup-2023-main/docs/
        unzip myproject.zip</copy>
        
10.  Desde la carpeta myproject, ejecute el siguiente comando para crear el proyecto. Utilice el terminal, donde ha definido las variables PATH y JAVA\_HOME.
    
        <copy>cd myproject
        mvn clean package</copy>
        
    
    > Debe ver _BUILD SUCCESS_ al final de la ejecución de este comando.
    
11.  Copie y pegue el siguiente comando en el terminal para ejecutar esta aplicación. Verá una salida similar a la que se muestra en la siguiente captura de pantalla.
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    Aparecerá una pantalla similar a la siguiente:
    
        $ java -jar target/myproject.jar
        2023.02.28 03:48:36 INFO io.helidon.common.LogConfig Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:48:39 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:48:40 INFO io.helidon.webserver.NettyWebServer Thread[#31,nioEventLoopGroup-2-1,10,main]: Channel '@default' started: [id: 0x5b66bd2a, L:/0.0.0.0:8080]
        2023.02.28 03:48:41 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 4816 milliseconds (since JVM startup).
        2023.02.28 03:48:41 INFO io.helidon.common.HelidonFeatures Thread[#32,features-thread,5,main]: Helidon MP 3.1.2 features: [CDI, Config, Health, JAX-RS, Metrics, Open API, Server]
        
12.  Vuelva al terminal, desde donde ejecuta los comandos curl y ejecute los siguientes comandos para comprobar la aplicación:
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
13.  Para detener la aplicación _myproject_, introduzca `Ctrl + C` en el terminal donde se ejecuta el comando "java -jar target/myproject.jar".
    
14.  Edite el archivo _src/main/java/com/example/myproject/GreetResource.java_, busque el método _createResponse(String who)_ y agregue la siguiente línea como se muestra en la captura de pantalla.
    
        <copy>System.out.println("Running on thread " + Thread.currentThread());</copy>
        
    
    ![modificar aplicación](images/modify-application.png)
    
15.  Reconstruya, ejecute y ejerza la aplicación como se describe en los pasos 10, 11 y 12.
    
16.  Observe la salida del servidor en el terminal donde inició el servidor. Tenga en cuenta que el thread se denomina helidon-server-n. Se trata de un thread de plataforma tradicional en un pool de threads creado por Helidon para manejar solicitudes JAX-RS. Tendrá una salida de servidor similar a la siguiente:
    
        Running on thread Thread[#25,helidon-server-1,5,server]
        
17.  Para detener la aplicación _myproject_, introduzca `Ctrl + C` en el terminal donde se ejecuta el comando "java -jar target/myproject.jar".
    

## Tarea 2: Migrar la aplicación Helidom MicroProfile a Helidon 4

1.  Para myproject, abra el archivo _pom.xml_ y cambie el pom principal de _3.1.1_ a _4.0.0-ALPHA5_.
    
        <parent>
            <groupId>io.helidon.applications</groupId>
            <artifactId>helidon-mp</artifactId>
            <version>4.0.0-ALPHA5</version>
            <relativePath/>
        </parent>
        
    
    ![Versión Pom](images/pom-version.png)
    
2.  Edite src/main/resources/logging.properties y cambie _io.helidon.common.HelidonConsoleHandler_ a _io.helidon.logging.jul.HelidonConsoleHandler_. ![Editar registro](images/edit-logging.png)
    
3.  La aplicación se ha migrado a Helidon 4. Copie y pegue el siguiente comando para crear la aplicación.
    
        <copy>mvn clean package -DskipTests</copy>
        
4.  Copie y pegue el siguiente comando para ejecutar la aplicación.
    
        <copy>java --enable-preview  -jar target/myproject.jar</copy>
        
    
    Tendrá una salida similar a la siguiente.
    
        $ java --enable-preview  -jar target/myproject.jar
        2023.02.28 03:56:17 INFO io.helidon.logging.jul.JulProvider Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:56:21 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] http://0.0.0.0:8080 bound for socket '@default'
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] direct writes
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Started all channels in 19 milliseconds. 5131 milliseconds since JVM startup. Java 19.0.2+7-44
        2023.02.28 03:56:22 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 5144 milliseconds (since JVM startup).
        2023.02.28 03:56:22 INFO io.helidon.common.features.HelidonFeatures Thread[#28,features-thread,5,main]: Helidon MP 4.0.0-ALPHA5 features: [CDI, Config, Health, Metrics, Open API, Server, WebServer]
        
5.  Observe la salida del servidor y observe que el thread ahora es VirtualThread.
    
6.  Para detener la aplicación _myproject_, introduzca `Ctrl + C` en el terminal donde se ejecuta el comando "java --enable-preview -jar target/myproject.jar".
    

Enhorabuena, ha completado el taller de threads virtuales de Helidon.

## Reconocimientos

*   **Autor**: Joe DiPol
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/fecha**: Ankit Pandey, febrero de 2023