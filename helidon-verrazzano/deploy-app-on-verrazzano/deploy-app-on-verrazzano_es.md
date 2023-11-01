# Despliegue de la aplicación Helidon en Verrazzano

## Introducción

Este laboratorio le guiará por el proceso de despliegue de la aplicación quickstart-mp de Helidon.

Tiempo estimado: 10 minutos

### Verrazzano y despliegue de aplicaciones

Verrazzano soporta la definición de aplicaciones mediante [Open Application Model (OAM)](https://oam.dev/). Las aplicaciones de Verrazzano se componen de componentes y configuraciones de aplicaciones.

Al desplegar aplicaciones con Verrazzano, la plataforma configura conexiones y políticas de red, entra en la malla de servicios y conecta una pila de supervisión para capturar las métricas, los logs y los rastreos. Verrazzano emplea componentes de OAM para definir las unidades funcionales de un sistema que, a continuación, se ensamblan y configuran mediante la definición de configuraciones de aplicación asociadas.

### Componentes de Verrazzano

Un componente de OAM de Verrazzano es un [Recurso personalizado de Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) que describe los requisitos generales de composición y entorno de una aplicación.

El siguiente código muestra un componente de aplicación de Helidon simple para la aplicación _quickstart-mp_ de Helidon utilizada en este laboratorio. Este recurso describe un componente implantado por una única imagen de Docker que contiene una aplicación de Helidon que muestra un único punto final.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: hello-helidon-component
      namespace: hello-helidon
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: hello-helidon-workload
          labels:
            app: hello-helidon
        spec:
          deploymentTemplate:
            metadata:
              name: hello-helidon-deployment
            podSpec:
              containers:
                - name: hello-helidon-container
                  image: "END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp:1.0"
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

### Configuraciones de aplicación de Verrazzano

Una configuración de aplicación de Verrazzano es un recurso personalizado de Kubernetes que proporciona personalizaciones específicas del entorno. El siguiente código muestra la configuración de la aplicación para el ejemplo _quickstart-mp_ de Helidon utilizado en este laboratorio. Este recurso especifica el despliegue de la aplicación en el espacio de nombres hello-helidon.

Las funciones de tiempo de ejecución adicionales se especifican mediante rasgos o superposiciones de tiempo de ejecución que aumentan la carga de trabajo. Por ejemplo, el rasgo de entrada especifica el host y la ruta de entrada, mientras que el rasgo de métricas proporciona el raspador de Prometheus utilizado para obtener las métricas relacionadas con la aplicación.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: hello-helidon-appconf
      namespace: hello-helidon
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
                    port: 8080
                    scraper: verrazzano-system/vmi-system-prometheus-0
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: hello-helidon-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/help/allGreetings"
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

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Verifique la instalación correcta del entorno de Verrazzano.
*   Despliegue la aplicación _quickstart-mp_ de Helidon.
*   Verifique el despliegue de la aplicación _quickstart-mp_ de Helidon.

### Requisitos

Para ejecutar este laboratorio, debe tener:

*   Cluster de Kubernetes (OKE) que se ejecuta en Oracle Cloud Infrastructure.
*   La instalación de Verrazzano se ha iniciado en un cluster de Kubernetes (OKE).
*   Aplicación _quickstart-mp_ de Helidon de contenedor empaquetada disponible en un registro de contenedor.

## Tarea 1: Verificación de una instalación correcta de Verrazzano

Verrazzano instala varios objetos en varios espacios de nombres. Los componentes de Verrazzano se instalan en el espacio de nombres _verrazzano-system_.

1.  Verifique que todos los pods asociados a varios objetos tengan el estado _En ejecución_. Tendrá pods con el estado _En ejecución_, como se muestra a continuación.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        $   kubectl get pods -n verrazzano-system
        NAME                                     READY   STATUS    RESTARTS  AGE
        coherence-operator-b5dc669c6-rk2sm       1/1     Running     4       46m
        fluentd-54f5x                            2/2     Running     2       46m
        fluentd-h7mgh                            2/2     Running     1       46m
        fluentd-xcdfz                            2/2     Running     0       46m
        oam-kubernetes-runtime-5b48f944b-cx7b9   1/1     Running     0       46m
        verrazzano-application-operator-665c5c94 1/1     Running     0       46m
        verrazzano-application-operator-webhook  1/1     Running     0       46m
        verrazzano-authproxy-67776ff58b-8lkzs    3/3     Running     0       46m
        verrazzano-cluster-operator-67dc569555   1/1     Running     0       46m
        verrazzano-cluster-operator-webhook-57f  1/1     Running     0       46m
        verrazzano-console-7d95c98cb9-9ql2x      2/2     Running     0       46m
        verrazzano-monitoring-operator-59ff9576  2/2     Running     0       46m
        vmi-system-es-master-0                   2/2     Running     0       46m
        vmi-system-grafana-7fd956b585-2tbgl      3/3     Running     0       46m
        vmi-system-kiali-dd87546d6-ddxss         2/2     Running     0       46m
        vmi-system-osd-7687d6fccf-nm7kt          2/2     Running     0       46m
        weblogic-operator-54979449f4-njgrq       2/2     Running     0       46m
        weblogic-operator-webhook-f7ff8c8cf      1/1     Running     0       46m
        

## Tarea 2: Despliegue de la aplicación quickstart-mp de Helidon

1.  Descargue el archivo yaml del componente de OAM de Verrazzano y los archivos de configuración de la aplicación de Verrazzano en el entorno de Cloud Shell:
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-app.yaml >~/hello-helidon-app.yaml
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-comp.yaml >~/hello-helidon-comp.yaml
        cd ~
        </copy>
        
2.  Modifique el nombre de la imagen en _hello-helidon-comp.yaml_. Puede utilizar el editor `vi`:
    
        <copy>vi ~/hello-helidon-comp.yaml</copy>
        
3.  Utilice `i` para cambiar el modo de inserción y modificar el nombre de la imagen para reflejar la ruta del repositorio en la línea 23:
    
        image: "END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0"
        
    
    Por ejemplo:
    
        image: "ocir.io/tenancynamespace/quickstart-mp-your_first_name:1.0"
        
4.  Utilice `Esc` para salir del modo de inserción y escriba `:wq` para guardar los cambios y cerrar el editor.
    
5.  Cree un espacio de nombres `hello-helidon` para la aplicación Quickstart-mp de Helidon. Conservaremos todos los artefactos de Kubernetes en un espacio de nombres independiente.
    
        <copy>
        kubectl create namespace hello-helidon
        </copy>
        
    
    > Los espacios de nombres son una forma de organizar clusters en subclusters virtuales. Podemos tener cualquier número de espacios de nombres dentro de un cluster, cada uno separado lógicamente de otros, pero con la capacidad de comunicarse entre sí.
    
6.  Necesitamos hacer que Verrazzano sepa que almacenamos en ese espacio de nombres artefactos de Verrazzano. Por lo tanto, debemos agregar una etiqueta que identifique el espacio de nombres `hello-helidon` gestionado por Verrazzano. Las etiquetas están destinadas a utilizarse para especificar atributos de identificación de objetos que son significativos y relevantes para los usuarios.
    
    Aquí, para el espacio de nombres `hello-helidon`, le estamos adjuntando una etiqueta, que marca este espacio de nombres como gestionado por Verrazzano. _istio-injection=enabled_, activa un "sidecar" de Istio y, como tal, ayuda a establecer un proxy de Istio. Con un proxy Istio, podemos acceder a otros servicios Istio como un gateway Istio. Para agregar la etiqueta al espacio de nombres `hello-helidon` con los atributos mencionados anteriormente, copie el siguiente comando y ejecútelo en Cloud Shell:
    
        <copy>
        kubectl label namespace hello-helidon verrazzano-managed=true istio-injection=enabled
        </copy>
        
7.  Ahora, queremos desplegar la aplicación en contenedores _quickstart-mp_ de Helidon en _cluster1_. Para ello, necesitamos una configuración de despliegue de Kubernetes. Este despliegue indica a Kubernetes que cree y actualice instancias para la aplicación _quickstart-mp_ de Helidon. Aquí, tenemos el archivo `hello-helidon-comp.yaml`, que indica a Kubernetes.
    
    Para desplegar la aplicación _quickstart-mp_ de Helidon, copie y pegue los dos comandos siguientes, como se muestra. El archivo `hello-helidon-comp.yaml` contiene definiciones de varios componentes de OAM, donde un componente de OAM es un recurso personalizado de Kubernetes que describe los requisitos generales de composición y entorno de una aplicación.
    
        <copy>kubectl apply -f ~/hello-helidon-comp.yaml -n hello-helidon</copy>
        
    
    El archivo `hello-helidon-app.yaml` es un archivo de configuración de la aplicación Verrazzano, que proporciona personalizaciones específicas del entorno.
    
        <copy>kubectl apply -f ~/hello-helidon-app.yaml -n hello-helidon</copy>
        
8.  Espere a que los pods tengan el estado _En ejecución_. Utilice este comando _kubectl_ para esperar a que todos los pods tengan el estado _Running_ dentro del espacio de nombres hello-helidon. Se tarda alrededor de 1-2 minutos.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s</copy>
        
    
    Cuando los pods estén listos, puede ver una respuesta similar:
    
        $ kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s
        pod/hello-helidon-deployment-58fdd5cd4-94wjf condition met
        
    
    También puede mostrar los pods directamente para comprobar su estado:
    
        $ kubectl  get po -n hello-helidon
        NAME                                       READY   STATUS    RESTARTS   AGE
        hello-helidon-deployment-58fdd5cd4-94wjf   2/2     Running   0          34m
        

## Tarea 3: Verificación del despliegue correcto de la aplicación quickstart-mp de Helidon

1.  Verifique el punto final `help/allGreetings`. Para determinar la URL creada a partir de la IP del equilibrador de carga/externo y la configuración de la aplicación, ejecute el siguiente comando:
    
        <copy>echo https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings</copy>
        
    
    De esta forma, se imprimirá la URL adecuada en el punto final de REST, por ejemplo:
    
        https://hello-helidon-appconf.hello-helidon.xx.xx.xx.xx.nip.io/help/allGreetings
        
2.  Utilice este enlace para probarlo desde el explorador. Sin embargo, debido a los certificados autofirmados, debe aceptar el riesgo y permitir que el explorador continúe el procesamiento de la solicitud.
    
    Puede que le resulte más fácil utilizar `curl` porque la respuesta es solo una cadena:
    
        <copy>curl -k https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings; echo</copy>
        
    
    Debe ver el mismo resultado que recibió durante el desarrollo:
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
3.  Deje _Cloud Shell_ abierto; lo utilizaremos para el siguiente laboratorio.
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023