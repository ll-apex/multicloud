# Transferencia de la imagen de la aplicación Springboot a Oracle Cloud Container Registry

## Introducción

En este laboratorio, creará una imagen de Docker con la aplicación springboot y transferirá esa imagen a un repositorio dentro de Oracle Cloud Container Registry.

Tiempo estimado: 10 minutos

### Objetivos

*   Cree y empaquete su aplicación con Docker.
*   Genere un token de autenticación para conectarse a Oracle Cloud Container Registry.
*   Transfiera la imagen de Docker de la aplicación Springboot al repositorio de Oracle Cloud Container Registry.

### Requisitos

*   Docker
*   Cuenta de Oracle Cloud

## Tarea 1: Descargar el código fuente de la aplicación y el JDK necesario

1.  Copie y pegue el siguiente comando para descargar el código fuente de este taller.
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/EeNNvCVLObhXjHIwu5M-J_zbSXJsbLop6wcsFFnDfneY3zqbAFfdKikZe-Q0GT3I/n/lrv4zdykjqrj/b/ankit-bucket/o/springboot-app.zip >~/springboot-app.zip</copy>
        
2.  Copie y pegue el siguiente comando para descomprimir el código fuente y cambiar el directorio actual a la carpeta de la aplicación.
    
        <copy>unzip ~/springboot-app.zip
        cd ~/springboot-app/</copy>
        
3.  Vamos a crear una imagen de Docker para la aplicación springboot, pero esta aplicación utiliza una versión específica de JDK y no queremos cambiar los archivos de Docker que crean la nueva imagen. Por lo tanto, descargamos el JDK necesario. Para descargar la versión de JDK necesaria, copie el siguiente comando y péguelo en Cloud Shell.
    
        <copy>wget https://download.java.net/java/ga/jdk11/openjdk-11_linux-x64_bin.tar.gz</copy>
        
4.  Copie y pegue el siguiente comando para crear la aplicación springboot.
    
        <copy>mvn clean package</copy>
        

## Tarea 2: Creación de la imagen de Springboot Application Docker

Comenzaremos preparando la imagen de Docker que utilizará para desplegar en Verrazzano.

Estamos creando una imagen de Docker que cargará en Oracle Cloud Container Registry que pertenezca a su cuenta de OCI. Para ello es necesario crear un nombre de imagen que refleje las coordenadas del registro.

Necesita la siguiente información:

*   Espacio de nombres de arrendamiento
    
*   Punto final para la región
    
    > Copie esta información en un archivo de texto para que pueda consultarla durante toda la práctica.
    

1.  Para buscar el espacio de nombres del arrendamiento, haga clic en el icono _Usuario_ -> _Arrendamiento_, como se muestra. En la **configuración de almacenamiento de objetos**, encontrará el espacio de nombres. Copie y guárdelo en su archivo de texto, porque también lo usaremos más adelante.
    
    ![Copiar espacio de nombres de arrendamiento](images/copy-tenancynamespace.png " ")
    
2.  Localice el _punto final de la región_.  
    Consulte la tabla documentada en esta URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). En el ejemplo que se muestra, el punto final de la región es _Sur de Reino Unido (Londres)_ (como nombre de región) y su punto final es _lhr.ocir.io_. Localice el punto final de su propio _nombre de región_ y guárdelo en el archivo de texto.
    
    ![Puntos finales](images/end-points.png)
    
    > Ahora tiene tanto el espacio de nombres de arrendamiento como el punto final para su región.
    
3.  Copie el siguiente comando y péguelo en el archivo de texto. A continuación, sustituya _`ENDPOINT_OF_YOUR_REGION`_ por el punto final del nombre de región, _`NAMESPACE_OF_YOUR_TENANCY`_ por el espacio de nombres de su arrendamiento y _`your_first_name`_ por el nombre de su arrendamiento.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-your_first_name:v1 .</copy>
        
    
    Cuando el comando esté listo, ejecútelo en Cloud Shell desde el directorio `~/springboot-app/`. La creación producirá el siguiente resultado:
    
        $ cd ~/springboot-app/
        $ docker build -t lhr.ocir.io/tenancy-namespace/springboot-ankit:v1 .
        Sending build context to Docker daemon  206.7MB
        Step 1/14 : FROM ghcr.io/oracle/oraclelinux:7-slim AS build_base
        Trying to pull repository ghcr.io/oracle/oraclelinux ... 
        7-slim: Pulling from ghcr.io/oracle/oraclelinux
        6cb086706000: Pull complete 
        Digest: sha256:4353fdc8664c386c0a443eb40b10a7662b4eb8d6eb5d6dcefe218e9783132c71
        Status: Downloaded newer image for ghcr.io/oracle/oraclelinux:7-slim
        ---> 1d56b1a0fd84
        Step 2/14 : RUN yum update -y && yum-config-manager --save --setopt=ol7_ociyum_config.skip_if_unavailable=true     && yum install -y tar unzip gzip     && yum clean all; rm -rf /var/cache/yum     && mkdir -p /license
        ---> Running in 92c013a8e84f
        Loaded plugins: ovl
        No packages marked for update
        Loaded plugins: ovl
        ===================================== main =====================================
        [main]
        alwaysprompt = True
        assumeno = False
        assumeyes = False
        autocheck_running_kernel = True
        autosavets = True
        bandwidth = 0
        bugtracker_url = https://linux.oracle.com
        cache = 0
        cachedir = /var/cache/yum/x86_64/7Server
        check_config_file_age = True
        clean_requirements_on_remove = False
        ----------------------------------------------------------------------
        Step 3/14 : ENV JAVA_HOME=/usr/java
        ---> Running in 96b99fba7e50
        Removing intermediate container 96b99fba7e50
        ---> 1cc6c7a63b89
        Step 4/14 : ENV PATH $JAVA_HOME/bin:$PATH
        ---> Running in 4a88eb052547
        Removing intermediate container 4a88eb052547
        ---> 48e7fa9b7b0c
        Step 5/14 : ARG JDK_BINARY="${JDK_BINARY:-openjdk-11_linux-x64_bin.tar.gz}"
        ---> Running in e922e8b35bfd
        Removing intermediate container e922e8b35bfd
        ---> 6888b690a4b0
        Step 6/14 : COPY ${JDK_BINARY} jdk.tar.gz
        ---> e9c5ffd0f2a5
        Step 7/14 : ENV JDK_DOWNLOAD_SHA256=3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e
        ---> Running in a7a89b057f25
        Removing intermediate container a7a89b057f25
        ---> 12ecfaa002bf
        Step 8/14 : RUN set -eux     echo "Checking JDK hash";     echo "${JDK_DOWNLOAD_SHA256} *jdk.tar.gz" | sha256sum --check -;     echo "Installing JDK";     mkdir -p "$JAVA_HOME";     tar xzf jdk.tar.gz --directory "${JAVA_HOME}" --strip-components=1;     rm -f jdk.tar.gz;
        ---> Running in a6a590adeee6
        + echo '3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e *jdk.tar.gz'
        + sha256sum --check -
        jdk.tar.gz: OK
        + echo 'Installing JDK'
        + mkdir -p /usr/java
        Installing JDK
        + tar xzf jdk.tar.gz --directory /usr/java --strip-components=1
        + rm -f jdk.tar.gz
        Removing intermediate container a6a590adeee6
        ---> 1f37a6cb044d
        Step 9/14 : COPY LICENSE.txt /license/
        ---> dc69f24f5be6
        Step 10/14 : COPY THIRD_PARTY_LICENSES.txt /license/
        ---> 5ef683dbda22
        Step 11/14 : ARG JAR_FILE=target/*.jar
        ---> Running in 3b80032f8310
        Removing intermediate container 3b80032f8310
        ---> 8eca16289bd7
        Step 12/14 : COPY ${JAR_FILE} app.jar
        ---> dcb7e3ed0871
        Step 13/14 : ENTRYPOINT ["java","-jar","/app.jar"]
        ---> Running in 2191623bf524
        Removing intermediate container 2191623bf524
        ---> 11e59e19cfb4
        Step 14/14 : USER 1000
        ---> Running in 16446779b92b
        Removing intermediate container 16446779b92b
        ---> a701fa912f2e
        Successfully built a701fa912f2e
        Successfully tagged lhr.ocir.io/tenancynamespace/springboot-ankit:v1
        $
        
4.  De esta forma se crea la imagen de Docker, que puede proteger en el repositorio local.
    
        $ docker images
        REPOSITORY                            TAG     IMAGE ID    CREATED      SIZE
        lhr.ocir.io/namespace/springboot-ankit v1    a701fa912f2 3 minutes ago 664MB
        ghcr.io/oracle/oraclelinux            7-slim 1d56b1a0fd8 3 weeks ago   138MB
        $ 
        
    
    Copie en el archivo de texto el nombre de imagen completa sustituido `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1` porque lo necesitará más tarde.
    

## Tarea 3: Generación de un token de autenticación para conectarse a Oracle Cloud Container Registry

En este paso, vamos a generar un _token de autenticación_ que utilizaremos para conectarnos a Oracle Cloud Container Registry.

1.  Seleccione el icono de usuario en la esquina superior derecha y, a continuación, seleccione _Mi perfil_.
    
    ![Mi perfil](images/my-profile.png " ")
    
2.  Desplácese hacia abajo y seleccione _Tokens de autenticación_.
    
    ![Tokens de autenticación](images/auth-token.png " ")
    
3.  Haga clic en _Generar token_.
    
    ![Generar token](images/generate-token.png " ")
    
4.  Copie _`springboot-your_first_name`_ y péguelo en el cuadro _Descripción_ y haga clic en _Generar token_.
    
    ![Creación de Token](images/token-create.png " ")
    
5.  Seleccione _Copiar_ en Token generado y péguelo en el archivo de texto. No podemos copiarlo más tarde. A continuación, haga clic en _Cerrar_.
    
    ![Copiar token](images/copy-token.png " ")
    

## Tarea 4: Transferencia de la imagen de Docker de la aplicación Springboot al repositorio de Container Registry

1.  En la tarea 1 de esta práctica de laboratorio, ha abierto una URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) y ha determinado el punto final para el nombre de región y lo ha copiado en un archivo de texto. En nuestro ejemplo, el nombre de región es UK South (Londres). Necesitará esta información para esta tarea. ![Punto final](images/end-points.png)
    
2.  Copie el siguiente comando, péguelo en el editor de texto y, a continuación, sustituya `ENDPOINT_OF_REGION_NAME` por el punto final de su región.
    
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
    
5.  Seleccione el compartimento y, a continuación, haga clic en **Crear**. ![Repositorio creado](images/repository-create.png)
    
6.  Seleccione el compartimento e introduzca _`springboot-your_first_name`_ como nombre de repositorio y, a continuación, seleccione Access como **Public** y haga clic en **Create Repository**.
    
    ![Descripción del repositorio](images/describe-repository.png)
    
7.  Para transferir la imagen de Docker al repositorio dentro de Oracle Cloud Container Registry, copie y pegue el siguiente comando en el archivo de texto y, a continuación, sustituya _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/springboot-your\_firstname:1.0_ por el nombre completo de la imagen de Docker, que ha guardado anteriormente.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1</copy>
        
    
    El resultado debe ser similar a:
    
        $ docker push lhr.ocir.io/namespace/springboot-ankit:v1
        The push refers to repository [lhr.ocir.io/namespace/springboot-ankit]
        31118271414e: Pushed 
        e6144652ec48: Pushed 
        a5ac4d4576aa: Pushed 
        2f93ab3a0c42: Pushed 
        3ed60ad88e51: Pushed 
        f47db30f116a: Pushed 
        f50ba2e0b2f9: Pushed 
        v1: digest: sha256:96aacff31cb255ea815213aba837f16f40d73b14d67449d4744ed811c7a864c8 size: 1795
        $ 
        
8.  Después de que el comando _docker push_ se ejecute correctamente, amplíe el repositorio _`springboot-ankit:v1`_ y observará que se ha cargado una nueva imagen en este repositorio.
    

Ahora puede **proceder al siguiente laboratorio**.

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023