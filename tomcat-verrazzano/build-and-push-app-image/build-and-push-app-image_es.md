# Transferir imagen de aplicación Tomcat a Oracle Cloud Container Registry

## Introducción

En este laboratorio, creará una imagen de Docker con la aplicación tomcat y la transferirá a un repositorio dentro de Oracle Cloud Container Registry.

Tiempo estimado: 10 minutos

### Objetivos

*   Cree y empaquete su aplicación con Docker.
*   Genere un token de autenticación para conectarse a Oracle Cloud Container Registry.
*   Transfiera la imagen de Docker de la aplicación Tomcat al repositorio de Oracle Cloud Container Registry.

### Requisitos

*   Docker
*   Cuenta de Oracle Cloud

## Tarea 1: Descargar el código fuente de la aplicación

1.  Copie y pegue el siguiente comando para descargar el código fuente de este taller.
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/5DhoxZ22MseWaoSjG3EoBRf6DF5JbQ1QyR7tHa2qem-7IHb7nNEymF1ZSm2eA1ix/n/lrv4zdykjqrj/b/ankit-bucket/o/tomcat-examples.zip >~/tomcat-examples.zip</copy>
        
2.  Copie y pegue el siguiente comando para descomprimir el código fuente y cambiar el directorio actual a la carpeta de la aplicación.
    
        <copy>unzip ~/tomcat-examples.zip
        cd ~/tomcat-examples/</copy>
        

## Tarea 2: Creación de la imagen de Docker de la aplicación Tomcat

Comenzaremos preparando la imagen de Docker que utilizará para desplegar en Verrazzano.

Estamos creando una imagen de Docker que cargará en Oracle Cloud Container Registry que pertenezca a su cuenta de OCI. Para ello es necesario crear un nombre de imagen que refleje las coordenadas del registro.

Necesita la siguiente información:

*   Espacio de nombres de arrendamiento
    
*   Punto final para la región
    
    > Copie esta información en un archivo de texto para que pueda consultarla durante toda la práctica.
    

1.  Para buscar el espacio de nombres del arrendamiento, haga clic en el icono _Usuario_ -> _Arrendamiento_, como se muestra. En la **configuración de almacenamiento de objetos**, encontrará el espacio de nombres. Copie y guárdelo en su archivo de texto, porque también lo usaremos más adelante.
    
    ![Copiar espacio de nombres de arrendamiento](images/copy-tenancynamespace.png " ")
    
2.  Abra una URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) y determine el punto final para el nombre de la región y cópielo en un archivo de texto. En nuestro ejemplo, el nombre de región es UK South (Londres). Necesitará esta información para esta tarea. ![Punto final](images/end-point.png)
    
3.  Copie el siguiente comando y péguelo en el editor de texto. A continuación, sustituya _`ENDPOINT_OF_YOUR_REGION`_ por el punto final del nombre de región, _`NAMESPACE_OF_YOUR_TENANCY`_ por el espacio de nombres de su arrendamiento y _`your_first_name`_ por el nombre de su arrendamiento.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-your_first_name:v1 .</copy>
        
    
    Cuando el comando esté listo, ejecútelo en Cloud Shell desde el directorio `~/tomcat-examples/`. La creación producirá el siguiente resultado:
    
        $ cd ~/tomcat-examples/
        $ $ docker build -t lhr.ocir.io/tenancy-namespace/tomcat-example-ankit:v1 .
        Sending build context to Docker daemon  1.097MB
        Step 1/10 : FROM tomcat:8.0-alpine
        Trying to pull repository docker.io/library/tomcat ... 
        8.0-alpine: Pulling from docker.io/library/tomcat
        4fe2ade4980c: Pull complete 
        6fc58a8d4ae4: Pull complete 
        7d9bd64c803b: Pull complete 
        a22aedc5ac11: Pull complete 
        5bde63ae3587: Pull complete 
        69cb0c9b940a: Pull complete 
        Digest: sha256:d02a16c0147fcae13d812fa670a4b3c9944f5328b10a5a463ad697d2aa5bb063
        Status: Downloaded newer image for tomcat:8.0-alpine
        ---> 624fb61775c3
        Step 2/10 : LABEL maintainer="ankit.x.pandey@oracle.com"
        ---> Running in 20cc23726499
        Removing intermediate container 20cc23726499
        ---> 50245c696fb6
        Step 3/10 : ADD sample-webapp.war /usr/local/tomcat/webapps/
        ---> 727c55f91bb5
        Step 4/10 : RUN mkdir /data
        ---> Running in f3129a859e11
        Removing intermediate container f3129a859e11
        ---> 9ce0f5674f51
        Step 5/10 : ADD jmx_prometheus_javaagent-0.17.0.jar /data/jmx_prometheus_javaagent-0.17.0.jar
        ---> f03cc9ee1bee
        Step 6/10 : ADD prometheus-jmx-config.yaml /data/prometheus-jmx-config.yaml
        ---> 50c51ae6a148
        Step 7/10 : ENV JAVA_OPTS="-javaagent:/data/jmx_prometheus_javaagent-0.17.0.jar=8088:/data/prometheus-jmx-config.yaml"
        ---> Running in 5e9effd5d494
        Removing intermediate container 5e9effd5d494
        ---> 85ca06fcd965
        Step 8/10 : EXPOSE 8088
        ---> Running in 795325f82526
        Removing intermediate container 795325f82526
        ---> 19dfc6fd903c
        Step 9/10 : EXPOSE 8080
        ---> Running in 43be96f20275
        Removing intermediate container 43be96f20275
        ---> 7d9bcaa7a271
        Step 10/10 : CMD ["catalina.sh", "run"]
        ---> Running in 3e25cd78ab88
        Removing intermediate container 3e25cd78ab88
        ---> 516065fe1bf5
        Successfully built 516065fe1bf5
        Successfully tagged lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        $
        
4.  De esta forma se crea la imagen de Docker, que puede proteger en el repositorio local.
    
        $ docker images
        REPOSITORY                           TAG IMAGE ID      CREATED       SIZE
        lhr.ocir.io/name/tomcat-example-ankit v1 516065fe1bf5 2 minutes ago  147MB
        tomcat                       8.0-alpine  624fb61775c3 4 years ago    147MB
        
    
    Copie en el editor de texto el nombre de imagen completa sustituido `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1` porque lo necesitará más tarde.
    

## Tarea 3: Generación de un token de autenticación para conectarse a Oracle Cloud Container Registry

En este paso, vamos a generar un _token de autenticación_ que utilizaremos para conectarnos a Oracle Cloud Container Registry.

1.  Seleccione el icono de usuario en la esquina superior derecha y, a continuación, seleccione _Mi perfil_.
    
    ![Mi perfil](images/my-profile.png " ")
    
2.  Desplácese hacia abajo y seleccione _Tokens de autenticación_.
    
    ![Tokens de autenticación](images/auth-token.png " ")
    
3.  Haga clic en _Generar token_.
    
    ![Generar token](images/generate-token.png " ")
    
4.  Copie _`tomcat-example-your_first_name`_ y péguelo en el cuadro _Descripción_ y haga clic en _Generar token_.
    
    ![Creación de Token](images/token-create.png " ")
    
5.  Seleccione _Copiar_ en Token generado y péguelo en el editor de texto. No podemos copiarlo más tarde. A continuación, haga clic en _Cerrar_.
    
    ![Copiar token](images/copy-token.png " ")
    

## Tarea 4: Transferencia de la imagen de Docker de la aplicación tomcat al repositorio de Container Registry

1.  En la tarea 2 de esta práctica de laboratorio, ha abierto una URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) y ha determinado el punto final para el nombre de región y lo ha copiado en un archivo de texto. En nuestro ejemplo, el nombre de región es UK South (Londres). Necesitará esta información para esta tarea. ![Punto final](images/end-point.png)
    
2.  Copie el siguiente comando, péguelo en el editor de texto y, a continuación, sustituya `ENDPOINT_OF_REGION_NAME` por el punto final de su región.
    
    > En nuestro ejemplo, el nombre de región es _Sur de Reino Unido (Londres)_ y el punto final es _lhr.ocir.io_. Necesitará su información específica para esta tarea.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  En el paso anterior, también ha determinado el espacio de nombres del arrendamiento. Introduzca el nombre de usuario de la siguiente forma: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Sustituya `NAMESPACE_OF_YOUR_TENANCY` por el espacio de nombres de su arrendamiento
    *   Sustituya `YOUR_ORACLE_CLOUD_USERNAME` por su nombre de usuario de cuenta de Oracle Cloud y, a continuación, copie el nombre de usuario sustituido del editor de texto y péguelo en _Cloud Shell_.
    *   Para Contraseña, copie y pegue el token de autenticación desde el editor de texto (o donde lo haya guardado).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Vuelva a Container Registry. En la consola, abra el menú de navegación y haga clic en **Servicios para desarrolladores**. En **Contenedores y artefactos**, haga clic en **Container Registry**. ![Container Registry](images/container-registry.png)
    
5.  Seleccione el compartimento y, a continuación, haga clic en **Crear Repositorio**. ![Repositorio creado](images/repository-create.png)
    
6.  Seleccione el compartimento e introduzca _`tomcat-example-your_first_name`_ como nombre de repositorio y, a continuación, seleccione Access como **Public** y haga clic en **Create**.
    
    ![Descripción del repositorio](images/describe-repository.png)
    
7.  Para transferir la imagen de Docker al repositorio dentro de Oracle Cloud Container Registry, copie y pegue el siguiente comando en el editor de texto y, a continuación, sustituya `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`tomcat-example-your_first_name`:1.0 por el nombre completo de la imagen de Docker, que ha guardado anteriormente.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1</copy>
        
    
    El resultado debe ser similar a:
    
        $ docker push lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        The push refers to repository [lhr.ocir.io/tenancynamespace/tomcat-example-ankit]
        4b193f4c616d: Pushed 
        0469528628db: Pushed 
        cce8193c4190: Pushed 
        ca36c0db4673: Pushed 
        0136a6a85859: Pushed 
        98a0db77a14c: Pushed 
        9072514c7af0: Pushed 
        f6146a44a7d3: Pushed 
        0c3170905795: Pushed 
        df64d3292fd6: Pushed 
        v1: digest: sha256:65b562a7117870540f1807e0d796fe964e6428bda0ae290b8a6389bf637d1aba size: 2405
        $ 
        
8.  Después de que el comando _docker push_ se ejecute correctamente, amplíe el repositorio _`tomcat-example-ankit:v1`_ y observará que se ha cargado una nueva imagen en este repositorio.
    

Ahora puede **proceder al siguiente laboratorio**.

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023