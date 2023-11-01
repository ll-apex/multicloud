# Preparar configuración

## Introducción

En este laboratorio se mostrará cómo descargar el archivo zip de pila de Oracle Resource Manager (ORM) necesario para configurar el recurso necesario para ejecutar este taller. Este taller requiere una instancia informática y una red virtual en la nube (VCN).

Tiempo estimado: 10 minutos

### Objetivos

*   Descargar pila ORM
*   Configurar una red virtual en la nube (VCN) existente

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta gratuita o de pago en la nube de Oracle

## Tarea 1: Descarga del archivo zip de pila de Oracle Resource Manager (ORM)

1.  Haga clic en el siguiente enlace para descargar el archivo zip del gestor de recursos que necesita para crear su entorno:
    
    _Nota 1:_ Si proporciona una única descarga de pila para el taller, utilice esta expresión simple.
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  Guardar en la carpeta de descargas.
    

Recomendamos utilizar esta pila para crear una VCN autónoma/dedicada con sus instancias. Vaya a la _Tarea 3_ para seguir nuestras recomendaciones. Si prefiere utilizar una VCN existente, continúe con el siguiente paso, como se indica a continuación, para actualizar la VCN existente con las reglas de salida necesarias.

## Tarea 2: Adición de reglas de seguridad a una VCN existente

Este taller necesita que haya un determinado número de puertos disponibles, un requisito que se puede cumplir mediante la ejecución de pila ORM por defecto que crea una VCN dedicada. Para utilizar una VCN existente, se deben agregar los siguientes puertos a las reglas de entrada.

| Puerto | Descripción |
| :-- | :-- |
| 22 | SSH |
| 80 | noVNC Escritorio remoto (proxy GNINX) |
| 6080 | noVNC Escritorio remoto |

1.  Vaya a _Networking >> Virtual Cloud Networks_.
2.  Seleccione la red.
3.  En Recursos, seleccione Listas de seguridad.
4.  Haga clic en Default Security Lists bajo el botón Create Security List
5.  Haga clic en el botón Agregar regla de entrada.
6.  Introduzca lo siguiente:
    *   CIDR de origen: 0.0.0.0/0
    *   Rango de puertos de destino: _consulte la tabla anterior_
7.  Haga clic en el botón Add Ingress Rules.

## Tarea 3: Configuración de Compute

Con los detalles de las dos tareas anteriores, vaya al laboratorio _Configuración del entorno_ para configurar el entorno del taller mediante Oracle Resource Manager (ORM) y una de las siguientes opciones:

*   Crear pila: _Recursos informáticos + redes_
*   Crear pila: _solo recursos informáticos_ con una VCN existente donde las listas de seguridad se han actualizado según la _Tarea 2_ anterior

Ahora puede pasar a la siguiente práctica de laboratorio.

## Reconocimientos

*   **Autor**: Rene Fontcha, responsable de plataforma de LiveLabs, NA Technology
*   **Contribuyentes**: Meghana Banka
*   **Última actualización por/Fecha**: Rene Fontcha, LiveLabs Platform Lead, NA Technology, febrero de 2022