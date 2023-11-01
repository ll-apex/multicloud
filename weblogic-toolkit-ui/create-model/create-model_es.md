# Modificar un proyecto de interfaz de usuario de WKT y crear un archivo de modelo

## Introducción

En este laboratorio, exploramos el dominio WebLogic local. Navegamos por la consola de administración para ver la aplicación, los orígenes de datos y los servidores desplegados en _test-domain_. También abrimos el archivo _`base_project.wktproj`_ creado previamente, que ya tiene valores rellenados previamente para la sección _Configuración de proyecto_. A continuación, creamos el archivo de modelo mediante la introspección de un dominio local fuera de línea. Por fin, validamos el modelo y preparamos el modelo para que se despliegue en el cluster de Kubernetes (OKE) de Oracle.

Tiempo estimado: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Creación de modelo para OKE en OCI](videohub:1_qdch3qqg)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Explore el dominio WebLogic local _test-domain_.
*   Abra el proyecto WKT base.
*   Introspección de un dominio local fuera de línea.
*   Permite validar y preparar el modelo.

### Requisitos

Para ejecutar este laboratorio, debe tener:

*   Acceso a noVNC Remote Desktop creado en el laboratorio 2.

## Tarea 1: Exploración del dominio local

En esta tarea, navegamos por los recursos del _dominio de prueba_ local mediante la consola de administración WebLogic.

1.  En el lado izquierdo, haga clic en _Icono de flecha_. ![Portapapeles](images/clipboard.png)

> **Importante**: puede ver el _Clipboard_. Para copiar y pegar entre la máquina host y el escritorio remoto, utilizamos el _Clipboard_. Por ejemplo, si desea copiar desde la máquina host y desea pegarlo en el escritorio remoto, primero debe pegar en el portapapeles y, a continuación, puede pegarlo en el escritorio remoto. Vuelva a hacer clic en _Icono de flecha_ para ocultar la opción _Configuración_.

2.  Haga clic en el separador _Oracle WebLogic Server_ e introduzca _weblogic/Welcome1%_ como `Username/Password` y, a continuación, haga clic en _Iniciar sesión_. Puede ver que tenemos la versión de servidor WebLogic _12.2.1.3.0_.  
    ![Consola de administración de conexión](images/login-admin-console.png)
    
3.  Para ver los servidores disponibles, amplíe _Entorno_ y haga clic en _Servidores_. Puede ver que tenemos un cluster dinámico con 5 servidores gestionados. ![Ver servidores](images/view-servers.png)
    
4.  Para ver los orígenes de datos, amplíe _Servicios_ y haga clic en _Orígenes de datos_. ![Ver orígenes de datos](images/view-datasources.png)
    
5.  Para ver la aplicación desplegada, haga clic en _Despliegue_. Puede ver que tenemos _opdemo_ como aplicación desplegada. ![Ver despliegue](images/view-deployments.png)
    

## Tarea 2: Apertura del proyecto base de interfaz de usuario de WKT

Para simplificar el laboratorio, hemos creado _`base_project.wktproj`_, que establece previamente la ubicación de docker, Java, directorio raíz de Oracle, etiqueta de imagen principal. En esta tarea, abrimos el proyecto _`base_project.wktproj`_.

1.  Haga clic en _Actividades_ y, a continuación, escriba **WebLogic** en el cuadro de búsqueda. Haga clic en el icono de _WebLogic IU de Kubernetes Toolkit_. ![Abrir WKTUI](images/open-wktui.png)
    
2.  Para abrir el proyecto _base\_project.wktproj_, haga clic en _Archivo_ -> _Abrir proyecto_. ![Abrir proyecto](images/open-project.png)
    
3.  Haga clic en _Descargas_ en el lado izquierdo y, a continuación, seleccione _base\_project.wktproj_ y haga clic en _Abrir proyecto_. ![Ubicación del proyecto](images/project-location.png)
    
    > **For your information only:**  
    > As _Credential Story Policy_, we select **Store in Native OS Credentials Store**. It means the credentials (like for WebLogic Server and datasources) are only stored on the local machine.  
    > For _Where would you like the target Oracle Fusion Middleware domain to live?_, we select **Created in the container from the model in the image**. In this case, the set of model-related files are added to the image. So when the WebLogic Kubernetes Operator domain object is deployed, its inspector process runs and creates the WebLogic Server domain inside a running container on-the-fly.  
    > ![Configuración de Proyecto](images/project-settings.png) As _Kubernetes Environment Target Type_, we select **WebLogic Kubernetes Operator**. This means, you want this domain to be deployed in Kubernetes managed by the WebLogic Kubernetes Operator. This settings also determine what sections and their associated actions within the application, to display.  
    > we also specify the location for _JAVA HOME_ and _ORACLE\_HOME_. WebLogic Kubernetes Toolkit UI uses this directory when invoking the WebLogic Deployer Tooling and WebLogic Image Tool.  
    > To build new images, inspect images and interact with image repositories, the WKT UI application uses an image build tool, which defaults to docker.  
    > ![Tipo de cluster de Kubernetes](images/kubernetes-cluster-type.png)
    
4.  Introduzca _welcome1_ como **Contraseña** y, a continuación, haga clic en _Desbloquear_. ![desbloquear](images/unlock.png)
    

## Tarea 3: Introspección de un dominio local fuera de línea

En esta tarea, realizamos la introspección de un dominio local, que crea un archivo de modelo que consta de la configuración del dominio.

1.  En la interfaz de usuario del kit de herramientas de Kubernetes WebLogic, haga clic en _Modelo_. ![Modelo](images/click-model.png)
    
2.  Haga clic en _File_ -> _Add Model_ -> _Discover Model(offline)_. ![Detección de modelo](images/discover-model.png)
    
3.  Haga clic en el _icono_ de Abrir carpeta para abrir el _directorio raíz de dominio_. ![Abrir Dominio Hom](images/open-domain-home.png)
    
4.  En la carpeta de inicio, vaya al directorio _`/home/opc/Oracle/Middleware/Oracle_Home/user_projects/domains/`_ y seleccione la carpeta _test-domain_ y, a continuación, haga clic en _Seleccionar_. Haga clic en _Aceptar_. ![Navegar por ubicación](images/navigate-location.png) ![Especificar Ubicación](images/specify-location.png)
    
    > Si busca en la consola, verá que esto llama a WebLogic Deployer Tool para realizar una introspección de la configuración de dominio en modo fuera de línea.
    
5.  Puede ver la ventana como se muestra a continuación, al final, tendrá el modelo listo para usted. ![Ver modelo](images/view-model.png)
    
    > El resultado de esta introspección de WDT son el modelo (una representación de metadatos de la configuración del dominio), el marcador de posición, donde puede especificar los valores (como la contraseña para el origen de datos) y la aplicación en el archivo de la aplicación.
    

## Tarea 4: Validar y preparar el modelo

En esta tarea, validamos el modelo y preparamos el modelo para desplegarlo en el cluster de Kubernetes de Oracle (OKE).

1.  Haga clic en _Vista de código_ y, para validar el modelo, haga clic en _Validar modelo_. ![Validar modelo](images/validate-model.png)
    
    > **Solo para su información:**  
    > Validar modelo llama a la [herramienta Validar modelo](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/validate/) de WDT, que valida que el modelo y sus artefactos relacionados tienen el formato correcto y proporciona ayuda sobre los atributos y subcarpeta válidos para una ubicación de modelo concreta.
    
2.  Una vez que vea la ventana _Validar modelo completo_, haga clic en _Aceptar_. ![Validación finalizada](images/validate-complete.png)
    
3.  Para preparar el modelo, que se desplegará en el cluster de Kubernetes, haga clic en _Preparar modelo_ ![Preparar modelo](images/prepare-model.png)
    
    > **Solo para su información:**  
    > el modelo de preparación llama a la [herramienta de preparación de modelos](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/prepare/) de WDT para modificar el modelo para que funcione en un cluster de Kubernetes con el operador de Kubernetes WebLogic o Verrazzano instalado.  
    > Prepare Model hace lo siguiente:
    
    *   Elimina secciones y campos de modelo que no son compatibles con el entorno de destino.
    *   Sustituye los valores de punto final por tokens de modelo que hacen referencia a variables.
    *   Sustituye los valores de credenciales por tokens de modelo que hacen referencia a un campo de un secreto de Kubernetes o a una variable.
    *   Proporciona valores por defecto para los campos que se muestran en la variable de la aplicación, las sustituciones de variables y los editores de secretos.
    *   Extrae información de topología en la aplicación que utiliza para generar el archivo de recursos utilizado para desplegar el dominio.
4.  Una vez que vea la ventana _Preparar modelo completo_, haga clic en _Aceptar_. ![Preparación finalizada](images/prepare-complete.png)
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023