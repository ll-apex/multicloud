# Simulación de un Caso de Parche

## Introducción:

En esta práctica se intentará simular un caso de aplicación de parches. Asuma un caso en el que el entorno inicial utilice _Open JDK_ para la prueba y, finalmente, pase a utilizar _Oracle JDK_ cuando esté listo para producción. El pipeline de compilación y despliegue anterior ya define _Open JDK 20_ como el tipo de Java que se va a utilizar. En este laboratorio, se sustituirá por _Oracle JDK 20_.

Tiempo estimado: 10 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Simulación de un escenario de parche](videohub:1_4pecvhmf)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Modificar el tipo de JDK
*   Verificar el nuevo tipo de JDK en el pipeline de despliegue

### Requisitos

*   Una cuenta en la nube de Oracle Free Tier (prueba), de pago o LiveLabs

## Tarea 1: Cambiar el Installer de JDK

1.  Como hemos visto en el **Laboratorio 2**, en el **log de pipeline de despliegue** completado anteriormente y hemos observado que, cerca de la parte inferior del log, estaba utilizando **Open JDK**. ![jdk abierto](images/open-jdk.png)
    
2.  En el **editor de códigos**, haga clic en el nombre de archivo **`build_spec.yaml`** para abrirlo. Comente la variable de entorno **`JDK20_TAR_GZ_INSTALLER`** que define la ruta de URL a un instalador de **Open JDK** en el número de línea 15 y elimine el comentario **`JDK20_TAR_GZ_INSTALLER`** que define la ruta de URL a un instalador de **Oracle JDK** en el número de línea 16, como se muestra a continuación. ![modificar jdk](images/modify-jdk.png)
    

## Tarea 2: Transferencia del cambio y disparo del pipeline DevOps

1.  Copie y pegue el siguiente comando en el terminal **para confirmar y aplicar** el cambio.
    
        <copy>git add .
        git status
        git commit -m "Replace OpenJDK 20 with OracleJDK 20"
        git push</copy>
        
    
    > El pipeline se activará mediante esta transferencia de Git.
    

## Tarea 3: Verificación del nuevo tipo de JDK en el pipeline de despliegue

1.  Abra la [consola en la nube](https://cloud.oracle.com/) en el nuevo separador. Haga clic en _Menú de hamburguesa_ -> _Servicios para desarrolladores_ -> _Proyectos_ en **DevOps**. ![proyecto devops](images/devops-project.png)
    
2.  Seleccione el compartimento que ha creado en el **Laboratorio 1** y, a continuación, haga clic en _devops-project-helidon-ocw-hol-string_ para abrir el **proyecto DevOps**. ![seleccionar compartimento](images/select-compartment.png)
    
3.  En **Último historial de compilación**, verá **Ejecuciones** y el estado como **Aceptado/En curso**. Haga clic en las últimas ejecuciones, como se muestra a continuación. ![historial de creación](images/build-history.png)
    
4.  Una vez que el pipeline de compilación **ha finalizado las tres etapas**, verá la salida como se muestra a continuación. ![primera ejecución de compilación](images/build-run-first.png)
    
5.  En el progreso de ejecución de compilación, en la tercera etapa, haga clic en **Tres puntos** y, a continuación, haga clic en **Ver despliegue** como se muestra a continuación. Esto abrirá el pipeline de despliegue. ![ver despliegue](images/view-deployment.png)
    
6.  Aquí puede ver **Progreso de despliegue**. Una vez finalizado el pipeline de despliegue, verá la salida como se muestra a continuación. ![ejecución de despliegue](images/deployment-run.png)
    
    > De esta forma se despliega correctamente la aplicación Helidon en instancias informáticas de OCI.
    
7.  Para ver los logs del pipeline de despliegue, haga clic en **Tres puntos** cerca de la etapa de despliegue y haga clic en **Ver detalles** como se muestra a continuación. ![ver logs](images/view-logs.png)
    
8.  Desplácese por los logs y verifique el tipo de JDK. Debe ser **Oracle JDK**, como se muestra a continuación. ![jdk de oracle](images/oracle-jdk.png)
    

Ahora puede **proceder al siguiente laboratorio.**

## Reconocimientos

*   **Autor**: Keith Lustria
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023