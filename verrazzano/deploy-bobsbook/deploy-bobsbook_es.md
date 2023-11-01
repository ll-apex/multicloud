# Desplegar la aplicación de ejemplo Bobby's Books

## Introducción:

### Acerca de la aplicación Libros de Bobby

![Aplicación de Libros Bobby'](images/bobbyoverview.png " ")

[Bobby's Books](https://verrazzano.io/docs/samples/bobs-books/) consta de tres partes principales:

*   Una aplicación backend de _"procesamiento de pedidos"_, que es una aplicación Java EE con servicios REST y una interfaz de usuario JSP muy sencilla, que almacena datos en una base de datos MySQL. Esta aplicación se ejecuta en el servidor WebLogic.
*   Tienda web de front-end _"Libros de Robert"_, que es un vendedor de libros general. Esto se implanta como microservicio de Helidon, que obtiene datos de libro de Coherence, utiliza un almacén de caché de Coherence para mantener los datos del gestor de pedidos y tiene una interfaz de usuario web de React.
*   Tienda web de front-end _"Bobby's Books"_, que es una librería infantil especializada. Esto se implanta como un microservicio de Helidon, que obtiene datos de libro de un almacén de caché de Coherence (diferente), interactúa directamente con el gestor de pedidos y tiene una interfaz de usuario web de JSF que se ejecuta en el servidor WebLogic.

Para obtener más información y el código fuente de esta aplicación, consulte los [Ejemplos de Verrazzano](https://github.com/verrazzano/examples).

### Verrazzano y despliegue de aplicaciones

Verrazzano soporta la definición de aplicaciones mediante [Open Application Model (OAM)](https://oam.dev/). Las aplicaciones de Verrrazzano se componen de componentes y configuraciones de aplicación.

Al desplegar aplicaciones con Verrazzano, la plataforma configura conexiones, políticas de red e entradas en la malla de servicios, y conecta una pila de supervisión para capturar las métricas, los logs y los rastreos. Verrazzano emplea componentes de OAM para definir las unidades funcionales de un sistema que, a continuación, se ensamblan y configuran mediante la definición de configuraciones de aplicación asociadas.

### Componentes de Verrazzano

Un componente de OAM de Verrazzano es un [Recurso personalizado de Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) que describe los requisitos generales de composición y entorno de una aplicación.

El siguiente código muestra el componente de la aplicación de ejemplo Bobby's Books utilizada en esta práctica de laboratorio. Este recurso describe un componente implantado por una única imagen de Docker que contiene una aplicación de Helidon que muestra un único punto final.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: bobby-helidon
      namespace: bobs-books
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: bobbys-helidon-stock-application
          labels:
            app: bobbys-helidon-stock-application
        spec:
          deploymentTemplate:
            metadata:
              name: bobbys-helidon-stock-application
            podSpec:
              containers:
                - name: bobbys-helidon-stock-application
                  image: container-registry.oracle.com/verrazzano/example-bobbys-helidon-stock-application:0.1.12-1-20210624160519-017d358
                  imagePullPolicy: IfNotPresent
                  ports:
                    - containerPort: 8080
                      name: http
                  env:
                    - name: BACKEND_PORT
                      value: "8001"
                    - name: BACKEND_HOSTNAME
                      value: bobs-bookstore-cluster-cluster-1.bobs-books.svc.cluster.local
                    - name: COH_CLUSTER
                      value: bobbys-coherence
                    - name: COH_CACHE_CONFIG
                      value: coherence-cache-config.xml
                    - name: COH_POF_CONFIG
                      value: pof-config.xml
              imagePullSecrets:
                - name: bobs-books-repo-credentials
    

Una breve descripción de cada campo del componente:

*   **apiVersion**: versión de la definición de recurso personalizado del componente
*   **kind**: nombre estándar de la definición de recursos personalizados del componente
*   **metadata.name**: nombre utilizado para crear el recurso personalizado del componente
*   **metadata.namespace**: espacio de nombres utilizado para crear el recurso personalizado de este componente
*   **spec.workload.kind**: VerrazzanoHelidonWorkload define una carga de trabajo sin estado de Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name**: nombre utilizado para crear la carga de trabajo sin estado de Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.containers**: contenedores de implementación
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports**: puertos expuestos por el contenedor

Puede encontrar la descripción completa del componente para la aplicación de libros de Bobbys en el archivo [bobs-books-comp.yaml](https://github.com/verrazzano/verrazzano/blob/master/examples/bobs-books/bobs-books-comp.yaml).

### Configuraciones de aplicación de Verrazzano

Una configuración de aplicación de Verrazzano es un recurso personalizado de Kubernetes que proporciona personalizaciones específicas del entorno. El siguiente código muestra la configuración de la aplicación para el ejemplo Bob's Books utilizado en esta práctica de laboratorio. Este recurso especifica el despliegue de la aplicación en el espacio de nombres bobs-books.

Las funciones de tiempo de ejecución adicionales se especifican mediante rasgos o superposiciones de tiempo de ejecución que aumentan la carga de trabajo. Por ejemplo, el rasgo de entrada especifica el host y la ruta de entrada, mientras que el rasgo de métricas proporciona el raspador de Prometheus utilizado para obtener las métricas relacionadas con la aplicación.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: bobs-books
      namespace: bobs-books
      annotations:
        version: v1.0.0
        description: "Bob's Books"
    spec:
      components:
        - componentName: robert-helidon
          traits:
            - trait:
                apiVersion: core.oam.dev/v1alpha2
                kind: ManualScalerTrait
                spec:
                  replicaCount: 2
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
        - componentName: robert-coh
        - componentName: bobby-coh
        - componentName: bobby-helidon
        - componentName: bobby-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobbys-front-end"
                          pathType: Prefix
        - componentName: bobs-orders-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobs-bookstore-order-manager/orders"
                          pathType: Prefix
        - componentName: bobs-orders-configmap
        - componentName: bobs-mysql-deployment
        - componentName: bobs-mysql-service
        - componentName: bobs-mysql-configmap
    

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

Este laboratorio le guiará por el proceso de despliegue de la aplicación de ejemplo Bobby's Books.

Tiempo estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Acepte la licencia para descargar las imágenes de los repositorios de Oracle Container Registry.
*   Despliegue la aplicación de ejemplo Bobby's Books.
*   Verifique el despliegue correcto de la aplicación de ejemplo Bobby's Books.

### Requisitos

Para ejecutar el laboratorio 3, debe tener:

*   Ejecute el laboratorio 1, que crea un cluster de OKE en Oracle Cloud Infrastructure.

\* Ejecute el laboratorio 1, que configura kubectl para acceder a un cluster de OKE en Oracle Cloud Infrastructure.

*   Ejecute el laboratorio 2, que instala Verrazzano en el cluster de Kubernetes.
*   Editor de texto, donde puede pegar los comandos y las URL y modificarlas, según su entorno. A continuación, puede copiar y pegar los comandos modificados para ejecutarlos en _Cloud Shell_.

## Tarea 1: Aceptar el acuerdo de licencia para descargar las imágenes de los repositorios de Oracle Container Registry

Para el despliegue de la aplicación de ejemplo _Bobby's Books_, utilizaremos las imágenes de ejemplo. Puesto que estas imágenes contienen productos de Oracle, deberá aceptar el acuerdo de licencia antes de utilizar estas imágenes en el archivo bobs-books, `bobs-books-comp.yaml`.

1.  Haga clic en el enlace de Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) e inicie sesión. Para ello, necesita una cuenta de Oracle.
    
    ![Conexión](images/registrysignin.png " ")
    
2.  Introduzca las _credenciales de cuenta de Oracle_ en los campos Nombre de usuario y Contraseña y, a continuación, haga clic en _Conectar_. Más adelante utilizaremos estas credenciales para crear el secreto en Kubernetes.
    
    ![Oracle SSO](images/oraclesso.png " ")
    
3.  En la página de inicio, seleccione _Verrazzano_.
    
    ![Página inicial de registro](images/registryhomepage.png " ")
    
4.  Por ejemplo-bobbys-coherence, example-bobbys-front-end, example-bobs-books-order-manager y example-roberts-coherence repository, seleccione _English_ como idioma y, a continuación, haga clic en _Continue_.
    
    ![Continuar](images/continue.png " ")
    
5.  Haga clic en _Aceptar_ para aceptar el contrato de licencia.
    
    ![Aceptar acuerdo](images/acceptagreement.png " ")
    
6.  Verifique que ha aceptado el acuerdo de licencia para los repositorios relacionados con Verrazzano como se muestra en la siguiente imagen.
    
    ![Verificar acuerdo de licencia](images/verifyaggrement.png " ")
    
7.  En la página de inicio de Oracle Container Registry, busque _weblogic_
    
    ![Buscar en WebLogic](images/searchweblogic.png " ")
    
8.  Haga clic en _weblogic_ como se muestra y acepte la licencia como lo hizo para Verrazzano imagaes. ![hacer clic en weblogic](images/clickweblogic.png) ![aceptar weblogic](images/acceptagreement.png " ")
    

## Tarea 2: Despliegue de la aplicación Libros de Bobby

Necesitamos descargar el código fuente, donde tenemos archivos de configuración, `bobs-books-app.yaml` y `bobs-books-comp.yaml`.

1.  Descargue el archivo yaml del componente OAM de Verrazzano y los archivos de configuración de aplicaciones de Verrazzano del libro de Bobby. Haga clic en _Copiar_ y pegue el comando en Cloud Shell como se muestra a continuación:
    
        <copy>
        curl -LSs  https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-app.yaml >~/bobs-books-app.yaml
        curl -LSs https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-comp.yaml >~/bobs-books-comp.yaml
        cd ~
        </copy>
        
2.  Conservaremos todos los artefactos de Kubernetes en el espacio de nombres independiente. Cree un espacio de nombres para la aplicación de ejemplo Bob's Books. Los espacios de nombres son una forma de organizar clusters en subclusters virtuales. Podemos tener cualquier número de espacios de nombres dentro de un cluster, cada uno separado lógicamente de otros, pero con la capacidad de comunicarse entre sí. También tenemos que hacer que Verrazzano sepa que almacenamos en ese espacio de nombres artefactos de Verrazzano. Por lo tanto, debemos agregar una etiqueta que identifique el espacio de nombres de bobs-books gestionado por Verrazzano. Las etiquetas están destinadas a utilizarse para especificar atributos de identificación de objetos que son significativos y relevantes para los usuarios. Aquí, para el espacio de nombres bobs-book, le estamos adjuntando una etiqueta, que marca este espacio de nombres como gestionado por Verrazzano. _istio-injection=enabled_, activa un "sidecar" de Istio y, como tal, ayuda a establecer un proxy de Istio. Con un proxy de Istio, podemos acceder a otros servicios de Istio, como una puerta de enlace de Istio y otros. Para agregar la etiqueta al espacio de nombres bobs-books con los atributos mencionados anteriormente, copie el siguiente comando y ejecútelo en _Cloud Shell_.
    
        <copy>
        kubectl create namespace bobs-books
        kubectl label namespace bobs-books verrazzano-managed=true istio-injection=enabled
        </copy>
        
    
    ![Crear espacio de nombres](images/createnamespace.png " ")
    
3.  Copie el siguiente comando para descargar la secuencia de comandos. Este script autentica al usuario de Oracle Container Registry. Si la autenticación es correcta, se crea el secreto de registro de docker. El registro de Docker es una forma de almacenar y versionar imágenes, como GitHub para código normal pero para contenedores (que Kubernetes puede extraer). Aquí, crearemos un secreto de registro de docker para permitir extraer la imagen de ejemplo Libros de Bobby de Oracle Container Registry. Haga clic en _Copiar_ en el siguiente comando y péguelo en cualquier editor de texto que elija y sustituya el nombre de usuario y la contraseña por el ID de correo electrónico y la contraseña, respectivamente, que utilizó en la tarea 1, para aceptar el acuerdo de licencia para descargar imágenes desde Oracle Container Registry. A continuación, en Cloud Shell, pegue el comando modificado como se muestra:
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/deploy-bobsbook/create_secret.sh >~/create_secret.sh
        chmod 777 create_secret.sh
        ./create_secret.sh username 'password'    
        </copy>
        
    
    ![Crear secreto](images/createsecret.png " ")
    
    > Introduzca la contraseña entre comillas simples.
    
4.  Necesitamos crear varios secretos de Kubernetes con credenciales. En la aplicación Libros de Bobby, tenemos dos dominios WebLogic _bobby-front-end_ y _bobs-bookstore_. Las credenciales para el dominio WebLogic se mantienen en un secreto de Kubernetes donde el nombre del secreto se especifica mediante _webLogicCredentialsSecret_ en el recurso de dominio WebLogic. Además, el secreto de credenciales de dominio se debe crear en el espacio de nombres en el que se ejecutará el dominio. Necesitamos crear los secretos denominados _bobbys-front-end-weblogic-credentials_ y _bobs-bookstore-weblogic-credentials_ utilizados por los dominios del servidor WebLogic, con un valor de nombre de usuario de `weblogic` y una contraseña que se genera aleatoriamente en el espacio de nombres de bobs-books. Nuestra aplicación Bobby's Books utiliza una base de datos _mysql_. Por lo tanto, creamos un nuevo secreto denominado _mysql-credentials_, con un valor de nombre de usuario `weblogic`, una contraseña generada aleatoriamente y una URL de JDBC, _jdbc:mysql://mysql.bobs-books.svc.cluster.local:3306/books_, en el espacio de nombres bobs-books. Utilizaremos estos valores en la cadena de conexión JDBC del objeto WebLogic DataSource. Copie y pegue el bloque de comandos en _Cloud Shell_.
    
        <copy>
        export WLS_USERNAME=weblogic
        export WLS_PASSWORD=$((< /dev/urandom tr -dc 'A-Za-z0-9"'\''/<=>?\^_`|~' | head -c10);(date +%S))
        echo $WLS_PASSWORD
        kubectl create secret generic bobbys-front-end-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic bobs-bookstore-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic mysql-credentials \
            --from-literal=username=$WLS_USERNAME \
            --from-literal=password=$WLS_PASSWORD \
            --from-literal=url=jdbc:mysql://mysql.bobs-books.svc.cluster.local:3306/books \
            -n bobs-books
        cd ~
        </copy>
        
    
    ![Crear recurso](images/createresource.png " ")
    
5.  Tenemos un cluster de Kuberneter, _cluster1__verrazzano_, con tres nodos. Ahora, queremos desplegar la aplicación en contenedores Bobby's Books en _cluster1__verrazzano_. Para ello, necesitamos una configuración de despliegue de Kubernetes. Este despliegue indica a Kubernetes que cree y actualice instancias para la aplicación Libros de Bobby. Aquí, tenemos el archivo `bobs-books-comp.yaml`, que indica a Kubernetes que despliegue la aplicación Libros de Bobby. Copie y pegue los dos comandos siguientes, como se muestra. El archivo `bobs-books-comp.yaml` contiene definiciones de varios componentes de OAM, donde un componente de OAM es un recurso personalizado de Kubernetes que describe los requisitos generales de composición y entorno de una aplicación. Para obtener más información sobre el archivo `bobs-books-comp.yaml`, consulte los componentes de Verrazzano en la sección Introducción de este laboratorio 3.
    
        <copy>kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh created
        component.core.oam.dev/robert-helidon created
        component.core.oam.dev/bobby-coh created
        component.core.oam.dev/bobby-helidon created
        component.core.oam.dev/bobby-wls created
        component.core.oam.dev/bobs-mysql-configmap created
        component.core.oam.dev/bobs-mysql-service created
        component.core.oam.dev/bobs-mysql-deployment created
        component.core.oam.dev/bobs-orders-configmap created
        component.core.oam.dev/bobs-orders-wls created
        $
        
6.  El archivo `bobs-books-app.yaml` es un archivo de configuración de la aplicación Verrazzano, que proporciona personalizaciones específicas del entorno. Para obtener más información sobre el archivo `bobs-books-app.yaml`, consulte Configuración de la aplicación de Verrazzano en la sección Introducción de este laboratorio 3.
    
        <copy>kubectl apply -f ~/bobs-books-app.yaml -n bobs-books</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        $ kubectl apply -f ~/bobs-books-app.yaml -n bobs-books
        applicationconfiguration.core.oam.dev/bobs-books created
        $
        
7.  Espere a que todos los pods de la aplicación de ejemplo Bobby's Books tengan el estado _Running_. Puede que tenga que repetir este comando varias veces antes de que se realice correctamente. Los pods de Coherence y servidor WebLogic pueden tardar un poco en crearse y estar listos. Este comando _kubectl_ esperará a que todos los pods tengan el estado _Running_ dentro del espacio de nombres bobs-books. Se tarda alrededor de 4-5 minutos. Si espera más de 5 minutos, puede volver a ejecutar el comando.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
          $ kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s
          pod/bobbys-coherence-0 condition met
          pod/bobbys-front-end-adminserver condition met
          pod/bobbys-front-end-managed-server1 condition met
          pod/bobbys-helidon-stock-application-5f74cbcc8b-cw4x4 condition met
          pod/bobs-bookstore-adminserver condition met
          pod/bobs-bookstore-managed-server1 condition met
          pod/mysql-6bc8f9f785-n4qjh condition met
          pod/robert-helidon-65b8874988-7x5vj condition met
          pod/robert-helidon-65b8874988-vnntp condition met
          pod/roberts-coherence-0 condition met
          pod/roberts-coherence-1 condition met
          $
        

## Tarea 3: Verificar el despliegue correcto de la aplicación Bobby's Book

Verifique que la configuración de la aplicación, los dominios, los recursos de Coherence y el rasgo de entrada existen.

1.  Para verificar que la aplicación _Libros de Bobby_ se ha desplegado correctamente en el espacio de nombres bobs-books.
    
        <copy>kubectl get ApplicationConfiguration -n bobs-books</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        $ kubectl get ApplicationConfiguration -n bobs-books
          NAME         AGE
          bobs-books   72m
        $
        
2.  Para verificar que ambos dominios WebLogic se hayan creado correctamente en el espacio de nombres bobs-books.
    
        <copy>kubectl get Domain -n bobs-books</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        $ kubectl get Domain -n bobs-books
          NAME               AGE
          bobbys-front-end   73m
          bobs-orders-wls    73m
        $
        
3.  Para verificar que ambos clusters de Coherence se han creado correctamente en el espacio de nombres bobs-books.
    
        <copy>kubectl get Coherence -n bobs-books</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        $ kubectl get Coherence -n bobs-books
        NAME              CLUSTER           ROLE           REPLICAS  READY PHASE
        bobbys-coherence  bobbys-coherence  bobbys-coherence  1       1    Ready
        roberts-coherence roberts-coherence roberts-coherence 2       2    Ready
        $
        
4.  Para obtener IngressTrait para la aplicación Bobby's Book, ejecute el siguiente comando en _Cloud Shell_.
    
        <copy>kubectl get IngressTrait -n bobs-books</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        $ kubectl get IngressTrait -n bobs-books
          NAME                               AGE
          bobby-wls-trait-79b67d9d88         76m
          bobs-orders-wls-trait-57b4d8cb4b   76m
          robert-helidon-trait-54d7bcd54b    76m
        $
        
5.  Verifique que los pods de servicio se hayan creado correctamente y que pasen al estado _Running_. Tenga en cuenta que esto puede tardar unos minutos y que puede ver que algunos de los servicios terminan y se reinician. Por último, observará que todos los pods asociados al espacio de nombres bobs-books tienen el estado _En ejecución_. Copie los detalles de los pods para _bobbys-helidon-stock-application_.
    
        <copy>kubectl get pods -n bobs-books</copy>
        
    
        $ kubectl get pods -n bobs-books
        NAME                                          READY STATUS    RESTARTS   AGE
        bobbys-coherence-0                             2/2  Running   0          7m51s
        bobbys-front-end-adminserver                   4/4  Running   0          5m28s
        bobbys-front-end-managed-server1               4/4  Running   0          4m30s
        bobbys-helidon-stock-application-5f74cbc-cw4x4 2/2  Running   0          7m54s
        bobs-bookstore-adminserver                     4/4  Running   0          4m31s
        bobs-bookstore-managed-server1                 4/4  Running   0          3m41s
        mysql-6bc8f9f785-n4qjh                         2/2  Running   0          5m52s
        robert-helidon-65b8874988-7x5vj                2/2  Running   0          7m53s
        robert-helidon-65b8874988-vnntp                2/2  Running   0          7m54s
        roberts-coherence-0                            2/2  Running   0          7m52s
        roberts-coherence-1                            2/2  Running   0          7m51s
        $
        
    
    > Observe el nombre del pod para **bobbys-helidon-stock-application**. Cuando volvamos a desplegar este componente, observará que este pod pasará al estado _Terminando_ y que el nuevo pod se iniciará y tendrá el estado _En ejecución_ en el laboratorio 7.
    
    Deje _Cloud Shell_ abierto; lo utilizaremos también para las próximas prácticas.
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023