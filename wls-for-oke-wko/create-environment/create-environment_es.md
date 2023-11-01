# Configurar el entorno de prácticas

## Introducción

En este laboratorio, creará un repositorio dentro de Oracle Cloud Container Image Registry. Además, aceptará el acuerdo de licencia para las imágenes del servidor WebLogic en Oracle Container Registry.

Tiempo estimado: 15 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Cree un repositorio dentro de Oracle Cloud Container Image Registry.
*   Acepte la licencia para las imágenes del servidor WebLogic en Oracle Container Registry.

### Requisitos

*   Debe tener una cuenta activada de [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).
*   Debe tener una cuenta de Oracle.
*   Debe tener un editor de texto.

## Tarea 1: Creación de un repositorio

En esta tarea, creará un repositorio público. En el laboratorio 5, insertaremos la imagen auxiliar en este repositorio.

1.  En Escritorio remoto, para abrir Firefox, haga clic en **Actividades**, escriba **Fire** en el cuadro de búsqueda y, a continuación, haga clic en **Firefox** como se muestra. ![abrir firefox](images/open-firefox.png)
    
2.  Conéctese a la [consola de Oracle Cloud](https://cloud.oracle.com) con sus credenciales.
    
3.  En la consola, seleccione el _menú de hamburguesa_ -> _Servicios para desarrolladores_ -> _Registro de contenedores_, como se muestra. ![Container Registry](images/container-registry.png)
    
4.  Seleccione el compartimento en el que puede crear el repositorio. Haga clic en _Crear repositorio_. ![Crear Repositorio](images/create-repository.png)
    
5.  Introduzca _`test-model-your_firstname`_ como nombre de repositorio y Acceso como _Público_ y, a continuación, haga clic en _Crear_. ![Detalles del Repositorio](images/repository-details.png)
    
6.  Una vez que el repositorio esté listo. Anote el espacio de nombres del arrendamiento en el archivo de texto. ![Tenencia de nota NameSpace](images/tenancy-namespace.png)
    

## Tarea 2: Aceptación de la licencia para las imágenes del servidor WebLogic

En esta tarea, aceptamos el acuerdo de licencia para las imágenes del servidor WebLogic que residen en Oracle Container Registry. Al igual que en el laboratorio 5, utilizaremos la imagen del servidor WebLogic 12.2.1.3.0 como nuestra imagen principal. Por lo tanto, para obtener acceso a WebLogic Server Images, aceptamos el acuerdo de licencia.

1.  Copie y pegue el enlace de Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) en el explorador e inicie sesión. Para ello, necesita una cuenta de Oracle. ![Conexión a Container Registry](images/container-registry-sign-in.png)
    
2.  Introduzca sus _credenciales de cuenta de Oracle_ en los campos Nombre de usuario y Contraseña y, a continuación, haga clic en _Conectar_. ![Conexión a Container Registry](images/login-container-registry.png)
    
3.  En la página de inicio de Oracle Container Registry, busque _weblogic_. ![Buscar en WebLogic](images/search-weblogic.png)
    
4.  Haga clic en _weblogic_ como se muestra y seleccione _Inglés_ como idioma y, a continuación, haga clic en _Continuar_. ![Haga clic en WebLogic](images/click-weblogic.png) ![Seleccionar Idioma](images/select-language.png)
    
5.  Haga clic en _Aceptar_ para aceptar el contrato de licencia. ![Aceptar licencia](images/accept-license.png)
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, octubre de 2023