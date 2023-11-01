# Modificar la aplicación Helidon MP en Code Editor

## Introducción

En esta práctica, agregará un punto final personalizado a la clase JAVA en Code Editor.

Tiempo estimado: 10 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Modificar la aplicación Helidon MP en el editor de códigos](videohub:1_sv1iug41)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Agregar un punto final personalizado en la clase Java
*   Crear y ejecutar la aplicación modificada

### Requisitos

*   Debe tener una cuenta activada de [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Tarea 1: Agregar un punto final personalizado

1.  En el editor de códigos, haga clic en el archivo _GreetResource.java_ para abrirlo. ![abrir archivo](images/open-file.png)
    
2.  Como puede ver en el código, está totalmente basado en MicroProfile. Esto significa que toda la funcionalidad se logra con POJO y anotaciones. Estas anotaciones son estándar, por lo que se pueden transportar entre diferentes proveedores. Esto significa que puede ejecutar fácilmente código en otras plataformas que soporten la misma versión de MicroProfile. Para obtener más información, la encontrará [aquí](https://microprofile.io/).
    
3.  Cree un nuevo punto final que proporcione títulos aleatoriamente de un grupo de cinco títulos. Para crear este punto final, copie y pegue el siguiente código en la línea número 80, como se muestra a continuación.
    
        <copy>@Path("/title")
        @GET
        @Produces(MediaType.APPLICATION_JSON)
        public String getRandomTitle() {
            return new String[] {"Mr. ", "Mrs.", "Miss", "Dr.", "Dr. sc."} [(int)(Math.random()*5)];
        }</copy>
        
    
    ![agregar código](images/add-code.png)
    
4.  Para guardar el contenido del archivo, en el editor de códigos, haga clic en _File_ -> _Save_.
    
    > AutoSave está activado por defecto en Code Editor.
    

## Tarea 2: Ejecutar la aplicación

1.  Copie y pegue el siguiente comando en el terminal para ejecutar la aplicación.
    
        <copy>mvn clean package
        java -jar target/myproject.jar</copy>
        
2.  Abra un terminal o una consola nuevos y ejecute los siguientes comandos para comprobar la aplicación:
    
        <copy>curl -X GET http://localhost:8080/greet/title</copy>
        Mr.
        
    
    > Proporciona aleatoriamente el título de 5 opciones, puede ejecutar este comando varias veces.
    
3.  _Para detener la aplicación **myproject**, introduzca `Ctrl + C` en el terminal donde se ejecuta el comando "java -jar target/myproject.jar"_. ES MUY IMPORTANTE, DE OTRA MANERA USTED ENFRENTARÁ CUESTIONES EN LA LAB LATERAL.
    

## Reconocimientos

*   **Autor**: Dmitry Aleksandrov
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, abril de 2023