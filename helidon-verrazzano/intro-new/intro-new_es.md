# Introducción

## Acerca de este taller

En este laboratorio se ofrece a los asistentes una sesión práctica de nivel de introducción con Oracle Kubernetes Engine, Verrazzano y Helidon. Aprenderá a desplegar una aplicación hello helidon de ejemplo en OKE mediante Verrazzano.

Durante este laboratorio, configurará Oracle Kubernetes Engine (OKE) en Oracle Cloud Infrastructure (OCI) mediante la cuenta gratuita de Oracle Cloud. La cuenta gratuita es suficiente para explorar y aprender a ejecutar y operar aplicaciones de microservicios a nivel empresarial.

El objetivo de este taller es que aprendas los conceptos básicos del uso de OKE y Verrazzano y entiendas cómo pueden ayudarte en tu empresa. Si desea obtener más información sobre las capacidades de estos proyectos, continúe explorando el uso de su cuenta en la nube de Oracle Free Tier y Oracle Cloud Infrastructure.

Este taller está diseñado para ser lo más explicativo posible, pero siéntase libre de pedir aclaraciones o asistencia en el camino.

Tiempo estimado: 60 minutos

### Objetivos

*   Configure su cuenta gratuita de Oracle Cloud (si aún no lo ha hecho).
*   Configure una instancia del motor de Kubernetes de Oracle en Oracle Cloud Infrastructure.
*   Instale la plataforma Verrazzano.
*   Despliegue la aplicación _Simple Hello Helidon_.
*   Supervise la aplicación _Simple Hello Helidon_ mediante las herramientas de Verrazzano, incluidos Rancher, Opensearch, Grafana y Prometheus.

### Requisitos

*   Debe tener una cuenta activada de [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

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
*   **Última actualización por/fecha**: Ankit Pandey, marzo de 2023