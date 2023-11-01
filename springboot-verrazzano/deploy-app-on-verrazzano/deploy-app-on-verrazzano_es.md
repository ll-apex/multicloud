# Despliegue de la aplicación Springboot en Verrazzano

## Introducción

Este laboratorio le guiará por el proceso de despliegue de la imagen de docker de la aplicación Springboot en OKE.

Tiempo estimado: 10 minutos

### Verrazzano y despliegue de aplicaciones

Verrazzano soporta la definición de aplicaciones mediante [Open Application Model (OAM)](https://oam.dev/). Las aplicaciones de Verrazzano se componen de componentes y configuraciones de aplicaciones.

Al desplegar aplicaciones con Verrazzano, la plataforma configura conexiones y políticas de red, entra en la malla de servicios y conecta una pila de supervisión para capturar las métricas, los logs y los rastreos. Verrazzano emplea componentes de OAM para definir las unidades funcionales de un sistema que, a continuación, se ensamblan y configuran mediante la definición de configuraciones de aplicación asociadas.

### Componentes de Verrazzano

Un componente de OAM de Verrazzano es un [Recurso personalizado de Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) que describe los requisitos generales de composición y entorno de una aplicación.

El siguiente código muestra un componente simple de la aplicación springboot para la aplicación de ejemplo tomcat utilizada en este laboratorio. Este recurso describe un componente implementado por una sola imagen de Docker que contiene una aplicación Springboot que expone un único punto final.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: springboot-component
    spec:
      workload:
        apiVersion: core.oam.dev/v1alpha2
        kind: ContainerizedWorkload
        metadata:
          name: springboot-workload
          labels:
            app: springboot
            version: v1
        spec:
          containers:
          - name: springboot-container
            image: "EndpointOfYourRegion/TenancyNamespace/springboot-your_firstname:v1"
            ports:
              - containerPort: 8080
                name: springboot
    

Una breve descripción de cada campo del componente:

*   **apiVersion**: versión de la definición de recurso personalizado del componente
*   **kind**: nombre estándar de la definición de recursos personalizados del componente
*   **metadata.name**: nombre utilizado para crear el recurso personalizado del componente
*   **spec.workload.kind**: ContainerizedWorkload define una carga de trabajo sin estado de Kubernetes
*   **spec.workload.spec.containers.ports.containerPort**: puertos expuestos por el contenedor

### Configuraciones de aplicación de Verrazzano

Una configuración de aplicación de Verrazzano es un recurso personalizado de Kubernetes que proporciona personalizaciones específicas del entorno. El siguiente código muestra la configuración de la aplicación para el ejemplo de aplicación Springboot que se utiliza en esta práctica. Este recurso especifica el despliegue de la aplicación en el espacio de nombres springboot.

Las funciones de tiempo de ejecución adicionales se especifican mediante rasgos o superposiciones de tiempo de ejecución que aumentan la carga de trabajo. Por ejemplo, el rasgo de entrada especifica el host y la ruta de entrada, mientras que el rasgo de métricas proporciona el raspador de Prometheus utilizado para obtener las métricas relacionadas con la aplicación.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: springboot-appconf
      annotations:
        version: v1.0.0
        description: "Spring Boot application"
    spec:
      components:
        - componentName: springboot-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                  scraper: verrazzano-system/vmi-system-prometheus-0
                  path: "/actuator/prometheus"
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: springboot-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
    

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

*   Despliegue la aplicación springboot.
*   Verifique la implementación de la aplicación springboot.

### Requisitos

Para ejecutar este laboratorio, debe tener:

*   Cluster de Kubernetes (OKE) que se ejecuta en Oracle Cloud Infrastructure.
*   La instalación de Verrazzano se ha iniciado en un cluster de Kubernetes (OKE).
*   La aplicación _`springboot-your_firstname`_ empaquetada en contenedor está disponible en un registro de contenedor.

## Tarea 1: Despliegue de la aplicación de inicio Spring

1.  Cree un espacio de nombres `springboot` para la aplicación springboot. Conservaremos todos los artefactos de Kubernetes en un espacio de nombres independiente.
    
        <copy>kubectl create namespace springboot</copy>
        
    
    > Los espacios de nombres son una forma de organizar clusters en subclusters virtuales. Podemos tener cualquier número de espacios de nombres dentro de un cluster, cada uno separado lógicamente de otros, pero con la capacidad de comunicarse entre sí.
    
2.  Necesitamos hacer que Verrazzano sepa que almacenamos en ese espacio de nombres artefactos de Verrazzano. Por lo tanto, debemos agregar una etiqueta que identifique el espacio de nombres `springboot` gestionado por Verrazzano. Las etiquetas están destinadas a utilizarse para especificar atributos de identificación de objetos que son significativos y relevantes para los usuarios.
    
    Aquí, para el espacio de nombres `springboot`, le estamos adjuntando una etiqueta, que marca este espacio de nombres como gestionado por Verrazzano. _istio-injection=enabled_, activa un "sidecar" de Istio y, como tal, ayuda a establecer un proxy de Istio. Con un proxy Istio, podemos acceder a otros servicios Istio como un gateway Istio. Para agregar la etiqueta al espacio de nombres `springboot` con los atributos mencionados anteriormente, copie el siguiente comando y ejecútelo en Cloud Shell:
    
        <copy>kubectl label namespace springboot verrazzano-managed=true istio-injection=enabled</copy>
        
3.  En _~/springboot-app_, tenemos el archivo de configuración para la aplicación springboot. Como parte del laboratorio 2, hemos creado una nueva imagen de Docker para el componente de aplicación. Además, hemos transferido esa imagen de Docker al repositorio de Oracle Cloud Container Registry. Ahora, en este laboratorio, modificaremos el archivo _springboot-comp.yaml_ para que tome la nueva imagen de Docker del repositorio de Oracle Cloud Container Registry. Para modificar el archivo _springboot-comp.yaml_, copie el siguiente comando y péguelo en _Cloud Shell_.
    
        <copy>vi ~/springboot-app/springboot-comp.yaml</copy>
        
4.  Como parte del laboratorio 2, ha guardado el nombre completo de la imagen de Docker. Debe copiar la siguiente línea y pegarla en el archivo de texto. A continuación, debe sustituir `docker image full name` por el nombre de la imagen de Docker. A continuación, copie la línea modificada y pulse _i_ para insertar el texto en el archivo `*springboot-comp.yaml*`. Pegue la salida en el número de línea **19** (asegúrese de mantener la sangría), pulse _Esc_ y, a continuación, escriba _:wq_ para guardar el archivo.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![Insertar Línea](images/insert-line.png " ")
    
5.  Ahora, queremos desplegar la aplicación en contenedores Springboot en _cluster1_. Para ello, necesitamos una configuración de despliegue de Kubernetes. Este despliegue indica a Kubernetes que cree y actualice instancias para la aplicación springboot. Aquí, tenemos el archivo `springboot-comp.yaml`, que indica a Kubernetes.
    
    Para desplegar la aplicación springboot, copie y pegue los dos comandos siguientes, como se muestra. El archivo `springboot-comp.yaml` contiene definiciones de varios componentes de OAM, donde un componente de OAM es un recurso personalizado de Kubernetes que describe los requisitos generales de composición y entorno de una aplicación.
    
        <copy>kubectl apply -f ~/springboot-app/springboot-comp.yaml -n springboot</copy>
        
    
    El archivo `springboot-app.yaml` es un archivo de configuración de la aplicación Verrazzano, que proporciona personalizaciones específicas del entorno.
    
        <copy>kubectl apply -f ~/springboot-app/springboot-app.yaml -n springboot</copy>
        
6.  Espere a que los pods tengan el estado _En ejecución_. Utilice este comando _kubectl_ para esperar a que todos los pods tengan el estado _Running_ dentro del espacio de nombres _springboot_. Se tarda alrededor de 1-2 minutos.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s</copy>
        
    
    Cuando los pods estén listos, puede ver una respuesta similar:
    
        $ kubectl wait --for=condition=Ready pods --all -n kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s --timeout=600s
        pod/springboot-workload-5c4ffc8954-mbgbb condition met
        pod/springboot-workload-bfb9d487-dwp7q condition met
        
    
    También puede mostrar los pods directamente para comprobar su estado:
    
        <copy>kubectl  get po -n springboot</copy>
        
    
        $ kubectl  get po -n springboot
        NAME                                 READY   STATUS    RESTARTS   AGE
        springboot-workload-bfb9d487-dwp7q   2/2     Running   0          68s
        $
        

## Tarea 2: Verificación del despliegue correcto de la aplicación Springboot

1.  Verifique el punto final `/facts`. Para determinar la URL creada a partir de la IP del equilibrador de carga/externo y la configuración de la aplicación, ejecute el siguiente comando:
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io springboot-springboot-appconf-gw -n springboot -o jsonpath={.spec.servers[0].hosts[0]})/facts</copy>
        
    
    De esta forma, se imprimirá la URL adecuada en el punto final de REST, por ejemplo:
    
        https://springboot-appconf.springboot.xx.xx.xx.xx.nip.io/facts
        
2.  Pegue la URL en el explorador y haga clic en el botón _Refrescar_ del explorador. Cada vez que haces clic en el botón Actualizar, te proporciona un hecho aleatorio sobre Verrazzano. ![URL de Aplicación](images/application-url.png) ![Otro hecho](images/another-fact.png)
    

Ahora puede **proceder al siguiente laboratorio**.

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023