# Creación y ejecución de la aplicación Helidon Nima y Reactive

## Introducción

Este laboratorio le guiará por el proceso de creación y ejecución de aplicaciones de Nima y Helidon reactivas en Oracle Code Editor dentro de Oracle Cloud Infrastructure.

[Recorrido virtual por Lab2](videohub:1_e88ydqwt)

Tiempo estimado: 15 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear, ejecutar y probar la aplicación Helidon Nima
*   Creación, ejecución y prueba de la aplicación Helidon Reactive
*   Analizar la simplicidad de la aplicación Nima en comparación con la aplicación Reactiva

### Requisitos

Para ejecutar este laboratorio, debe tener:

*   Cuenta de Oracle Cloud
*   Laboratorio 1 completado, que instala el JDK y maven necesarios.

## Tarea 1: Crear y ejecutar la aplicación Nima

1.  Haga clic en _File_ -> _Open_ en el editor de códigos. ![Abrir proyecto](images/open-project.png)
    
2.  Seleccione la carpeta _helidon-levelup-2023-main_ y haga clic en _Open_ (Abrir). ![Proyecto Helidon](images/helidon-project.png)
    
    > Ignore las advertencias/problemas, observará en el editor de códigos al abrir esta carpeta.
    
3.  Abra un nuevo terminal y, a continuación, copie y pegue el siguiente comando para definir la variable PATH y JAVA\_HOME.
    
        <copy>PATH=~/jdk-19.0.2/bin:~/apache-maven-3.8.3/bin:$PATH
        JAVA_HOME=~/jdk-19.0.2</copy>
        
4.  Copie el siguiente comando y ejecútelo en el terminal para verificar que la versión de JDK y Maven necesaria esté configurada correctamente.
    
        <copy>mvn -v</copy>
        
    
    tendrá una salida similar a la siguiente:
    
        $ mvn -v 
        Apache Maven 3.8.3 (ff8e977a158738155dc465c6a97ffaf31982d739)
        Maven home: /home/ankit_x_pa/apache-maven-3.8.3
        Java version: 19.0.2, vendor: Oracle Corporation, runtime: /home/ankit_x_pa/jdk-19.0.2
        Default locale: en_US, platform encoding: UTF-8
        OS name: "linux", version: "4.14.35-2047.520.3.1.el7uek.x86_64", arch: "amd64", family: "unix"
        $ 
        
    
    > _Solo utilizará este terminal para crear y ejecutar la aplicación, ya que tiene la versión de JDK y Maven necesaria._
    
5.  Copie y pegue el siguiente comando para crear la aplicación nima.
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-blocking ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/nima/target/example-nima-blocking.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  7.562 s
        [INFO] Finished at: 2023-02-27T11:42:52Z
        [INFO] ------------------------------------------------------------------------
        
6.  Copie y pegue el siguiente comando en el terminal para ejecutar la versión nima de bloqueo de la aplicación:
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.27 11:43:18.589 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:43:18.902 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:43:19.700 [0x314d2373] http://127.0.0.1:33193 bound for socket '@default'
        2023.02.27 11:43:19.703 [0x314d2373] direct writes
        2023.02.27 11:43:19.722 Helidon Níma 4.0.0-ALPHA5
        
7.  Anote el número de puerto en el que se ejecuta el servidor (consulte la entrada de log para @default). Por ejemplo, en nuestra salida, el número de puerto es 33193. Del mismo modo, averigüe el número de puerto del servidor.
    
8.  Para abrir un nuevo terminal, haga clic en _Terminal_ -> _New Terminal_. Utilizaremos este terminal para ejecutar comandos _curl_. ![Nuevo terminal](images/new-terminal.png)
    
9.  Copie y pegue el siguiente comando en el nuevo terminal. No olvide sustituir _`<port>`_ por el puerto del servidor.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        curl http://localhost:33193/one
        remote_1
        $
        
10.  Copie y pegue el siguiente comando en el nuevo terminal. No olvide sustituir _`<port>`_ por el puerto del servidor.
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        $ curl http://localhost:33193/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > Observe el orden de los resultados. Como sugiere el nombre, esta solicitud llama a un recurso remoto varias veces en secuencia.
    
11.  Copie y pegue el siguiente comando en el nuevo terminal. No olvide sustituir _`<port>`_ por el puerto del servidor.
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        $ curl http://localhost:33193/parallel
        Combined results: [remote_21, remote_18, remote_12, remote_13, remote_14, remote_15, remote_16, remote_17, remote_19, remote_20]
        $
        
    
    > Observe el orden de los resultados. Como sugiere el nombre, esta solicitud llama a un recurso remoto varias veces en paralelo.
    
12.  Pulse _Ctrl + C_ en el terminal donde se ejecuta el comando \*java -jar \* para detener el servidor.
    

## Tarea 2: Creación y ejecución de la aplicación reactiva

1.  Copie y pegue el siguiente comando para crear la aplicación nima.
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-reactive ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/reactive/target/example-nima-reactive.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  6.196 s
        [INFO] Finished at: 2023-02-27T11:51:17Z
        [INFO] ------------------------------------------------------------------------
        $
        
2.  Copie y pegue el siguiente comando en el terminal para ejecutar la versión reactiva de la aplicación:
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.27 11:51:26.227 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:51:26.723 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:51:27.286 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.27 11:51:27.618 Channel '@default' started: [id: 0x53fadd43, L:/0.0.0.0:45765]
        
3.  Anote el número de puerto en el que se ejecuta el servidor (consulte la entrada de log para @default). Por ejemplo, en nuestra salida, el número de puerto es 45765. Del mismo modo, averigüe el número de puerto del servidor.
    
4.  Vuelva al terminal, que abrimos en la Tarea 1 para ejecutar el comando curl.
    
5.  Copie y pegue el siguiente comando en el nuevo terminal. No olvide sustituir _`<port>`_ por el puerto del servidor.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        $ curl http://localhost:45765/one
        remote_1
        $
        
6.  Copie y pegue el siguiente comando en el nuevo terminal. No olvide sustituir _`<port>`_ por el puerto del servidor.
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        $ curl http://localhost:45765/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > Observe el orden de los resultados. Como sugiere el nombre, esta solicitud llama a un recurso remoto varias veces en secuencia.
    
7.  Copie y pegue el siguiente comando en el nuevo terminal. No olvide sustituir _`<port>`_ por el puerto del servidor.
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    Tendrá una salida similar a la siguiente:
    
        $ curl http://localhost:45765/parallel
        Combined results: [remote_17, remote_16, remote_13, remote_20, remote_12, remote_19, remote_18, remote_14, remote_21, remote_15]
        $
        
    
    > Observe el orden de los resultados. Como sugiere el nombre, esta solicitud llama a un recurso remoto varias veces en paralelo.
    
8.  Pulse _Ctrl + C_ en el terminal donde se ejecuta el comando \*java -jar \* para detener el servidor.
    

## Tarea 3: Analizar la simplicidad de la aplicación Nima

**Bloqueo frente a reactivo**

Comparemos las implementaciones entre Níma (bloqueo) y Helidon SE (reactivo) para la misma tarea.

*   Ambas implantaciones ejecutan llamadas REST mediante Helidon WebClient
*   La implementación de bloqueo es fácil de seguir:
    *   El flujo de ejecución se refleja en el orden de las sentencias en el código
    *   No hay llamadas a llamadas a bibliotecas complejas o semánticamente ricas
    *   La depuración es sencilla
*   Las versiones reactivas requieren una buena comprensión de las bibliotecas reactivas
    *   Incluyendo Multi, flatMap, manejo de errores, etc.
    *   El flujo de control ya no es obvio debido al uso de manejadores reactivos
    *   Es necesario comprender la capacidad de flatMap para activar/borrar la simultaneidad
    *   La depuración es más difícil

1.  Abra el archivo _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ para ver cómo se implementan los puntos finales en la versión nima de la aplicación. ![Nima Block](images/nima-block.png)
    
2.  Abra el archivo _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ para ver cómo se implementan los puntos finales en la versión reactiva de la aplicación. ![Servicio reactivo](images/reactive-service.png)
    
3.  En la sección _Abrir editores_, haga clic con el botón derecho en el archivo _BlockingService.java_ y seleccione _Seleccionar para comparar_. ![Seleccionar Comparar](images/select-compare.png)
    
4.  Haga clic con el botón derecho en el archivo _ReactiveService.java_ y seleccione _Comparar con seleccionado_. ![Comparar seleccionadas](images/compare-selected.png)
    
5.  Ver que el código reactivo es más complicado que el bloqueo (subproceso virtual) ![Comparar código](images/compare-code.png)
    
    > Compruebe los métodos uno, secuencia y paralelo en BlockingService y ReactiveService respectivamente. ¡Mira si entiendes cómo funcionan!
    

Ahora puede _proceder al siguiente laboratorio_.

## Reconocimientos

*   **Autor**: Joe DiPol
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/fecha**: Ankit Pandey, febrero de 2023