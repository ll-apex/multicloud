# Modificación de la aplicación de libro de Bobby y creación de una nueva imagen de componente de aplicación

## Introducción

En este laboratorio, modificaremos bobbys-helidon-stock-application mediante _Cloud Shell_. Más adelante, crearemos una nueva imagen de Docker para la aplicación bobbys-helidon-stock. Esta imagen de aplicación bobbys-helidon-stock es un componente de la aplicación Libros de Bobby.

Tiempo estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Modificación de la aplicación bobbys-helidon-stock.
*   Cree una nueva imagen de Docker para bobbys-helidon-stock-application.

### Requisitos

Debe tener un editor de texto, donde puede pegar los comandos y las URL y modificarlas, según su entorno. A continuación, puede copiar y pegar los comandos modificados para ejecutarlos en _Cloud Shell_.

## Tarea 1: Modificar bobbys-helidon-stock-application

1.  Seleccione la ficha Libro de Bobby, haga clic en _Libros_ y, a continuación, haga clic en la imagen del libro _El Hobbit_, como se muestra a continuación:
    
    ![Haga clic en Libros](images/clickbooks.png " ")
    
    Muestra el nombre del libro con el formato _El Hobbit_, como se muestra en la imagen.
    
    ![El Hobbit](images/thehobbit.png " ")
    
2.  Queremos convertir el nombre del libro en letras mayúsculas (THE HOBBIT). Necesitamos descargar el código fuente de la aplicación Bobby's Books. Asegúrese de que está en la carpeta de inicio. Copie los siguientes comandos y péguelos en _Cloud Shell_.
    
        <copy>cd ~
        git clone -b v0.16.0 https://github.com/verrazzano/examples.git</copy>
        
    
    ![Repositorio de Clonaciones](images/clonerepository.png " ")
    
3.  Para ver los archivos dentro de la aplicación Bobby's Book, copie el siguiente comando y péguelo en _Cloud Shell_.
    
        <copy>ls -la ~/examples/bobs-books/</copy>
        
    
    La salida debe ser similar a la siguiente: `bash $ ls -la ~/examples/bobs-books/ total 16 drwxr-xr-x. 5 ankit_x_pa oci 100 Mar 21 12:14 . drwxr-xr-x. 7 ankit_x_pa oci 4096 Mar 21 12:14 .. drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobbys-books drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobs-bookstore-order-manager -rw-r--r--. 1 ankit_x_pa oci 942 Mar 21 12:14 README.md drwxr-xr-x. 4 ankit_x_pa oci 72 Mar 21 12:14 roberts-books $`
    
4.  Ahora, vamos a hacer cambios en el JAVA\_FILE relevante. Para abrir el archivo, copie el siguiente comando y péguelo en _Cloud Shell_.
    
        <copy>vi ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/src/main/java/org/books/bobby/BookResource.java</copy>
        
    
    ![Abrir Archivo](images/openfile.png " ")
    
5.  Pulse _i_ para modificar el código. Para agregar una nueva línea en la línea número 84, copie la siguiente línea y péguela en la línea número 84 como se muestra a continuación:
    
        <copy>optional.get().setTitle(optional.get().getTitle().toUpperCase());</copy>
        
    
    ![Insertar Línea](images/insertline.png " ")
    
6.  Pulse _Esc_ y, a continuación, _:wq_ para guardar los cambios.
    
    ![Guardar cambios](images/savechanges.png " ")
    

## Tarea 2: Crear una nueva imagen de Docker para la aplicación bobbys-helidon-stock

1.  Dado que vamos a crear una nueva imagen de Docker para _bobbys-helidon-stock-application_, que tiene dependencias en _bobbys-coherence-application_, ejecutamos un comando _Maven_ para limpiar el archivo existente bobbys-coherence-application y compilar, crear, empaquetar e instalar un nuevo archivo bobby-coherence-application en un repositorio local de Maven. Para cambiar a la carpeta de directorio _bobbys-coherence_ y compilar, crear e instalar el archivo de aplicación _bobbys-coherence_ en un repositorio de Maven local, copie el siguiente comando y péguelo en _Cloud Shell_.
    
        <copy>
        cd ~/examples/bobs-books/bobbys-books/bobbys-coherence/
        mvn clean install
        </copy>
        
    
    La salida resultante es similar a la siguiente (abreviado para mostrar solo el inicio y el final de la salida): `bash $ mvn clean install [INFO] Scanning for projects... [INFO] [INFO] ------------< io.verrazzano.example.books:bobbys-coherence >------------ [INFO] Building bobbys-coherence 1.0-SNAPSHOT [INFO] --------------------------------[ jar ]--------------------------------- [INFO] [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ bobbys-coherence --- [[INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/target/bobbys-coherence.jar to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.jar [INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/pom.xml to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.pom [INFO] ------------------------------------------------------------------------ [INFO] BUILD SUCCESS [INFO] ------------------------------------------------------------------------ [INFO] Total time: 8.887 s [INFO] Finished at: 2023-03-22T04:09:11Z [INFO] ------------------------------------------------------------------------ $`
    
2.  Como hemos modificado _bobbys-helidon-stock-application_, necesitamos compilar, crear y empaquetar esta aplicación. Para cambiar al directorio _bobbys-helidon-stock-application_ y empaquetar _bobbys-helidon-stock-application_ en un archivo JAR, copie el siguiente comando y péguelo en _Cloud Shell_. En la segunda imagen, puede ver la creación del archivo `bobbys-helidon-stock-application.jar` en `~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target`.
    
        <copy>cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/
        mvn clean package
        </copy>
        
    
    The resulting output is similar to the following (abbreviated to show only the start and end of output): \`\`\`bash $ cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/ $ mvn clean package \[INFO\] Scanning for projects... \[INFO\] ------------------------------------------------------------------------ \[INFO\] Detecting the operating system and CPU architecture \[INFO\] ------------------------------------------------------------------------
    
         [INFO] --- maven-jar-plugin:2.5:jar (default-jar) @ bobbys-helidon-stock-application ---
         [INFO] Building jar: /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target/bobbys-helidon-stock-application.jar
         [INFO] ------------------------------------------------------------------------
         [INFO] BUILD SUCCESS
         [INFO] ------------------------------------------------------------------------
         [INFO] Total time:  10.106 s
         [INFO] Finished at: 2023-03-22T04:11:38Z
         [INFO] ------------------------------------------------------------------------
         $
         ```
        
3.  Vamos a crear una imagen de Docker para la aplicación bobby-stock-helidon, pero esta aplicación utiliza una versión específica de JDK y no queremos cambiar los archivos de Docker que crean la nueva imagen. Por lo tanto, descargamos el JDK necesario. Para descargar la versión de JDK necesaria, copie el siguiente comando y péguelo en _Cloud Shell_.
    
        <copy>wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2_linux-x64_bin.tar.gz</copy>
        
    
    La salida debe ser similar a la siguiente: \`\`\`bash $ wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz --2023-03-22 04:20:16-- https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz Resolviendo download.java.net (download.java.net)... 2.23.220.73 Conectando a download.java.net (download.java.net)|2.23.220.73|:443... conectado. Solicitud HTTP enviada, esperando respuesta... 200 Longitud correcta: 198606200 (189M) \[application/x-gzip\] Guardando en: 'openjdk-14.0.2\_linux-x64\_bin.tar.gz'
    
         100%[==============================================================================================================================>] 198,606,200  194MB/s   in 1.0s   
        
         2023-03-22 04:20:17 (194 MB/s) - ‘openjdk-14.0.2_linux-x64_bin.tar.gz’ saved [198606200/198606200]
         $
         ```
        
4.  Estamos creando una imagen de Docker que cargaremos en Oracle Cloud Container Registry en el laboratorio 6. Para crear el nombre de archivo, necesitamos la siguiente información:
    
    *   Espacio de nombres de arrendamiento
    *   Punto final de la región
5.  Para buscar el espacio de nombres del arrendamiento, haga clic en el icono _Usuario_ -> _Arrendamiento_, como se muestra. En la **configuración de almacenamiento de objetos**, encontrará el espacio de nombres. Copie y guárdelo en algún lugar del archivo de texto, ya que también lo utilizaremos en el laboratorio 6.
    
    ![Copiar espacio de nombres de arrendamiento](images/copy-tenancynamespace.png " ")
    
6.  Localice el _punto final de la región_. Consulte la tabla documentada en esta URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). En el ejemplo que se muestra, el punto final de la región es _Sur de Reino Unido (Londres)_ (como nombre de región) y su punto final es _lhr.ocir.io_. Localice el punto final de su propio _nombre de región_ y guárdelo en el archivo de texto. También lo necesitará para la siguiente práctica de laboratorio.
    
    ![Puntos finales](images/end-point.png " ")
    
    > Ahora tiene tanto el espacio de nombres de arrendamiento como el punto final para su región.
    
7.  Ahora tiene tanto el espacio de nombres de arrendamiento como el punto final para su región. Copie el siguiente comando y péguelo en el editor de texto. A continuación, sustituya **`END_POINT_OF_YOUR_REGION`** por el punto final del nombre de región, **`NAMESPACE_OF_YOUR_TENANCY`** por el espacio de nombres de su arrendamiento y **`your_first_name`** por su nombre en minúsculas.
    
        <copy>docker build --force-rm=true -f Dockerfile -t END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0 .</copy>
        
    
    ![Creación de Docker](images/docker-build.png " ")
    
    ![Imagen creada](images/image-created.png " ")
    
    > Por ejemplo, en mi caso, el comando es `docker build --force-rm=true -f Dockerfile -t lhr.ocir.io/tenancynamespace/helidon-stock-application-ankit`.
    
    De esta forma se crea la imagen de Docker, que insertaremos en el repositorio de Oracle Cloud Container Registry en el laboratorio 6. Debe copiar el nombre de imagen completa sustituida **`END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0`** en el texto editor.In Laboratorio 6, cuando necesite crear el repositorio, debe asignarle el nombre **`helidon-stock-application-your_first_name`**.
    
    Deje _Cloud Shell_ abierto; lo necesitamos para el siguiente laboratorio.
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023