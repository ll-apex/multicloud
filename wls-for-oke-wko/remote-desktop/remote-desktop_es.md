# Configurar instancia informática

## Introducción

Este laboratorio le mostrará cómo configurar una pila de **Oracle WebLogic Suite para OKE BYOL** que generará los objetos de Oracle Cloud necesarios para ejecutar el taller.

Tiempo estimado: 15 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Genere un token de autenticación.
*   Creación de un secreto en un almacén
*   Crear una pila: Oracle WebLogic Suite para OKE BYOL
*   Conectarse a la instancia informática

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta de Oracle Cloud
*   Ha generado el par de claves SSH.
*   Ha completado: **Laboratorio: Preparación de la configuración**

## Tarea 1: Generación de un token de autenticación

En esta tarea, generaremos un _token de autenticación_. En el laboratorio 5, utilizaremos este token de autenticación para transferir la imagen auxiliar al repositorio de Oracle Cloud Container Registry. Además, utilizamos este token de autenticación para extraer imágenes de docker WebLogic a Oracle Cloud Infrastructure Registry (también conocido como Container Registry).

1.  Seleccione el icono de usuario en la esquina superior derecha y, a continuación, seleccione _MyProfile_.
    
    ![Mi perfil](images/my-profile.png)
    
2.  Desplácese hacia abajo y seleccione _Tokens de autenticación_ y, a continuación, haga clic en _Generar token_.
    
    ![Token de autenticación](images/auth-token.png)
    
3.  Copie _`test-model-your_first_name`_ y péguelo en el cuadro _Descripción_ y haga clic en _Generar token_.
    
    ![Crear Token](images/create-token.png)
    
4.  Seleccione _Copiar_ en Token generado y péguelo en el archivo de texto. No podemos copiarlo más tarde. Haga clic en _Cerrar_.
    
    ![Copiar token](images/copy-token.png)
    
    > En la siguiente tarea, almacenaremos este token de autenticación en **Secreto**.
    

## Tarea 2: Creación de un secreto en un almacén

Los secretos son credenciales como contraseñas, certificados, claves SSH o **tokens de autenticación** que se utilizan con los servicios de Oracle Cloud Infrastructure.

1.  En la consola de OCI, haga clic en **Menú de hamburguesa** -> **Identidad y seguridad** -> **Almacén**. ![almacén de menús](images/menu-vault.png)
    
2.  Seleccione el compartimento en **Ámbito de lista** y, a continuación, haga clic en **Crear almacén**.
    
3.  Introduzca el nombre del almacén y haga clic en **Crear almacén**. ![crear almacén](images/create-vault.png)
    
4.  Una vez que vea que el almacén que ha creado tiene el estado **Activo**, haga clic en el nombre del almacén como se muestra a continuación. ![nombre de almacén](images/vault-name.png)
    
5.  Dentro del almacén, haga clic en **Master Encryption Keys** y, a continuación, en **Create Key**. ![menú mek](images/menu-mek.png)
    
6.  Introduzca el nombre de la **clave** y, a continuación, haga clic en **Crear clave** como se muestra a continuación. ![crear mek](images/create-mek.png)
    
7.  Una vez que vea que el estado de la nueva clave de cifrado maestra cambia a **Activado** y, a continuación, haga clic en **Secretos** en **Recursos**, como se muestra a continuación. ![secretos de menú](images/menu-secret.png)
    
8.  Haga clic en **Create Secret**.
    
9.  Introduzca el nombre del secreto y seleccione la nueva clave de cifrado e introduzca el token de autenticación como **Contenido secreto**, como se muestra a continuación. Haga clic en **Crear secreto**. ![crear secreto](images/create-secret.png)
    

## Tarea 3: Crear pila: Oracle WebLogic Suite para OKE BYOL

1.  En la consola de OCI, haga clic en **Menú de hamburguesa** -> **Marketplace** -> **Todas las aplicaciones**. ![mercado de menús](images/menu-marketplace.png)
    
2.  Escriba **WebLogic OKE** en el cuadro de búsqueda y, a continuación, haga clic en **Oracle WebLogic Suite para OKE BYOL**. ![pila de menús](images/menu-stack.png)
    
3.  Seleccione la última versión disponible, elija su compartimento y active la casilla para aceptar las condiciones. Haga clic en **Iniciar pila**. ![iniciar pila](images/launch-stack.png)
    
4.  En la sección de información de pila, deje todo por defecto y haga clic en **Siguiente**. ![información de pila](images/stack-info.png)
    
5.  Introduzca **wko** como prefijo de nombre de recurso y haga clic en Browse (Examinar) para seleccionar la clave pública SSH. ![prefijo-ssh](images/prefix-ssh.png)
    
    > Asegúrese de utilizar **wko** como prefijo, ya que lo utilizaremos en otras prácticas.
    
6.  En Verrazzano Integration, deje la casilla por defecto desactivada para **Activar Verrazzano**. ![desmarcar verrazzano](images/uncheck-verrazzano.png)
    
7.  En Network, seleccione **Create a New VCN** as Virtual Cloud Network Strategy y deje todo por defecto. ![red](images/network.png)
    
8.  En Configuración de cluster de contenedores (OKE), seleccione lo siguiente.
    
    **Versión de Kubernetes:** deje la versión por defecto **v1.26.2**.
    
    **Unidad de pool de nodos no WebLogic (necesaria)**: seleccione **VM.Standard.E4. Flexible** como unidad y elige **1** como número de OCPU y **16** como cantidad de memoria.
    
    **Nodo en NodePool para pods que no sean WebLogic**: deje el valor por defecto **1**.
    
    ![no weblogic](images/non-weblogic.png)
    
    **Crear unidad de pool de nodos WebLogic**: deje la casilla por defecto marcada.
    
    **WebLogic Unidad de pool de nodos (necesaria)**: seleccione **VM.Standard.E4. Flexible** como unidad y elige **1** como número de OCPU y **16** como cantidad de memoria.
    
    **Nodo en NodePool para pods WebLogic**: deje el valor por defecto **1**.
    
    ![weblogic](images/weblogic-pool.png)
    
9.  En Administration Instances, introduzca o seleccione lo siguiente como se muestra. **Dominio de disponibilidad para instancias informáticas**: seleccione el dominio de disponibilidad en la lista desplegable.
    
    **Unidad de cálculo de instancia de administración (necesaria)**: seleccione **VM.Standard.E4. Flexible** como unidad y elige **1** como número de OCPU y **16** como cantidad de memoria.
    
    ![instancia de administrador](images/admin-instance.png)
    
    **Unidad de instancia de Bastion (necesaria)**: seleccione **VM.Standard.E4. Flexible** como unidad y elige **1** como número de OCPU y **16** como cantidad de memoria.
    
    ![bastión](images/bastion.png)
    
10.  En la sección Sistema de archivos, seleccione lo siguiente como se muestra. **Dominio de disponibilidad para sistema de archivos**: seleccione el dominio de disponibilidad en la lista desplegable. ![archivo avd](images/file-avd.png)
    
11.  En la sección Registry (OCIR), introduzca lo siguiente como se muestra.
    
    **Nombre de usuario de registro**: nombre de usuario que utiliza Kubernetes para acceder al registro de imágenes de contenedor, que tiene el formato {identity domain name}/{username}. Si su arrendamiento utiliza Oracle Identity Cloud Service, utilice el formato oracleidentitycloudservice/{username}.
    
    **Compartimento de token de autenticación de OCIR**: seleccione el compartimento en el que tiene el token de autenticación de OCIR.
    
    **Secreto validado para token de autenticación de OCIR**: secreto que contiene el token de autenticación de OCIR que ha generado para que el usuario acceda al registro de imágenes.
    
    ![secreto de ocir](images/ocir-secret.png)
    
12.  En Políticas de OCI, active la casilla de control de **Políticas de OCI**. Crea políticas para leer secretos de Vault y gestionar la base de datos de Autonomous Transaction Processing (si corresponde). Haga clic en **Siguiente**.
    
13.  En la sección Revisar, marque la casilla **Ejecutar aplicación** y, a continuación, haga clic en **Crear**. ![ejecutar aplicación](images/run-apply.png)
    
    > Esto creará un trabajo, que creará los recursos necesarios en la pila. No necesita esperar, continúe con la siguiente tarea.
    

## Tarea 4: Acceso al escritorio remoto gráfico

Para facilitar la ejecución de este taller, su instancia de máquina virtual se ha preconfigurado con un escritorio gráfico remoto accesible mediante cualquier explorador moderno en su portátil o estación de trabajo. Continúe como se detalla a continuación para conectarse.

1.  Abre el menú de hamburguesa en la esquina superior izquierda. Haga clic en **Servicios para desarrolladores** y seleccione **Gestor de recursos** > **Pilas**.
    
2.  Haga clic en el nombre de pila que ha creado en el laboratorio 1. ![hacer clic en la pila](images/click-stack.png)
    
3.  Vaya al separador **Información de la aplicación**, copie la **URL de escritorio remoto** y péguela en el nuevo separador del explorador. ![URL de escritorio](images/desktop-url.png)
    
    > Ahora debe seguir todas las instrucciones dentro de este escritorio remoto.
    

Ahora puede pasar a la siguiente práctica de laboratorio.

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, octubre de 2023