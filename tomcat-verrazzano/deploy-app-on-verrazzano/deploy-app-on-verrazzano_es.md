# Despliegue de la aplicación Tomcat en Verrazzano

## Introducción

Este laboratorio le guiará por el proceso de despliegue de la imagen de docker de la aplicación de ejemplo tomcat.

Tiempo estimado: 10 minutos

### Verrazzano y despliegue de aplicaciones

Verrazzano soporta la definición de aplicaciones mediante [Open Application Model (OAM)](https://oam.dev/). Las aplicaciones de Verrazzano se componen de componentes y configuraciones de aplicaciones.

Al desplegar aplicaciones con Verrazzano, la plataforma configura conexiones y políticas de red, entra en la malla de servicios y conecta una pila de supervisión para capturar las métricas, los logs y los rastreos. Verrazzano emplea componentes de OAM para definir las unidades funcionales de un sistema que, a continuación, se ensamblan y configuran mediante la definición de configuraciones de aplicación asociadas.

### Componentes de Verrazzano

Un componente de OAM de Verrazzano es un [Recurso personalizado de Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) que describe los requisitos generales de composición y entorno de una aplicación.

El siguiente código muestra un componente de aplicación tomcat simple para la aplicación de ejemplo tomcat utilizada en este laboratorio. Este recurso describe un componente implementado por una única imagen de Docker que contiene una aplicación tomcat que muestra un único punto final.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: tomcat-component
    spec:
      workload:
        apiVersion: core.oam.dev/v1alpha2
        kind: ContainerizedWorkload
        metadata:
          name: tomcat-workload
          labels:
            app: tomcat
            version: v2
        spec:
          containers:
          - name: tomcat-container
            image: "ENDPOINT_OF_REGION/TENANCY_NAMESPACE/tomcat-example-your_firstname:v1"
            ports:
              - containerPort: 8080
                name: ecnj
    

Una breve descripción de cada campo del componente:

*   **apiVersion**: versión de la definición de recurso personalizado del componente
*   **kind**: nombre estándar de la definición de recursos personalizados del componente
*   **metadata.name**: nombre utilizado para crear el recurso personalizado del componente
*   **spec.workload.kind**: ContainerizedWorkload define una carga de trabajo sin estado de Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports**: puertos expuestos por el contenedor

### Configuraciones de aplicación de Verrazzano

Una configuración de aplicación de Verrazzano es un recurso personalizado de Kubernetes que proporciona personalizaciones específicas del entorno. El siguiente código muestra la configuración de la aplicación para el ejemplo de aplicación tomcat utilizado en este laboratorio. Este recurso especifica el despliegue de la aplicación en el espacio de nombres tomcat-ns.

Las funciones de tiempo de ejecución adicionales se especifican mediante rasgos o superposiciones de tiempo de ejecución que aumentan la carga de trabajo. Por ejemplo, el rasgo de entrada especifica el host y la ruta de entrada, mientras que el rasgo de métricas proporciona el raspador de Prometheus utilizado para obtener las métricas relacionadas con la aplicación.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: tomcat-appconf
      annotations:
        version: v1.0.0
        description: "Verrazzano demo application"
    spec:
      components:
        - componentName: tomcat-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                  scraper: verrazzano-system/vmi-system-prometheus-0
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: tomcat-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
            - trait:
                apiVersion: core.oam.dev/v1alpha2
                kind: ManualScalerTrait
                spec:
                  replicaCount: 2
    

Una breve descripción de cada campo en la configuración de la aplicación:

*   **apiVersion**: versión de la definición de recurso personalizado ApplicationConfiguration
*   **kind**: nombre estándar de la definición de recursos personalizados de configuración de la aplicación
*   **metadata.name**: nombre utilizado para crear este recurso de configuración de aplicación
*   **spec.components**: referencia a los componentes de la aplicación utilizados para especificar la configuración en tiempo de ejecución
*   **spec.components\[\].traits**: rasgos especificados para los componentes de la aplicación

Para explorar rasgos, podemos examinar los campos de un rasgo de entrada:

*   **apiVersion**: versión de la definición de recurso personalizado de rasgo de OAM
*   **kind**: IngressTrait es el nombre de la definición de recursos personalizados de rasgos de entrada de la aplicación OAM
*   **spec.rules.paths**: rutas de contexto para acceder a la aplicación

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Despliegue la aplicación de ejemplo tomcat.
*   Verifique el despliegue de la aplicación de ejemplo tomcat.

### Requisitos

Para ejecutar este laboratorio, debe tener:

*   Cluster de Kubernetes (OKE) que se ejecuta en Oracle Cloud Infrastructure.
*   La instalación de Verrazzano se ha iniciado en un cluster de Kubernetes (OKE).
*   La aplicación _`tomcat-example-your_first_name`_ empaquetada en contenedor está disponible en un registro de contenedor.

## Tarea 1: Despliegue de la aplicación de ejemplo Tomcat

1.  Cree un espacio de nombres `tomcat-ns` para la aplicación de ejemplo tomcat. Conservaremos todos los artefactos de Kubernetes en un espacio de nombres independiente.
    
        <copy>kubectl create namespace tomcat-ns</copy>
        
    
    > Los espacios de nombres son una forma de organizar clusters en subclusters virtuales. Podemos tener cualquier número de espacios de nombres dentro de un cluster, cada uno separado lógicamente de otros, pero con la capacidad de comunicarse entre sí.
    
2.  Necesitamos hacer que Verrazzano sepa que almacenamos en ese espacio de nombres artefactos de Verrazzano. Por lo tanto, debemos agregar una etiqueta que identifique el espacio de nombres `tomcat-ns` gestionado por Verrazzano. Las etiquetas están destinadas a utilizarse para especificar atributos de identificación de objetos que son significativos y relevantes para los usuarios.
    
    Aquí, para el espacio de nombres `tomcat-ns`, le estamos adjuntando una etiqueta, que marca este espacio de nombres como gestionado por Verrazzano. _istio-injection=enabled_, activa un "sidecar" de Istio y, como tal, ayuda a establecer un proxy de Istio. Con un proxy Istio, podemos acceder a otros servicios Istio como un gateway Istio. Para agregar la etiqueta al espacio de nombres `tomcat-ns` con los atributos mencionados anteriormente, copie el siguiente comando y ejecútelo en Cloud Shell:
    
        <copy>kubectl label namespace tomcat-ns verrazzano-managed=true istio-injection=enabled</copy>
        
3.  En _~/tomcat-examples/_, tenemos el archivo de configuración para la aplicación tomcatsample. Como parte del laboratorio 2, creamos una nueva imagen de Docker para el componente de aplicación. Además, hemos transferido esa imagen de Docker al repositorio de Oracle Cloud Container Registry. Ahora, en este laboratorio, modificaremos el archivo _tomcat-comp.yaml_ para que tome la nueva imagen de Docker del repositorio de Oracle Cloud Container Registry. Para modificar el archivo _tomcat-comp.yaml_, copie el siguiente comando y péguelo en _Cloud Shell_.
    
        <copy>vi ~/tomcat-examples/tomcat-comp.yaml</copy>
        
4.  Como parte del laboratorio 3, ha guardado el nombre completo de la imagen de Docker. Debe copiar la siguiente línea y pegarla en el editor de texto. A continuación, debe sustituir `docker image full name` por el nombre de la imagen de Docker. A continuación, copie la línea modificada y pulse _i_ para insertar el texto en el archivo `*tomcat-comp.yaml*`. Pegue la salida en el número de línea **19** (asegúrese de mantener la sangría), pulse _Esc_ y, a continuación, escriba _:wq_ para guardar el archivo.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![Insertar Línea](images/insert-line.png " ")
    
5.  Ahora, queremos desplegar la aplicación en contenedores tomcat en _cluster1_. Para ello, necesitamos una configuración de despliegue de Kubernetes. Este despliegue indica a Kubernetes que cree y actualice instancias para la aplicación tomcat. Aquí, tenemos el archivo `tomcat-comp.yaml`, que indica a Kubernetes.
    
    Para desplegar la aplicación de ejemplo tomcat, copie y pegue los dos comandos siguientes, como se muestra. El archivo `tomcat-comp.yaml` contiene definiciones de varios componentes de OAM, donde un componente de OAM es un recurso personalizado de Kubernetes que describe los requisitos generales de composición y entorno de una aplicación.
    
        <copy>kubectl apply -f ~/tomcat-examples/tomcat-comp.yaml -n tomcat-ns</copy>
        
    
    El archivo `tomcat-app.yaml` es un archivo de configuración de la aplicación Verrazzano, que proporciona personalizaciones específicas del entorno.
    
        <copy>kubectl apply -f ~/tomcat-examples/tomcat-app.yaml -n tomcat-ns</copy>
        
6.  Espere a que los pods tengan el estado _En ejecución_. Utilice este comando _kubectl_ para esperar a que todos los pods tengan el estado _Running_ dentro del espacio de nombres _tomcat-ns_. Se tarda alrededor de 1-2 minutos.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n tomcat-ns --timeout=600s</copy>
        
    
    Cuando los pods estén listos, puede ver una respuesta similar:
    
        $ kubectl wait --for=condition=Ready pods --all -n tomcat-ns --timeout=600s
        pod/tomcat-workload-57d75d66d9-598lq condition met
        pod/tomcat-workload-57d75d66d9-b2m4l condition met
        
    
    También puede mostrar los pods directamente para comprobar su estado:
    
        <copy>kubectl  get po -n tomcat-ns</copy>
        
    
        $ kubectl  get po -n tomcat-ns
        NAME                               READY   STATUS    RESTARTS   AGE
        tomcat-workload-57d75d66d9-598lq   2/2     Running   0          57s
        tomcat-workload-57d75d66d9-b2m4l   2/2     Running   0          54s
        $
        

## Tarea 2: Verificación del despliegue correcto de la aplicación Tomcat

1.  Verifique el punto final `/sample-webapp/`. Para determinar la URL creada a partir de la IP del equilibrador de carga/externo y la configuración de la aplicación, ejecute el siguiente comando:
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io tomcat-ns-tomcat-appconf-gw -n tomcat-ns -o jsonpath={.spec.servers[0].hosts[0]})/sample-webapp/</copy>
        
    
    De esta forma, se imprimirá la URL adecuada en el punto final de REST, por ejemplo:
    
        https://tomcat-appconf.tomcat-ns.xx.xx.xx.xx.nip.io/sample-webapp/
        
2.  Pegue la URL en el explorador y haga clic en _Haga clic para llamar a un servlet_. ![URL de Aplicación](images/application-url.png)
    
3.  Verá _Host Name_ (Nombre de host) y _IP Address_ (Dirección IP). Abra la misma URL en otra _ventana de incógnito_. Observará que otra instancia del servidor servirá esta solicitud. ![Nombre del Host](images/host-name.png) ![Mostrar equilibrio de carga](images/show-loadbalancing.png)
    

> Muestra el equilibrio de carga entre dos réplicas.

Ahora puede **proceder al siguiente laboratorio**.

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023