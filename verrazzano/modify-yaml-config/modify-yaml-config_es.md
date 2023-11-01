# Modificación del archivo YAML de configuración de libros de Bobby

## Introducción

En el laboratorio 6, hemos transferido la imagen de Docker actualizada para _bobby-helidon-stock-application_. Ahora, queremos que Verrazzano vuelva a desplegar la aplicación y los componentes actualizados sin que esto afecte a los servicios. Para ello, necesitamos configurar el archivo YAML para que Verrazzano recoja la nueva imagen e inicie un pod para ella. Una vez que el pod tiene el estado _En ejecución_, termina el pod asociado a la aplicación y los componentes anteriores.

Tiempo estimado: 10 minutos

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Modifique el archivo `bobs-books-comp.yaml`.
*   Aplique los cambios mediante `kubectl`.

### Requisitos

Debe tener un editor de texto, donde puede pegar los comandos y las URL y modificarlas, según su entorno. A continuación, puede copiar y pegar los comandos modificados para ejecutarlos en _Cloud Shell_.

## Tarea 1: Modificación del archivo bobs-books-comp.yaml

1.  Tenemos un archivo de configuración de aplicación, _bobs-books-comp.yaml_. En el laboratorio 2, descargamos los archivos yaml de la aplicación. Para cambiar al directorio raíz que contiene el archivo yaml, copie el siguiente comando y péguelo en _Cloud Shell_.
    
        <copy>cd ~</copy>
        
2.  En esta ubicación, tenemos el archivo de configuración para la aplicación bobs-books. Como parte del laboratorio 5, modificamos bobbys-helidon-stock-application y creamos una nueva imagen de Docker para ese componente. En el laboratorio 6, hemos transferido esa imagen de Docker al repositorio de Oracle Cloud Container Registry. Ahora, en este laboratorio, modificaremos el archivo _bobs-books-comp.yaml_ para que tome la nueva imagen de Docker actualizada del repositorio de Oracle Cloud Container Registry. Para modificar el archivo _bobs-books-comp.yaml_, copie el siguiente comando y péguelo en _Cloud Shell_.
    
        <copy>vi bobs-books-comp.yaml</copy>
        
    
    ![Abrir Archivo](images/openfile.png " ")
    
3.  Como parte del laboratorio 5, ha guardado el nombre completo de la imagen de Docker. Debe copiar la siguiente línea y pegarla en el editor de texto. A continuación, debe sustituir `docker image full name` por el nombre de la imagen de Docker. A continuación, copie la línea modificada y pulse _i_ para insertar el texto en el archivo `*bobs-books-comp.yaml*`. Pegue la salida en el número de línea 148 (asegúrese de mantener la sangría) y comente el número de línea 147 que sale con _#_, como se muestra en la siguiente imagen, pulse _Esc_ y, a continuación, escriba _:wq_ para guardar el archivo.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![Insertar Línea](images/insert-line.png " ")
    

## Tarea 2: Aplicación de los cambios mediante `kubectl`

1.  Para aplicar los cambios, copie y pegue el siguiente comando en _Cloud Shell_. Cuando aplique el cambio, se inicializará un nuevo pod para servir solicitudes para el nuevo componente, mientras que el pod asociado al componente antiguo seguirá sirviendo solicitudes. Más tarde, después de que el nuevo pod alcance el estado _En ejecución_, el pod antiguo comenzará a ser _Finalizado_. Finalmente, solo el nuevo pod tendrá el estado _En ejecución_.
    
        <copy>kubectl apply -f bobs-books-comp.yaml -n bobs-books</copy>
        
    
    La salida debe ser similar a lo siguiente:
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh unchanged
        component.core.oam.dev/robert-helidon unchanged
        component.core.oam.dev/bobby-coh unchanged
        component.core.oam.dev/bobby-helidon configured
        component.core.oam.dev/bobby-wls unchanged
        component.core.oam.dev/bobs-mysql-configmap unchanged
        component.core.oam.dev/bobs-mysql-service unchanged
        component.core.oam.dev/bobs-mysql-deployment unchanged
        component.core.oam.dev/bobs-orders-configmap unchanged
        component.core.oam.dev/bobs-orders-wls unchanged
        $
        
    
    Puede observar en la salida; sólo se configura _component.core.oam.dev/bobby-helidon_ y otros componentes no se modifican.
    
2.  Para ver cómo se inicializa el nuevo pod y cómo pasa al estado _Terminating_, copie y pegue el siguiente comando en _Cloud Shell_.
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    Aparecerá una pantalla similar a la siguiente:
    
        $ kubectl get pods -n bobs-books
        NAME                                         READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                           2/2    Running      0         130m
        bobbys-front-end-adminserver                 4/4    Running      0         127m
        bobbys-front-end-managed-server1             4/4    Running      0         126m
        bobbys-helidon-stock-application-64fb55-zzp  0/2    PodInitializing  0     10s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Running      0         130m
        bobs-bookstore-adminserver                   4/4    Running      0         127m
        bobs-bookstore-managed-server1               4/4    Running      0         126m
        mysql-65d864bf8c-xf64p                       2/2    Running      0         130m
        robert-helidon-bfdfb58b8-58qfs               2/2    Running      0         130m
        robert-helidon-bfdfb58b8-lkw8m               2/2    Running      0         130m
        roberts-coherence-0                          2/2    Running      0         130m
        roberts-coherence-1                          2/2    Running      0         130m
        bobbys-helidon-stock-application-64fb55-zzp  1/2    Running      0         28s
        bobbys-helidon-stock-application-64fb55-zzp  2/2    Running      0         34s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        $
        
    
    Después de ver que todos los pods están en el estado _Running_ (En ejecución), pulse _CTRL + C_ para terminar este comando.
    
    Deje _Cloud Shell_ abierto, ya que también lo necesitamos para nuestro último laboratorio.
    

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023