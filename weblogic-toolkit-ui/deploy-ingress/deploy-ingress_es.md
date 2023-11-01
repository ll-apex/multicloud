# Despliegue del controlador de entrada en el cluster de Kubernetes de Oracle (OKE)

## Introducción

En este laboratorio, instalamos el controlador de entrada _Traefik_. Posteriormente, actualizamos las _rutas de entrada_ para acceder a la aplicación y al servidor de administración.

Tiempo estimado: 10 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Despliegue del controlador de entrada en el cluster de OKE](videohub:1_4kih00fi)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Instalar el controlador de entrada en el cluster de Kubernetes.
*   Actualice las rutas de entrada.

## Tarea 1: Instalación del controlador de entrada en el cluster de Oracle Kubernetes

En esta tarea, instalamos el _controlador de entrada_.

1.  Haga clic en _Controlador de entrada_. Puede ver algunos valores rellenados previamente, dejar que sigan siendo los mismos y hacer clic en _Install Ingress Controller_. ![Instalar controlador de entrada](images/install-ingress-controller.png)
    
    > **Solo para su información:**  
    > Esto instala correctamente el controlador de entrada _traefik-operator_ en el espacio de nombres de Kubernetes _traefik-ns_.
    
2.  Una vez que vea _Ingress Controller Installation Complete_, haga clic en _Ok_ (Aceptar). ![Controlador de entrada instalado](images/ingress-controller-installed.png)
    

## Tarea 2: Actualización de las rutas de entrada para acceder a la aplicación

En esta tarea, agregamos las rutas de entrada para acceder a la consola de administración, la aplicación. Al final, obtenemos los puntos finales accesibles.

1.  Desplácese hacia abajo y haga clic en _+_ icone para agregar la _configuración de ruta de entrada_. ![Agregar rutas de entrada](images/add-ingress-routes.png)
    
2.  Haga clic en el icono Editar como se muestra para modificar los valores. ![Editar entrada](images/edit-ingress.png)
    
3.  Introduzca los siguientes detalles y haga clic en _Aceptar_.  
    Nombre: consola  
    Expresión de ruta: /console  
    Espacio de nombres de servicio de destino: test-domain-ns  
    Servicio de destino: test-domain-admin-server  
    Puerto de destino: 7001  
    
    ![Entrada de consola](images/console-ingress.png)
    
4.  De forma similar, agregue también las siguientes rutas de entrada de _opdemo_:  
    Nombre: opdemo  
    Expresión de ruta: /opdemo  
    Espacio de nombres de servicio de destino: test-domain-ns  
    Servicio de destino: test-domain-cluster-cluster-1  
    Puerto de destino: 8001  
    ![Entrada de Opdemo](images/opdemo-ingress.png)
    
5.  De forma similar, agregue también las siguientes rutas de entrada de la _consola remota_:  
    Nombre: consola remota  
    Expresión de ruta: /  
    Espacio de nombres de servicio de destino: test-domain-ns  
    Servicio de destino: test-domain-admin-server  
    Puerto de destino: 7001  
    ![Entrada de consola remota](images/remote-console-ingress.png)
    
6.  Para actualizar las rutas de entrada, haga clic en _Actualizar rutas de entrada_. ![Actualizar rutas de entrada](images/update-ingress-routes.png)
    
7.  En _Actualizar rutas de entrada existentes_, haga clic en _Sí_.
    
8.  Una vez que vea la ventana _Actualización de rutas de entrada finalizada_, haga clic en _Aceptar_. ![Actualizar entrada Terminada](images/update-ingress-complete.png)
    
    > Debe anotar esta IP y guardarla en el archivo de texto.
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023