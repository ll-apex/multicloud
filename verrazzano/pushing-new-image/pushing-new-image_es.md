# Transferencia de la imagen de Docker para la aplicación bobbys-helidon-stock a Oracle Cloud Container Registry

## Introducción

En el laboratorio 5, modificamos bobbys-helidon-stock-application y creamos una nueva imagen de Docker. En este laboratorio, insertaremos esa imagen en un repositorio dentro de Oracle Cloud Container Registry.

Tiempo estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Genere un token de autenticación para conectarse a Oracle Cloud Container Registry.
*   Transfiera la imagen de Docker de bobbys-helidon-stock-application a su repositorio de Oracle Cloud Container Registry.

### Requisitos

Debe tener un editor de texto, donde puede pegar los comandos y las URL y modificarlas, según su entorno. A continuación, puede copiar y pegar el comando modificado para ejecutarlo en _Cloud Shell_.

## Tarea 1: Generación de un token de autenticación para conectarse a Oracle Cloud Container Registry

En este paso, vamos a generar un _token de autenticación_ que utilizaremos para conectarnos a Oracle Cloud Container Registry.

1.  Seleccione el icono de usuario en la esquina superior derecha y, a continuación, seleccione _Mi perfil_.
    
    ![Configuración de usuario](images/user-settings.png " ")
    
2.  Desplácese hacia abajo y seleccione _Tokens de autenticación_.
    
    ![Tokens de autenticación](images/auth-token.png " ")
    
3.  Haga clic en _Generar token_.
    
    ![Generar token](images/generate-token.png " ")
    
4.  Copie _`helidon-stock-application-your_first_name`_ y péguelo en el cuadro _Descripción_ y haga clic en _Generar token_.
    
    ![Creación de Token](images/token-create.png " ")
    
5.  Seleccione _Copiar_ en Token generado y péguelo en el archivo de texto. No podemos copiarlo más tarde.
    
    ![Copiar token](images/copy-token.png " ")
    

## Tarea 2: Transferencia de la imagen de Docker bobbys-helidon-stock-application al repositorio de Oracle Cloud Container Registry

1.  En la tarea 1 de esta práctica de laboratorio, ha abierto una URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) y ha determinado el punto final para el nombre de región y lo ha copiado en un editor de texto. En nuestro ejemplo, el nombre de región es UK South (Londres). Necesitará esta información para esta tarea. ![Punto final](images/end-point.png)
    
2.  Copie el siguiente comando, péguelo en el editor de texto y, a continuación, sustituya **`END_POINT_OF_REGION_NAME`** por el punto final de la región.
    
        <copy> docker login `END_POINT_OF_REGION_NAME`</copy>
        

3\. En la práctica anterior, determinó el espacio de nombres de arrendamiento. Defina el nombre de usuario de la siguiente forma: \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`/oracleidentitycloudservice/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\*. Aquí, sustituya \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`\*\* por el espacio de nombres de su arrendamiento y \*\*\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\* por su nombre de usuario de cuenta de Oracle Cloud y, a continuación, copie el nombre de usuario sustituido del archivo de texto y péguelo en \*Cloud Shell\*. Para Contraseña, pegue el token de autenticación del editor de texto o donde lo haya guardado. !\[Login Registry\](images/login-registry.png " ") 3\. En la práctica anterior, determinó el espacio de nombres de arrendamiento. Defina el nombre de usuario de la siguiente forma: \`NAMESPACE\_OF\_YOUR\_TENANCY\`/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`. Debe utilizar el espacio de nombres de arrendamiento y el nombre de usuario en minúsculas al realizar la conexión a docker. Aquí, sustituya \`NAMESPACE\_OF\_YOUR\_TENANCY\` por el espacio de nombres de su arrendamiento y \`YOUR\_ORACLE\_CLOUD\_USERNAME\` por su nombre de usuario de cuenta de Oracle Cloud y, a continuación, copie el nombre de usuario sustituido del editor de texto y péguelo en \*Cloud Shell\*. Para Contraseña, pegue el token de autenticación en el editor de texto o en el lugar donde lo haya guardado.

4.  Vuelva a Container Registry y seleccione _Menú de hamburguesa -> Servicios para desarrolladores -> Container Registry_.
    
    ![Container Registry](images/container-registry.png " ")
    
5.  Seleccione el compartimento y, a continuación, haga clic en _Crear Repositorio_.
    
    ![Repositorio creado](images/repository-create.png " ")
    
6.  Seleccione el compartimento e introduzca _`helidon-stock-application-your_first_name`_ como nombre de repositorio y, a continuación, seleccione Access como _Public_ y haga clic en _Create_.
    
    ![Datos de repositorio](images/repository-data.png " ")
    
    Una vez creado el repositorio _`helidon-stock-application-your_first_name`_, puede verificar el espacio de nombres y debe ser el mismo que el espacio de nombres de su arrendamiento.
    
    !\[Verificar espacio de nombres\](images/verify-namespace.png" ")
    
7.  Como parte del laboratorio 5, ha copiado el nombre completo de la imagen de Docker en su editor de texto. Para transferir la imagen de Docker al repositorio dentro de Oracle Cloud Container Registry, copie y pegue el siguiente comando en el editor de texto y, a continuación, sustituya _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`helidon-stock-application-your_first_name`:1.0_ por el nombre completo de la imagen de Docker, que ha guardado en el editor de texto.
    
        <copy>docker push `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/helidon-stock-application-your_first_name:1.0</copy>
        
    
    ![Push de Docker](images/docker-push.png " ")
    
    Después de que el comando _docker push_ se ejecute correctamente, amplíe el repositorio _`helidon-stock-application-your_first_name`_ y observará que se ha cargado una nueva imagen en este repositorio.
    
    Deje abierta la página del repositorio de _Cloud Shell_ y Container Registry; las necesitaremos para las próximas prácticas.
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/fecha**: Ankit Pandey, marzo de 2023