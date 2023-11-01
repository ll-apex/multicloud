# Creación y transferencia de la imagen de la aplicación Helidon a Oracle Cloud Container Registry

## Introducción

En este laboratorio, creará una imagen de Docker _nativa_ con la aplicación Helidon y transferirá esa imagen a un repositorio dentro de Oracle Cloud Container Registry.

Tiempo estimado: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Creación y transferencia de la imagen de aplicación de Helidon a Oracle Cloud Container Registry](videohub:1_mh1brw5t)

### Objetivos

*   Cree y empaquete su aplicación con Docker.
*   Genere un token de autenticación para conectarse a Oracle Cloud Container Registry.
*   Transfiera la imagen de Docker de la aplicación Helidon al repositorio de Oracle Cloud Container Registry.

### Requisitos

*   La aplicación Helidon que ha creado en la práctica de laboratorio anterior
*   Docker
*   Cuenta de Oracle Cloud

## Tarea 1: Creación de la imagen de Docker de la aplicación de Helidon

Estamos creando una imagen de Docker que cargará en Oracle Cloud Container Registry que pertenezca a su cuenta de OCI. Para ello es necesario crear un nombre de imagen que refleje las coordenadas del registro.

Necesita la siguiente información:

*   Nombre de región
*   Espacio de nombres de arrendamiento
*   Punto final para la región
    
    > Copie esta información en un archivo de texto para que pueda consultarla durante toda la práctica.
    

1.  Busque su _nombre de región_.  
    El _nombre de región_ se encuentra en la esquina superior derecha de la consola de Oracle Cloud. En este ejemplo, se muestra como _Sur de Reino Unido (Londres)_. El tuyo puede ser diferente.
    
    ![Container Registry](images/region-name.png)
    
2.  Localice el _espacio de nombres de arrendamiento_.  
    En la consola, abra el menú de navegación y haga clic en **Servicios para desarrolladores**. En **Contenedores y artefactos**, haga clic en **Container Registry**.
    
    ![Espacio de nombres de arrendamiento](images/container-registry.png)
    
    > El espacio de nombres del arrendamiento se muestra en el compartimento. Copie y guárdelo en un archivo de texto. También utilizará esta información en la siguiente práctica de laboratorio. ![Espacio de nombres de arrendamiento](images/name-space.png)
    
3.  Localice el _punto final de la región_.  
    Consulte la tabla documentada en esta URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). En el ejemplo que se muestra, el punto final de la región es _Sur de Reino Unido (Londres)_ (como nombre de región) y su punto final es _lhr.ocir.io_. Localice el punto final de su propio _nombre de región_ y guárdelo en el archivo de texto. También lo necesitará para la siguiente práctica de laboratorio.
    
    ![Puntos finales](images/end-points.png)
    
    > Ahora tiene tanto el espacio de nombres de arrendamiento como el punto final para su región.
    
4.  Copie el siguiente comando y péguelo en el archivo de texto. A continuación, sustituya _`ENDPOINT_OF_YOUR_REGION`_ por el punto final del nombre de región, _`NAMESPACE_OF_YOUR_TENANCY`_ por el espacio de nombres de su arrendamiento y _`your_first_name`_ por el nombre de su arrendamiento.
    
    > Esto realiza una creación completa dentro del contenedor de Docker. La primera vez que la ejecute, tardará un tiempo porque está descargando todas las dependencias de Maven y almacenándolas en caché en una capa de Docker. Las compilaciones posteriores serán mucho más rápidas siempre que no cambie el archivo pom.xml. Si se modifica el pom, se volverán a descargar las dependencias.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0 -f Dockerfile.native .</copy>
        
    
    Cuando el comando esté listo, ejecútelo en el terminal dentro del editor de códigos desde el directorio _`~/helidon-project/myproject/myproject`_. La creación producirá el siguiente resultado al final:
    
        $ docker build -t lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 -f Dockerfile.native .
        
        [1/7] Initializing...                                                                                   (15.7s @ 0.14GB)
        Version info: 'GraalVM 22.3.0 Java 17 CE'
        Java version info: '17.0.5+8-jvmci-22.3-b08'
        C compiler: gcc (redhat, x86_64, 11.3.1)
        Garbage collector: Serial GC
        2 user-specific feature(s)
        - io.helidon.integrations.graal.mp.nativeimage.extension.WeldFeature
        - io.helidon.integrations.graal.nativeimage.extension.HelidonReflectionFeature
        2023.04.06 05:41:01 INFO io.helidon.common.LogConfig Thread[main,5,main]: Logging at initialization configured using classpath: /logging.properties
        [2/7] Performing analysis...  [**********]                                                             (202.8s @ 1.92GB)
        18,812 (92.77%) of 20,278 classes reachable
        27,564 (63.52%) of 43,392 fields reachable
        87,900 (62.22%) of 141,268 methods reachable
        1,068 classes,   565 fields, and 6,864 methods registered for reflection
            65 classes,    70 fields, and    58 methods registered for JNI access
            6 native libraries: dl, m, pthread, rt, stdc++, z
        [3/7] Building universe...                                                                              (27.6s @ 3.15GB)
        [4/7] Parsing methods...      [*****]                                                                   (22.5s @ 3.00GB)
        [5/7] Inlining methods...     [***]                                                                     (11.9s @ 1.84GB)
        [6/7] Compiling methods...    [************]                                                           (156.5s @ 3.05GB)
        [7/7] Creating image...                                                                                 (15.6s @ 2.44GB)
        35.03MB (45.80%) for code area:    57,947 compilation units
        39.02MB (51.01%) for image heap:  477,987 objects and 128 resources
        2.44MB ( 3.19%) for other data
        76.49MB in total
        ------------------------------------------------------------------------------------------------------------------------
        Top 10 packages in code area:                               Top 10 object types in image heap:
        1.63MB sun.security.ssl                                     7.71MB byte[] for code metadata
        1.20MB com.sun.media.sound                                  4.60MB java.lang.Class
        1.17MB java.util                                            3.93MB java.lang.String
        822.87KB java.lang.invoke                                     3.41MB byte[] for java.lang.String
        717.54KB com.sun.crypto.provider                              3.22MB byte[] for general heap data
        517.57KB io.helidon.config                                    1.58MB com.oracle.svm.core.hub.DynamicHubCompanion
        510.02KB java.util.concurrent                                 1.13MB byte[] for reflection metadata
        481.49KB jdk.proxy4                                           1.03MB byte[] for embedded resources
        474.98KB java.lang                                          915.61KB java.util.HashMap$Node
        468.42KB com.sun.org.apache.xerces.internal.impl            781.21KB java.lang.String[]
        26.70MB for 671 more packages                                9.83MB for 4584 more object types
        ------------------------------------------------------------------------------------------------------------------------
                                31.0s (6.6% of total time) in 59 GCs | Peak RSS: 4.80GB | CPU load: 1.60
        ------------------------------------------------------------------------------------------------------------------------
        Produced artifacts:
        /helidon/target/myproject (executable)
        /helidon/target/myproject.build_artifacts.txt (txt)
        ========================================================================================================================
        Finished generating 'myproject' in 7m 48s.
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  08:04 min
        [INFO] Finished at: 2023-04-06T05:48:33Z
        [INFO] ------------------------------------------------------------------------
        Removing intermediate container e400c5c6897b
        ---> 20099e4619d6
        Step 10/15 : RUN echo "done!"
        ---> Running in a8eddd448e48
        done!
        Removing intermediate container a8eddd448e48
        ---> ebfd3064dc68
        Step 11/15 : FROM scratch
        ---> 
        Step 12/15 : WORKDIR /helidon
        ---> Running in 46be56a98462
        Removing intermediate container 46be56a98462
        ---> eaf15b746a1c
        Step 13/15 : COPY --from=build /helidon/target/myproject .
        ---> a69ac5933048
        Step 14/15 : ENTRYPOINT ["./myproject"]
        ---> Running in 71633a601e7f
        Removing intermediate container 71633a601e7f
        ---> cd9f22bfa4b3
        Step 15/15 : EXPOSE 8080
        ---> Running in 4b9763eb49fa
        Removing intermediate container 4b9763eb49fa
        ---> aa8b6e7b04c0
        Successfully built aa8b6e7b04c0
        Successfully tagged lhr.ocir.io/lrv4zdykjqrj/myproject-ankit:1.0
        
5.  De esta forma se crea la imagen de Docker, que puede proteger en el repositorio local.
    
        $ docker images
        REPOSITORY                     TAG IMAGE ID        CREATED           SIZE
        lhr.ocir.io/tn/myproject-ankit 1.0 aa8b6e7b04c0 About a minute ago   80.2MB
        
    
    Copie en el editor de texto el nombre de imagen completa sustituido `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0` porque lo necesitará más tarde.
    
6.  Copie y pegue el siguiente comando en el terminal para ejecutar la imagen de docker en Cloud Shell del editor de códigos.
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    ![ejecución de docker](images/docker-run.png)
    
7.  Abra un terminal o una consola nuevos y ejecute los siguientes comandos para comprobar la aplicación:
    
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
        

## Tarea 2: Generación de un token de autenticación para conectarse a Oracle Cloud Container Registry

En este paso, vamos a generar un _token de autenticación_ que utilizaremos para conectarnos a Oracle Cloud Container Registry.

1.  Seleccione el icono de usuario en la esquina superior derecha y, a continuación, seleccione _Mi perfil_.
    
    ![Mi perfil](images/my-profile.png " ")
    
2.  Desplácese hacia abajo y seleccione _Tokens de autenticación_.
    
    ![Tokens de autenticación](images/auth-token.png " ")
    
3.  Haga clic en _Generar token_.
    
    ![Generar token](images/generate-token.png " ")
    
4.  Copie _`myproject-your_first_name`_ y péguelo en el cuadro _Descripción_ y haga clic en _Generar token_.
    
    ![Creación de Token](images/token-create.png " ")
    
5.  Seleccione _Copiar_ en Token generado y péguelo en el editor de texto. No podemos copiarlo más tarde. A continuación, haga clic en _Cerrar_.
    
    ![Copiar token](images/copy-token.png " ")
    

## Tarea 3: Transferencia de la imagen de Docker de la aplicación Helidon (myproject) al repositorio de Container Registry

1.  En la tarea 1 de esta práctica de laboratorio, ha abierto una URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) y ha determinado el punto final para el nombre de región y lo ha copiado en un archivo de texto. En nuestro ejemplo, el nombre de región es UK South (Londres). Necesitará esta información para esta tarea. ![Punto final](images/end-points.png)
    
2.  Copie el siguiente comando, péguelo en el archivo de texto y, a continuación, sustituya `ENDPOINT_OF_REGION_NAME` por el punto final de la región.
    
    > En nuestro ejemplo, el nombre de región es _Sur de Reino Unido (Londres)_ y el punto final es _lhr.ocir.io_. Necesitará su información específica para esta tarea.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  En el paso anterior, también ha determinado el espacio de nombres del arrendamiento. Introduzca el nombre de usuario de la siguiente forma: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Sustituya `NAMESPACE_OF_YOUR_TENANCY` por el espacio de nombres de su arrendamiento
    *   Sustituya `YOUR_ORACLE_CLOUD_USERNAME` por su nombre de usuario de cuenta de Oracle Cloud y, a continuación, copie el nombre de usuario sustituido del archivo de texto y péguelo en _Cloud Shell_.
    *   Para Contraseña, copie y pegue el token de autenticación del archivo de texto (o donde lo haya guardado).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Vuelva a Container Registry. En la consola, abra el menú de navegación y haga clic en **Servicios para desarrolladores**. En **Contenedores y artefactos**, haga clic en **Container Registry**. ![Container Registry](images/container-registry.png)
    
5.  Seleccione el compartimento y, a continuación, haga clic en **Crear Repositorio**. ![Repositorio creado](images/repository-create.png)
    
6.  Seleccione el compartimento e introduzca _`myproject-your_first_name`_ como nombre de repositorio y, a continuación, seleccione Access como **Public** y haga clic en **Create Repository**.
    
    ![Descripción del repositorio](images/describe-repository.png)
    
7.  Una vez creado el repositorio _`myproject-your_first_name`_, puede verificarlo en la lista de repositorios y su configuración.
    
    ![Verificar espacio de nombres](images/verify-namespace.png)
    
8.  Para transferir la imagen de Docker al repositorio dentro de Oracle Cloud Container Registry, copie y pegue el siguiente comando en el archivo de texto y, a continuación, sustituya `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/myproject-your\_first\_name:1.0 por el nombre completo de la imagen de Docker, que ha guardado anteriormente.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    El resultado debe ser similar a:
    
        $ docker push lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 The push refers to repository [lhr.ocir.io/tenancynamespace/myproject-ankit]
        0bf9e88b7284: Pushed 
        0acc08535a89: Pushed 
        1.0: digest: sha256:3e8cc0ff23d256499dff8d907150b639925687aed0c41008cbe1794bc02433e2 size: 735
        $
        
9.  Después de que el comando _docker push_ se ejecute correctamente, amplíe el repositorio _`myproject-your_first_name`_ y observará que se ha cargado una nueva imagen en este repositorio.
    
    ![Imagen Cargada](images/verify-push.png)
    

## Reconocimientos

*   **Autor**: Dmitry Aleksandrov
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, abril de 2023