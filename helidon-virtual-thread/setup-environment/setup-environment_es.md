# Configurar el entorno

## Introducción

Este laboratorio le guiará por los pasos para configurar el entorno necesario para el taller.

[Recorrido virtual por Lab1](videohub:1_far2bboa)

Tiempo estimado: 10 minutos

### Acerca de Code Editor

El editor de códigos permite editar y desplegar código para varios servicios de OCI directamente desde la consola de OCI. Ahora puede actualizar scripts y flujos de trabajo de servicio sin tener que cambiar entre la consola y los entornos de desarrollo locales. Esto facilita la creación rápida de prototipos de soluciones en la nube, la exploración de nuevos servicios y el cumplimiento de tareas de codificación rápidas.

La integración directa del editor de códigos con Cloud Shell permite acceder a GraalVM Enterprise Native Image y JDK 17 (Java Development Kit) preinstalados en Cloud Shell.

### Objetivos

*   Configurar el editor de códigos
*   Descargue la versión de Maven y JDK necesaria
*   Descarga el código fuente de Helidon

### Requisitos

*   Debe tener una cuenta activada de [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).
*   Debe tener Firefox como explorador para abrir Code Editor.

## Tarea 1: Configuración del editor de códigos

1.  En Cloud Console, haga clic en el icono Code Editor como se muestra. ![Editor de códigos](images/code-editor.png)
    
2.  En Code Editor, haga clic en Terminal -> New Terminal. ![Abrir terminal](images/open-terminal.png)
    

## Tarea 2: Descarga de la versión de Maven y JDK necesaria

1.  Copie los siguientes comandos y péguelos en el terminal. Descarga la versión necesaria de JDK y Maven.
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/archive/jdk-19.0.2_linux-x64_bin.tar.gz
        tar -xzvf jdk-19.0.2_linux-x64_bin.tar.gz</copy>
        

## Tarea 3: Descargar el código fuente de Helidon

1.  Copie los siguientes comandos y péguelos en el terminal para descargar el código fuente de la aplicación helidon.
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  Copie y pegue el siguiente comando para descomprimir _helidon-levelup-2023-main.zip_.
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

Ahora puede _proceder al laboratorio 2_.

## Reconocimientos

*   **Autor**: Joe DiPol
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/fecha**: Ankit Pandey, febrero de 2023