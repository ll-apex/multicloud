# Prueba del Despliegue de la Aplicación

## Introducción

### Acerca de WebLogic Remote Console

WebLogic Remote Console es una consola ligera de código abierto que puede utilizar para gestionar el dominio del servidor WebLogic que se ejecuta en cualquier lugar, como en una máquina física o virtual, en un contenedor, Kubernetes o en Oracle Cloud. La consola remota WebLogic no tiene que estar colocada con el dominio de servidor WebLogic.

Puede instalar y ejecutar la consola remota WebLogic en cualquier lugar y conectarse a su dominio mediante las API de REST WebLogic. Simplemente inicie la aplicación de escritorio y conéctese al servidor de administración de su dominio. O bien, puede iniciar el servidor de la consola, iniciar la consola en un explorador y, a continuación, conectarse al servidor de administración.

La consola remota WebLogic está totalmente soportada con el servidor WebLogic 12.2.1.3, 12.2.1.4 y 14.1.1.0.

**Funciones clave de WebLogic Remote Console**

*   Configurar instancias y clusters de servidor WebLogic
*   Crear o modificar modelos de metadatos WDT
*   Configurar servicios de servidor WebLogic, como conectividad de base de datos (JDBC) y mensajes (JMS)
*   Despliegue y anulación de despliegue de aplicaciones
*   Inicio y Parada de Servidores y Aplicaciones
*   Supervisar el rendimiento del servidor y las aplicaciones

En este laboratorio, accedemos a la aplicación _opdemo_ y verificamos la migración correcta de un dominio local fuera de línea. También verificamos el equilibrio de carga entre los pods de servidor gestionado. Posteriormente, utilizamos WebLogic Remote Console para verificar el despliegue correcto de recursos de dominio de prueba en el entorno de kubernetes.

Tiempo estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Acceda a la aplicación a través del explorador.
*   Explore el dominio WebLogic mediante WebLogic Remote Console.

## Tarea 1: Acceso a la aplicación a través del explorador

En esta tarea, accedemos a la aplicación _opdemo_. Hacemos clic en el icono de refrescamiento para realizar varias solicitudes a la aplicación, para verificar el equilibrio de carga entre dos pods de servidor gestionado.

1.  Copie la siguiente URL y sustituya _XX.XX.XX.XX_ por su IP, que anotó en la última práctica. Puede ver la siguiente salida.
    
        <copy>http://XX.XX.XX.XX/opdemo/?dsname=testDatasource</copy>
        
    
    ![Abrir Aplicación](images/open-application.png)
    
2.  Si hace clic en el icono Refrescar, puede ver el equilibrio de carga entre dos pods de servidor gestionado. ![Mostrar equilibrio de carga](images/show-load-balancing.png)
    

## Tarea 2: Exploración del dominio WebLogic en el cluster de Kubernetes mediante WebLogic Remote Console

En esta tarea, exploramos WebLogic Remote Console. Creamos una conexión al _servidor de administración_ en Remote Console y verificamos los recursos en el dominio WebLogic. Esto verifica la migración correcta de un dominio local al cluster de Kubernetes de Oracle.

1.  Para abrir WebLogic Remote Console, haga clic en _Actividades_, escriba _WebLogic_ en el cuadro de búsqueda y haga clic en el icono _WebLogic Remote Console_.
    
2.  Haga clic en `Three dots` en _Quiosco_, seleccione _Agregar proveedor de conexión de servidor de administración_ y haga clic en _Seleccionar_. ![Conexión de servidor de administración](images/adminserver-connection.png)
    
3.  Introduzca los siguientes datos y haga clic en _Aceptar_.  
    Nombre de proveedor de conexión: AdminServer  
    Nombre de usuario: weblogic  
    Contraseña: welcome1  
    URL: `Copy_IP_From_TextFile`  
    ![Detalles de la Conexión](images/connection-details.png)
    
4.  Haga clic en el icono _Editar árbol_ y, a continuación, seleccione _Servicios_ -> _Orígenes de datos_. Puede observar el mismo Datasouce, que habíamos visto en el dominio local. ![Verificar orígenes de datos](images/verify-datasources.png)
    
5.  Para mostrar los servidores que se están ejecutando en el dominio. Haga clic en el icono **Árbol de Supervisión** como se muestra y, a continuación, seleccione **Entorno** -> **Servidores**. Puede ver que tenemos **servidor de administración** y 2 pods de servidor gestionado en ejecución. ![Servidores en ejecución](images/running-server-status.png)
    
6.  Haga clic en **admin-server**; puede ver que la versión WebLogic es **12.2.1.3.0**. ![Estado del Servidor](images/wls-version.png)
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, octubre de 2023