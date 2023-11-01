# Generar aplicación MP de Helidon

## Introducción

En este laboratorio, se muestran los pasos para crear una aplicación de **Helidon MP** mediante la **CLI de Helidon**. Además, modificará la aplicación Helidon para mostrar su integración con los servicios **OCI Logging and Monitoring**.

Tiempo estimado: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Generar aplicación Helidon MP](videohub:1_i4j33ed4)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Descarga de la CLI de Helidon
*   Generar la aplicación Helidon MP con la CLI de Helidon
*   Realizar integración con el servicio OCI Logging and Metrics
*   Transferir el código de aplicación al repositorio de código de OCI
*   Disparar el pipeline DevOps

### Requisitos

*   Una cuenta en la nube de Oracle Free Tier (prueba), de pago o LiveLabs
*   Familiaridad con los comandos de git

## Tarea 1: Descarga de la CLI de Helidon

1.  Abra un nuevo terminal en el **editor de códigos de OCI** y pegue el siguiente comando para navegar a la carpeta de inicio.
    
        <copy>cd ~</copy>
        
2.  Copie y pegue el siguiente comando para descargar la **CLI de Helidon** y cambie los **permisos** para que sea ejecutable.
    
        <copy>curl -L -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon</copy>
        

## Tarea 2: Generación de una aplicación de microperfil de Helidon mediante la CLI de Helidon

1.  Ejecute la CLI para **generar una aplicación de microperfil de Helidon**.
    
        <copy>./helidon init</copy>
        
2.  Cuando solicite la _versión de Helidon_, introduzca _4_ para ver todas las versiones y, a continuación, introduzca _35_ para seleccionar la versión _4.0.0-ALPHA6_ como se muestra a continuación.
    
        Helidon versions
        (1) 3.2.2 
        (2) 2.6.2 
        (3) 4.0.0-M1 
        (4) Show all versions 
        Enter selection (default: 1): 4
        -----------------------------
        -----------------------------
        (33) 2.0.1 
        (34) 2.0.0 
        (35) 4.0.0-ALPHA6 
        (36) 4.0.0-M1 
        Enter selection (default: 1): 35 
        
3.  Cuando se le solicite _Select a Flavor_, copie y pegue el siguiente valor en el terminal.
    
            | Helidon Flavor
        
            Select a Flavor
            (1) se   | Helidon SE
            (2) mp   | Helidon MP
            (3) nima | Helidon Níma
            Enter selection (default: 1): <copy>2</copy>
        
4.  Cuando se le solicite que _Seleccione un tipo de aplicación_, copie y pegue el siguiente valor en el terminal.
    
            Select an Application Type
        (1) quickstart | Quickstart
        (2) database   | Database
        (3) custom     | Custom
        (4) oci        | OCI
        Enter selection (default:1):<copy>4</copy>
        
5.  Cuando se le solicite _Project groupId_, _Project artifactId_ y _Project version_, **acepte los valores por defecto**.
    
6.  Cuando se le solicite el _nombre del paquete Java_, copie y pegue el siguiente valor en el terminal.
    
        Java package name (default: me.username.mp.oci): <copy>ocw.hol.mp.oci</copy>
        
7.  Cuando se le solicite _Start development loop? (valor por defecto: n):_, pulse _Intro_ para seleccionar el valor por defecto.
    
    > Una vez completado, se generará un proyecto **oci-mp**.
    

## Tarea 3: Modificar la aplicación Helidon para el explorador de métricas y registros

1.  Para abrir el proyecto _oci-mp_ en el **Editor de códigos**, haga clic en _Archivo_ -> _Abrir_. ![abrir proyecto](images/open-project.png)
    
2.  Haga clic en la _Flecha Arriba_ para navegar a la carpeta principal y, a continuación, seleccione la carpeta _oci-mp_ y haga clic en _Abrir_. ![abrir oci mp](images/open-ocimp.png)
    
    > Esto abrirá la aplicación _oci-mp_ en Explorer.
    
3.  Abra un nuevo terminal, copie y pegue el siguiente comando para **copiar las especificaciones de pipeline de compilación y despliegue** de la carpeta _`devops_helidon_to_instance_ocw_hol`_.
    
        <copy>cd ~/oci-mp/
        cp ~/devops_helidon_to_instance_ocw_hol/pipeline_specs/* .</copy>
        
4.  Agregue **.gitignore** para que git ignore los archivos y directorios que no sean necesarios para formar parte del repositorio.
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/.gitignore .</copy>
        
5.  Copie y pegue el siguiente comando para ejecutar la secuencia de comandos de la utilidad desde la carpeta principal _`devops_helidon_to_instance_ocw_hol`_ para actualizar los parámetros **config**. Cuando solicite **Introduzca el directorio raíz del proyecto Helidon MP**, pulse Intro para seleccionar el valor **default**.
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/update_config_values.sh</copy>
        
    
    Tendrá una salida similar a la que se muestra a continuación: ![actualizar configuración](images/update-config.png)
    
    > **Lea:-**
    
    *   La llamada a este script realizará lo siguiente:
    *   Actualizaciones en el archivo de configuración _~/oci-mp/server/src/main/resources/application.yaml_ para configurar una función de Helidon que envíe métricas generadas por Helidon al servicio de supervisión de OCI.
        *   **compartmentId**: ocid de compartimento que se utiliza para esta demostración
        *   **namespace**: puede ser cualquier cadena, pero para esta demostración se definirá en helidon\_metrics.
    *   Actualizaciones en el archivo de configuración _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ para configurar los parámetros de configuración utilizados por el código de la aplicación Helidon para realizar la integración con el servicio OCI Logging and Metrics.
        *   **oci.monitoring.compartmentId**: ocid de compartimento que se utiliza para esta demostración
        *   **oci.monitoring.namespace**: puede ser cualquier cadena, pero para esta demostración se definirá en helidon\_application.
        *   **oci.logging.id**: ID de log de aplicación aprovisionado por los scripts de Terraform.
    *   Actualice el archivo de configuración _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ para configurar la propiedad _oci.bucket.name_ para que contenga el nombre del cubo de Object Storage aprovisionado por los scripts de terraform que se utilizarán en un ejercicio posterior para demostrar el soporte de Object Storage desde una aplicación Helidon.
6.  Abra el archivo _~/oci-mp/server/src/main/resources/application.yaml_ en el editor de códigos para verificar que se actualicen el valor de **compartmentId** y **namespace**. ![yaml de aplicación](images/application-yaml.png)
    
7.  Abra el archivo _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ en el editor de códigos para verificar que se actualicen los valores de **oci.monitoring.compartmentId**, **oci.monitoring.namespace**, **oci.logging.id** y **oci.bucket.name**. _oci.bucket.name_ se utilizará más adelante en el laboratorio 5.
    

## Tarea 4: Generación de un token de autenticación para transferir el código al repositorio de código de OCI

En este paso, vamos a generar un _token de autenticación_ que utilizaremos para transferir el código de aplicación de Helidon al _repositorio de código de OCI_.

1.  Seleccione el _icono de usuario_ en la esquina superior derecha y, a continuación, seleccione _Mi perfil_. ![icono de usuario](images/user-icon.png)
    
2.  Desplácese hacia abajo y seleccione _Tokens de autenticación_. ![tokens de autenticación](images/auth-tokens.png)
    
3.  Haga clic en _Generar token_. ![generar token](images/generate-token.png)
    
4.  Copie _oci-mp_, péguelo en el cuadro Descripción y haga clic en _Generar token_. ![descripción de token](images/token-description.png)
    
5.  Seleccione **Copiar** en Token generado y péguelo o guárdelo en un archivo de texto con un editor de su elección. Recuerde que el token no se puede recuperar más adelante, por lo que es importante mantener una copia de esto ahora. Cuando haya terminado, haga clic en **Cerrar**. ![copiar token](images/copy-token.png)
    

## Tarea 5: Sincronización del proyecto de aplicación de Helidon con el repositorio de código de OCI en el proyecto DevOps

1.  Abra un nuevo terminal, copie y pegue el siguiente comando para navegar al directorio _oci-mp_.
    
        <copy>cd ~/oci-mp</copy>
        
2.  Inicialice el directorio del proyecto _oci-mp_ para que se convierta en un **repositorio de git**.
    
        <copy>git init</copy>
        
3.  Establezca la rama en **main** para que coincida con la rama remota correspondiente.
    
        <copy>git checkout -b main</copy>
        
4.  Configure el **repositorio remoto**. Utilice la URL https del repositorio de código de OCI que se muestra en la última salida de terraform o utilice la herramienta get.sh de `devops_helidon_to_instance_ocw_hol` para recuperar ese valor.
    
        <copy>git remote add origin $(~/devops_helidon_to_instance_ocw_hol/main/get.sh code_repo_https_url)
        git remote -v</copy>
        
    
    > **git remote -v** es para verificar que el origen se haya establecido.
    
5.  Configure git para utilizar el almacén de **ayudantes de credenciales** para que el nombre de usuario y la contraseña del repositorio de OCI se introduzcan solo una vez en los comandos de git que los necesiten. Además, establezca **user.name** y **user.email**, que es necesario para la confirmación de git.
    
        <copy>git config credential.helper store
        git config --global user.email "my.name@example.com"
        git config --global user.name "FIRST_NAME LAST_NAME"</copy>
        
6.  Sincronice el log de git del repositorio de oci con el repositorio local mediante **git pull**.
    
        <copy>git pull origin main</copy>
        
7.  Esto solicitará un nombre de usuario y una contraseña. Utilice **nombre de arrendamiento**/**nombre de usuario** para el nombre de usuario y el **token de autenticación** de usuario de OCI que se ha generado en la **tarea 4** para la contraseña. ![nombre de usuario de git](images/git-username.png) ![contraseña de git](images/git-password.png)
    
8.  Tendrá una salida similar a la siguiente. Si no es así, compruebe que ha ejecutado todos los comandos de git correctamente. ![sincronización de extracción de git](images/git-pull-sync.png)
    

## Tarea 6: Transferencia del código de aplicación de Helidon y disparo del pipeline DevOps

1.  **Almacenar en Zona Intermedia** todos los archivos para la confirmación de git.
    
        <copy>git add .
        git status</copy>
        
    
    > El estado de git mostrará todos los archivos del repositorio.
    
2.  Realice el primer **commit**.
    
        <copy>git commit -m "Helidon oci-mp first commit"</copy>
        
3.  **Transfiera** los cambios al repositorio remoto.
    
        <copy>git push -u origin main</copy>
        
    
    > Esto disparará DevOps para iniciar el pipeline de compilación.
    
4.  Abra la [consola en la nube](https://cloud.oracle.com/) en el nuevo separador. Haga clic en _Menú de hamburguesa_ -> _Servicios para desarrolladores_ -> _Proyectos_ en **DevOps**. ![proyecto devops](images/devops-project.png)
    
5.  Seleccione el compartimento que ha creado en el **Laboratorio 1** y, a continuación, haga clic en _devops-project-helidon-ocw-hol-string_ para abrir el **proyecto DevOps**. ![seleccionar compartimento](images/select-compartment.png)
    
    > Refresque el explorador. Si no ve el nuevo compartimento.
    
6.  En _Último historial de compilación_, verá _Ejecuciones_ y el estado como _Aceptado/En curso_. Haga clic en las últimas ejecuciones como se muestra a continuación. ![historial de creación](images/build-history.png)
    
7.  Una vez que el pipeline de compilación haya finalizado las tres etapas, verá la salida como se muestra a continuación. Puede hacer clic en la flecha justo antes de las etapas para ver qué acción están realizando. Esta acción se ha definido en el archivo _`build_spec.yaml`_ de la carpeta _oci-mp_. ![primera ejecución de compilación](images/build-run-first.png)
    
8.  En el progreso de ejecución de compilación, en la tercera etapa, haga clic en **Tres puntos** y, a continuación, haga clic en **Ver despliegue** como se muestra a continuación. Esto abrirá el **pipeline de despliegue**. ![ver despliegue](images/view-deployment.png)
    
9.  Aquí puede ver _Progreso de despliegue_. Una vez finalizado el pipeline de despliegue, verá la salida como se muestra a continuación. Puede hacer clic en la flecha justo antes de la etapa para ver qué acción están realizando. Esta acción se ha definido en el archivo _`deployment_spec.yaml`_ de la carpeta _oci-mp_. ![ejecución de despliegue](images/deployment-run.png)
    
    > De esta forma, se despliega correctamente la aplicación Helidon en **instancias informáticas** de OCI.
    
10.  Para ver los logs del pipeline de despliegue, haga clic en **Tres puntos** cerca de la etapa de despliegue y haga clic en **Ver detalles** como se muestra a continuación. ![ver logs](images/view-logs.png)
    
11.  Desplácese hacia abajo por los logs y verifique que el tipo de JDK sea **Open JDK**. Observe también de los logs que la aplicación Helidon utiliza la nueva función **Threads Virtuales** en Java, como se muestra a continuación. ![jdk abierto](images/open-jdk.png)
    
    > Como parte del **Laboratorio 4**, reemplazaremos **JDK abierto** por **Oracle JDK**.
    

Ahora puede **proceder al siguiente laboratorio.**

## Más información

*   [CLI de Helidon](https://helidon.io/docs/v3/#/about/cli)
*   [Guía de inicio rápido de Helidon MP](https://helidon.io/docs/v3/#/mp/guides/quickstart)
*   [Orígenes de configuración de MP de Helidon](https://helidon.io/docs/v3/#/mp/config/advanced-configuration)
*   [Orígenes de configuración de MP de Helidon](https://helidon.io/docs/v3/#/mp/guides/config)

## Reconocimientos

*   **Autor**: Keith Lustria
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023