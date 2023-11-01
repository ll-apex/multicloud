# Despliegue de la aplicación Helidon en Verrazzano

## Introducción

Este laboratorio le guiará por el proceso de despliegue de la aplicación Hello Helidon.

Tiempo estimado: 10 minutos

### Verrazzano y despliegue de aplicaciones

Verrazzano soporta la definición de aplicaciones mediante [Open Application Model (OAM)](https://oam.dev/). Las aplicaciones de Verrazzano se componen de componentes y configuraciones de aplicaciones.

Al implementar aplicaciones con Verrazzano, Verrazzano realiza una gestión inteligente de la carga de trabajo. La plataforma configura conexiones y políticas de red, entra en la malla de servicios y conecta una pila de supervisión para capturar las métricas, los logs y los rastreos. Verrazzano emplea componentes de OAM para definir las unidades funcionales de un sistema que, a continuación, se ensamblan y configuran mediante la definición de configuraciones de aplicación asociadas.

![Verrazzano](images/Verrazzano_IntelligentWorkload.png)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Verifique la instalación correcta del entorno de Verrazzano.
*   Despliegue la aplicación Hello Helidon.
*   Verifique el despliegue de la aplicación Hello Helidon.

### Requisitos

Para ejecutar este laboratorio, debe tener:

*   Cluster de Kubernetes (OKE) que se ejecuta en Oracle Cloud Infrastructure.
*   La instalación de Verrazzano se ha iniciado en un cluster de Kubernetes (OKE).

## Tarea 1: Verificación de una instalación correcta de Verrazzano

Verrazzano instala varios objetos en varios espacios de nombres. Los componentes de Verrazzano se instalan en el espacio de nombres _verrazzano-system_.

1.  Verifique que todos los pods asociados a varios objetos tengan el estado _En ejecución_.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        $   kubectl get pods -n verrazzano-system
        NAME                                         READY   STATUS RESTARTS AGE
        coherence-operator-b5dc669c6-zm4dw           1/1     Running   1     153m
        fluentd-5s7hh                                2/2     Running   1     144m
        fluentd-bt4t4                                2/2     Running   1     144m
        fluentd-ghmkx                                2/2     Running   1     144m
        oam-kubernetes-runtime-5b48f944b-9bv2v       1/1     Running   0     154m
        verrazzano-application-operator-f976cf96f    1/1     Running   0     152m
        verrazzano-application-operator-webhook      1/1     Running   0     152m
        verrazzano-authproxy-58f8975c5d-jhx72        3/3     Running   0     151m
        verrazzano-cluster-operator-544d494f49       1/1     Running   0     144m
        verrazzano-cluster-operator-webhook-79       1/1     Running   0     144m
        verrazzano-console-6f59f7485d-xrdt8          2/2     Running   0     147m
        verrazzano-monitoring-operator-d88cdd66c     2/2     Running   0     151m
        vmi-system-es-master-0                       2/2     Running   0     151m
        vmi-system-grafana-78cd975dd9-vfjv5          3/3     Running   0     151m
        vmi-system-kiali-57d859dc4b-bnvjh            2/2     Running   0     151m
        vmi-system-osd-7687d6fccf-cjwqr              2/2     Running   0     147m
        weblogic-operator-54979449f4-t9q26           2/2     Running   0     152m
        weblogic-operator-webhook-f7ff8c8cf-nc4r     1/1     Running   0     152m
        

## Tarea 2: Despliegue de la aplicación Hello Helidon

1.  Cree un espacio de nombres `hello-helidon` para la aplicación Quickstart-mp de Helidon. Conservaremos todos los artefactos de Kubernetes en un espacio de nombres independiente.
    
        <copy>kubectl create namespace hello-helidon</copy>
        
    
    > Los espacios de nombres son una forma de organizar clusters en subclusters virtuales. Podemos tener cualquier número de espacios de nombres dentro de un cluster, cada uno separado lógicamente de otros, pero con la capacidad de comunicarse entre sí.
    
2.  Necesitamos hacer que Verrazzano sepa que almacenamos en ese espacio de nombres artefactos de Verrazzano. Por lo tanto, debemos agregar una etiqueta que identifique el espacio de nombres `hello-helidon` gestionado por Verrazzano. Las etiquetas están destinadas a utilizarse para especificar atributos de identificación de objetos que son significativos y relevantes para los usuarios.
    
    Aquí, para el espacio de nombres `hello-helidon`, le estamos adjuntando una etiqueta, que marca este espacio de nombres como gestionado por Verrazzano. _istio-injection=enabled_, activa un "sidecar" de Istio y, como tal, ayuda a establecer un proxy de Istio. Con un proxy Istio, podemos acceder a otros servicios Istio como un gateway Istio. Para agregar la etiqueta al espacio de nombres `hello-helidon` con los atributos mencionados anteriormente, copie el siguiente comando y ejecútelo en Cloud Shell:
    
        <copy>kubectl label namespace hello-helidon verrazzano-managed=true istio-injection=enabled</copy>
        
3.  Ahora, queremos desplegar la aplicación en contenedores Hello Helidon en _cluster1_. Para ello, necesitamos una configuración de despliegue de Kubernetes. Este despliegue indica a Kubernetes que cree y actualice instancias para la aplicación Hello Helidon. Aquí, tenemos el archivo `hello-helidon-comp.yaml`, que indica a Kubernetes.
    
    Para desplegar la aplicación Hello Helidon, copie y pegue los dos comandos siguientes, como se muestra. El archivo `hello-helidon-comp.yaml` contiene definiciones de varios componentes de OAM, donde un componente de OAM es un recurso personalizado de Kubernetes que describe los requisitos generales de composición y entorno de una aplicación.
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/verrazzano/verrazzano/v1.5.2/examples/hello-helidon/hello-helidon-comp.yaml -n hello-helidon</copy>
        
    
    El archivo `hello-helidon-app.yaml` es un archivo de configuración de la aplicación Verrazzano, que proporciona personalizaciones específicas del entorno.
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/verrazzano/verrazzano/v1.5.2/examples/hello-helidon/hello-helidon-app.yaml -n hello-helidon</copy>
        
    
    > La acción de implementar la aplicación con verrazzano realiza la siguiente tarea inteligente de gestión de cargas de trabajo.
    
    *   Creación de cuentas de servicio y política de autorización si es necesario
    *   Desplegar la aplicación
    *   Configure el gateway y los servicios virtuales según sea necesario.
    *   Configure certificados, secretos y políticas de red según sea necesario con Istio.
    *   Inicio de la gestión automatizada de la observabilidad
        *   Desecho de logs en OpenSearch
        *   Extracción de toda la información de supervisión a Prometheus y Grafana
4.  Espere a que los pods tengan el estado _En ejecución_. Utilice este comando _kubectl_ para esperar a que todos los pods tengan el estado _Running_ dentro del espacio de nombres hello-helidon. Se tarda alrededor de 1-2 minutos.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s</copy>
        
    
    Cuando los pods estén listos, puede ver una respuesta similar:
    
        $ kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s
        pod/hello-helidon-deployment-58fdd5cd4-94wjf condition met
        
    
    También puede mostrar los pods directamente para comprobar su estado:
    
        $ kubectl  get po -n hello-helidon
        NAME                                       READY   STATUS    RESTARTS   AGE
        hello-helidon-deployment-58fdd5cd4-94wjf   2/2     Running   0          34m
        

## Tarea 3: Verificación del despliegue correcto de la aplicación Hello Helidon

1.  Verifique el punto final `/greet`. Para determinar la URL creada a partir de la IP del equilibrador de carga/externo y la configuración de la aplicación, ejecute el siguiente comando:
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io hello-helidon-hello-helidon-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/greet</copy>
        
    
    De esta forma, se imprimirá la URL adecuada en el punto final de REST, por ejemplo:
    
        https://hello-helidon.hello-helidon.xx.xx.xx.xx.nip.io/greet
        
2.  Utilice este enlace para probarlo desde el explorador. Sin embargo, debido a los certificados autofirmados, debe aceptar el riesgo y permitir que el explorador continúe el procesamiento de la solicitud.
    
    Puede que le resulte más fácil utilizar `curl` porque la respuesta es solo una cadena:
    
        <copy>curl -k https://$(kubectl get gateways.networking.istio.io hello-helidon-hello-helidon-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/greet; echo</copy>
        
    
    Debe ver el mismo resultado que recibió durante el desarrollo:
    
        {"message":"Hello World!"}
        
3.  Deje _Cloud Shell_ abierto; lo utilizaremos para el siguiente laboratorio.
    

## Más información sobre Open Application Model

**Componentes de Verrazzano**

Un componente de OAM de Verrazzano es un [Recurso personalizado de Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) que describe los requisitos generales de composición y entorno de una aplicación.

El siguiente código muestra un componente de aplicación de Helidon simple para la aplicación Hello Helidon utilizada en esta práctica. Este recurso describe un componente implantado por una única imagen de Docker que contiene una aplicación de Helidon que muestra un único punto final.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: hello-helidon-component
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: hello-helidon-workload
          labels:
            app: hello-helidon
            version: v1
        spec:
          deploymentTemplate:
            metadata:
              name: hello-helidon-deployment
            podSpec:
              containers:
                - name: hello-helidon-container
                  image: "ghcr.io/verrazzano/example-helidon-greet-app-v1:1.0.0-1-20230126194830-31cd41f"
                  ports:
                    - containerPort: 8080
                      name: http
    

Una breve descripción de cada campo del componente:

*   **apiVersion**: versión de la definición de recurso personalizado del componente
*   **kind**: nombre estándar de la definición de recursos personalizados del componente
*   **metadata.name**: nombre utilizado para crear el recurso personalizado del componente
*   **metadata.namespace**: espacio de nombres utilizado para crear el recurso personalizado de este componente
*   **spec.workload.kind**: VerrazzanoHelidonWorkload define una carga de trabajo sin estado de Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name**: nombre utilizado para crear la carga de trabajo sin estado de Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.containers**: contenedores de implementación
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports**: puertos expuestos por el contenedor

**Configuraciones de aplicación de Verrazzano**

Una configuración de aplicación de Verrazzano es un recurso personalizado de Kubernetes que proporciona personalizaciones específicas del entorno. El siguiente código muestra la configuración de la aplicación para el ejemplo _quickstart-mp_ de Helidon utilizado en este laboratorio. Este recurso especifica el despliegue de la aplicación en el espacio de nombres hello-helidon.

Las funciones de tiempo de ejecución adicionales se especifican mediante rasgos o superposiciones de tiempo de ejecución que aumentan la carga de trabajo. Por ejemplo, el rasgo de entrada especifica el host y la ruta de entrada, mientras que el rasgo de métricas proporciona el raspador de Prometheus utilizado para obtener las métricas relacionadas con la aplicación.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: hello-helidon
      annotations:
        version: v1.0.0
        description: "Hello Helidon application"
    spec:
      components:
        - componentName: hello-helidon-component
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
                  name: hello-helidon-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/greet"
                          pathType: Prefix
    

Una breve descripción de cada campo en la configuración de la aplicación:

*   **apiVersion**: versión de la definición de recurso personalizado ApplicationConfiguration
*   **kind**: nombre estándar de la definición de recursos personalizados de configuración de la aplicación
*   **metadata.name**: nombre utilizado para crear este recurso de configuración de aplicación
*   **metadata.namespace**: espacio de nombres utilizado para este recurso personalizado de configuración de aplicación
*   **spec.components**: referencia a los componentes de la aplicación utilizados para especificar la configuración en tiempo de ejecución
*   **spec.components\[\].traits**: rasgos especificados para los componentes de la aplicación

Para explorar rasgos, podemos examinar los campos de un rasgo de entrada:

*   **apiVersion**: versión de la definición de recurso personalizado de rasgo de OAM
*   **kind**: IngressTrait es el nombre de la definición de recursos personalizados de rasgos de entrada de la aplicación OAM
*   **spec.rules.paths**: rutas de contexto para acceder a la aplicación

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023