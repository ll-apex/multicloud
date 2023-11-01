# Introducción

## Acerca de este taller

En este laboratorio se muestra cómo instalar la plataforma Verrazzano en un único cluster de Kubernetes y cómo desplegar una aplicación de ejemplo con conceptos de Verrazzano.

La aplicación de ejemplo [Bobby's Books](https://verrazzano.io/docs/samples/bobs-books/) consta de tres partes principales:

*   Una aplicación backend de "procesamiento de pedidos", que es una aplicación Java EE con servicios REST y una interfaz de usuario JSP muy sencilla, que almacena datos en una base de datos MySQL. Esta aplicación se ejecuta en el servidor WebLogic.
*   Una tienda web de front-end "Robert's Books", que es un vendedor general de libros. Esto se implanta como microservicio de Helidon, que obtiene datos de libro de Coherence, utiliza un almacén de caché de Coherence para mantener los datos del gestor de pedidos y tiene una interfaz de usuario web de React.
*   Una tienda de front-end "Bobby's Books", que es una tienda de libros para niños especializada. Esto se implanta como un microservicio de Helidon, que obtiene datos de libro de un almacén de caché de Coherence (diferente), interactúa directamente con el gestor de pedidos y tiene una interfaz de usuario web de JSF que se ejecuta en el servidor WebLogic.

### Acerca de Producto/Tecnología

Verrazzano es una plataforma integral de contenedores empresariales para implementar aplicaciones tradicionales y nativas de la nube en entornos híbridos y multinube. Está compuesto por un conjunto curado de componentes de código abierto, muchos de los cuales ya puedes usar y confiar, y algunos que fueron escritos específicamente para reunir todas las piezas que hacen de Verrazzano una plataforma cohesiva y fácil de usar.

Verrazzano incluye las siguientes capacidades:

*   Gestión de cargas de trabajo híbridas y de varios clusters
*   Manejo especial para aplicaciones WebLogic, Coherence y Helidon
*   Gestión de infraestructura de varios clusters
*   Supervisión de aplicaciones integrada y preconectada
*   Seguridad integrada
*   Activación DevOps y GitOps

![Verrazzano](images/verrazzano.png)

### Objetivos

1\. Configure una instancia de Oracle Kubernetes Engine en Oracle Cloud Infrastructure. 1\. Configure \`kubectl\` para interactuar con la instancia de Kubernetes Engine de Oracle en Oracle Cloud Infrastructure. 2\. Instala y conoce la plataforma Verrazzano. 3. Despliegue la aplicación de ejemplo \*Bobby's Books\*. 4. Modifique y vuelva a desplegar el componente de aplicación \*Stock\* basado en Helidon.

## Más información

*   [Verrazzano](https://verrazzano.io/)

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023