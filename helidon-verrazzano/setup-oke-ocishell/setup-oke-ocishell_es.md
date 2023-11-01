# Configurar una instancia de Oracle Kubernetes Engine en Oracle Cloud Infrastructure

## Introducción

En este laboratorio, creará un cluster de Kubernetes de 3 nodos configurado con todos los recursos de red necesarios. Los nodos se desplegarán en distintos dominios de errores para garantizar una alta disponibilidad.

Para obtener más información sobre OKE y el despliegue de cluster personalizado, consulte la documentación de [Oracle Kubernetes Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm).

Tiempo estimado: 15 minutos

### Acerca de Producto/Tecnología

Oracle Cloud Infrastructure Container Engine for Kubernetes es un servicio totalmente gestionado, escalable y de alta disponibilidad que puede utilizar para desplegar las aplicaciones de contenedor en la nube. Utilice Container Engine for Kubernetes (a veces, OKE abreviado) cuando su equipo de desarrollo desee crear, desplegar y gestionar de forma fiable aplicaciones en la nube. Especifique los recursos informáticos que necesitan sus aplicaciones y OKE los aprovisionará en Oracle Cloud Infrastructure en un arrendamiento de OCI existente.

### Objetivos

Utilizará la función de cluster _Creación rápida_ para crear una instancia de OKE (Oracle Kubernetes Engine) con los recursos de red necesarios, un pool de nodos y tres nodos de trabajador. El enfoque de _creación rápida_ es la forma más rápida de crear un nuevo cluster. Si acepta todos los valores por defecto, puede crear un nuevo cluster con unos pocos clics.

### Requisitos

*   Debe tener una cuenta activada de [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Tarea 1: Crear un cluster de OKE

La función _Creación rápida_ utiliza los valores por defecto para crear un _cluster rápido_ con nuevos recursos de red según sea necesario. Este enfoque es la forma más rápida de crear un nuevo cluster. Si acepta todos los valores por defecto, puede crear un nuevo cluster con unos pocos clics. Los nuevos recursos de red para el cluster se crean automáticamente, junto con un pool de nodos y tres nodos de trabajador.

1.  En la consola, seleccione _Menú de hamburguesa -> Servicios para desarrolladores -> Clusters de Kubernetes (OKE)_, como se muestra.
    
    ![Menú de hamburger](images/hamburger-menu.png " ")
    
2.  En la página Lista de clusters, seleccione el compartimento que desee, donde podrá crear un cluster y, a continuación, haga clic en _Crear cluster_.
    
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
    *   **Unidad**: unidad que se va a utilizar para cada nodo del pool de nodos. La unidad determina el número de CPU y la cantidad de memoria asignada a cada nodo. En la lista se muestran solo las unidades disponibles en su arrendamiento soportadas por OKE. Seleccione _VM.Standard.E4. Flex_ (que suele estar disponible en la cuenta gratuita de Oracle). Seleccione las 2 OCPU y 32 GB como cantidad de memoria.
    *   **Imagen**: seleccione la imagen por defecto con la versión de Kubernetes _1.26.2_.
    *   **Número de nodos**: número de nodos de trabajador que se van a crear. Deje el valor por defecto, _3_.
    
    > _TENGA MUCHO CUIDADO DE NO DEJAR LA FORMA PREDETERMINADA; LA FORMA PREDETERMINADA ES DEMASIADO PEQUEÑA PARA ADAPTARSE A TODOS LOS COMPONENTES DE VERRAZZANO_
    
    ![Cluster rápido](images/quick-cluster.png " ") ![Número de nodo](images/node-number.png " ")
    
4.  Haga clic en _Siguiente_ para revisar los detalles que ha introducido para el nuevo cluster.
    
5.  En la página _Revisar_, seleccione la casilla de control _Crear un cluster básico_ y, a continuación, haga clic en _Crear cluster_ para crear los nuevos recursos de red y el nuevo cluster.
    
    ![Revisión de cluster](images/review-cluster.png " ")
    
    > Verá los recursos de red que se están creando. Espere hasta que se inicie la solicitud para crear el pool de nodos y, a continuación, haga clic en _Cerrar_.
    
    ![Recurso de red](images/network-resource.png " ")
    
    > A continuación, el nuevo cluster se muestra en la página _Detalles de cluster_. Cuando se crean los nodos maestros, el nuevo cluster adquiere el estado _Activo_ (tarda unos 7 minutos). A continuación, puede continuar con los laboratorios.
    
    ![aprovisionamiento de cluster](images/cluster-provision.png " ")
    
    ![acceso de cluster](images/cluster-access.png " ")
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023