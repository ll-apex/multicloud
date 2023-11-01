# Introducción

## Acerca de este taller

En este taller, exploraremos cómo desplegar aplicaciones de Spring Boot en un cluster de Oracle Kubernetes Engine (OKE) mediante Verrazzano.

En la era digital actual, las empresas buscan formas de desarrollar e implementar aplicaciones de forma más rápida y eficiente. Kubernetes se ha convertido en el estándar de facto para la orquestación de contenedores, lo que permite a las organizaciones desplegar, gestionar y escalar aplicaciones en contenedores. Oracle Kubernetes Engine (OKE) proporciona un servicio de Kubernetes gestionado que simplifica el despliegue y la gestión de aplicaciones en contenedores en Oracle Cloud Infrastructure (OCI).

Spring Boot es un conocido marco basado en Java que se utiliza para crear aplicaciones web y microservicios. Ofrece una experiencia de desarrollo optimizada con un enfoque de sobreconfiguración de convenciones, lo que la convierte en una excelente opción para crear y desplegar aplicaciones en contenedores en Kubernetes.

Verrazzano es una plataforma nativa de Kubernetes de código abierto que proporciona una solución integral completa para desplegar y gestionar aplicaciones en la nube. Simplifica el despliegue de aplicaciones complejas de varios componentes al proporcionar una vista unificada de la topología de la aplicación, así como un juego de herramientas integradas para el despliegue, la supervisión y la gestión.

Este taller está diseñado para ser lo más explicativo posible, pero siéntase libre de pedir aclaraciones o asistencia en el camino.

Tiempo estimado: 90 minutos

### Objetivos

*   Configure su cuenta gratuita de Oracle Cloud (si aún no lo ha hecho).
*   Configuración de un cluster de OKE en OCI
*   Instalación y configuración de Verrazzano en el cluster
*   Creación de una imagen de contenedor para una aplicación Spring Boot
*   Despliegue de la aplicación en el cluster de OKE mediante Verrazzano
*   Supervisión y gestión de la aplicación mediante las herramientas integradas de Verrazzano

### Requisitos

*   Debe tener una cuenta activada de [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Más información

**Acerca de Springboot**

Spring Boot es un conocido marco basado en Java que se utiliza para crear aplicaciones web y microservicios. Ofrece una experiencia de desarrollo optimizada con un enfoque de sobreconfiguración de convenciones, lo que la convierte en una excelente opción para crear y desplegar aplicaciones en contenedores en Kubernetes.

Spring Boot proporciona una variedad de funciones que facilitan la creación e implementación de aplicaciones rápidamente, como servidores integrados, configuración automática y una amplia gama de bibliotecas y herramientas para crear aplicaciones web, sistemas de mensajería y aplicaciones basadas en datos. También ofrece un rico conjunto de herramientas para pruebas y depuración, lo que facilita la escritura de código robusto y confiable.

Una de las ventajas clave de Spring Boot es su capacidad para integrarse fácilmente con otras tecnologías y marcos, incluidos sistemas de base de datos, colas de mensajes y sistemas de almacenamiento en caché. Esto permite a los desarrolladores crear sistemas distribuidos complejos que pueden manejar cargas de trabajo a gran escala y satisfacer las demandas de las aplicaciones modernas basadas en la nube.

Con su enfoque en la simplicidad y la facilidad de uso, Spring Boot se ha convertido en una opción popular entre los desarrolladores y organizaciones que buscan crear aplicaciones modernas nativas de la nube. Cuenta con una vibrante comunidad de colaboradores y usuarios, y está evolucionando continuamente para satisfacer las necesidades cambiantes de la industria.

**Acerca de Verrazzano**

Verrazzano es una plataforma integral de contenedores empresariales para implementar aplicaciones tradicionales y nativas de la nube en entornos híbridos y multinube. Está compuesto por un conjunto curado de componentes de código abierto, muchos de los cuales ya puedes usar y confiar, y algunos que fueron escritos específicamente para reunir todas las piezas que hacen de Verrazzano una plataforma cohesiva y fácil de usar.

Verrazzano incluye las siguientes capacidades:

*   Gestión de cargas de trabajo híbridas y de varios clusters
*   Manejo especial para aplicaciones WebLogic, Coherence y Helidon
*   Gestión de infraestructura de varios clusters
*   Supervisión de aplicaciones integrada y preconectada
*   Seguridad integrada
*   Activación DevOps y GitOps

![Verrazzano](images/verrazzano.png)

*   [https://spring.io/](https://spring.io/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, abril de 2023