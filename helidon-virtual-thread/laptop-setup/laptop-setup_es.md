# Crear imagen nativa de la aplicación MP de Helidon

## Introducción

En este laboratorio, aprenderá a configurar el entorno en su máquina linux local.

Tiempo estimado: 10 minutos

### Objetivos

*   Configure el JDK y el Maven necesarios.
*   Descargue el código fuente de la aplicación.

### Requisitos

*   Debe tener un IDE para modificar, crear y ejecutar el proyecto.

## Tarea 1: Descarga de la versión de Maven y JDK necesaria

1.  En su IDE, abra un terminal/consola.
    
2.  Copie los siguientes comandos y péguelos en el terminal. Descarga la versión necesaria de JDK y Maven y define la variable PATH para utilizar el Maven y JDK necesarios.
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/latest/jdk-19_linux-x64_bin.tar.gz
        tar -xzvf jdk-19_linux-x64_bin.tar.gz</copy>
        

## Tarea 2: Descargar el código fuente de Helidon

1.  Copie los siguientes comandos y péguelos en el terminal para descargar el código fuente de la aplicación helidon.
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  Copie y pegue el siguiente comando para descomprimir _helidon-levelup-2023-main.zip_.
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

Ahora puede _proceder al siguiente laboratorio_.

## Reconocimientos

*   **Autor**: Joe DiPol
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/fecha**: Ankit Pandey, febrero de 2023