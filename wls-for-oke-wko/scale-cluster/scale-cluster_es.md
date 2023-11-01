# Escalar un cluster WebLogic (opcional)

## Introducción

En este laboratorio, escalamos un cluster WebLogic. En este caso, modificamos el valor a _3_ y volvemos a desplegar el dominio.

Tiempo estimado: 5 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Escala un cluster WebLogic.

## Tarea 1: Escala de un cluster WebLogic mediante la interfaz de usuario del kit de herramientas de Kubernetes WebLogic

En esta tarea, solo tiene que modificar el valor de _Réplica_ de 2 a 3 y volver a desplegar el dominio.

1.  Vuelva a la interfaz de usuario del kit de herramientas de Kubernetes WebLogic y haga clic en _Dominio WebLogic_. Vaya a la sección _Clusters_ y haga clic en el icono _Editar_.  
    ![Cambio de tamaño de cluster](images/cluster-resize.png)
    
2.  Cambie las réplicas de _2_ a _3_ y haga clic en _Aceptar_. ![Cambiar réplicas](images/change-replicas.png)
    
3.  Para volver a desplegar el dominio, haga clic en _Desplegar Dominio_. ![Volver a desplegar dominio](images/redeploy-domain.png)
    
4.  Una vez que vea la ventana _WebLogic Despliegue de dominio en Kubernetes completo_, haga clic en _Aceptar_. ![Despliegue finalizado](images/deployment-complete.png)
    
5.  Vuelva a la ventana _Terminal_, haga clic en _Actividades_ y seleccione la ventana _Terminal_. Copie el siguiente comando y péguelo en el terminal.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Ver escala](images/view-scaling.png)
    
    > Puede ver que, al volver a desplegar el dominio, se inicia el trabajo de introspector, que inicia el proceso de creación de pod para test-domain-managed-server3 y, en algún momento, este pod pasa al estado _En ejecución_.
    
6.  Vuelva al explorador, donde tiene la página de la aplicación abierta. Haga clic en el botón Refresh. Ahora verá el equilibrio de carga entre tres servidores gestionados. ![nuevo servidor](images/new-server.png)
    
7.  Vuelva a WebLogic Remote Console y haga clic en _Monitoring Tree_ (Árbol de supervisión) -> _Environment_ (Entorno) -> _Servers_ (Servidores). Notará Managed-server3 aquí también. ![consola remota](images/remote-console.png)
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, octubre de 2023