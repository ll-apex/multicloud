# Transferir imagen de aplicación de Helidon a Oracle Cloud Container Registry

## Introducción

En este laboratorio, creará una imagen de Docker con la aplicación Helidon y transferirá esa imagen a un repositorio dentro de Oracle Cloud Container Registry.

Tiempo estimado: 10 minutos

### Objetivos

*   Cree y empaquete su aplicación con Docker.
*   Genere un token de autenticación para conectarse a Oracle Cloud Container Registry.
*   Transfiera la imagen de Docker de la aplicación Helidon al repositorio de Oracle Cloud Container Registry.

### Requisitos

*   La aplicación Helidon que ha creado en la práctica de laboratorio anterior
*   Docker
*   Cuenta de Oracle Cloud

## Tarea 1: Creación de la imagen de Docker de la aplicación de Helidon

Comenzaremos preparando la imagen de Docker que utilizará para desplegar en Verrazzano.

Estamos creando una imagen de Docker que cargará en Oracle Cloud Container Registry que pertenezca a su cuenta de OCI. Para ello es necesario crear un nombre de imagen que refleje las coordenadas del registro.

Necesita la siguiente información:

*   Espacio de nombres de arrendamiento
*   Punto final para la región

1.  Para buscar el espacio de nombres del arrendamiento, haga clic en el icono _Usuario_ -> _Arrendamiento_, como se muestra. En la **configuración de almacenamiento de objetos**, encontrará el espacio de nombres. Copie y guárdelo en su archivo de texto, porque también lo usaremos más adelante.
    
    ![Copiar espacio de nombres de arrendamiento](images/copy-tenancynamespace.png " ")
    
2.  Localice el _punto final de la región_. Consulte la tabla documentada en esta URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). En el ejemplo que se muestra, el punto final de la región es _Sur de Reino Unido (Londres)_ (como nombre de región) y su punto final es _lhr.ocir.io_. Localice el punto final de su propio _nombre de región_ y guárdelo en el archivo de texto. También lo necesitará para la siguiente práctica de laboratorio.
    
    ![Puntos finales](images/end-point.png " ")
    
    > Ahora tiene tanto el espacio de nombres de arrendamiento como el punto final para su región.
    
3.  Copie el siguiente comando y péguelo en el editor de texto. A continuación, sustituya _`ENDPOINT_OF_YOUR_REGION`_ por el punto final del nombre de región, _`NAMESPACE_OF_YOUR_TENANCY`_ por el espacio de nombres de su arrendamiento y _`your_first_name`_ por el nombre de su arrendamiento.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0 .</copy>
        
    
    Cuando el comando esté listo, ejecútelo en Cloud Shell desde el directorio _`~/quickstart-mp/`_. La creación producirá el siguiente resultado:
    
        $ cd ~/quickstart-mp/
        $ docker build lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0 .
        > docker pull lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        [+] Building 107.5s (19/19) FINISHED                                                                                                            
        => [internal] load build definition from Dockerfile                                                                                       0.1s
        => => transferring dockerfile: 785B                                                                                                       0.1s
        => [internal] load .dockerignore                                                                                                          0.1s
        => => transferring context: 48B                                                                                                           0.0s
        => [internal] load metadata for docker.io/library/openjdk:11-jre-slim                                                                     3.7s
        => [internal] load metadata for docker.io/library/maven:3.6-jdk-11                                                                        2.8s
        => [auth] library/openjdk:pull token for registry-1.docker.io                                                                             0.0s
        => [auth] library/maven:pull token for registry-1.docker.io                                                                               0.0s
        => [stage-1 1/4] FROM docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527      33.3s
        => => resolve docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527               0.0s
        => => sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527 549B / 549B                                                 0.0s
        => => sha256:f3cdb8fd164057f4ef3e60674fca986f3cd7b3081d55875c7ce75b7a214fca6d 1.16kB / 1.16kB                                             0.0s
        => => sha256:9c9e40a31d4fa290f933d76f3b0a4183ba02a7298a309f6bfa892d618e7196ef 7.56kB / 7.56kB                                             0.0s
        => => sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717 31.36MB / 31.36MB                                          18.6s
        => => sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251 1.58MB / 1.58MB                                             1.6s
        => => sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c 211B / 211B                                                 0.7s
        => => sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358 47.13MB / 47.13MB                                          24.4s
        => => extracting sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717                                                  7.7s
        => => extracting sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251                                                  0.3s
        => => extracting sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c                                                  0.0s
        => => extracting sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358                                                  5.7s
        => [build 1/7] FROM docker.io/library/maven:3.6-jdk-11@sha256:1d29ccf46ef2a5e64f7de3d79a63f9bcffb4dc56be0ae3daed5ca5542b38aa2d            0.0s
        => [internal] load build context                                                                                                          0.1s
        => => transferring context: 13.99kB                                                                                                       0.1s
        => CACHED [build 2/7] WORKDIR /helidon                                                                                                    0.0s
        => [build 3/7] ADD pom.xml .                                                                                                              0.1s
        => [build 4/7] RUN mvn package -Dmaven.test.skip -Declipselink.weave.skip                                                                91.7s
        => [stage-1 2/4] WORKDIR /helidon                                                                                                         0.8s
        => [build 5/7] ADD src src                                                                                                                0.1s
        => [build 6/7] RUN mvn package -DskipTests                                                                                               10.8s
        => [build 7/7] RUN echo "done!"                                                                                                           0.5s
        => [stage-1 3/4] COPY --from=build /helidon/target/quickstart-mp-ankit.jar ./                                                                   0.1s
        => [stage-1 4/4] COPY --from=build /helidon/target/libs ./libs                                                                            0.1s
        => exporting to image                                                                                                                     0.1s
        => => exporting layers                                                                                                                    0.1s
        => => writing image sha256:587a079ad854fc79e768acda11fc05dd87d37013261249e778e80749798c2837                                               0.0s
        => => naming to lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0                                                                           0.0s
        
4.  De esta forma se crea la imagen de Docker, que puede proteger en el repositorio local.
    
        $ docker images
        
        REPOSITORY                                                                           TAG                               IMAGE ID       CREATED         SIZE
        lhr.ocir.io/tenancynamespace/quickstart-mp-ankit                                               1.0                               587a079ad854   5 minutes ago   243MB
        
    
    Copie en el archivo de texto el nombre de imagen completa sustituido `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-ankit:1.0` porque lo necesitará más tarde.
    

## Tarea 2: Generación de un token de autenticación para conectarse a Oracle Cloud Container Registry

En este paso, vamos a generar un _token de autenticación_ que utilizaremos para conectarnos a Oracle Cloud Container Registry.

1.  Seleccione el icono de usuario en la esquina superior derecha y, a continuación, seleccione _Mi perfil_.
    
    ![Mi perfil](images/my-profile.png)
    
2.  Desplácese hacia abajo y seleccione _Tokens de autenticación_.
    
    ![Token de autenticación](images/auth-token.png)
    
3.  Haga clic en _Generar token_.
    
    ![Generar tokens](images/generate-token.png)
    
4.  Copie _`quickstart-mp-your_first_name`_, péguelo en el cuadro Descripción y haga clic en _Generar token_.
    
    ![Crear Token](images/create-token.png)
    
5.  Haga clic en _Copiar_ en Token generado y péguelo en el editor de texto. No puede copiarlo más tarde, así que asegúrese de guardar una copia de este token. A continuación, haga clic en _Cerrar_.
    
    ![Copiar token](images/copy-token.png)
    

## Tarea 3: Transferencia de la imagen de Docker de la aplicación Helidon (quickstart-mp) al repositorio de Container Registry

1.  En la tarea 1 de esta práctica de laboratorio, ha abierto una URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) y ha determinado el punto final para el nombre de región y lo ha copiado en un editor de texto. En nuestro ejemplo, el nombre de región es UK South (Londres). Necesitará esta información para esta tarea. ![Punto final](images/end-point.png)
    
2.  Copie el siguiente comando, péguelo en el editor de texto y, a continuación, sustituya `ENDPOINT_OF_REGION_NAME` por el punto final de su región.
    
    > En nuestro ejemplo, el nombre de región es _Sur de Reino Unido (Londres)_ y el punto final es _lhr.ocir.io_. Necesitará su información específica para esta tarea.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  En el paso anterior, también ha determinado el espacio de nombres del arrendamiento. Introduzca el nombre de usuario de la siguiente forma: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Sustituya _`NAMESPACE_OF_YOUR_TENANCY`_ por el espacio de nombres de su arrendamiento
    *   Sustituya _`YOUR_ORACLE_CLOUD_USERNAME`_ por su nombre de usuario de cuenta de Oracle Cloud y, a continuación, copie el nombre de usuario sustituido del editor de texto y péguelo en _Cloud Shell_.
    *   Para Contraseña, copie y pegue el token de autenticación desde el editor de texto (o donde lo haya guardado).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Vuelva a Container Registry. En la consola, abra el menú de navegación y haga clic en **Servicios para desarrolladores**. En **Contenedores y artefactos**, haga clic en **Container Registry**. ![Container Registry](images/container-registry.png)
    
5.  Seleccione el compartimento y, a continuación, haga clic en **Crear**. ![Repositorio creado](images/repository-create.png)
    
6.  Seleccione el compartimento e introduzca _`quickstart-mp-your_first_name`_ como nombre de repositorio y, a continuación, seleccione Access como **Public** y haga clic en **Create Repository**.
    
    ![Descripción del repositorio](images/describe-repository.png)
    
7.  Una vez creado el repositorio _`quickstart-mp-your_first_name`_, puede verificarlo en la lista de repositorios y su configuración.
    
    ![Verificar espacio de nombres](images/verify-namespace.png)
    
8.  Para transferir la imagen de Docker al repositorio dentro de Oracle Cloud Container Registry, copie y pegue el siguiente comando en el editor de texto y, a continuación, sustituya `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`quickstar-mp-your_first_name`:1.0 por el nombre completo de la imagen de Docker, que ha guardado anteriormente.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0</copy>
        
    
    El resultado debe ser similar a:
    
        $ docker push lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        The push refers to a repository [lhr.ocir.io/tenancynamespace/quickstart-mp-ankit]
        0795b8384c47: Pushed
        131452972f9d: Pushed
        93c53f2e9519: Pushed
        3b78b65a4be9: Pushed
        e1434e7d0308: Pushed
        17679d5f39bd: Pushed
        300b011056d9: Pushed
        1.0: digest: sha256:355fa56eab185535a58c5038186381b6d02fd8e0bcb534872107fc249f98256a size: 1786
        
9.  Después de que el comando _docker push_ se ejecute correctamente, amplíe el repositorio _`quickstart-mp-your_first_name`_ y observará que se ha cargado una nueva imagen en este repositorio.
    
10.  Deje abierta la página del repositorio de _Cloud Shell_ y Container Registry; las necesitará para las próximas prácticas.
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023