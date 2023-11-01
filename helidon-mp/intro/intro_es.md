# Introducción

## Acerca de este taller

Durante este laboratorio, utilizará Helidon Project Starter para desarrollar la aplicación de microservicio HTTP. Es altamente personalizable, proporcionando varias opciones que permiten a los usuarios seleccionar las funciones de Helidon que desean agregar al proyecto.

A continuación, explorará el editor de códigos de OCI y abrirá la aplicación Helidon en él. También aprenderá a utilizar GraalVM Enterprise Edition en Oracle Cloud Infrastructure(OCI) Cloud Shell.

Creará una imagen nativa GraalVM para una aplicación MP de Helidon mediante maven. A continuación, modificará la aplicación agregando un punto final personalizado en la clase Java existente. Posteriormente, creará una imagen de docker nativa de esta aplicación y la transferirá a Oracle Cloud Container Image Registry. También creará una instancia informática, extraerá la imagen de docker aquí y ejecutará el contenedor de docker para esta aplicación.

Este taller está diseñado para ser lo más explicativo posible, pero siéntase libre de pedir aclaraciones o asistencia en el camino.

Tiempo estimado: 90 minutos

### Objetivos

*   Generar una aplicación Helidon MP
*   Explora OCI Code Editor
*   Crear una imagen nativa para la aplicación Helidon MP
*   Crear imagen de docker nativa GraalVM para la aplicación Helidon
*   Creación de una Máquina Virtual
*   Ejecutar contenedor de docker para la aplicación Helidon MP

### Requisitos

En este laboratorio se asume que tiene:

*   Cuenta de Oracle Cloud

## Acerca de Helidon

Helidon es un marco de microservicios de código abierto introducido por Oracle que proporciona una recopilación de bibliotecas Java diseñadas para crear aplicaciones ligeras y rápidas basadas en microservicios. El marco admite dos modelos de programación para escribir microservicios: Helidon SE y Helidon MP.

Mientras que Helidon SE está diseñado para ser un micromarco que admite el modelo de programación reactiva, Helidon MP es una implementación de la especificación MicroProfile. Dado que MicroProfile tiene sus raíces en Java EE, las API de MicroProfile siguen un enfoque declarativo familiar con un uso intensivo de las anotaciones. Esto lo convierte en una buena opción para los desarrolladores de Java EE.

Las funciones MicroProfile tienen como objetivo la implantación de microservicios. Puede encontrar API para definir clientes REST, supervisar la aplicación, leer estadísticas técnicas y funcionales y configurar la aplicación. Helidon también ha agregado API adicionales al conjunto principal de API de Microprofile, lo que le brinda todas las capacidades que necesita para escribir aplicaciones modernas nativas de la nube.

> El estándar [MicroProfile](https://microprofile.io/) se basa en Jakarta EE. Al igual que Jakarta EE, MicroProfile es de código abierto y está desarrollado por la Fundación Eclipse. La implementación con MicroProfile se lleva a cabo en las bibliotecas o servidores de aplicaciones que implementan el estándar, al igual que Jakarta EE.

## Más información

*   [https://helidon.io](https://helidon.io)

## Reconocimientos

*   **Autor**: Dmitry Aleksandrov
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, abril de 2023