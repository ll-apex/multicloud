# Configurar el entorno de prácticas

## Introducción

En este laboratorio, creará un cluster de Kubernetes de 3 nodos configurado con todos los recursos de red necesarios. Además, creará un repositorio dentro de Oracle Cloud Container Image Registry. A continuación, generará un token de autenticación. Además, aceptará el acuerdo de licencia para las imágenes del servidor WebLogic en Oracle Container Registry.

Tiempo estimado: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Configuración del entorno de prácticas](videohub:1_zhvohpqq)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Cree un cluster de Kubernetes de Oracle.
*   Cree un repositorio dentro de Oracle Cloud Container Image Registry.
*   Genere un token de autenticación.
*   Acepte la licencia para las imágenes del servidor WebLogic en Oracle Container Registry.

### Requisitos

*   Debe tener una cuenta activada de [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).
*   Debe tener una cuenta de Oracle.
*   Debe tener un editor de texto.

## Tarea 1: Creación de un cluster de Kubernetes de Oracle

La función _Creación rápida_ utiliza los valores por defecto para crear un _cluster rápido_ con nuevos recursos de red según sea necesario. Este enfoque es la forma más rápida de crear un nuevo cluster. Si acepta todos los valores por defecto, puede crear un nuevo cluster con unos pocos clics. Los nuevos recursos de red para el cluster se crean automáticamente, junto con un pool de nodos y tres nodos de trabajador.

1.  En la consola, seleccione _Menú de hamburguesa -> Servicios para desarrolladores -> Clusters de Kubernetes (OKE)_, como se muestra.
    
    ![Menú de hamburger](images/hamburger-menu.png " ")
    
2.  En la página Lista de clusters, seleccione el compartimento y, a continuación, haga clic en _Crear cluster_.
    
    > Debe seleccionar un compartimento en el que pueda crear un cluster y también un repositorio dentro de Oracle Container Registry.
    
    ![Seleccionar compartimento](images/select-compartment.png " ")
    
3.  En el cuadro de diálogo Crear solución de cluster, seleccione _Creación rápida_ y haga clic en _Enviar_.
    
    ![Iniciar flujo de trabajo](images/launch-workflow.png " ")
    
    La _creación rápida_ creará un nuevo cluster con la configuración por defecto, junto con los nuevos recursos de red para el nuevo cluster.
    
    Especifique los siguientes detalles de configuración en la página Creación de cluster (preste atención al valor que coloca en el campo _Unidad_):
    
    *   **Nombre**: nombre del cluster. Deje el valor por defecto.
    *   **Compartimento**: nombre del compartimento. Seleccione el compartimento en el que puede crear recursos.
    *   **Versión de Kubernetes**: versión de Kubernetes. Seleccione _1.26.2_ como versión de Kubernetes.
    *   **Punto final de API de Kubernetes**: ¿los nodos maestros del cluster van a ser enrutables o no? Seleccione el valor _Punto final público_.
    *   **Tipo de nodo**: seleccione _Gestionado_ como tipo de nodo.
    *   **Nodos de trabajador de Kubernetes**: ¿los nodos de trabajador del cluster van a ser enrutables o no? Deje el valor por defecto _Trabajadores privados_.
    *   **Unidad**: unidad que se va a utilizar para cada nodo del pool de nodos. La unidad determina el número de CPU y la cantidad de memoria asignada a cada nodo. En la lista se muestran solo las unidades disponibles en su arrendamiento soportadas por OKE. Seleccione _VM.Standard.E4. Flex_ (que suele estar disponible en la cuenta gratuita de Oracle). Seleccione las 1 OCPU y 16 GB como cantidad de memoria.
    *   **Imagen**: seleccione la imagen por defecto con la versión de Kubernetes _1.26.2_.
    *   **Número de nodos**: número de nodos de trabajador que se van a crear. Deje el valor por defecto, _3_.
    
    ![Cluster rápido](images/quick-cluster1.png " ") ![Introducir datos](images/enter-data.png " ")
    
4.  Haga clic en _Siguiente_ para revisar los detalles que ha introducido para el nuevo cluster.
    
5.  En la página _Revisar_, seleccione la casilla de control _Crear un cluster básico_ y, a continuación, haga clic en _Crear cluster_ para crear los nuevos recursos de red y el nuevo cluster.
    
    ![Revisión de cluster](images/review-cluster.png " ")
    
    > Verá los recursos de red que se están creando. Espere hasta que se inicie la solicitud para crear el pool de nodos y, a continuación, haga clic en _Cerrar_.
    
    ![Recurso de red](images/network-resource.png " ")
    
    > A continuación, el nuevo cluster se muestra en la página _Detalles de cluster_. Cuando se crean los nodos maestros, el nuevo cluster adquiere el estado _Activo_ (tarda unos 7 minutos). No necesita esperar, continúe con la siguiente tarea.
    
    ![acceso de cluster](images/cluster-access.png " ")
    

## Tarea 2: Creación de un repositorio

En esta tarea, creará un repositorio público. En el laboratorio 5, insertaremos la imagen auxiliar en este repositorio.

1.  En la consola, seleccione el _menú de hamburguesa_ -> _Servicios para desarrolladores_ -> _Registro de contenedores_, como se muestra. ![Container Registry](images/container-registry.png)
    
2.  Seleccione el compartimento en el que puede crear el repositorio. Haga clic en _Crear repositorio_. ![Crear Repositorio](images/create-repository.png)
    
3.  Introduzca _`test-model-your_firstname`_ como nombre de repositorio y Acceso como _Público_ y, a continuación, haga clic en _Crear_. ![Detalles del Repositorio](images/repository-details.png)
    
4.  Una vez que el repositorio esté listo. Anote el espacio de nombres del arrendamiento en el archivo de texto dentro del editor de texto. ![Tenencia de nota NameSpace](images/tenancy-namespace.png)
    

## Tarea 3: Generación de un token de autenticación

En esta tarea, generaremos un _token de autenticación_. En el laboratorio 5, utilizaremos este token de autenticación para transferir la imagen auxiliar al repositorio de Oracle Cloud Container Registry.

1.  Seleccione el icono de usuario en la esquina superior derecha y, a continuación, seleccione _MyProfile_.
    
    ![Mi perfil](images/my-profile.png)
    
2.  Desplácese hacia abajo y seleccione _Tokens de autenticación_ y, a continuación, haga clic en _Generar token_.
    
    ![Token de autenticación](images/auth-token.png)
    
3.  Copie _`test-model-your_first_name`_ y péguelo en el cuadro _Descripción_ y haga clic en _Generar token_.
    
    ![Crear Token](images/create-token.png)
    
4.  Seleccione _Copiar_ en Token generado y péguelo en el editor de texto. No podemos copiarlo más tarde. Haga clic en _Cerrar_.
    
    ![Copiar token](images/copy-token.png)
    

## Tarea 4: Aceptación de la licencia para las imágenes del servidor WebLogic

En esta tarea, aceptamos el acuerdo de licencia para las imágenes del servidor WebLogic que residen en Oracle Container Registry. Al igual que en el laboratorio 3, utilizaremos la imagen del servidor WebLogic 12.2.1.3.0 como nuestra imagen principal. Por lo tanto, para obtener acceso a WebLogic Server Images, aceptamos el acuerdo de licencia.

1.  Haga clic en el enlace de Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) e inicie sesión. Para ello, necesita una cuenta de Oracle. ![Conexión a Container Registry](images/container-registry-sign-in.png)
    
2.  Introduzca sus _credenciales de cuenta de Oracle_ en los campos Nombre de usuario y Contraseña y, a continuación, haga clic en _Conectar_. ![Conexión a Container Registry](images/login-container-registry.png)
    
3.  En la página de inicio de Oracle Container Registry, busque _weblogic_. ![Buscar en WebLogic](images/search-weblogic.png)
    
4.  Haga clic en _weblogic_ como se muestra y seleccione _Inglés_ como idioma y, a continuación, haga clic en _Continuar_. ![Haga clic en WebLogic](images/click-weblogic.png) ![Seleccionar Idioma](images/select-language.png)
    
5.  Haga clic en _Aceptar_ para aceptar el contrato de licencia. ![Aceptar licencia](images/accept-license.png)
    

## Más información

_Acerca de Oracle Cloud Infrastructure Container Engine for Kubernetes_

Oracle Cloud Infrastructure Container Engine for Kubernetes es un servicio totalmente gestionado, escalable y de alta disponibilidad que puede utilizar para desplegar las aplicaciones de contenedor en la nube. Utilice Container Engine for Kubernetes (a veces, OKE abreviado) cuando su equipo de desarrollo desee crear, desplegar y gestionar de forma fiable aplicaciones en la nube. Especifique los recursos informáticos que necesitan sus aplicaciones y OKE los aprovisionará en Oracle Cloud Infrastructure en un arrendamiento de OCI existente.

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023