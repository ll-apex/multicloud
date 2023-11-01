# Prestación de la infraestructura

## Introducción

En este laboratorio, creará un **compartimento**, **grupos dinámicos**, **grupo de usuarios y políticas**. A continuación, creará un **proyecto DevOps** y sus recursos relacionados con el servicio Terraform en **OCI Code Editor**.

Tiempo estimado: 10 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Aprovisionamiento de la infraestructura](videohub:1_tnhjvsxr)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Abra Code Editor para descargar el script de Terraform.
*   Aprovisione el compartimento, los grupos dinámicos, el grupo de usuarios y las políticas.
*   Aprovisione los recursos necesarios para el proyecto DevOps

### Requisitos

*   Una cuenta en la nube de Oracle Free Tier (prueba), de pago o LiveLabs
*   Familiaridad con compartimentos, grupos dinámicos, grupos de usuarios y políticas

## Tarea 1: Abrir el editor de código y descargar el código fuente

1.  En la consola en la nube, haga clic en el icono _Herramientas de desarrollador_ como se muestra y, a continuación, haga clic en _Editor de códigos_. ![abrir editor de código](images/open-codeeditor.png)
    
2.  Haga clic en _Terminal_\-> _New Terminal_ para abrir el terminal. Durante el taller, se le pedirá que abra un nuevo terminal. De esta forma, puede abrir un nuevo terminal en Code Editor. ![abrir terminal](images/open-terminal.png)
    
3.  Copie y pegue el siguiente comando en el terminal para descargar el código fuente. Este código fuente contiene los scripts de terraform que crean los recursos de OCI necesarios para este taller.
    
        <copy>curl -O https://objectstorage.uk-london-1.oraclecloud.com/p/IxV3cO7Uf5VnbniLEmx8zOBhr6liix40QWNTnTy0TTBcGdrLaRNSt2IJYxBPHqdw/n/lrv4zdykjqrj/b/ankit-bucket/o/devops_helidon_to_instance_ocw_hol.zip
        unzip ~/devops_helidon_to_instance_ocw_hol.zip</copy>
        

![descargar código de origen](images/download-sourcecode.png)

4.  Para abrir el código fuente _`devops_helidon_to_instance_ocw_hol`_ en el **editor de códigos**, haga clic en _Archivo_\-> _Abrir_. ![código fuente abierto](images/open-sourcecode.png)
    
5.  Seleccione _`devops_helidon_to_instance_ocw_hol`_ en el directorio raíz y haga clic en _Abrir_. ![abrir devops](images/open-devops.png)
    
6.  Haga clic en el nombre de archivo _terraform.tfvars_ dentro de la carpeta _`devops_helidon_to_instance_ocw_hol`_, como se muestra. Puede ver que tenemos variables **`tenancy_ocid`**, **region**, **`compartment_ocid`**, **`user_ocid`** que personalizaremos para su cuenta de prueba en la nube. ![abrir tfvars](images/open-tfvars.png)
    

> Si no ve el proyecto. Puede que tenga que hacer clic en el icono _Explorador_ del editor de códigos. ![exlorante](images/explorer.png)

7.  En el explorador, abra un **nuevo separador** para [Cloud Console](https://cloud.oracle.com/). Utilizaremos este separador para obtener el valor de las variables anteriores.
    
8.  Para obtener **`tenancy_ocid`**, haga clic en _Icono de usuario_ y, a continuación, haga clic en _Arrendamiento_ como se muestra. ![obtener OCID de arrendamiento](images/get-tenancyocid.png)
    
9.  Haga clic en _Copiar_ para copiar el **OCID** del arrendamiento y pegarlo en el archivo _terraform.tfvars_ como valor de _`tenancy_ocid`_. ![copiar OCID de arrendamiento](images/copy-tenancyocid.png)
    
10.  Para obtener **`user_ocid`**, haga clic en _Icono de usuario_ y, a continuación, haga clic en _Mi perfil_ como se muestra. ![mi perfil](images/my-profile.png)
    
11.  Haga clic en _Copiar_ para copiar el **OCID** del usuario y pegarlo en el archivo _terraform.tfvars_ como valor de _`user_ocid`_. ![copiar ocid de usuario](images/copy-userocid.png)
    
12.  Para buscar el nombre de la región, haga clic en **Gestionar regiones**, como se muestra a continuación. A continuación, copie el **identificador de región** de la región principal y péguelo en el archivo _terraform.tfvars_ como valor de _región_. ![gestionar región](images/manage-region.png) ![nombre de región](images/region-name.png)
    
13.  Por último, _terraform.tfvars_ debería tener este aspecto. Deje el valor de _`compartment_ocid`_ tal cual. Sustituiremos el valor una vez que el compartimento se cree como parte de la tarea 2. ![tfvars de inicialización](images/init-tfvars.png)
    

## Tarea 2: Creación de un compartimento, grupos dinámicos, grupos de usuarios y políticas

El objetivo de esta tarea es preparar el entorno para la configuración de DevOps mediante la creación de un compartimento, un grupo dinámico, un grupo de usuarios y políticas. Esta sección requiere un usuario con privilegios de administrador. Si no lo tiene, asegúrese de solicitar a otro usuario con dicho privilegio que lo ejecute por usted.

1.  Abra un terminal, copie y pegue el siguiente comando para navegar a la carpeta _init_.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/init/</copy>
        
2.  En el editor de códigos, puede ver varios archivos en la carpeta _init_. Estos son los scripts de terraform que crean el compartimento, los grupos dinámicos, el grupo de usuarios y las políticas. ![archivos de inicialización](images/init-files.png)
    
3.  Copie y pegue los siguientes comandos para aprovisionar el compartimento, los grupos dinámicos, el grupo de usuarios y las políticas.
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    verá una salida similar a la siguiente. **Obtenga la salida** para saber lo que crea la secuencia de comandos terraform. Además, puede consultar el código para ver su implantación. ![inicialización creada](images/init-created.png)
    
    > Si hay errores, compruebe que haya definido correctamente los valores en el archivo **terraform.tfvars**.
    

## Tarea 3: Crear un proyecto DevOps y sus recursos

1.  En el terminal, copie y pegue el siguiente comando para navegar a la carpeta _main_.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main/</copy>
        
2.  Copie y pegue el siguiente comando para actualizar **compartment\_ocid** de un compartimento recién creado en _terraform.tfvars_ dentro de la carpeta _`devops_helidon_to_instance_ocw_hol`_.
    
        <copy>../utils/update_compartment.sh</copy>
        
3.  Copie y pegue el siguiente comando en el terminal para aprovisionar todos los recursos DevOps.
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    > **Lea:** proporcionará los siguientes recursos necesarios para DevOps:
    
    *   **Servicio DevOps de OCI**
        *   **Proyecto DevOps de OCI** que contendrá todos los componentes DevOps necesarios para este proyecto.
        *   **Repositorio de código OCI** que alojará el proyecto de código fuente de la aplicación.
        *   **DevOps Pipeline de creación** con las siguientes etapas:
            *   **Gestionar compilación**: ejecuta pasos para descargar JDK20, guardar y crear la aplicación de Helidon
            *   **Entregar artefactos**: carga la aplicación de Helidon y el despliegue creados en el repositorio de artefactos
            *   **Despliegue de disparador**: dispara el pipeline de despliegue
        *   **DevOps Pipeline de despliegue** que realizará lo siguiente en el entorno de destino:
            *   Descargar JDK20
            *   Instalar la CLI de OCI y utilizarla para descargar el entregable de aplicación
            *   Ejecutar la aplicación
        *   **Entorno de grupo de instancias DevOps** que utilizará el pipeline de despliegue para identificar la instancia informática de OCI creada como destino de despliegue.
        *   **Disparador DevOps** que llamará al ciclo de vida del pipeline de principio a fin cuando se produzca un evento push en el repositorio de código de OCI.
    *   **Registro de artefactos de OCI**
        *   **Repositorio de artefactos de OCI** que alojará los binarios de la aplicación de Helidon y el manifiesto de despliegue creados como artefactos versionados.
    *   **Plataforma OCI**
        *   **Instancia informática de OCI** que abre el puerto 8080 desde el firewall. Aquí es donde la aplicación se desplegará finalmente.
    *   **Red virtual en la nube (VCN) de OCI con lista de seguridad** que contiene una entrada que abre el puerto 8080. El puerto 8080 es desde donde se accederá a la aplicación Helidon. La instancia informática de OCI utilizará la VCN de OCI para sus necesidades de red.
4.  El siguiente diagrama muestra cómo funcionará la configuración de DevOps: ![diagrama de devops](images/devops-diagram.png)
    
5.  Obtendrá una salida similar a la que se muestra a continuación. ![salida de tf](images/tf-output.png)
    

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Keith Lustria
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023