# Introducción

## Acerca de este taller

En este taller, aprenderá a crear, mantener y actualizar una **aplicación de microservicios de Helidon** de principio a fin mediante el **servicio DevOps de OCI**. Vamos a demostrar cómo acelerar y agilizar todo el proceso de gestión del ciclo de vida, utilizando JDK20 con la tecnología de hilos virtuales.

Tiempo estimado: 5 minutos

### Objetivos

En este taller:

*   Crear un compartimento, grupo dinámico y políticas
*   Cree un proyecto DevOps y recursos relacionados con Terraform
*   Crear y desplegar la aplicación Helidon MP en instancias informáticas mediante OCI DevOps
*   Mostrar integración de SDK de OCI Monitoring
*   Validar la integración de OCI Logging SDK para transferir mensajes al servicio Logging
*   Simular un caso de aplicación de parches
*   Agregar acceso de almacenamiento de objetos desde la aplicación Helidon MP

### Requisitos

*   Una cuenta en la nube de Oracle Free Tier (prueba), de pago o LiveLabs
*   [Conocimientos sobre la consola de OCI](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/console.htm)
*   [Visión general de Networking](https://docs.us-phoenix-1.oraclecloud.com/Content/Network/Concepts/overview.htm)
*   [Conocimientos de compartimentos](https://docs.us-phoenix-1.oraclecloud.com/Content/GSG/Concepts/concepts.htm)
*   [Servicios DevOps de OCI](https://docs.oracle.com/en-us/iaas/Content/devops/using/home.htm)

## Oracle DevOps

El servicio Oracle Cloud Infrastructure DevOps proporciona una plataforma integral de integración y despliegue continuos para desarrolladores. Los servicios DevOps de OCI cubren ampliamente todas las necesidades esenciales de un ciclo de vida de software. Como

*   **Pipelines de despliegue de OCI**: automatice versiones con estrategias de lanzamiento de pipeline declarativas en plataformas OCI como VM y Baremetals, Oracle Container Engine for Kubernetes (OKE) y OCI Functions
*   **Repositorios de artefactos de OCI**: un lugar para almacenar artefactos versionados, incluidos los inmutables.
*   **Repositorios de código de OCI**: OCI proporcionó un servicio de repositorio de código escalable.
*   **Pipelines de compilación de OCI**: un servicio sin servidor y escalable para automatizar la creación, prueba, artefactos e invocaciones de despliegue.

![Arquitectura de Devops](images/oci-devops.png)

## Arquitectura de juego de roles

Creará y desplegará la arquitectura mencionada a continuación mediante servicios y funciones de OCI.

![Diagrama de Devops](images/devops-diagram.png)

Ahora puede **proceder al siguiente laboratorio.**

## Más información

*   [Arquitectura de referencia: Comprenda las estrategias de despliegue de aplicaciones modernas con Oracle Cloud Infrastructure DevOps](https://docs.oracle.com/en/solutions/mod-app-deploy-strategies-oci/index.html)
*   [https://helidon.io/](https://helidon.io/)
*   [https://medium.com/helidon](https://medium.com/helidon)
*   [https://github.com/helidon-io/helidon](https://github.com/helidon-io/helidon)
*   [https://www.youtube.com/Helidon\_Project](https://www.youtube.com/Helidon_Project)
*   [https://helidon.slack.com](https://helidon.slack.com)

## Reconocimientos

*   **Autor**: Keith Lustria
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, mayo de 2023