# Instalar Verrazzano

## Introducción

En este laboratorio se muestran los pasos para instalar Verrazzano en un cluster de Kubernetes en Oracle Cloud Infrastructure.

Tiempo estimado: 15 minutos

### Acerca de Producto/Tecnología

Verrazzano es una plataforma integral de contenedores empresariales para implementar aplicaciones tradicionales y nativas de la nube en entornos híbridos y multinube. Está compuesto por un conjunto curado de componentes de código abierto, muchos de los cuales ya puedes usar y confiar, y algunos que fueron escritos específicamente para reunir todas las piezas que hacen de Verrazzano una plataforma cohesiva y fácil de usar.

Verrazzano incluye las siguientes capacidades:

*   Gestión de cargas de trabajo híbridas y de varios clusters
*   Manejo especial para aplicaciones WebLogic, Coherence y Helidon
*   Gestión de infraestructura de varios clusters
*   Supervisión de aplicaciones integrada y preconectada
*   Seguridad integrada
*   Activación DevOps y GitOps

Verrazzano admite los siguientes perfiles de instalación: desarrollo (`dev`), producción (`prod`) y cluster gestionado (`managed-cluster`).

En la siguiente imagen se describen los perfiles de instalación de Verrazzano. ![Instalar perfil](images/installprofile.png)

Para cambiar los perfiles en cualquiera de los siguientes comandos, establezca la variable de entorno _VZ\_PROFILE_ en el nombre del perfil que desea instalar.

Para obtener una descripción completa de las opciones de configuración de Verrazzano, consulte la [Definición de recurso personalizado de Verrazzano](https://verrazzano.io/docs/reference/api/verrazzano/verrazzano/).

En este laboratorio, vamos a instalar el _perfil de desarrollo de Verrazzano_, que tiene las siguientes características:

*   Comodín (nip.io) DNS
*   Certificados autofirmados
*   Pila de observabilidad compartida utilizada por los componentes del sistema y todas las aplicaciones
*   Almacenamiento efímero para la pila de observación (si se reinician los pods, se pierden todos los logs y métricas)
*   Tiene una instalación ligera.
*   Es para fines de evaluación.
*   Topología de clúster de Opensearch de un solo nodo.

Verrazzano instala un conjunto curado de componentes de código abierto. En la siguiente tabla, se muestra cada componente, su versión y una breve descripción.

![Perfil de Verrazzano](images/verrazzano-profile.png " ") ![Perfiles de Verrazzano](images/verrazzano-profiles.png " ")

Según nuestra elección de DNS, podemos utilizar nip.io (DNS comodín) o [Oracle OCI DNS](https://docs.cloud.oracle.com/en-us/iaas/Content/DNS/Concepts/dnszonemanagement.htm). En este laboratorio, vamos a realizar la instalación mediante nip.io (DNS comodín).

Un controlador de entrada es algo que ayuda a proporcionar acceso a contenedores de Docker al mundo exterior (al proporcionar una dirección IP). La entrada enruta la dirección IP a diferentes clusters.

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Configurar `kubectl` para utilizar el cluster de Oracle Kubernetes Engine
*   Instale la CLI de Verrazzano vz.
*   Instale el perfil de desarrollo (`dev`) de Verrazzano.

### Requisitos

Verrazzano requiere lo siguiente:

*   Un cluster de Kubernetes y un `kubectl` compatible.
*   Al menos 2 CPU, 100 GB de almacenamiento en disco y 16 GB de RAM disponibles en los nodos de trabajador de Kubernetes. Esto es suficiente para instalar el perfil de desarrollo de Verrazzano. En función de los requisitos de recursos de las aplicaciones que despliegue, puede que esto sea suficiente o no para desplegar las aplicaciones.
*   En el laboratorio 1, ha creado un cluster de Kubernetes en Oracle Cloud Infrastructure. Utilizará ese cluster de Kubernetes, _cluster1_, para instalar el perfil de desarrollo de Verrazzano.

## Tarea 1: Configuración de `kubectl` (CLI de cluster de Kubernetes)

Oracle Cloud Infrastructure (OCI) Cloud Shell es un terminal basado en explorador web, accesible desde la consola de Oracle Cloud. Cloud Shell proporciona acceso a un shell de Linux, con una CLI de Oracle Cloud Infrastructure autenticada previamente y otras herramientas útiles (_Git, kubectl, timón, CLI de OCI_) para completar los tutoriales de Verrazzano. Se puede acceder a Cloud Shell desde la consola. Cloud Shell aparecerá en la consola de Oracle Cloud como un marco persistente de la consola y permanecerá activo mientras navega a diferentes páginas de la consola.

Utilizará _Cloud Shell_ para completar este taller.

Utilizaremos `kubectl` para gestionar el cluster de forma remota mediante Cloud Shell. Necesita un archivo `kubeconfig`. Esto se generará mediante la CLI de OCI que está autenticada previamente, por lo que no hay que realizar ninguna configuración antes de empezar a utilizarla.

1.  Haga clic en _Access Cluster_ en la página de detalles del cluster.
    
    > Si se ha alejado de esa página, abra el menú de navegación y, en _Servicios para desarrolladores_, seleccione _Clusters de Kubernetes (OKE)_. Seleccione el cluster y vaya a la página de detalles.
    
    ![Acceder al cluster](images/access-cluster.png " ")
    
    > Se muestra un cuadro de diálogo desde el que puede abrir Cloud Shell y contiene el comando personalizado de OCI que necesita ejecutar para crear un archivo de configuración de Kubernetes.
    
2.  Deje el _acceso a Cloud Shell_ por defecto y, en primer lugar, seleccione el enlace _Copiar_ para copiar el comando `oci ce...` en Cloud Shell.
    
    ![Copiar Configuración de kubectl](images/copy-config.png " ")
    
3.  Ahora, haga clic en _Iniciar Cloud Shell_ para abrir la consola incorporada. A continuación, cierre el cuadro de diálogo de configuración antes de pegar el comando en _Cloud Shell_.
    
    ![Iniciar Cloud Shell](images/launch-cloudshell.png " ")
    
4.  Copie el comando del portapapeles (Ctrl+V o haga clic con el botón derecho y cópielo) en Cloud Shell y ejecute el comando.
    
    Por ejemplo, el comando tiene el siguiente aspecto:
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![Configuración de kubectl](images/kube-config.png " ")
    
5.  Ahora compruebe que `kubectl` está funcionando, por ejemplo, mediante el comando `get node`. Puede que necesite ejecutar este comando varias veces hasta que vea la salida similar a la siguiente.
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.101   Ready    node    11m   v1.26.2
        10.0.10.29    Ready    node    11m   v1.26.2
        10.0.10.37    Ready    node    11m   v1.26.2
        
    
    > Si ve la información del nodo, la configuración se ha realizado correctamente.
    

## Tarea 2: Instalación de la CLI de Verrazzano

1.  Descargue la CLI de vz más reciente.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
        

0 0 0 0 0 0 0 0 --:--:-- --:-- --:--:-- 0 100 39.7M 100 39.7M 0 0 23.6M 0 0:00:01 0:00:01 --:--:-- 32.7M \`\`\`

2.  Descargue el archivo de total de control.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        

La salida debe ser similar a lo siguiente:

    ```bash
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    

0 0 0 0 0 0 0 0 --:--:-- --:--:--:-- 0 100 102 100 102 0 0 218 0 --:--:-- --:-- --:--:-- 218 \`\`

3.  Valide el binario con el archivo de total de control.
    
        <copy>sha256sum -c verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        verrazzano-1.6.2-linux-amd64.tar.gz: OK
        
4.  Desempaquetar y mover al directorio binario vz,
    
        <copy>tar xvf verrazzano-1.6.2-linux-amd64.tar.gz
        cd ~/verrazzano-1.6.2/bin/</copy>
        
5.  Realice una prueba para asegurarse de que la versión instalada esté actualizada.
    
        <copy>./vz version</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        Version: v1.6.2
        BuildDate: 2023-07-21T12:06:02Z
        GitCommit: 5cd8a2f8670000d2ef0b02404e3169699d87f26b
        

## Tarea 3: Instalación del perfil de desarrollo de Verrazzano

Un perfil de instalación es una configuración conocida de la configuración de Verrazzano a la que se puede hacer referencia por nombre, que luego se puede personalizar según sea necesario.

Verrazzano admite los siguientes perfiles de instalación: desarrollo (`dev`), producción (`prod`) y cluster gestionado (`managed-cluster`).

1.  Realice la instalación mediante el método DNS nip.io. Copie el siguiente comando y péguelo en _Cloud Shell_ para instalar Verrazzano.
    
        <copy>./vz install -f - <<EOF
        apiVersion: install.verrazzano.io/v1beta1
        kind: Verrazzano
        metadata:
          name: example-verrazzano
        spec:
          profile: dev
        EOF
        </copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        Installing Verrazzano version v1.6.2
        Applying the file https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-platform-operator.yaml
        customresourcedefinition.apiextensions.k8s.io/verrazzanos.install.verrazzano.io created
        namespace/verrazzano-install created
        serviceaccount/verrazzano-platform-operator created
        clusterrole.rbac.authorization.k8s.io/verrazzano-managed-cluster created
        clusterrolebinding.rbac.authorization.k8s.io/verrazzano-platform-operator created
        service/verrazzano-platform-operator created
        service/verrazzano-platform-operator-webhook created
        deployment.apps/verrazzano-platform-operator created
        deployment.apps/verrazzano-platform-operator-webhook created
        mutatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-mysql-backup created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-operator-webhook created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-mysqlinstalloverrides created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-requirements-validator created
        Waiting for verrazzano-platform-operator to be ready before starting install - 23 seconds
        
    
    > Se tarda entre 15 y 20 minutos en completar la instalación. Este comando instala el operador de la plataforma Verrazzano y aplica el recurso personalizado de Verrazzano. Los logs de instalación se transmitirán a la ventana de comandos hasta que finalice la instalación o hasta que se alcance el timeout por defecto (30 m).
    
2.  Deje _Cloud Shell_ abierto y deje que se ejecute la instalación.
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023