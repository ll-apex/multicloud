# Preparar configuración

## Introducción

En este laboratorio se mostrará cómo descargar el archivo zip de pila de Oracle Resource Manager (ORM) necesario para configurar el recurso necesario para ejecutar este taller. A continuación, crea una instancia informática y una red virtual en la nube (VCN) que le permite acceder a un escritorio remoto.

Tiempo estimado: 10 minutos

### Objetivos

*   Descargar pila ORM
*   Creación de recursos informáticos + redes con la pila del gestor de recursos

### Requisitos

En este laboratorio se asume que tiene:

*   Una cuenta gratuita o de pago en la nube de Oracle

## Tarea 1: Descarga del archivo zip de pila de Oracle Resource Manager (ORM)

1.  Haga clic en el siguiente enlace para descargar el archivo zip del gestor de recursos que necesita para crear su entorno:
    
    _Nota 1:_ Si proporciona una única descarga de pila para el taller, utilice esta expresión simple.
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  Guardar en la carpeta de descargas.
    

## Tarea 2: Crear pila: recursos informáticos + redes

1.  Identifique el archivo zip de pila ORM descargado en la **Tarea 1: descarga del archivo zip de pila de Oracle Resource Manager (ORM)**.
    
2.  Abre el menú de hamburguesa en la esquina superior izquierda. Haga clic en **Servicios para desarrolladores** y seleccione **Gestor de recursos** > **Pilas**. Seleccione el compartimento en el que desea instalar la pila. Haga clic en **Crear pila**. ![pila de menús](images/menu-stack.png) ![seleccionar compartimento](images/select-compartment.png)
    
3.  Seleccione **Mi configuración** y elija **. Botón Zip** del archivo, haga clic en el enlace **Examinar** y seleccione el archivo zip que ha descargado o arrastre y suelte para el explorador de archivos. Haga clic en **Siguiente**. ![examinar zip](images/browse-zip.png)
    
4.  Introduzca o seleccione lo siguiente y haga clic en **Siguiente**.
    
    **Recuento de instancias:** acepte el valor por defecto, 1.
    
    **Seleccionar dominio de disponibilidad**: seleccione un dominio de disponibilidad de la lista desplegable.
    
    **¿Necesita acceso remoto a través de SSH?** Mantener sin marcar solo el acceso al escritorio remoto: el valor predeterminado.
    
    **¿Usar unidad de instancia flexible con recuento de OCPU ajustable?:** mantenga el valor por defecto marcado (a menos que tenga previsto utilizar una unidad fija).
    
    **Unidad de instancia:** mantenga el valor por defecto o seleccione uno de la lista de unidades flexibles en el menú desplegable (por ejemplo, VM.Standard.E4). Flex).
    
    **Seleccionar recuento de OCPU por instancia:** acepte el valor por defecto mostrado. Por ejemplo, (1) aprovisionará 1 OCPU y 16 GB de memoria.
    
    **¿Usar VCN existente?:** acepte el valor por defecto dejando esta opción sin marcar. Esto creará una nueva VCN. ![configuración principal](images/main-config.png) ![unidad de instancia](images/instance-shape.png)
    
5.  Seleccione **Ejecutar Aplicación** y haga clic en **Crear**. ![ejecutar aplicación](images/run-apply.png)
    

Ahora puede pasar a la siguiente práctica de laboratorio.

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, octubre de 2023