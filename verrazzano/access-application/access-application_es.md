# Accede a la aplicación de ejemplo Libros de Bobby Explora Verrazzano, Grafana y la consola de Kiali

## Introducción

En el laboratorio 3, desplegamos la aplicación Bobby's Book. En este laboratorio, accederemos a la aplicación y verificaremos que está funcionando. Más adelante, exploraremos las consolas Verrazzano y Grafana.

Tiempo estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Acceda a la aplicación Bobby's Book.
*   Explore la consola de Verrazzano.
*   Explore la consola de Grafana.

### Requisitos

\* Ejecute el laboratorio 1, que crea un cluster de OKE en Oracle Cloud Infrastructure. \* Ejecute el laboratorio 1, que configura kubectl para acceder a un cluster de OKE en Oracle Cloud Infrastructure.

*   Ejecute el laboratorio 2, que instala Verrazzano en un cluster de Kubernetes.
*   Ejecute el laboratorio 3, que despliega la aplicación Bobby's Book.
*   Debe tener un editor de texto, donde puede pegar los comandos y las URL y modificarlas, según su entorno. A continuación, puede copiar y pegar los comandos modificados para ejecutarlos en _Cloud Shell_.
*   Recomendamos utilizar Firefox para abrir las URL de la aplicación Libros de Bobby, Verrazano, Grafana y Kiali Console. Sin embargo, si desea usar Chrome, debe usar la solución indocumentada "thisisunsafe" para permitir que Chrome acepte el certificado.

## Tarea 1: Acceder a la aplicación Bobby's Book

1.  Necesitamos una dirección `EXTERNAL_IP` a través de la cual podamos acceder a la aplicación Bobby's Book. Para obtener la dirección `EXTERNAL_IP` del servicio istio-ingressgateway, copie el siguiente comando y péguelo en _Cloud Shell_.
    
        <copy> kubectl get service \
        -n "istio-system" "istio-ingressgateway" \
        -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
           $ kubectl get service \
           > -n "istio-system" "istio-ingressgateway" \
           > -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo
           XX.XX.XX.XX
        
2.  Para abrir la página de inicio del almacén de libros de Robert, copie la siguiente URL y sustituya _XX.XX.XX.XX_ por la dirección _EXTERNAL\_IP_ que hemos obtenido en el último paso, como se muestra en la siguiente imagen.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/</copy>
        
    
    ![Página de libros de Roberts](images/robertbooks.png " ")
    
3.  Haga clic en _Avanzado_, como se muestra a continuación:
    
    ![Robert Advanced](images/robertadvanced.png " ")
    
4.  Seleccione _Continuar con bobs-books.bobs-books. EXTERNAL\_IP .nip.io(no seguro)_ para acceder a la aplicación. Si no está obteniendo esta opción para continuar, escriba _thisisunsafe_ sin espacio en ninguna parte de esta ventana de explorador cromada. A medida que escribe en la ventana del explorador cromado, no puede verla, pero tan pronto como termine de escribir _thisisunsafe_, puede ver la página de la aplicación inmediatamente. Puede encontrar más detalles [aquí](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Robert Unsafe](images/robertunsafe.png " ") ![Robert Page](images/robertpage.png " ")
    
5.  Para abrir la página de inicio del almacén de libros de Bobby, abra un nuevo separador y copie la siguiente URL y sustituya _XX.XX.XX.XX_ por la dirección `EXTERNAL_IP`, como se muestra en la siguiente imagen.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![URL de Bobbys](images/bobbysurl.png " ")
    
    ![Librería Bobby](images/bobbysbookstore.png " ")
    
    > Deje esta página abierta porque la utilizaremos en el laboratorio 8.
    
6.  Para abrir la interfaz de usuario del gestor de órdenes de libros de Bobby, abra un nuevo separador y copie la siguiente URL y sustituya _XX.XX.XX.XX_ por la dirección _EXTERNAL\_IP_, como se muestra en la siguiente imagen.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobs-bookstore-order-manager/orders</copy>
        
    
    ![Gestión de Órdenes](images/ordermanager.png " ")
    
7.  Vuelva a la página _Libros de Bobby_ y compremos un libro. Haga clic en _Libros_ como se muestra en la siguiente imagen.
    
    ![Bloquear orden](images/checkoutorder.png " ")
    
8.  Seleccione la imagen del libro _Crepúsculo_, como se muestra en la siguiente imagen.
    
    ![Libro de compra](images/purchasebook.png " ")
    
9.  En primer lugar, haga clic en _Agregar al carro_ y, a continuación, en _Desproteger_, como se muestra en la siguiente imagen.
    
    ![Haga clic en addcart](images/clickaddcart.png " ")
    
10.  Introduzca los detalles para comprar el libro. En _Su estado_, introduzca el código de estado de dos dígitos y, a continuación, haga clic en _Enviar orden_.
    

![Enviar orden](images/submitorder.png " ") 11. Vuelva a la página _Gestor de órdenes_ y seleccione el botón _Refrescar_ para comprobar si la orden se ha registrado correctamente en el gestor de órdenes.

![Verificar orden](images/verify-order.png " ")

## Tarea 2: Exploración de la consola de Rancher

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
    
10.  Desde la página de inicio de la consola del ranchero, puede ver los enlaces de Verrazzano. Haga clic en _Menú de hamburguesa_ -> _Verrazzano_.
    
    ![Casa del ranchero](images/rancher-home.png)
    
11.  Haga clic en _Aplicaciones_. Esta sección muestra todas las aplicaciones con su espacio de nombres y es gestionada por Verrazzano. Haga clic en la aplicación _bobs-books_ en el espacio de nombres _bobs-books_. ![Aplicación Bobs](images/bobs-application.png)
    
12.  Puede ver los pods asociados a la aplicación. El nombre del pod contiene una cadena única generada automáticamente para identificar esa réplica concreta. Para ver los logs de los pods _bobbys-helidon-stok-application_, haga clic en _Tres puntos_ -> _Ver logs_. ![Logs de Bobbys](images/bobs-logs.png)
    
13.  Puede ver los logs de la aplicación. Si no puede ver el log de la aplicación, haga clic en **Configuración** (botón azul con el icono de engranaje) y cambie el filtro de tiempo para mostrar todas las entradas del log desde el inicio del contenedor. Para ver el componente asociado a la aplicación, haga clic en _Componentes_. ![Log de Bobbys](images/bobs-log.png)
    
14.  Puede ver todos los componentes de la aplicación _bobs-books_ aquí. Para ver cuáles son los recursos relacionados, haga clic en _Recursos relacionados_. ![Recurso de Bobs](images/bobs-resource.png) ![Recursos de Bobs](images/bobs-resources.png)
    
15.  Haga clic en _Menú de Hamburgar_ -> _local_ para abrir el _Explorador de clusters_. El _Explorador de clusters_ permite ver y manipular todos los recursos personalizados y CRD de un cluster de Kubernetes desde la interfaz de usuario de Rancher. ![Cluster de Verrazzano](images/verrazzano-cluster.png)
    
16.  El panel de control ofrece una visión general del cluster y las aplicaciones desplegadas. El número de recursos pertenece a los _Espacios de nombres de usuario_, que son prácticamente todos los recursos, incluido el sistema también. Puede filtrar por espacio de nombres en la parte superior del panel de control, pero esto no es necesario ahora. Haga clic en la opción **Nodos** del menú de la izquierda para obtener una visión general de la carga actual de los nodos.
    
    ![Explorador de cluster](images/cluster-dashboard.png)
    
17.  Todo el despliegue no tiene ningún impacto en el cluster de OKE. Ahora, haga clic en el elemento **Despliegue** del menú de la izquierda para comprobar las aplicaciones desplegadas.
    
    ![nodos clustr](images/cluster-nodes.png)
    
18.  Puede ver varios despliegues.
    
    ![Despliegues](images/cluster-deployments.png)
    

## Tarea 3: Exploración de la consola de Grafana

1.  Haga clic en _Menú de Hamburgar_ -> _Inicio_ para abrir la página de inicio de Rancher. ![Inicio](images/ranchar-menu.png)
    
2.  En la página de inicio, verá el enlace para abrir la _consola de Grafana_. Haga clic en el enlace de la **consola de Grafana** como se muestra:
    
    ![Página inicial de Grafana](images/grafana-link.png)
    
3.  Haga clic en _Avanzado_.
    
    ![Grafana avanzado](images/grafanaadvanced.png " ")
    
4.  Seleccione _grafana.vmi.system.default.XX.XX.XX.XX.nip.io(unsafe)_. Si no obtiene esta opción para continuar, escriba _thisisunsafe_ sin espacio.
    
    ![Continuar con Grafana](images/grafanaproceed.png " ")
    
5.  Se abre la página de inicio de Grafana. Seleccione _Inicio_ en la parte superior izquierda.
    
    ![Página inicial de Grafana](images/grafana-homepage.png " ")
    
6.  Escriba _WebLogic_ y verá _WebLogic Server Dashboard_ en _General_. Seleccione _WebLogic Panel de control del servidor_.
    
    !\[Buscar WebLogic\](images/search-weblogic.png" ")
    
    Aquí puede observar los dos dominios en _Dominio_ y Servidores en Ejecución, Aplicaciones Desplegadas, Nombre de Servidor y su Estado, Uso de Pila, Tiempo de Ejecución, Pila de JVM. Si la aplicación tiene recursos como JDBC y JMS, también puede obtener más información aquí.
    
    ![Panel de control WebLogic](images/weblogic-dashboard.png " ")
    
7.  Ahora, seleccione WebLogic Panel de control del servidor y escriba _Helidon_ y verá _Panel de control de Helidon_. Seleccione _Panel de control de Helidon_.
    
    ![Helidon](images/search-helidon.png " ")
    
    Aquí puede ver varios detalles, como el _estado_ de la aplicación y su _tiempo de actividad_, el recopilador de basura y el total de transferencia automática de Mark y su tiempo, recuento de threads.
    
    ![Panel de control de Helidon](images/helidon-dashboard.png " ")
    
8.  Ahora, seleccione Helidon Monitoring Dashboard y escriba _Coherence_ y verá _Coherence Dashboard Main_. Seleccione _Panel de Control de Coherence Principal_.
    
    ![Coherence](images/search-coherence.png " ")
    
9.  Aquí puede ver los detalles del _Cluster de Coherence_. Para la aplicación Libros de Bobby, tenemos dos clusters de Coherence, uno para Libros de Bobby y otro para Libros de Robert. Debe seleccionar el menú desplegable de _Nombre de cluster_ para ver ambos clusters.
    
    ![Bobby Cluster](images/bobby-cluster.png " ")
    
    ![Robert Cluster](images/robert-cluster.png " ")
    

## Tarea 4: Exploración de la consola de Kiali

1.  Vuelva a la página inicial de Verrazzano y haga clic en la consola de **Kiali**.
    
    ![Enlace de ganadero](images/kiali-link.png)
    
2.  En el lado izquierdo, haga clic en Gráfico.
    
    ![Panel de control de Kiali](images/kiali-dashboard.png " ")
    
3.  En la lista desplegable Espacio de nombres, marque la casilla de _bobs-books_ y haga que el cursor se mueva fuera de la lista desplegable. ![Espacio de nombres de Bobs](images/bobs-namespace.png " ")
    
4.  Puede ver la vista gráfica de la aplicación _bobs-books_. Haga clic en _Leyenda_ para ver la vista Leyenda.
    
    ![Vista gráfica](images/graphical-view.png " ")
    
5.  Aquí puede ver lo que representa cada unidad, como un círculo que representa las _cargas de trabajo_.
    
    ![Vista de leyenda](images/legend-view.png " ")
    
6.  En el lado izquierdo, haga clic en _Aplicaciones_ para ver todas las aplicaciones desplegadas.
    
    ![Aplicaciones](images/kiali-applications.png " ")
    

## Tarea 5: Exploración de los paneles de control de OpenSearch

1.  Vuelva a la página inicial de Verrazzano y haga clic en la consola **OpenSearch Dashboards**.
    
    ![Enlace de Kibana](images/opensearch-link.png)
    
2.  Haga clic en _Continuar con... valor por defecto XX.XX.XX.XX.nip.io(inseguro)_ si es necesario.
    
3.  En la página inicial OpenSearch, haga clic en **Inicio** -> **Detectar**.
    
    ![Haga clic en el panel de Kibana](images/discover-1.png)
    
4.  Seleccione el espacio de nombres _`verrazzano-applications*`_ como se muestra y, a continuación, busque _libros_ y pulse **Intro** o haga clic en **Refrescar**. Debe obtener los logs que contienen _libros_. ![Resultado de log](images/search-books.png)
    

## Tarea 6: Exploración de la consola de Prometheus

1.  Vuelva a la página inicial de Verrazzano y haga clic en la consola de **Prometheus**.
    
    ![Enlace de Prometheus](images/prometheus-link.png)
    
2.  Haga clic en **Continuar con... valor por defecto XX.XX.XX.XX.nip.io(inseguro)** si se le solicita.
    
3.  En la página del panel de control de Prometheus, escriba _get_ en el campo de búsqueda y haga clic en la métrica personalizada _`application_org_books_bobby_BookResource_getBook_total`_.
    
    ![Prometeo ejecutar](images/prometheus-query.png)
    
4.  Haga clic en **Ejecutar** y compruebe el siguiente resultado. Debe ver el valor actual de la métrica, lo que significa cuántas solicitudes ha completado el punto final. También puede cambiar a la vista _Gráfico_ en lugar del modo de _consola_.
    
    ![Valor de Prometheus](images/execute-query.png)
    
    > También puede agregar otra métrica al panel de control. Detecte las métricas disponibles por defecto en la lista.
    

## Tarea 7: Exploración de la consola de Keycloak

1.  Vuelva a la página inicial de Verrazzano y haga clic en la consola de **Keycloak**.
    
    ![Enlace Keycloak](images/keycloak-link.png)
    
2.  Haga clic en **Continuar con... valor por defecto XX.XX.XX.XX.nip.io(inseguro)** si se le solicita.
    
3.  En la página Bienvenido a Keycloak, haga clic en _Consola de administración_. ![Casa Keycloak](images/keycloak-home.png)
    
4.  Ahora necesitamos el nombre de usuario y la contraseña para la consola de Keycloak. _Nombre de usuario_ es _keycloakadmin_ y, para averiguar la contraseña, vuelva a _Cloud Shell_ y pegue el siguiente comando para averiguar la contraseña de la _consola de Keycloak_.
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  Copie la contraseña y vuelva al explorador, donde está abierta la _consola de Keycloak_. Pegue la contraseña en el campo _Password_ (Contraseña), introduzca _keycloakadmin_ como _Username_ (Nombre de usuario) y, a continuación, haga clic en **Sign In** (Iniciar sesión).
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  Aquí puede ver la configuración predeterminada realizada por Verrazzano. ![Inicio de Keycloak](images/keycloak-realms.png)
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023