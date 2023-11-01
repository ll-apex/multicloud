# Introducción

## Acerca de este taller

Este laboratorio ofrece a los asistentes una sesión práctica de nivel introductorio con Helidon y Verrazzano. Aprenderá a crear y ensamblar servicios y desplegarlos en una plataforma de contenedor empresarial.

Esta es una sesión BYOL (Traiga su computadora portátil), así que traiga su computadora portátil Windows, OSX o Linux. Necesitará JDK 11+, Apache Maven (3.6) y Docker instalados antes de empezar.

Durante este laboratorio, instalará las herramientas de CLI de Helidon y desarrollará la aplicación de microservicios HTTP. Para Verrazzano, configurará Oracle Kubernetes Engine (OKE) en Oracle Cloud Infrastructure mediante la cuenta gratuita de Oracle Cloud. La cuenta gratuita es suficiente para explorar y aprender a ejecutar y operar aplicaciones de microservicios a nivel empresarial.

El objetivo de este taller es que aprendas los conceptos básicos del uso de Helidon y Verrazzano y entiendas cómo pueden ayudarte en tus proyectos. Si desea obtener más información sobre las capacidades de estos proyectos, continúe explorando el uso de su cuenta en la nube de Oracle Free Tier y Oracle Cloud Infrastructure.

Este taller está diseñado para ser lo más explicativo posible, pero siéntase libre de pedir aclaraciones o asistencia en el camino.

> El aprovisionamiento del motor de Kubernetes (OKE) de Oracle y la instalación de Verrazzano pueden tardar varios minutos. Para ahorrar tiempo, se le pedirá que configure el entorno y el desarrollo en paralelo. Siga las instrucciones y cambie entre tareas cuando sea necesario.

Tiempo estimado: 90 minutos

### Objetivos

*   Configure su cuenta gratuita de Oracle Cloud (si aún no lo ha hecho).
*   Configure una instancia del motor de Kubernetes de Oracle en Oracle Cloud Infrastructure.
*   Instale la plataforma Verrazzano.
*   Despliegue la aplicación _Helidon MP_.
*   Supervise la aplicación _Helidon MP_ mediante las herramientas de Verrazzano, incluidas OpenSearch, Grafana y Prometheus.

### Requisitos

En esta práctica se asume que tiene instalado lo siguiente en la máquina:

*   JDK 11+
*   Apache Maven (3.6)
*   Docker

## Acerca de Helidon

Helidon es un marco de microservicios de código abierto introducido por Oracle que proporciona una recopilación de bibliotecas Java diseñadas para crear aplicaciones ligeras y rápidas basadas en microservicios. El marco admite dos modelos de programación para escribir microservicios: Helidon SE y Helidon MP.

Mientras que Helidon SE está diseñado para ser un micromarco que admite el modelo de programación reactiva, Helidon MP es una implementación de la especificación MicroProfile. Dado que MicroProfile tiene sus raíces en Java EE, las API de MicroProfile siguen un enfoque declarativo familiar con un uso intensivo de las anotaciones. Esto lo convierte en una buena opción para los desarrolladores de Java EE.

Las funciones MicroProfile tienen como objetivo la implantación de microservicios. Puede encontrar API para definir clientes REST, supervisar la aplicación, leer estadísticas técnicas y funcionales y configurar la aplicación. Helidon también ha agregado API adicionales al conjunto principal de API de Microprofile, lo que le brinda todas las capacidades que necesita para escribir aplicaciones modernas nativas de la nube.

> El estándar [MicroProfile](https://microprofile.io/) se basa en Jakarta EE. Al igual que Jakarta EE, MicroProfile es de código abierto y está desarrollado por la Fundación Eclipse. La implementación con MicroProfile se lleva a cabo en las bibliotecas o servidores de aplicaciones que implementan el estándar, al igual que Jakarta EE.

## Acerca de Verrazzano

Verrazzano es una plataforma integral de contenedores empresariales para implementar aplicaciones tradicionales y nativas de la nube en entornos híbridos y multinube. Está compuesto por un conjunto curado de componentes de código abierto, muchos de los cuales ya puedes usar y confiar, y algunos que fueron escritos específicamente para reunir todas las piezas que hacen de Verrazzano una plataforma cohesiva y fácil de usar.

Verrazzano incluye las siguientes capacidades:

*   Gestión de cargas de trabajo híbridas y de varios clusters
*   Manejo especial para aplicaciones WebLogic, Coherence y Helidon
*   Gestión de infraestructura de varios clusters
*   Supervisión de aplicaciones integrada y preconectada
*   Seguridad integrada
*   Activación DevOps y GitOps

![Verrazzano](images/verrazzano.png)

## Más información

*   [https://helidon.io](https://helidon.io)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023