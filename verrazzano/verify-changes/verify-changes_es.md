# Verificación de los cambios mediante el acceso a la aplicación y la pila depurada

## Introducción

En el laboratorio 7, aplicamos cambios en bobbys-helidon-stock-application y el pod correspondiente tiene el estado _Running_. En este laboratorio, verificaremos los cambios en la aplicación, la consola de Verrazzano y la consola de Grafana.

Tiempo estimado: 05 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Verifique los cambios en la aplicación Bobby's Books.
*   Verifique los cambios en la consola de Grafana.

### Requisitos

*   Debe tener un editor de texto, donde puede pegar los comandos y las URL y modificarlas, según su entorno. A continuación, puede copiar y pegar los comandos modificados para ejecutarlos en _Cloud Shell_.

## Tarea 1: Verificar los cambios en la aplicación Libros de Bobby

1.  Abra la ficha Libros de Bobby y seleccione Actualizar. Si ha cerrado ese separador, copie y pegue el siguiente comando en el editor de texto y sustituya XX.XX.XX.XX por EXTERNAL\_IP para la aplicación. Observará que el nombre del libro está en mayúsculas.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![El libro de Bobby](images/bobbysbooks.png " ")
    

## Tarea 2: Verificación de los cambios en la consola de Grafana

1.  Vuelva a la página de inicio de Verrazzano.
    
    ![Inicio de Verrazzano](images/verrazzao-home.png " ")
    
2.  Seleccione el enlace para que Grafana abra la _consola de Grafana_.
    
    ![Enlace de Grafana](images/grafana-link.png " ")
    
3.  Seleccione _Inicio_, escriba _Helidon_ y, a continuación, seleccione _Panel de control de Helidon_.
    
    ![Buscar en Helidon](images/search-helidon.png " ")
    
4.  En ServiceID, seleccione _bobs-books\_default\_bobs-books\_bobby-helidon_ y, en la instancia, seleccione la instancia recién creada. En este caso, obtendrá información para la _bobby-helidon-stock-application_ modificada.
    
    ![Nuevo componente](images/new-component.png " ")
    
    ¡Felicitaciones! Ha completado correctamente las prácticas.
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023