# Taller con un solo juego de prácticas

## Instrucciones: elimine este archivo cuando haya terminado

1.  Abra la plantilla de taller de muestra en Atom o Visual Studio Code
2.  Hemos creado previamente 5 carpetas. Un taller se crea a partir de varias prácticas.
3.  Elimine los comentarios como este: _Mostrar objetivos para este laboratorio_
4.  Asegúrese de utilizar la carpeta en minúscula, el nombre de archivo y los guiones para los espacios (setup-adb NOT Setup\_ADB)
5.  Los nombres de imagen deben tener nombres descriptivos. No solo adb1, adb2, adb3. Para la accesibilidad de minusvalías, necesitamos las descripciones de la imagen para explicar cómo se ve la imagen. Recuerde todas las minúsculas y guiones.
6.  Descargue nuestro documento de control de calidad de WMS. Encontramos que los talleres entran en producción más rápido cuando sabes lo que se necesita para pasar a la producción por adelantado y usas el esqueleto.

PD: No necesitas un Readme.md. Los archivos Léame sólo existen en los niveles superiores de la biblioteca. Dirigimos todo el tráfico a LiveLabs, ya que no podemos realizar un seguimiento del uso en GitHub. No cree ningún enlace directo a GitHub, su taller puede ser muy popular, pero no podemos rastrearlo, por lo que nadie lo sabrá.

## Ruta de acceso absoluta para el menú Navegación de Oracle Cloud

**Laboratorio 1: Aprovisionamiento de una instancia -> Paso 0: Uso de estas imágenes estandarizadas para la navegación de Oracle Cloud (normalmente para el aprovisionamiento)**: hemos incluido una lista de capturas de pantalla comunes para navegar por el menú de Oracle Cloud. Lea esta sección y use las imágenes de ruta absoluta relevantes cuando corresponda. Esto probará su taller en el futuro en caso de actualizaciones de la interfaz de usuario de Oracle Cloud.

## Estructura de Carpeta

En este ejemplo, el objetivo es crear varios talleres para "niños" a partir de un taller más largo para "padres". Los niños están formados por partes del padre.

muestra-taller/ -- laboratorios individuales

        provision/
        setup/
        dataload/
        query/
        introduction/
          introduction.md       -- description of the everything workshop, note that it is a "lab" since there is only one
    
    workshops/
       freetier/                -- freetier version of the workshop
        index.html
        manifest.json
       livelabs/                -- livelabs version of the workshop
        index.html
        manifest.json
    

### FreeTier frente a LiveLabs

*   "FreeTier": incluye pruebas gratuitas, cuentas de pago y, para algunos talleres, cuentas siempre gratis (botón marrón)
*   "LiveLabs": talleres que utilizan arrendamientos proporcionados por Oracle (botón verde)
*   "Escritorio": este es un nuevo despliegue en el que el taller está encapsulado en un entorno NoVNC que se ejecuta en una instancia informática

### Acerca del taller

El taller incluye los 6 laboratorios individuales en una sola secuencia.

La estructura de carpetas incluye un "laboratorio" de introducción que describe el taller como un juego completo de 6 prácticas. Nota: es posible que no necesite tener una introducción diferente para cada una de las versiones padre e hijo de los talleres, esto es solo ilustrativo.

Consulte la carpeta product-name-workshop/freetier y el archivo manifest.json para ver la estructura.

> **Nota:** El uso de "Laboratorio n:" en los títulos es opcional

El "laboratorio" de requisitos previos es el primer laboratorio de una carpeta común en oracle-livelabs/common repo. Puesto que este laboratorio ya existe, podemos utilizar una URL RAW/absoluta en su lugar:

    "filename": "https://oracle-livelabs.github.io/common/labs/cloud-login/cloud-login-livelabs2.md"        },
    

El archivo manifest.json debe conocer la ubicación de cada laboratorio en relación con su ubicación en la jerarquía. En esta estructura, los laboratorios se encuentran en dos niveles superiores, por ejemplo:

    "filename": "../../provision/provision.md"
    

### Por ejemplo:

Este [taller de APEX](https://oracle-livelabs.github.io/apex/spreadsheet/workshops/freetier/) es un buen ejemplo de un taller con un solo juego de laboratorios: [https://github.com/oracle-livelabs/apex/tree/main/spreadsheet](https://github.com/oracle-livelabs/apex/tree/main/spreadsheet).