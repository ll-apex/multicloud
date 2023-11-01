# Exploración de logs y explorador de métricas

## Introducción

En este laboratorio, verificará el despliegue correcto de la aplicación Helidon. Además, explorará el servicio de explorador de logs y métricas.

Tiempo estimado: 5 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Explora los logs y el explorador de métricas](videohub:1_7a0qaaif)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Verifique el despliegue correcto de la aplicación Helidon.
*   Explora OCI Metric Explorer
*   Descubre el servicio OCI Logging
*   Validar el estado y la preparación de la aplicación Helidon

### Requisitos

*   Una cuenta en la nube de Oracle Free Tier (prueba), de pago o LiveLabs
*   Conocimientos sobre la consola de OCI

## Tarea 1: Acceso a la aplicación Helidon

En esta tarea, accederemos a la aplicación mediante curl para realizar solicitudes GET y PUT HTTP.

1.  Vuelva al **editor de códigos**, copie y pegue el siguiente comando en el terminal para configurar el nodo de despliegue **`PUBLIC_IP`** como variable de entorno.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Copie y pegue el siguiente comando para colocar una **solicitud GET**. Tendrá una salida similar a la que se muestra a continuación.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  Hola a un nombre, es decir, a **Joe**. Tendrá una salida similar a la que se muestra a continuación.
    
        <copy>curl http://$PUBLIC_IP:8080/greet/Joe</copy>
        {"message":"Hello Joe!","date":[2023,5,23]}
        
4.  Sustituya Hola por otra palabra de saludo, es decir, **Hola**. Tendrá una salida similar a la que se muestra a continuación.
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,23]}
        

## Tarea 2: Exploración del explorador de métricas de OCI

En esta tarea, validará que las **métricas de Helidon** se transfieren al **servicio OCI Monitoring**.

1.  En la consola de Cloud, haga clic en _Menú de hamburguesa_ -> _Observación y gestión_ -> _Explorador de métricas_ en **Supervisión**, como se muestra a continuación. ![explorador de métricas](images/metrics-explorer.png)
    
2.  Desplácese hacia abajo para ir a la sección **Consulta**, seleccione el compartimento correcto y, a continuación, seleccione **`helidon_metrics`** como espacio de nombres de métricas y **`request.count_counter`** como nombre de métrica para crear la consulta. A continuación, haga clic en _Actualizar gráfico_ como se muestra a continuación. ![crear consulta](images/create-query.png)
    
3.  Desplácese hacia arriba y verá una salida similar como se muestra a continuación. Esto le proporciona datos en formato Gráfico para el contador de solicitudes. ![editor de consultas](images/query-editor.png)
    
4.  Para mostrar los datos en la tabla de datos, active el botón **Mostrar tabla de datos** como se muestra a continuación. Al cambiar el botón, _Mostrar tabla de datos_ sustituye por **Mostrar gráfico**. ![tabla de datos](images/data-table.png)
    

## Tarea 3: Exploración del servicio OCI Logging

En esta tarea, validamos la integración del **SDK de registro de OCI** para transferir mensajes al servicio Logging explorando el log **app-log-helidon-ocw-hol** en el grupo de logs **app-log-group-helidon-ocw-hol**.

1.  En la consola de Cloud, haga clic en _Hamburger menu_ (Menú de hamburguesa) -> _Observability & Management_ (Observación y gestión) -> _Logs_ (Logs). ![menú logs](images/logs-menu.png)
    
2.  Seleccione el compartimento correcto y, a continuación, seleccione _`app-log-group-helidon-ocw-hol`_ como **Grupo de logs** y haga clic en **app-log-helidon-ocw-hol** como se muestra a continuación. ![seleccionar compartimento](images/select-compartment.png)
    
3.  En **Filtrar por hora**, seleccione **Hoy** en la lista desplegable y podrá ver los logs de la aplicación. ![ver logs](images/view-logs.png)
    

## Tarea 4: Comprobación del sistema de la aplicación Helidon para validar la vida útil y la preparación

La aplicación **oci-mp** de Helidon agrega una función de **comprobación del sistema** para validar la **actividad** y/o **preparación**. Puede comprobar los archivos de clase _GreetLivenessCheck_ y _GreetReadinessCheck_ respectivamente en el proyecto para ver cómo se realizan. Esto será especialmente útil cuando se ejecute la aplicación como microservicio en un entorno de orquestador como Kubernetes para determinar si el microservicio se debe reiniciar si no está en buen estado. Específico de este laboratorio, la comprobación de preparación se utiliza en la especificación de pipeline de despliegue DevOps después de iniciar la aplicación _para determinar si la aplicación Helidon se ha iniciado correctamente_. Consulte el código en la _línea 79_ de **deployment\_spec.yaml** para verlo en acción.

1.  Vuelva al **editor de códigos**, copie y pegue el siguiente comando en el terminal para configurar el nodo de despliegue **`PUBLIC_IP`** como variable de entorno.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Copie y pegue el siguiente comando para comprobar **liveness**. Tendrá una salida similar a la que se muestra a continuación.
    
        <copy>curl http://$PUBLIC_IP:8080/health/live</copy>
        {"status":"UP","checks":[{"name":"CustomLivenessCheck","status":"UP","data":{"time":1684391639448}}]}
        
3.  Copie y pegue el siguiente comando para comprobar la **preparación**. Tendrá una salida similar a la que se muestra a continuación.
    
        <copy>curl http://$PUBLIC_IP:8080/health/ready</copy>
        {"status":"UP","checks":[{"name":"CustomReadinessCheck","status":"UP","data":{"time":1684391438298}}]}
        

Ahora puede **proceder al siguiente laboratorio.**

## Más información

*   [Aplicación OCI con Helidon](https://medium.com/helidon/oci-application-with-helidon-caa78cacaee5)
*   [Métricas de MP de Helidon](https://helidon.io/docs/v3/#/mp/metrics/metrics)
*   [Guía de métricas de Helidon MP](https://helidon.io/docs/v3/#/mp/guides/metrics)
*   [Estado de MP de Helidon](https://helidon.io/docs/v3/#/mp/health)
*   [Guía de comprobación del sistema de Helidon MP](https://helidon.io/docs/v3/#/mp/guides/health)

## Reconocimientos

*   **Autor**: Keith Lustria
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023