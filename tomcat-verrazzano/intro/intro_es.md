# Introducción

## Acerca de este taller

El despliegue de una aplicación Tomcat en un cluster de Kubernetes puede ser un proceso complejo que implica gestionar varios contenedores, configurar redes y garantizar una alta disponibilidad y escalabilidad. Para simplificar este proceso, muchas organizaciones recurren a tecnologías de contenedorización como Docker y Kubernetes.

Docker permite la creación de imágenes de contenedor portátiles que se pueden desplegar en cualquier plataforma, mientras que Kubernetes proporciona un potente motor de orquestación que puede gestionar el despliegue y la ampliación de contenedores.

Oracle Verrazzano es una plataforma en la nube que proporciona una experiencia de gestión unificada para desplegar y operar aplicaciones en contenedores en clusters de Kubernetes. Simplifica el despliegue de aplicaciones complejas al proporcionar un conjunto coherente de herramientas y API que se pueden utilizar para gestionar y supervisar aplicaciones en varios clusters.

En este taller, exploraremos el proceso de despliegue de una imagen de Docker de aplicación Tomcat en un cluster de Kubernetes de Oracle mediante Verrazzano. Vamos a tratar los pasos necesarios para crear una imagen de Docker, configurar Verrazzano, desplegar la aplicación y supervisar su rendimiento.

Al finalizar este taller, sabrá cómo desplegar aplicaciones en contenedores en un cluster de Kubernetes de Oracle mediante Verrazzano.

Este taller está diseñado para ser lo más explicativo posible, pero siéntase libre de pedir aclaraciones o asistencia en el camino.

Tiempo estimado: 90 minutos

### Objetivos

*   Configure su cuenta gratuita de Oracle Cloud (si aún no lo ha hecho).
*   Configure una instancia del motor de Kubernetes de Oracle en Oracle Cloud Infrastructure.
*   Instale el perfil de producción de Verrazzano.
*   Cree una imagen de docker de aplicación tomcat de ejemplo.
*   Despliegue la aplicación tomcat en OKE mediante Verrazzano.
*   Explore la consola de Grafana, Prometheus y OpenSearch Dashboard.

### Requisitos

*   Debe tener una cuenta activada de [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Más información

**Acerca de Tomcat**

Apache Tomcat es un servidor web de código abierto y un contenedor de servlets que se utiliza ampliamente para desplegar aplicaciones web basadas en Java. Tomcat está diseñado para ser ligero, eficiente y fácil de usar, y proporciona un amplio conjunto de funciones para gestionar e implementar aplicaciones web.

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

*   [https://tomcat.apache.org/](https://tomcat.apache.org/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023