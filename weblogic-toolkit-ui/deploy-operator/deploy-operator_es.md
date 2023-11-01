# Despliegue del operador WebLogic en el cluster de Kubernetes de Oracle (OKE)

## Introducción

En este laboratorio, autenticamos la CLI de OCI mediante el explorador, que creará el archivo _.OCI/config_. Como usaremos kubectl para gestionar el cluster de manera remota mediante _Local Access_. Necesita un archivo _kubeconfig_. Este archivo kubeconfig se generará mediante la CLI de OCI. A continuación, verificamos la conectividad al cluster de Kubernetes desde la interfaz de usuario del kit de herramientas de Kubernetes WebLogic. Por fin, instalamos el operador de Kubernetes WebLogic en el cluster de Kubernetes (OKE).

Tiempo estimado: 10 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Despliegue del operador WebLogic en el cluster de OKE](videohub:1_0itbllhe)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configure kubectl (CLI de cluster de Kubernetes) para conectarse al cluster de Kubernetes.
*   Verifique la conectividad de la interfaz de usuario del kit de herramientas de Kubernetes WebLogic al cluster de Kubernetes.
*   Instale el operador de Kubernetes WebLogic en el cluster de Kubernetes.

## Tarea 1: Configuración de kubectl (CLI de cluster de Kubernetes) para conectarse al cluster de Kubernetes de Oracle

En esta tarea, creamos el archivo de configuración _.oci/config_ y _.kube/config_ en el directorio _/home/opc_. Este archivo de configuración nos permite acceder al cluster de Kubernetes de Oracle (OKE) desde esta máquina virtual.

1.  Haga clic en _Actividades_ y escriba _Firefox_ en el cuadro de búsqueda. Haga clic en el icono de _Firefox_. ![abrir firefox](images/open-firefox.png)
    
2.  Abra la URL [https://cloud.oracle.com](https://cloud.oracle.com). Introduzca el _nombre de la cuenta en la nube_ y, a continuación, la credencial de la cuenta de Oracle Cloud y haga clic en _Conectar_.
    
3.  En la consola, seleccione el _menú de hamburguesa_ -> _Servicios para desarrolladores_ -> _Clusters de Kubernetes (OKE)_, como se muestra. ![icono de OKE](images/oke-icon.png)
    
4.  Haga clic en el nombre del cluster que ha creado en el laboratorio 3 y, a continuación, haga clic en _Acceder a cluster_. ![Acceder al cluster](images/access-cluster.png)
    
5.  Seleccione _Acceso local_ y, a continuación, haga clic en _Copiar_ como se muestra. ![Acceso Local](images/local-access.png)
    
6.  Haga clic en _Actividades_ y seleccione el _terminal_. ![El terminal](images/click-terminal.png)
    
7.  Pegue el comando copiado en el terminal. En _¿Desea crear un nuevo archivo de configuración?_, escriba _y_ y, a continuación, pulse _Intro_. En _¿Desea crear el archivo de configuración iniciando sesión mediante un explorador?_, escriba _y_ y, a continuación, pulse _Intro_. ![Configuración de OCI](images/oci-config.png)
    
8.  En el navegador Firefox, haz clic en tu sesión activa.
    
    > Verá _Autorización finalizada_ como se muestra. ![Autorización finalizada](images/authorization-complete.png)
    
9.  En _Introduzca una frase de contraseña para la clave privada_, déjela vacía y pulse _Intro_. ![Frase de contraseña vacía](images/empty-passphrase.png)
    
10.  Utilice la tecla de flecha superior para volver a ejecutar el comando _oce ce ..._ y volver a ejecutarlo varias veces, hasta que vea la _Nueva configuración escrita en el archivo Kubeconfig /home/opc/.kube/config_. ![Crear KubeConfig](images/create-kubeconfig.png)
    

## Tarea 2: Verificación de la conectividad de la interfaz de usuario de Kubernetes Toolkit de WebLogic al cluster de Oracle Kubernetes

En esta tarea, verificamos la conectividad al _cluster de Kubernetes de Oracle (OKE)_ desde la aplicación `WebLogic Kubernetes Toolkit UI`.

1.  Vuelva a la interfaz de usuario del kit de herramientas de Kubernetes WebLogic, haga clic en _Actividades_ y seleccione la ventana de interfaz de usuario del kit de herramientas de Kubernetes WebLogic.
    
2.  Haga clic en _Kubernetes_ -> _Configuración de cliente_ y, a continuación, en _Verificar conectividad_. ![Verificar Conectividad](images/verify-connectivity.png)
    
3.  Una vez que vea la ventana _Verificar éxito de conectividad de cliente de Kubernetes_, haga clic en _Aceptar_. ![Conexión correcta](images/successfully-connected.png)
    

## Tarea 3: Instalación del operador de Kubernetes WebLogic en el cluster de Kubernetes de Oracle

En esta sección se proporciona soporte para instalar el operador de Kubernetes WebLogic (el "operador") en el cluster de Kubernetes de destino.

1.  Haga clic en _Operador WebLogic_. Especifique los siguientes detalles de configuración y haga clic en _Operador de instalación_.
    
    **Espacio de nombres de Kubernetes**: espacio de nombres de Kubernetes en el que instalar el operador. Deje el valor por defecto.  
    **Cuenta de servicio de Kubernetes**: cuenta de servicio de Kubernetes que el operador utilizará al realizar solicitudes de API de Kubernetes. Deje el valor por defecto.  
    **Nombre de la versión de Helm que se utilizará para la instalación del operador**: nombre de la versión de Helm que se utilizará para identificar esta instalación. Deje el valor por defecto.  
    
    ![WebLogic Operador](images/weblogic-operator.png)
    
    > **Solo para su información:**  
    > ! Por defecto, el campo _Etiqueta de imagen a utilizar_ del operador se define en la etiqueta de imagen correspondiente a la última versión del operador en GitHub Container Registry.  
    > El operador necesita saber qué dominios WebLogic del cluster de Kubernetes gestionará. Lo hace en el nivel de espacio de nombres de Kubernetes, por lo que cualquier dominio WebLogic en un espacio de nombres de Kubernetes que el operador esté configurado para gestionar será gestionado por la instancia de operador que se instale.  
    > En el campo _Estrategia de selección de espacio de nombres de Kubernetes_, seleccionamos _Selector de etiqueta_, lo que significa que este operador gestionará cualquier espacio de nombres de Kubernetes con una etiqueta _weblogic-operator=enabled_.  
    > ![Imagen del operador](images/operator-image.png)
    
    > Al activar Activar Enlace de Roles de Cluster, la instalación del operador creará un ClusterRole y ClusterRoleBinding de Kubernetes que el operador utilizará para todos los espacios de nombres gestionados.  
    > Por defecto, la API de REST del operador no se expone fuera del cluster de Kubernetes. Para activar la exposición de la API de REST, puede activar _Exponer API de REST externamente_. [Enlace de Roles](images/role-binding.png)  
    
    > Este panel permite sustituir la configuración de registro de Java del operador, que puede ser útil al depurar problemas con el operador.  
    > ![Registro de Java](images/java-logging.png)  
    
    > Para obtener más información sobre _WebLogic Kubernetes Operator Image_, _Kubernetes Namespace Selection Strategy_, _WebLogic Kubernetes Role Bindings_, _External REST API Access_, _Third Party Integrations_ y _Java Logging_, consulte la documentación del [WebLogic Kubernetes Operator](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/).
    
2.  Una vez que vea _WebLogic Instalación del operador de Kubernetes completa_, haga clic en _Aceptar_. ![Operador instalado](images/operator-installed.png)
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023