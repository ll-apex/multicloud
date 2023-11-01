# Introducción

## Acerca de este taller

En este taller se muestra una migración completa de un dominio de servidor WebLogic local a los contenedores y se puede ejecutar en OCI con Oracle Container Engine for Kubernetes (OKE). Demostramos la interfaz gráfica de la interfaz de usuario del kit de herramientas de Kubernetes WebLogic, así como la herramienta de despliegue WebLogic y el operador de Kubernetes de Weblogic. Demostramos cómo el proceso de migración podría simplificarse y acelerarse utilizando un conjunto de herramientas orientado a DevOps.

![Flujo de laboratorio](images/lab-flow.png)

Tiempo estimado: 120 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Taller completo](videohub:1_q1mmkimy)

### Acerca de Producto/Tecnología

WebLogic Kubernetes Toolkit (WKT) es una recopilación de herramientas de código abierto que le ayudan a aprovisionar aplicaciones basadas en WebLogic para ejecutarlas en contenedores Linux en un cluster de Kubernetes. WKT incluye las siguientes herramientas:  

*   [WebLogic Herramientas de despliegue (WDT)](https://github.com/oracle/weblogic-deploy-tooling): conjunto de herramientas de ciclo de vida de un solo propósito que funcionan a partir de una única representación de modelo de metadatos de un dominio WebLogic.
*   [WebLogic Image Tool (WIT)](https://github.com/oracle/weblogic-image-tool): herramienta para crear imágenes de contenedor de Linux para ejecutar dominios WebLogic.
*   [WebLogic Operador de Kubernetes (WKO)](https://github.com/oracle/weblogic-kubernetes-operator): operador de Kubernetes que permite que los dominios WebLogic se ejecuten de forma nativa en un cluster de Kubernetes.

La interfaz de usuario de WKT proporciona una interfaz gráfica de usuario que ajusta las herramientas de WKT, Docker, Helm y el cliente de Kubernetes (kubectl) y ayuda a guiarle en el proceso de creación y modificación de un modelo del dominio WebLogic, creación de una imagen de contenedor de Linux para ejecutar el dominio y configuración y despliegue del software y la configuración necesarios para desplegar y acceder al dominio en el cluster de Kubernetes.

### Objetivos

*   Creación de máquina virtual a partir de imagen de Marketplace
*   Configuración de una instancia del motor de Kubernetes de Oracle en Oracle Cloud Infrastructure
*   Modificación de un proyecto de interfaz de usuario de WKT y creación de un archivo de modelo
*   Creación de imagen auxiliar y envío en Oracle Container Image Registry
*   Despliegue del operador WebLogic en el cluster de Kubernetes de Oracle (OKE)
*   Despliegue del dominio WebLogic en el cluster de Kubernetes de Oracle (OKE)
*   Despliegue del controlador de entrada en el cluster de Kubernetes de Oracle (OKE)
*   Visualización del equilibrio de carga entre los pods del servidor gestionado y exploración de WebLogic-Consola Remota
*   Escala de un cluster WebLogic
*   Actualizar una aplicación desplegada mediante un reinicio sucesivo de la nueva imagen

## Más información

*   [WebLogic Interfaz de usuario de Kubernetes Toolkit](https://oracle.github.io/weblogic-toolkit-ui/)

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023