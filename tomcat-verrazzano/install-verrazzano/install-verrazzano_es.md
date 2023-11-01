# Instalar Verrazzano

## Introducción

En este laboratorio se muestran los pasos para instalar Verrazzano en un cluster de Kubernetes en Oracle Cloud Infrastructure.

Tiempo estimado: 20 minutos

### Acerca de Producto/Tecnología

Verrazzano es una plataforma integral de contenedores empresariales para implementar aplicaciones tradicionales y nativas de la nube en entornos híbridos y multinube. Está compuesto por un conjunto curado de componentes de código abierto, muchos de los cuales ya puedes usar y confiar, y algunos que fueron escritos específicamente para reunir todas las piezas que hacen de Verrazzano una plataforma cohesiva y fácil de usar.

Verrazzano incluye las siguientes capacidades:

*   Gestión de cargas de trabajo híbridas y de varios clusters
*   Manejo especial para aplicaciones WebLogic, Coherence y Helidon
*   Gestión de infraestructura de varios clusters
*   Supervisión de aplicaciones integrada y preconectada
*   Seguridad integrada
*   Activación DevOps y GitOps

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Instale la herramienta de línea de comandos de Verrazzano.
*   Instale el perfil de desarrollo (`dev`) de Verrazzano.
*   Verifique la instalación de Verrazzano.

### Requisitos

Verrazzano requiere lo siguiente:

*   Un cluster de Kubernetes y un `kubectl` compatible.
*   Al menos 2 CPU, 100 GB de almacenamiento en disco y 16 GB de RAM disponibles en los nodos de trabajador de Kubernetes. Esto es suficiente para instalar el perfil de desarrollo de Verrazzano. En función de los requisitos de recursos de las aplicaciones que despliegue, puede que esto sea suficiente o no para desplegar las aplicaciones.

En el laboratorio 1, ha creado un cluster de Kubernetes en Oracle Cloud Infrastructure. Utilizará ese cluster de Kubernetes, \*cluster1\*, para instalar el perfil de desarrollo de Verrazzano. En el laboratorio 1, ha creado un archivo de configuración para acceder al cluster de Kubernetes en Oracle Cloud Infrastructure. Utilizará ese cluster de Kubernetes, \*cluster1\*, para instalar el perfil de desarrollo de Verrazzano.

## Tarea 1: Instalación de la CLI de vz

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
        

## Tarea 2: Instalación del perfil de desarrollo de Verrazzano

Un perfil de instalación es una configuración conocida de la configuración de Verrazzano a la que se puede hacer referencia por nombre, que luego se puede personalizar según sea necesario.

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
        \> Se tarda entre 15 y 20 minutos en completar la instalación. Este comando instala el operador de plataforma de Verrazzano y aplica el recurso personalizado de Verrazzano. \> Se tarda entre 8 y 10 minutos en completar la instalación. Este comando instala el operador de la plataforma Verrazzano y aplica el recurso personalizado de Verrazzano.
2.  Espere a que finalice la instalación. Los logs de instalación se transmitirán a la ventana de comandos hasta que finalice la instalación o hasta que se alcance el timeout por defecto (30 m).
    

## Tarea 3: Verificación de una instalación correcta de Verrazzano

Verrazzano instala varios objetos en varios espacios de nombres. Los componentes de Verrazzano se instalan en el espacio de nombres _verrazzano-system_.

1.  Verifique que todos los pods asociados a varios objetos tengan el estado _En ejecución_.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        $   kubectl get pods -n verrazzano-system
        NAME                                    READY   STATUS  RESTARTS AGE
        coherence-operator-b5dc669c6-rk2sm      1/1     Running   1       23m
        fluentd-54f5x                           2/2     Running   1       11m
        fluentd-h7mgh                           2/2     Running   1       11m
        fluentd-xcdfz                           2/2     Running   0       11m
        oam-kubernetes-runtime-5b48f944b-cx7b9  1/1     Running   0       24m
        verrazzano-application-operator-665c5   1/1     Running   0       22m
        verrazzano-application-operator-webhook 1/1     Running   0       22m
        verrazzano-authproxy-67776ff58b-8l      3/3     Running   0       21m
        verrazzano-cluster-operator-67dc5695    1/1     Running   0       14m
        verrazzano-cluster-operator-webhook     1/1     Running   0       14m
        verrazzano-console-7d95c98cb9-9ql2      2/2     Running   0       20m
        verrazzano-monitoring-operator-59f      2/2     Running   0       21m
        vmi-system-es-master-0                  2/2     Running   0       19m
        vmi-system-grafana-7fd956b585-2tbgl     3/3     Running   0       19m
        vmi-system-kiali-dd87546d6-ddxss        2/2     Running   0       22m
        vmi-system-osd-7687d6fccf-nm7kt         2/2     Running   0       14m
        weblogic-operator-54979449f4-njgrq      2/2     Running   0       22m
        weblogic-operator-webhook-f7ff8c8cf     1/1     Running   0       22m
        
    
    Deje _Cloud Shell_ abierto; lo necesitamos para el laboratorio 4.
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023