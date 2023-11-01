# VERSIÓN ALTERNADA con solución simplificada

## Introducción

En este laboratorio, utilizaremos la imagen bobbys-helidon-stock-application modificada. Esta imagen se almacena en nuestro repositorio público dentro de Oracle Cloud Container Registry. También utilizamos el archivo bobs-books-comp-mod.yaml modificado, que apunta a la imagen bobbys-helidon-stock-application modificada.

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Descargue el archivo bobs-books-comp-mod.yaml modificado.
*   Aplicación de los cambios mediante kubectl

### Requisitos

*   Ejecute el laboratorio 1, que crea un cluster de OKE en Oracle Cloud Infrastructure.
*   Ejecute el laboratorio 2, que instala Verrazzano en el cluster de Kubernetes.
*   Ejecute el laboratorio 3, que despliega la aplicación Bobs-Books.
*   Debe tener un editor de texto, donde puede pegar los comandos y las URL y modificarlas, según su entorno. A continuación, puede copiar y pegar los comandos modificados para ejecutarlos en _Cloud Shell_.

## Tarea 1:Descargue el archivo bobs-books-comp-mod.yaml modificado

1.  Ejecute el siguiente comando para descargar el archivo bobs-books-comp-mod.yaml modificado.
    
        <copy>curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/alternate-version/bobs-books-comp-mod.yaml >~/bobs-books-comp-mod.yaml</copy>
        
    
    ![Descargar archivo](images/downloadfile.png " ")
    

## Tarea 2: Aplicación de los cambios con kubectl

1.  Para aplicar los cambios, copie y pegue el siguiente comando en _Cloud Shell_. Cuando aplique el cambio, se inicializará un nuevo pod para servir solicitudes para el nuevo componente, mientras que el pod asociado al componente antiguo seguirá sirviendo solicitudes. Más tarde, después de que el nuevo pod alcance el estado _En ejecución_, el pod antiguo comenzará a ser _Finalizado_. Finalmente, solo el nuevo pod tendrá el estado _En ejecución_.
    
        <copy>kubectl apply -f ~/bobs-books-comp-mod.yaml -n bobs-books</copy>
        
    
    ![Aplicar cambios](images/applychanges.png " ")
    
    Puede observar en la salida; sólo se configura _component.core.oam.dev/bobby-helidon_ y otros componentes no se modifican.
    
2.  Para ver cómo se inicializa el nuevo pod y cómo pasa al estado _Terminating_, copie y pegue el siguiente comando en _Cloud Shell_.
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    Aparecerá una pantalla similar a la siguiente:
    
        vera_zano@cloudshell:~ (us-ashburn-1)$ kubectl get pods -n bobs-books
        NAME                                               READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                                 2/2    Running  0         130m
        bobbys-front-end-adminserver                       4/4    Running  0         127m
        bobbys-front-end-managed-server1                   4/4    Running  0         126m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  0/2    PodInitializing  0         10s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Running  0         130m
        bobs-bookstore-adminserver                         4/4    Running  0         127m
        bobs-bookstore-managed-server1                     4/4    Running  0         126m
        mysql-65d864bf8c-xf64p                             2/2    Running  0         130m
        robert-helidon-bfdfb58b8-58qfs                     2/2    Running  0         130m
        robert-helidon-bfdfb58b8-lkw8m                     2/2    Running  0         130m
        roberts-coherence-0                                2/2    Running  0         130m
        roberts-coherence-1                                2/2    Running  0         130m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  1/2    Running  0         28s
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  2/2    Running  0         34s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        vera_zano@cloudshell:~ (us-ashburn-1)$
        
    
    Después de ver que todos los pods están en el estado _Running_ (En ejecución), pulse _CTRL + C_ para terminar este comando.
    

Deje _Cloud Shell_ abierto, ya que también lo necesitamos para nuestro último laboratorio.

## Reconocimientos

*   **Autor**: Ankit Pandey
*   **Contribuyentes**: Maciej Gruszka, Sid Joshi
*   **Última actualización por/Fecha**: Ankit Pandey, febrero de 2023