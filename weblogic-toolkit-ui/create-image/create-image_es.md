# Creación de una imagen auxiliar y transferencia a Oracle Container Image Registry

## Introducción

**Imagen principal**: imagen que contiene el software de Oracle Fusion Middleware. Se utiliza como base de todos los contenedores que ejecutan servidores WebLogic para el dominio.

**Imagen auxiliar**: imagen que proporciona el software WebLogic Deploy Tooling y los archivos de modelo. En tiempo de ejecución, el contenido de la imagen auxiliar se fusiona con el contenido de la imagen principal. ![Estructura de imagen](images/image-structure.png)

En este laboratorio, utilizamos la imagen 12.2.1.3.0-ol8 del servidor WebLogic como imagen principal. Además, creamos una imagen auxiliar y la transferimos al repositorio de Oracle Container Image Registry mediante el token de autenticación generado.

Tiempo estimado: 10 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Creación de imágenes para OKE en OCI](videohub:1_y5o56oe5)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Cree una imagen auxiliar y transfiera la imagen a Oracle Cloud Container Image Registry.

## Tarea 1: Preparación de la imagen auxiliar y transferencia de la imagen auxiliar

En esta tarea, estamos creando una imagen auxiliar que transferiremos a Oracle Cloud Container Registry.

1.  Haga clic en _Imagen_. Para la imagen principal, utilizaremos los siguientes valores por defecto de _weblogic_ Image.So en la sección _Imagen principal_, como se muestra
    
        <copy>container-registry.oracle.com/middleware/weblogic:12.2.1.3-ol8</copy>
        
    
    ![Imagen principal](images/primary-image.png)
    
    > **Solo para su información:**  
    > la imagen principal es la que se utiliza para ejecutar el dominio. Una imagen principal se puede reutilizar para cientos de dominios. La imagen principal contiene las instalaciones de software del sistema operativo, JDK y FMW.
    
2.  Para crear la etiqueta de imagen auxiliar, necesitamos la siguiente información:
    
    *   Punto final de la región
    *   Espacio de nombres de arrendamiento
3.  Localice el _punto final de la región_. Consulte la tabla documentada en esta URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). En el ejemplo que se muestra, el punto final de la región es _Sur de Reino Unido (Londres)_ (como nombre de región) y su punto final es _lhr.ocir.io_. Localice el punto final de su propio _nombre de región_ y guárdelo en el archivo de texto. También lo necesitará para la siguiente práctica de laboratorio.
    
    ![Puntos finales](images/end-point.png " ")
    
    > Ahora tiene tanto el espacio de nombres de arrendamiento como el punto final para su región.
    
4.  En el laboratorio 3, ya ha anotado el espacio de nombres de arrendamiento en el archivo de texto. Si no es así, para buscar el espacio de nombres del arrendamiento, seleccione el _menú de hamburguesa_ -> _Servicios para desarrolladores_ -> _Registro de contenedores_, como se muestra. Seleccione el repositorio que ha creado, encontrará el espacio de nombres como se muestra. ![Espacio de nombres de arrendamiento](images/tenancy-namespace.png)
    
5.  Ahora tiene tanto el espacio de nombres de arrendamiento como el punto final para su región. Copie el siguiente comando y péguelo en el archivo de texto. A continuación, sustituya `END_POINT_OF_YOUR_REGION` por el punto final del nombre de región, `NAMESPACE_OF_YOUR_TENANCY` por el espacio de nombres de su arrendamiento. Haga clic en el separador _Imagen auxiliar_, como se muestra. ![Ficha Auxiliar](images/auxiliary-tab.png)
    
        <copy>END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/test-model-your_first_name:v1</copy>
        

> Por ejemplo, en mi caso, la etiqueta de imagen auxiliar es `lhr.ocir.io/tenancynamespace/test-model-ankit:v1`.

6.  En el paso 4, también ha determinado el espacio de nombres del arrendamiento. Introduzca el nombre de usuario push del registro de imágenes auxiliar como se indica a continuación: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    

*   Sustituya `NAMESPACE_OF_YOUR_TENANCY` por el espacio de nombres de su arrendamiento
*   Sustituya `YOUR_ORACLE_CLOUD_USERNAME` por el nombre de usuario de su cuenta de Oracle Cloud y, a continuación, copie el nombre de usuario sustituido del archivo de texto y péguelo en _Nombre de usuario push de registro de imágenes auxiliares_.

> Por ejemplo, en mi caso, **Nombre de usuario push de registro de imagen auxiliar** es `tenancynamespace/lab.user@oracle.com`.

*   Para Contraseña, copie y pegue el token de autenticación del archivo de texto (o donde lo haya guardado) y péguelo en el **nombre de usuario push del registro de imágenes auxiliar**. ![Detalles de imagen auxiliar](images/auxiliary-image-details.png)

7.  Haga clic en _Crear imagen auxiliar_. ![Crear imagen auxiliar](images/create-auxiliary-image.png)
    
8.  Como ya hemos preparado el modelo en el laboratorio 2, haga clic en _No_. ![Preparar modelo](images/prepare-model.png)
    
9.  Seleccione la carpeta _Descargas_ en la que desea guardar _WebLogic Deployer_ y haga clic en _Seleccionar_ como se muestra. ![Ubicación de WDT](images/wdt-location.png)
    
10.  Una vez creadas correctamente las imágenes auxiliares, en la ventana _Crear imagen auxiliar completa_, haga clic en _Aceptar_. ![Auxiliar creado](images/auxiliary-created.png)
    
    > **Solo para su información:**  
    > una imagen auxiliar es específica del dominio. La imagen auxiliar contiene los datos que definen el dominio.
    
11.  Haga clic en _Transferir imagen auxiliar_ para transferir la imagen al repositorio dentro de Oracle Cloud Container Image Registry. ![Insertar auxiliar](images/push-auxiliary.png)
    
12.  Una vez que la imagen se haya transferido correctamente, en la ventana _Imagen push finalizada_, haga clic en _Aceptar_. ![Auxiliar empujado](images/auxiliary-pushed.png)
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023