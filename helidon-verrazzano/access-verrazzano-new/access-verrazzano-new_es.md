# Uso de Verrazzano para acceder y supervisar la aplicación

## Introducción

Ha desplegado la aplicación Hello Helidon. En este laboratorio, accederemos a la aplicación y la verificaremos utilizando varias herramientas de gestión proporcionadas por Verrazzano.

Tiempo estimado: 20 minutos

**Grafana**

Grafana es una solución de código abierto para ejecutar análisis de datos, obtener métricas que dan sentido a la gran cantidad de datos y supervisar nuestras aplicaciones con la ayuda de buenos paneles de control personalizables. La herramienta ayuda a estudiar, analizar y monitorear datos a lo largo de algún tiempo, técnicamente llamados análisis de series temporales. Resulta útil para realizar un seguimiento del comportamiento del usuario, el comportamiento de la aplicación, la frecuencia de los errores que aparecen en producción o en un entorno de preproducción, el tipo de errores que aparecen y los escenarios contextuales proporcionando datos relativos.

[https://grafana.com/grafana/](https://grafana.com/grafana/)

**Paneles de OpenSearch**

OpenSearch Dashboards es un panel de visualización para el contenido indexado en un cluster OpenSearch. Verrazzano crea un despliegue de paneles de control OpenSearch para proporcionar una interfaz de usuario para consultar y visualizar los datos de log recopilados en OpenSearch.

Para acceder a la consola de paneles de control OpenSearch, lea [Acceso a Verrazzano](https://verrazzano.io/latest/docs/access/).

Para ver los registros de un índice OpenSearch a través de los paneles de control OpenSearch, cree un patrón de índice para filtrar los registros en el índice deseado.

En la tarea 3, exploraremos los paneles de control de OpenSearch.

[https://opensearch.org/docs/latest/dashboards/index/](https://opensearch.org/docs/latest/dashboards/index/)

**Prometheus**

Prometheus es un kit de herramientas de monitoreo y alerta. Prometheus recopila y almacena sus métricas como datos de series temporales, es decir, la información de métricas se almacena con el registro de hora en el que se registró, junto con pares de clave-valor opcionales llamados etiquetas.

[https://prometheus.io/](https://prometheus.io/)

**Rancho**

Rancher es una plataforma que permite a Verrazzano ejecutar contenedores en varios clusters de Kubernetes en producción. Permite la gestión híbrida, lo que significa un único panel de gestión de clusters locales y alojados en servicios en la nube.

[https://rancher.com/](https://rancher.com/)

**Kiali**

Kiali es la interfaz de usuario de Istio, y es muy fácil de usar. Desde la consola de Verrazzano, puede acceder directamente a Kiali. A partir de ahí, puede ver los flujos de tráfico y cualquier punto caliente o áreas problemáticas. A continuación, puede aumentar detalle para ver los detalles de cada uno. Kiali utiliza datos de métricas de Envoy almacenados en Prometheus para crear el modelo gráfico

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Explore la consola de Rancher.
*   Explore la consola de Grafana.
*   Explore los paneles de control de OpenSearch.
*   Explora la consola de Prometeo.
*   Explore la consola de Rancher.
*   Explora la consola de Kiali.

### Requisitos

*   Cluster de Kubernetes (OKE) que se ejecuta en Oracle Cloud Infrastructure.
*   Verrazzano instalado en un cluster de Kubernetes (OKE).
*   Se ha desplegado la aplicación Hello Helidon.

## Tarea 1: Exploración de la consola de Rancher

Verrazzano instala varias consolas. Los puntos finales de una instalación se almacenan en el campo `Status` del recurso personalizado de Verrazzano instalado.

1.  Puede obtener los puntos finales para estas consolas mediante el siguiente comando:
    
        <copy>kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        $ kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .
        {
        "consoleUrl": "https://verrazzano.default.xx.xx.xx.xx.nip.io",
        "grafanaUrl": "https://grafana.vmi.system.default.xx.xx.xx.xx.nip.io",
        "keyCloakUrl": "https://keycloak.default.xx.xx.xx.xx.nip.io",
        "kialiUrl": "https://kiali.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchDashboardsUrl": "https://osd.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchUrl": "https://opensearch.vmi.system.default.xx.xx.xx.xx.nip.io",
        "prometheusUrl": "https://prometheus.vmi.system.default.xx.xx.xx.xx.nip.io",
        "rancherUrl": "https://rancher.default.xx.xx.xx.xx.nip.io"
        }
        $
        
2.  Utilice `https://rancher.default.YOUR_UNIQUE_IP.nip.io` para abrir la consola de Rancher.
    
3.  El perfil _dev_ de Verrazzano utiliza certificados autofirmados, por lo que debe hacer clic en **Avanzado** para aceptar el riesgo y omitir la advertencia.
    
    ![Avanzado](images/rancher-advanced.png)
    
4.  Haga clic en **Proceed to rancher default XX.XX.XX.XX.nip.io(unsafe)**. Si no está obteniendo esta opción para continuar, escriba _thisisunsafe_ sin espacio en ninguna parte de esta ventana del explorador cromada. A medida que escribes en la ventana del navegador Chrome, no puedes verlo, pero tan pronto como termines de escribir _thisisunsafe_, puedes ver la siguiente página inmediatamente. Puede encontrar más detalles [aquí](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Continuar](images/rancher-proceed.png)
    
5.  Haga clic en _Log in with Keycloak_ (Iniciar sesión con Keycloak). ![Inicio de sesión del ranchero](images/keycloak-login.png)
    
6.  Debido a que redirige a la URL de la consola de Keycloak para la autenticación, haga clic en **Avanzado**.
    
    ![Autenticación de Keycloak](images/keycloak-advanced.png)
    
7.  Haga clic en **Continuar con Keycloak default XX.XX.XX.XX.nip.io(unsafe)**. Si no está obteniendo esta opción para continuar, escriba _thisisunsafe_ sin espacio en ninguna parte de esta ventana del explorador cromada. A medida que escribes en la ventana del navegador Chrome, no puedes verlo, pero tan pronto como termines de escribir _thisisunsafe_, puedes ver la siguiente página inmediatamente. Puede encontrar más detalles [aquí](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Continuar](images/keycloak-proceed.png)
    
8.  Ahora necesitamos el nombre de usuario y la contraseña para la consola de Verrazzano. _Nombre de usuario_ es _verrazzano_ y, para averiguar la contraseña, vuelva a _Cloud Shell_ y pegue el siguiente comando para averiguar la contraseña de la _consola de Rancher_.
    
        <copy>kubectl get secret --namespace verrazzano-system verrazzano -o jsonpath={.data.password} | base64 --decode; echo</copy>
        
9.  Copie la contraseña y vuelva al explorador, donde está abierta la _consola de Rancher_. Pegue la contraseña en el campo _Contraseña_, introduzca _verrazzano_ como _Nombre de usuario_ y, a continuación, haga clic en **Conectar**.
    
    ![SignIn](images/verrazzano-sign-in.png)
    
10.  En la página de inicio de la consola del ranchero, puede ver los enlaces de Verrazzano. Puede acceder a cualquiera de estas consolas desde el ranchero console.Click _Menú de hamburguesa_ -> _Verrazzano_.
    
    ![Casa del ranchero](images/rancher-home.png)
    
11.  Haga clic en _Aplicaciones_. Esta sección muestra todas las aplicaciones con su espacio de nombres y es gestionada por Verrazzano. Haga clic en la aplicación _hello-helidon_ en el espacio de nombres _hello-helidon_. ![Aplicación Helidon](images/helidon-application.png)
    
12.  Puede ver los pods asociados a la aplicación. El nombre del pod contiene una cadena única generada automáticamente para identificar esa réplica concreta. Para ver los logs de pods, haga clic en _Tres puntos_ -> _Ver logs_. ![Componentes de Helidon](images/view-pod-logs.png)
    
13.  Puede ver los logs de la aplicación. Si no puede ver el log de la aplicación, haga clic en **Configuración** (botón azul con el icono de engranaje) y cambie el filtro de tiempo para mostrar todas las entradas del log desde el inicio del contenedor. Para ver el componente asociado a la aplicación, haga clic en _Componentes_. ![Logs de aplicaciones](images/view-pod-log.png)
    
14.  Esta aplicación solo tiene una vista component.To de los recursos relacionados. Haga clic en _Recursos relacionados_. ![Recurso de Helidon](images/helidon-resource.png) ![Recursos de Helidon](images/helidon-resources.png)
    
15.  Haga clic en _Menú de Hamburgar_ -> _local_ para abrir el _Explorador de clusters_. El _Explorador de clusters_ permite ver y manipular todos los recursos personalizados y CRD de un cluster de Kubernetes desde la interfaz de usuario de Rancher. ![Cluster de Verrazzano](images/verrazzano-cluster.png)
    
16.  El panel de control ofrece una visión general del cluster y las aplicaciones desplegadas. El número de recursos pertenece a los _Espacios de nombres de usuario_, que son prácticamente todos los recursos, incluido el sistema también. Puede filtrar por espacio de nombres en la parte superior del panel de control, pero esto no es necesario ahora. Haga clic en la opción **Nodos** del menú de la izquierda para obtener una visión general de la carga actual de los nodos.
    
    ![Explorador de cluster](images/cluster-dashboard.png)
    
17.  Todo el despliegue no tiene ningún impacto en el cluster de OKE. Ahora, haga clic en **Carga de trabajo** y, a continuación, en el elemento **Despliegues** del menú de la izquierda para comprobar la aplicación desplegada.
    
    ![Nodos](images/node.png)
    
18.  Puede ver varios despliegues.
    
    ![Despliegues](images/deployment.png)
    

## Tarea 2: Exploración de la consola de Grafana

1.  Haga clic en _Menú de Hamburgar_ -> _Inicio_. ![Inicio](images/ranchar-menu.png)
    
2.  En la página de inicio, verá el enlace para abrir la _consola de Grafana_. Haga clic en el enlace de la **consola de Grafana** como se muestra:
    
    ![Página inicial de Grafana](images/grafana-link.png)
    
3.  Haga clic en **Avanzado**.
    
    ![Avanzado](images/grafana-advanced.png)
    
4.  Haga clic en **Continuar con grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)**. Si no está obteniendo esta opción para continuar, escriba _thisisunsafe_ sin espacio en ninguna parte de esta ventana del explorador cromada. A medida que escribes en la ventana del navegador Chrome, no puedes verlo, pero tan pronto como termines de escribir _thisisunsafe_, puedes ver la siguiente página inmediatamente. Puede encontrar más detalles [aquí](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![continuar](images/grafana-proceed.png)
    
5.  Se abre la página inicial de Grafana. En la parte superior izquierda, haga clic en **Inicio**.
    
    ![Inicio](images/grafana-home.png)
    
6.  Escriba `Helidon` y verá _Helidon Monitoring Dashboard_ en **General**. Haga clic en **Panel de control de Helidon**.
    
    ![Panel de control de Helidon](images/search-helidon.png)
    
7.  Observe los detalles de JVM de la aplicación _quickstart-mp_ de Helidon, como Status, Heap Usage, Running Time, JVM Heap, Thread Count, HTTP Requests, etc. Se trata de un panel de control predefinido específicamente para cargas de trabajo de Helidon. Por supuesto, puede personalizar este panel de control según sus necesidades y agregar información de diagnóstico personalizada.
    
    ![Panel de control](images/helidon-dashboard.png)
    

## Tarea 3: Exploración de los paneles de control de OpenSearch

1.  Vuelva a la página inicial de Verrazzano y haga clic en la consola **OpenSearch Dashboards**.
    
    ![Enlace de Kibana](images/opensearch-link.png)
    
2.  Haga clic en _Continuar con... valor por defecto XX.XX.XX.XX.nip.io(inseguro)_ si es necesario. La primera vez que _OpenSearch Dashboards_ muestra la página de bienvenida. Ofrece datos de ejemplo incorporados para probar OpenSearch, pero puede seleccionar la opción **Explorar por mi cuenta** porque Verrazzano ha completado la configuración necesaria y los datos de la aplicación ya están disponibles.
    
    ![Página de bienvenida de Kibana](images/opensearch-proceed.png)
    
3.  En la página inicial OpenSearch, haga clic en **Inicio** -> **Detectar**.
    
    ![Haga clic en el panel de Kibana](images/discover-1.png)
    
4.  Seleccione el espacio de nombres _verrazzano-application_\* como se muestra a continuación. A continuación, escriba _hola_ en el cuadro de búsqueda para ver los logs de la aplicación hello helidon.
    
    ![Resultado de log](images/log-result.png)
    

## Tarea 4: Exploración de la consola de Prometheus

1.  Vuelva a la página inicial de Verrazzano y haga clic en la consola de **Prometheus**.
    
    ![Enlace de Prometheus](images/prometheus-link.png)
    
2.  Haga clic en **Continuar con... valor por defecto XX.XX.XX.XX.nip.io(inseguro)** si se le solicita.
    
3.  En la página del panel de control de Prometheus, escriba _proveedor_ en el campo de búsqueda y haga clic en la métrica personalizada _`vendor_requests_count_total`_.
    
    ![Prometeo ejecutar](images/prometheus-query.png)
    
4.  Haga clic en **Ejecutar** y compruebe el siguiente resultado. Debe ver el valor actual de la métrica. También puede cambiar a la vista _Gráfico_ en lugar del modo de _consola_.
    
    ![Valor de Prometheus](images/execute-query.png)
    
    > También puede agregar otra métrica al panel de control. Detecte las métricas disponibles por defecto en la lista.
    

## Tarea 5: Exploración de la consola de Kiali

1.  Vuelva a la página inicial de Verrazzano y haga clic en la consola de **Kiali**.
    
    ![Enlace de ganadero](images/kiali-link.png)
    
2.  Haga clic en **Continuar con... valor por defecto XX.XX.XX.XX.nip.io(inseguro)** si se le solicita.
    
3.  En el lado izquierdo, haga clic en Gráfico.
    
    ![Panel de control de Kiali](images/kiali-dashboard.png " ")
    
4.  En el menú desplegable Espacio de nombres, marque la casilla de _verrazzano-system_ y haga que el cursor se mueva fuera del menú desplegable. ![Espacio de nombres de Bobs](images/verrazzano-namespace.png " ")
    
5.  Puede ver la vista gráfica del espacio de nombres _verrazzano-system_. Haga clic en _Icono de leyenda_ para ver la vista Leyenda como se muestra en la captura de pantalla.
    
    ![Vista gráfica](images/graphical-view.png " ")
    
6.  Aquí puede ver lo que representa cada unidad, como un círculo que representa las _cargas de trabajo_.
    
    ![Vista de leyenda](images/legend-view.png " ")
    
7.  En el lado izquierdo, haga clic en _Aplicaciones_ para ver las aplicaciones desplegadas.
    
    ![Aplicaciones](images/applications.png " ")
    

## Tarea 6: Exploración de la consola de Keycloak

1.  Vuelva a la página inicial de Verrazzano y haga clic en la consola de **Keycloak**.
    
    ![Enlace Keycloak](images/keycloak-link.png)
    
2.  Haga clic en **Continuar con... valor por defecto XX.XX.XX.XX.nip.io(inseguro)** si se le solicita.
    
3.  En la página Bienvenido a Keycloak, haga clic en _Consola de administración_. ![Casa Keycloak](images/keycloak-home.png)
    
4.  Ahora necesitamos el nombre de usuario y la contraseña para la consola de Keycloak. _Nombre de usuario_ es _keycloakadmin_ y, para averiguar la contraseña, vuelva a _Cloud Shell_ y pegue el siguiente comando para averiguar la contraseña de la _consola de Keycloak_.
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  Copie la contraseña y vuelva al explorador, donde está abierta la _consola de Keycloak_. Pegue la contraseña en el campo _Password_ (Contraseña), introduzca _keycloakadmin_ como _Username_ (Nombre de usuario) y, a continuación, haga clic en **Sign In** (Iniciar sesión).
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  Aquí puede ver la configuración predeterminada realizada por Verrazzano. ![Inicio de Keycloak](images/keycloak-realms.png)
    

Enhorabuena por haber completado el despliegue de la aplicación Helidon en el laboratorio de Verrazzano.

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/fecha**: Ankit Pandey, marzo de 2023