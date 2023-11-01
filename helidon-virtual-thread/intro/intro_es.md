# Introducción

## Acerca de este taller

En este laboratorio, utilizará Oracle Code Editor para crear, ejecutar y modificar microservicios con Helidon Nima WebServer disponible en Helidon 4. Nima está escrito desde cero para aprovechar los hilos virtuales de Java 19 y proporciona un modelo de programación de bloqueo simple.

También verá cómo se compara con una programación reactiva más compleja.

Durante este laboratorio, utilizará Helidon Project Starter para desarrollar una aplicación de microperfil de Helidon. Es altamente personalizable, proporcionando varias opciones que permiten a los usuarios seleccionar las funciones de Helidon que desean agregar a su proyecto. A continuación, migrará la aplicación a Helidon 4 que se ejecuta en el nuevo Helidon Nima WebServer mediante threads virtuales.

Este taller está diseñado para ser lo más explicativo posible, pero siéntase libre de pedir aclaraciones o asistencia en el camino.

Tiempo estimado: 90 minutos

### Objetivos

*   Explora OCI Code Editor
*   Crear y ejecutar la aplicación Nima
*   Crear y ejecutar la aplicación Reactive
*   Analizar la simplicidad de la aplicación Nima
*   Comparar rastreo de pila para aplicación Nima y Reactiva
*   Generar una aplicación Helidon MP
*   Migración de la aplicación Helidon 3 a Helidon 4

**Acerca de Helidon Nima**

Helidon es un marco de microservicios de código abierto introducido por Oracle que proporciona una recopilación de bibliotecas de Java diseñadas para crear una aplicación ligera y rápida basada en microservicios.

En versiones anteriores de los desarrolladores de Helidon podían elegir entre dos modelos de programación. [Helidon MP](https://helidon.io/docs/v3/#/mp/introduction): implementación del estándar Eclipse MicroProfile con API conocidas por los desarrolladores de Jakarta EE. Y [Helidon SE](https://helidon.io/docs/v3/#/se/introduction) es un marco eficiente y reactivo.

En Helidon 4, Oracle presenta Níma, un servidor web escrito desde cero para utilizar threads virtuales JEP 425. Nima ofrece un modelo de programación fácil de usar con un rendimiento excepcional. Con los threads virtuales, los threads ya no son un recurso escaso que se debe agrupar y gestionar cuidadosamente. En su lugar, son un recurso abundante que se puede crear según sea necesario para manejar solicitudes simultáneas casi ilimitadas. Debido a que cada solicitud se ejecuta en su thread dedicado, es libre de realizar operaciones de bloqueo, como llamar a una base de datos u otro servicio. Y puede hacerlo de una manera simple sincrónica sin temor a bloquear un hilo de plataforma y matar de hambre otras solicitudes. Ya no es necesario recurrir a código asíncrono complicado para implantar un servicio muy concurrente y de baja sobrecarga.

El servidor web Helidon Níma pretende reemplazar a Netty en el ecosistema Helidon. También puede ser utilizado por otros marcos como un componente de servidor web incrustado.

### Requisitos

En este laboratorio se asume que tiene:

*   Cuenta de Oracle Cloud

## Más información

*   [https://helidon.io](https://helidon.io)

## Reconocimientos

*   **Autor**: Joe DiPol
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/fecha**: Ankit Pandey, febrero de 2023