# Generar un proyecto de Helidon MP y ejecutar el proyecto en Code Editor

## Introducción

En este laboratorio se muestran los pasos para crear una aplicación de MP de Helidon.

Tiempo estimado: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Generación de un proyecto de Helidon MP y ejecución del proyecto en el editor de códigos](videohub:1_22nv8v4q)

### Acerca de Producto/Tecnología

Helidon está diseñado para ser fácil de usar, con herramientas y ejemplos para ponerte en marcha rápidamente. Dado que Helidon es solo una colección de bibliotecas que se ejecutan en un núcleo de Netty rápido, no hay sobrecarga o hinchazón adicional. Helidon soporta MicroProfile y proporciona API conocidas como JAX-RS, CDI y JSON-P/B. Nuestra implantación MicroProfile se ejecuta en nuestro rápido Helidon Reactive WebServer. WebServer reactivo proporciona un modelo de programación moderno y funcional y se ejecuta sobre Netty. Ligero, flexible y reactivo, Helidon WebServer proporciona una base fácil de usar y rápida para sus microservicios.

Con soporte para comprobaciones del sistema, métricas, rastreo y tolerancia a fallos, Helidon tiene lo que necesita para escribir aplicaciones listas para la nube que se integren con Prometheus, Jaeger/Zipkin y Kubernetes.

### Acerca de Helidon Project Starter

Project Starter es una nueva interfaz de usuario web para crear proyectos de Helidon. Es altamente personalizable, proporcionando varias opciones que permiten a los usuarios seleccionar las funciones de Helidon que desean agregar al proyecto. Los usuarios finales podrán generar proyectos según sus necesidades específicas. Para obtener más información, haga clic en [Helidon Starter](https://helidon.io/starter).

### Acerca de Code Editor

El editor de códigos permite editar y desplegar código para varios servicios de OCI directamente desde la consola de OCI. Ahora puede actualizar scripts y flujos de trabajo de servicio sin tener que cambiar entre la consola y los entornos de desarrollo locales. Esto facilita la creación rápida de prototipos de soluciones en la nube, la exploración de nuevos servicios y el cumplimiento de tareas de codificación rápidas.

La integración directa del editor de códigos con Cloud Shell permite acceder a GraalVM Enterprise Native Image y JDK 17 (Java Development Kit) preinstalados en Cloud Shell.

### Acerca de OCI Cloud Shell

[OCI Cloud Shell](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm) es un terminal basado en explorador al que se puede acceder desde la consola de Oracle Cloud. Proporciona acceso a un shell de Linux con una interfaz de línea de comandos (CLI) de OCI autenticada previamente y herramientas de desarrollador preinstaladas, e incluye 5 GB de almacenamiento.

A partir de la versión 22.2.0, GraalVM Enterprise JDK 17 y Native Image están preinstalados en Cloud Shell.

> GraalVM Enterprise está disponible en Oracle Cloud Infrastructure sin costo adicional.

### Objetivos

*   Cree un microservicio soportado MicroProfile denominado aplicación Helidon Greeting
*   Abrir aplicación Helidon en el editor de códigos
*   Cambiar el JDK por defecto en Cloud Shell
*   Configurar el Maven necesario
*   Ejecutar y ejecutar la aplicación Helidon Greeting

### Requisitos

*   Debe tener una cuenta activada de [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Tarea 1: Generar proyecto de Helidon con el iniciador del proyecto

1.  Copie la siguiente URL y péguela en el explorador para abrir la página Proyecto de Helidon.
    
        <copy>https://helidon.io/starter/</copy>
        
2.  En Generar el proyecto, seleccione _Helidon MP_ como sabor de Helidon y, a continuación, haga clic en _Siguiente_.
    
3.  En Tipo de aplicación, seleccione _Inicio rápido_ y, a continuación, haga clic en _Siguiente_.
    
4.  Para Soporte de medios, seleccione _JSON-B_ y, a continuación, haga clic en _Siguiente_.
    
5.  Para Personalizar proyecto, seleccione los valores por defecto y haga clic en _Descargas_. Esto aparecerá en una ventana y guarde _myproject.zip_ en la ubicación que desee. En el resto de este taller, se utilizará el nombre de myproject. Si elige un nombre diferente, cambie respectivamente.
    

## Tarea 2: Construir y ejecutar el proyecto helidon localmente

1.  En la consola en la nube, haga clic en el icono _Herramientas de desarrollador_ como se muestra y, a continuación, haga clic en _Editor de códigos_. ![editor de código](images/code-editor.png)
    
2.  En el editor de códigos, haga clic en _Terminal_ -> _New Terminal_. ![abrir terminal](images/open-terminal.png)
    
3.  Copie y pegue el siguiente comando en el terminal para crear la carpeta _myproject_, donde descargaremos el archivo myproject.zip predeterminado.
    
        <copy>mkdir -p ~/helidon-project/myproject</copy>
        
4.  En el editor de códigos, haga clic en _Archivo_ -> _Abrir_. ![Abrir carpeta](images/open-folder.png)
    
5.  Seleccione la carpeta _helidon-project_ y haga clic en _Open_. ![Abrir Helidon](images/open-helidon.png)
    
6.  En el editor de códigos, en la ventana EXPLORER, puede ver _HELIDON-PROJECT_. Puede ver la carpeta _myproject_ aquí y hacer clic en ella. Ahora, haga clic en _Archivo_ -> _Cargar archivos..._ y, a continuación, especifique la ubicación donde ha descargado el proyecto, seleccione el archivo zip y haga clic en _Abrir_. ![miproyecto](images/myproject.png) ![carga de archivos](images/upload-files.png)
    
7.  Copie y pegue el siguiente comando para descomprimir el archivo _myproject.zip_.
    
        <copy>cd ~/helidon-project/myproject
        unzip myproject.zip
        </copy>
        
8.  Expanda la carpeta _myproject_ para ver la estructura del proyecto. ![Expandir proyecto](images/expand-project.png)
    
9.  Para ejecutar este proyecto utilizaremos Maven 3.8+ y JDK 17+. En la nube de Oracle, dispone de varios JDK. Aquí seleccionaremos GraalVM JDK. Copie y pegue el siguiente comando en el terminal para conocer el JDK por defecto.
    
        <copy>csruntimectl java list</copy>
        
    
    ![mostrar JDK](images/list-jdk.png)
    
    > El JDK con \* _asterisco_ al principio es el JDK por defecto. Si tiene otro JDK, graalvmeejdk, cambie la versión de JDK por defecto ejecutando el siguiente comando. Utilice la versión mostrada de graalvmeejdk, ya que puede ser diferente a lo que se muestra en el comando.
    
        <copy>csruntimectl java set graalvmeejdk-17</copy>
        
10.  Para configurar el maven necesario, copie y pegue el siguiente comando en el terminal.
    
        <copy>cd ~/helidon-project/myproject/
        wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz
        tar -xzvf apache-maven-3.8.8-bin.tar.gz
        PATH=~/helidon-project/myproject/apache-maven-3.8.8/bin:$PATH</copy>
        
    
    ![configurar maven](images/configure-maven.png)
    
11.  Para verificar que tiene la versión correcta de JDK y Maven, como se muestra a continuación, ejecute el siguiente comando en el terminal.
    
        <copy>mvn -v</copy>
        
    
    ![verificar requisito](images/verify-prerequisite.png)
    
12.  En la carpeta _myproject_, ejecute el siguiente comando para crear el proyecto.
    
        <copy>cd ~/helidon-project/myproject/myproject
        mvn clean package</copy>
        
    
    ![crear proyecto](images/build-project.png)
    
    > Debe ver _BUILD SUCCESS_ al final de la ejecución de este comando.
    
13.  Copie y pegue el siguiente comando en el terminal para ejecutar esta aplicación. Verá una salida similar a la que se muestra en la siguiente captura de pantalla.
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    ![ejecutar proyecto](images/run-project.png)
    

> Anote la hora de inicio, es de 4822 milisegundos. compararemos esta vez con el ejecutable de imagen nativa más adelante.

14.  Abra un nuevo terminal/consola en Code Editor y ejecute los siguientes comandos para comprobar la aplicación:
    
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
        
15.  _Para detener la aplicación **myproject**, introduzca `Ctrl + C` en el terminal donde se ejecuta el comando "java -jar target/myproject.jar"_. ES MUY IMPORTANTE, DE OTRA MANERA USTED ENFRENTARÁ CUESTIONES EN LA LAB LATERAL.
    

## Reconocimientos

*   **Autor**: Dmitry Aleksandrov
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, abril de 2023