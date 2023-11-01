# Realice una actualización en la versión del servidor WebLogic (opcional)

## Introducción

En este laboratorio, modificamos la imagen principal, utilizamos la imagen de servidor WebLogic con la etiqueta _12.2.1.4-slim-ol8_. A continuación, volvemos a desplegar el dominio mediante la interfaz de usuario del kit de herramientas de Kubernetes WebLogic. Por fin, verificamos que los pods de servidor recién gestionados están utilizando las imágenes de servidor WebLogic actualizadas mediante WebLogic Remote Console.

Tiempo estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Utilice WebLogic Server Image (12.2.1.4) como imagen principal.
*   Vuelva a desplegar el dominio WebLogic.

### Requisitos

*   Acceso a noVNC Remote Desktop creado en el laboratorio 1.

## Tarea 1: Introduzca el detalle de la nueva imagen del servidor WebLogic como imagen principal

En esta tarea, actualizamos la imagen principal para utilizar la imagen del servidor WebLogic 12.2.1.4.0 actualizada.

1.  Vuelva a la interfaz de usuario del kit de herramientas de Kubernetes WebLogic y haga clic en _Dominio WebLogic_. Se ha cambiado la etiqueta de servidor WebLogic a _12.2.1.4-slim-ol8_. ![Actualizar etiqueta de imagen principal](images/update-primary-image-tag.png)

## Tarea 2: Actualización de una aplicación desplegada mediante un reinicio sucesivo de los pods del servidor

En esta tarea, se vuelve a desplegar el dominio WebLogic. Posteriormente, utilizamos WebLogic Remote Console para verificar que los pods del servidor estén utilizando la imagen actualizada del servidor WebLogic 12.2.1.4.0.

1.  Haga clic en _Desplegar dominio_. De esta forma, se volverá a desplegar el dominio. ![Volver a desplegar dominio](images/redeploy-domain.png)
    
    > **Solo para su información:**  
    > A medida que cambiamos nuestra imagen principal, observaremos el reinicio sucesivo de los servidores uno por uno. Al hacer clic en _Desplegar dominio_, inicia un _trabajo de introspector_, que termina los pods del servidor de administración en ejecución y crea un nuevo pod para el servidor de administración que utiliza la imagen WebLogic Server 12.2.1.4.0. El introspector realiza el mismo proceso con ambos servidores gestionados.
    
2.  Una vez que vea la ventana _WebLogic Despliegue de dominio en Kubernetes completo_, haga clic en _Aceptar_. ![Despliegue finalizado](images/deployment-complete.png)
    
3.  Vuelva a _Terminal_, copie el siguiente comando y péguelo en el terminal. Observará el reinicio sucesivo de los servidores uno por uno. En primer lugar, los pods del servidor de administración terminan y tienen el estado _En ejecución_.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Ver pods](images/view-pods.png)
    
4.  Para verificar que los pods de servidor de administración y servidor gestionado estén utilizando la imagen de servidor WebLogic actualizada, haga clic en el icono de árbol de supervisión y, a continuación, seleccione _Entorno_ -> _Servidores_ -> _admin-server_. Puede ver que utiliza 12.2.1.4.0. ![Versión de WLS](images/wls-version.png)
    

Congratulation !!!

Este es el final del taller.

Esperamos que haya encontrado este taller útil.

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, octubre de 2023