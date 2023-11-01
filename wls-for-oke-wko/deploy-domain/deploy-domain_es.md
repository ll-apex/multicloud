# Despliegue del dominio WebLogic en el cluster de Kubernetes de Oracle (OKE)

## Introducción

En este laboratorio, desplegamos el dominio WebLogic en el cluster de kubernetes. En la sección de imagen principal, especificamos la credencial de cuenta oracle. En la sección de imagen auxiliar, especificamos la credencial de cuenta de oracle cloud. Aquí también especificamos la réplica para el cluster.

Tiempo estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Desplegar el dominio WebLogic en el cluster de Kubernetes.

## Tarea 1: Despliegue del dominio WebLogic en el cluster de Kubernetes de Oracle

En esta tarea, desplegamos el recurso personalizado de Kubernetes para el dominio WebLogic en el cluster de Kubernetes.

1.  Desplácese hacia abajo, introduzca lo siguiente en la sección Imagen principal, introduzca _domain-secret_ como _Image Pull Secret Name_ y utilice el nombre de usuario y la contraseña de la cuenta de Oracle en _Image Registry Pull Username_ y _Image Registry Pull Password_. Introduzca su ID de correo electrónico de Oracle en _Image Registry Pull Email Address_. Estas son las mismas credenciales que ha utilizado para aceptar licencias para imágenes _weblogic_ en Oracle Container Registry. ![Detalles de imagen principal](images/primary-image-details.png)
    
    > **Sólo para su información:**  
    > estamos extrayendo la imagen de Oracle Container Registry, por lo que estamos especificando la credencial, que hemos utilizado para aceptar el acuerdo de licencia para las imágenes de servidor WebLogic.
    
2.  Desplácese hacia abajo, en la sección Imagen auxiliar, desactive la casilla **Especificar credenciales de extracción de imagen auxiliar**.
    
3.  En la sección _Clusters_, haga clic en el icono _Editar_ como se muestra. ![Cambio de tamaño de cluster](images/cluster-resize.png)
    
4.  Introduzca _2_ como _Replicas_ y, a continuación, haga clic en _Aceptar_. El tamaño de la réplica decide el número de servidores gestionados en estado _En ejecución_ después del despliegue correcto del dominio WebLogic en el cluster de Kubernetes. ![Replicas de cluster](images/cluster-replicas.png)
    
5.  En la sección Orígenes de datos, haga doble clic para editar _contraseñas_ para dos orígenes de datos. Puede proporcionar _tiger_ como contraseña en ambos orígenes de datos. Cuando haya terminado, haga clic en _Desplegar dominio_. ![Contraseña de almacén de datos](images/datasource-password.png)
    
    > Este despliegue de dominio de prueba WebLogic en el espacio de nombres de Kubernetes _test-domain-ns_.
    
6.  Una vez que vea la ventana _WebLogic Despliegue de dominio en Kubernetes completo_, haga clic en _Aceptar_. ![Despliegue finalizado](images/deployment-complete.png)
    
7.  Vuelva al terminal, haga clic en _Actividades_ y seleccione la ventana _Terminal_. Copie el siguiente comando y péguelo en el terminal. Debe ver la salida similar, donde pod para introspector se ejecuta primero para el servidor de administración y luego pods para el servidor gestionado pasa al estado _En ejecución_.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Estado del pod](images/pod-status.png)
    
8.  También puede obtener el estado del dominio mediante _WebLogic IU de Kubernetes Toolkit_. Vuelva a _WebLogic IU de Kubernetes Toolkit_ y haga clic en _Obtener estado de dominio_. ![Estado de dominio](images/domain-status.png)
    
9.  En la ventana Domain Status (Estado de dominio), desplácese hacia abajo para ver el estado de todos los pods del servidor y, a continuación, haga clic en _OK_ (Aceptar). ![Estado del Servidor](images/server-status.png)
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, octubre de 2023