# Tablas en LiveLabs

## ¡Las mesas están frescas!

Puede definir una tabla en Markdown de la misma manera:

    | Tables        | Are           | Cool  |
    | ------------- |:-------------:| -----:|
    | **col 3 is**  | right-aligned | $1600 |
    | col 2 is      | *centered*    |   $12 |
    | zebra stripes | ~~are neat~~  |    $1 |
    

El resultado es similar al siguiente:

| Tablas | Soy | Genial |
| --- | :-: | --: |
| **col 3 es** | alineado a la derecha | $1600 |
| col 2 es | _centrado_ | $12 |
| rayas cebra | están ordenados | $1 |

Puede ver que hay una leyenda de tabla por defecto proporcionada que es por defecto una concatenación del título del taller y del título de la práctica.

Si no le gusta el valor por defecto, también puede proporcionar su propio título de tabla agregando la siguiente definición de tabla:

    {: title="My table title"}
    

La rebaja completa se ve así:

    | Tables        | Are           | Cool  |
    | ------------- |:-------------:| -----:|
    | **col 3 is**  | right-aligned | $1600 |
    | col 2 is      | *centered*    |   $12 |
    | zebra stripes | ~~are neat~~  |    $1 |
    {: title="My table title"}
    

Ahora nuestra tabla se ve así:

| Tablas | Soy | Genial |
| --- | :-: | --: |
| **col 3 es** | alineado a la derecha | $1600 |
| col 2 es | _centrado_ | $12 |
| rayas cebra | están ordenados | $1 |
| {: title="Título de mi tabla"} |  |  |

Como puede ver, la numeración se agrega automáticamente.

¿No es eso genial?

También puede consultar la [LiveLabs Markdown Cheatsheet](https://objectstorage.us-ashburn-1.oraclecloud.com/p/MKKRgodQ0WIIgL_R3QCgCRWCg30g22bXgxCdMk3YeKClB1238ZJXdau_Jsri0nzP/n/c4u04/b/qa-form/o/LiveLabs_MD_Cheat_Sheet.pdf).