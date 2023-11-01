# Configurar KUBECTL para interactuar con Oracle Container Engine for Kubernetes (OKE) en Oracle Cloud Infrastructure (OCI)

## Introducción

En este laboratorio se muestran los pasos para crear un archivo de configuración que permita el acceso al entorno de Kubernetes en Oracle Cloud Infrastructure.

### Acerca de Producto/Tecnología

Oracle Cloud Infrastructure Container Engine for Kubernetes es un servicio totalmente gestionado, escalable y de alta disponibilidad que puede utilizar para desplegar las aplicaciones de contenedor en la nube. Utilice Container Engine for Kubernetes (a veces, OKE abreviado) cuando su equipo de desarrollo desee crear, desplegar y gestionar de forma fiable aplicaciones en la nube. Especifique los recursos informáticos que necesitan sus aplicaciones y OKE los aprovisionará en Oracle Cloud Infrastructure en un arrendamiento de OCI existente.

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Abra OCI Cloud Shell y configure `kubectl` para interactuar con el cluster de Kubernetes.

### Requisitos

*   Este taller asume que tiene una reserva de este taller en LiveLabs.

## Tarea 1: Configuración de `kubectl` (CLI de cluster de Kubernetes)

Oracle Cloud Infrastructure (OCI) Cloud Shell es un terminal basado en explorador web, accesible desde la consola de Oracle Cloud. Cloud Shell proporciona acceso a un shell de Linux, con una CLI de Oracle Cloud Infrastructure autenticada previamente y otras herramientas útiles (_Git, kubectl, timón, CLI de OCI_) para completar los tutoriales de Verrazzano. Se puede acceder a Cloud Shell desde la consola. Cloud Shell aparecerá en la consola de Oracle Cloud como un marco persistente de la consola y permanecerá activo mientras navega a diferentes páginas de la consola.

Utilizará _Cloud Shell_ para completar este taller.

Utilizaremos `kubectl` para gestionar el cluster de forma remota mediante Cloud Shell. Necesita un archivo `kubeconfig`. Esto se generará mediante la CLI de OCI que está autenticada previamente, por lo que no hay que realizar ninguna configuración antes de empezar a utilizarla.

1.  En la consola, seleccione _Menú de hamburguesa -> Servicios para desarrolladores -> Clusters de Kubernetes (OKE)_, como se muestra.
    
    ![Menú de hamburger](../setup-oke-ocishell/images/hamburgermenu.png " ")
    
2.  Seleccione el compartimento, que tiene asignado. A continuación, haga clic en el cluster _cluster1_.
    
3.  Haga clic en _Access Cluster_ en la página de detalles del cluster.
    
    ![Acceder al cluster](../setup-oke-ocishell/images/accesscluster.png " ")
    
    > Se muestra un cuadro de diálogo desde el que puede abrir Cloud Shell y contiene el comando personalizado de OCI que necesita ejecutar para crear un archivo de configuración de Kubernetes.
    
4.  Deje el _acceso a Cloud Shell_ por defecto y, en primer lugar, seleccione el enlace _Copiar_ para copiar el comando `oci ce...` en Cloud Shell.
    
    ![Copiar Configuración de kubectl](../setup-oke-ocishell/images/copyconfig.png " ")
    
5.  Ahora, haga clic en _Iniciar Cloud Shell_ para abrir la consola incorporada. A continuación, cierre el cuadro de diálogo de configuración antes de pegar el comando en _Cloud Shell_.
    
    ![Iniciar Cloud Shell](../setup-oke-ocishell/images/launchcloudshell.png " ")
    
6.  Copie el comando del portapapeles (Ctrl+V o haga clic con el botón derecho y cópielo) en Cloud Shell y ejecute el comando.
    
    Por ejemplo, el comando tiene el siguiente aspecto:
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![Configuración de kubectl](../setup-oke-ocishell/images/kubeconfig.png " ")
    
7.  Ahora compruebe que `kubectl` está funcionando, por ejemplo, mediante el comando `get node`. Puede que necesite ejecutar este comando varias veces hasta que vea la salida similar a la siguiente.
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.17    Ready    node    11m   v1.21.5
        10.0.10.184   Ready    node    11m   v1.21.5
        10.0.10.31    Ready    node    11m   v1.21.5
        
    
    > Si ve la información del nodo, la configuración se ha realizado correctamente.
    
8.  Puede minimizar y restaurar el tamaño del terminal en cualquier momento mediante los controles de la esquina superior derecha de Cloud Shell.
    
    ![shell en la nube](../setup-oke-ocishell/images/cloudshell.png " ")
    

Deje abierta esta instancia de _Cloud Shell_; la utilizaremos para laboratorios adicionales.

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, febrero de 2023