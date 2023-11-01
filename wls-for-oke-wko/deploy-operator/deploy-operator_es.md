# Despliegue del operador WebLogic en el cluster de Kubernetes de Oracle (OKE)

## Introducción

En este laboratorio, autenticamos la CLI de OCI mediante el explorador, que creará el archivo _.OCI/config_. Como usaremos kubectl para gestionar el cluster de manera remota mediante _Local Access_. Necesita un archivo _kubeconfig_. Este archivo kubeconfig se generará mediante la CLI de OCI. A continuación, verificamos la conectividad al cluster de Kubernetes desde la interfaz de usuario del kit de herramientas de Kubernetes WebLogic. Por fin, instalamos el operador de Kubernetes WebLogic en el cluster de Kubernetes (OKE).

Tiempo estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configure kubectl (CLI de cluster de Kubernetes) para conectarse al cluster de Kubernetes.
*   Verifique la conectividad de la interfaz de usuario del kit de herramientas de Kubernetes WebLogic al cluster de Kubernetes.
*   Instale el operador de Kubernetes WebLogic en el cluster de Kubernetes.

## Tarea 1: Configuración de kubectl (CLI de cluster de Kubernetes) para conectarse al cluster de Kubernetes de Oracle

En esta tarea, creamos el archivo de configuración _.oci/config_ y _.kube/config_ en el directorio _/home/opc_. Este archivo de configuración nos permite acceder al cluster de Kubernetes de Oracle (OKE) desde esta máquina virtual.

1.  En Firefox, abra la consola en la nube, seleccione el **Menú de hamburguesa** -> **Servicios para desarrolladores** -> **Clusters de Kubernetes (OKE)**, como se muestra. ![icono de OKE](images/oke-icon.png)
    
2.  Haga clic en el nombre del cluster que ha creado en el laboratorio 3 y, a continuación, haga clic en **Acceder a cluster**. ![Acceder al cluster](images/access-cluster.png)
    
3.  Seleccione **Acceso local** y, a continuación, haga clic en **Copiar** como se muestra. ![Acceso Local](images/local-access.png)
    
4.  Haga clic en **Actividades** y seleccione el **terminal**. ![El terminal](images/click-terminal.png)
    
5.  Pegue el comando copiado en el terminal. En **¿Desea crear un nuevo archivo de configuración?**, escriba **y** y, a continuación, pulse **Intro**. En **¿Desea crear el archivo de configuración iniciando sesión mediante un explorador?**, escriba **y** y, a continuación, pulse **Intro**. ![Configuración de OCI](images/oci-config.png)
    
6.  En el navegador Firefox, haz clic en tu sesión activa.
    
    > Verá _Autorización finalizada_ como se muestra. ![Autorización finalizada](images/authorization-complete.png)
    
7.  En **Introduzca una frase de contraseña para la clave privada**, déjela vacía y pulse **Intro**. ![Frase de contraseña vacía](images/empty-passphrase.png)
    
8.  Utilice la tecla de flecha superior para volver a ejecutar el comando **oce ce ...** y volver a ejecutarlo varias veces, hasta que vea la **Nueva configuración escrita en el archivo Kubeconfig /home/opc/.kube/config**. ![Crear KubeConfig](images/create-kubeconfig.png)
    
9.  Copie y pegue el siguiente comando para cambiar los permisos de archivo **~/.kube/config**.
    
        <copy>chmod 600 ~/.kube/config</copy>
        

## Tarea 2: Visualización de los recursos en la nube de Oracle WebLogic Server para la pila de OKE

1.  En la consola de OCI, haga clic en el **menú de navegación** y seleccione **Servicios para desarrolladores**. En el grupo Gestor de recursos, haga clic en **Pilas**.
    
2.  Seleccione el **compartimento** que contiene la pila.
    
3.  Haga clic en el nombre de la pila.
    
4.  Vaya al separador **Información de la aplicación**. Puede ver los recursos creados por la pila. Copie la **IP pública de instancia de Bastion** y péguela en el archivo de texto. ![copiar bastión](images/copy-bastion.png)
    

## Tarea 3: Activar la configuración de proxy en el terminal

En esta tarea, activamos el **túnel SSH** del escritorio remoto a los nodos de cluster de Oracle Kubernetes.

1.  Abra el terminal, copie y pegue el siguiente comando para crear el archivo de clave privada en el escritorio remoto.
    
        <copy>vi ~/.ssh/opc.key</copy>
        
2.  Pulse **i** para entrar en modo de inserción y pegue el contenido de la clave privada en este archivo, pulse escape y, a continuación, introduzca **:wq** para guardar este archivo.
    
3.  Copie y pegue el siguiente comando para cambiar los permisos de archivo de clave privada.
    
        <copy>chmod 600 ~/.ssh/opc.key</copy>
        
4.  Copie el siguiente comando, péguelo en el terminal y, a continuación, sustituya **bastion-public-ip** por la IP pública que copió en la última tarea.
    
        <copy>ssh -D 1088 -fCqN -i ~/.ssh/opc.key opc@bastion-public-ip</copy>
        
5.  Abra el archivo **~/.kube/config** y, a continuación, copie y pegue lo siguiente en este archivo, como se muestra a continuación.
    
        <copy>proxy-url: socks5://localhost:1088</copy>
        
    
    ![configuración del proxy](images/proxy-config.png)
    

## Tarea 4: Verificación de la conectividad de la interfaz de usuario de Kubernetes Toolkit de WebLogic al cluster de Oracle Kubernetes

En esta tarea, verificamos la conectividad al _cluster de Kubernetes de Oracle (OKE)_ desde la aplicación `WebLogic Kubernetes Toolkit UI`.

1.  Vuelva a la interfaz de usuario del kit de herramientas de Kubernetes WebLogic, haga clic en _Actividades_ y seleccione la ventana de interfaz de usuario del kit de herramientas de Kubernetes WebLogic.
    
2.  Haga clic en _Kubernetes_ -> _Configuración de cliente_ y, a continuación, en _Verificar conectividad_. ![Verificar Conectividad](images/verify-connectivity.png)
    
3.  Una vez que vea la ventana _Verificar éxito de conectividad de cliente de Kubernetes_, haga clic en _Aceptar_. ![Conexión correcta](images/successfully-connected.png)
    

## Tarea 5: Actualización del operador de Kubernetes WebLogic al cluster de Kubernetes de Oracle

En esta sección se proporciona soporte para instalar el operador de Kubernetes WebLogic (el "operador") en el cluster de Kubernetes de destino.

1.  Haga clic en **Operador WebLogic**. Especifique los siguientes detalles de configuración y haga clic en **Actualizar operador**.
    
    **Espacio de nombres de Kubernetes**: espacio de nombres de Kubernetes en el que instalar el operador. Introduzca el valor **wko-operator-ns** como se muestra en la captura de pantalla siguiente.
    
    **Cuenta de servicio de Kubernetes**: cuenta de servicio de Kubernetes que el operador utilizará al realizar solicitudes de API de Kubernetes. Introduzca el valor **wko-operator-sa** como se muestra en la captura de pantalla siguiente.
    
    **Nombre de la versión de Helm que se utilizará para la instalación del operador**: nombre de la versión de Helm que se utilizará para identificar esta instalación. Introduzca el valor **wko-weblogic-operator**, como se muestra en la captura de pantalla siguiente.
    
    ![WebLogic Operador](images/weblogic-operator.png)
    
    > **Solo para su información:**  
    > ! Por defecto, el campo _Etiqueta de imagen a utilizar_ del operador se define en la etiqueta de imagen correspondiente a la última versión del operador en GitHub Container Registry.  
    > El operador necesita saber qué dominios WebLogic del cluster de Kubernetes gestionará. Lo hace en el nivel de espacio de nombres de Kubernetes, por lo que cualquier dominio WebLogic en un espacio de nombres de Kubernetes que el operador esté configurado para gestionar será gestionado por la instancia de operador que se instale.  
    > En el campo _Estrategia de selección de espacio de nombres de Kubernetes_, seleccionamos _Selector de etiqueta_, lo que significa que este operador gestionará cualquier espacio de nombres de Kubernetes con una etiqueta _weblogic-operator=enabled_.  
    > ![Imagen del operador](images/operator-image.png)
    
    > Al activar Activar Enlace de Roles de Cluster, la instalación del operador creará un ClusterRole y ClusterRoleBinding de Kubernetes que el operador utilizará para todos los espacios de nombres gestionados.  
    > Por defecto, la API de REST del operador no se expone fuera del cluster de Kubernetes. Para activar la exposición de la API de REST, puede activar _Exponer API de REST externamente_. ![Enlace de roles](images/role-binding.png)  
    
    > Este panel permite sustituir la configuración de registro de Java del operador, que puede ser útil al depurar problemas con el operador.  
    > ![Registro de Java](images/java-logging.png)  
    
    > Para obtener más información sobre _WebLogic Kubernetes Operator Image_, _Kubernetes Namespace Selection Strategy_, _WebLogic Kubernetes Role Bindings_, _External REST API Access_, _Third Party Integrations_ y _Java Logging_, consulte la documentación del [WebLogic Kubernetes Operator](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/).
    
2.  Una vez que vea _WebLogic Instalación del operador de Kubernetes completa_, haga clic en _Aceptar_. ![Operador instalado](images/operator-installed.png)
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, octubre de 2023