# Modificación de la aplicación Helidon para agregar soporte de Object Storage

## Introducción

El objetivo de este laboratorio es demostrar **cómo agregar acceso a Object Storage** desde la aplicación Helidon. Para ello, se sustituye una variable que se utiliza para almacenar la palabra de saludo por un objeto que ahora se convertirá en el nuevo contenedor de palabras de saludo y se almacena y **se recupera de un cubo de Object Storage**. Dado que el objeto se mantiene, el último valor de palabra de saludo sobrevivirá a los reinicios de la aplicación. Sin este cambio y con la palabra de saludo en la memoria a través de la variable, la palabra de saludo se restablecerá a un valor por defecto cuando se reinicie la aplicación.

Tiempo estimado: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Soporte de Object Storage](videohub:1_p5v2wehm)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Modificar la aplicación Helidon para mostrar su integración con servicios de OCI como Object Storage
*   Verificar la integración correcta de Object Storage

### Requisitos

*   Una cuenta en la nube de Oracle Free Tier (prueba), de pago o LiveLabs

## Tarea 1: Modificación de la aplicación Helidon para la integración de Object Storage

1.  Verifique que **oci.bucket.name** esté configurado correctamente en **~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties**, que ya debería haberse establecido en el paso 5 del laboratorio 2/tarea 3.
    
2.  En **Editor de códigos**, haga clic en el nombre de archivo **`pom.xml`** en _~/oci-mp/server/_ para abrirlo y agregar la dependencia del **SDK de OCI de Object Storage** dentro de la cláusula **dependencies**, como se muestra a continuación.
    
        <copy><dependency>
                    <groupId>com.oracle.oci.sdk</groupId>
                    <artifactId>oci-java-sdk-objectstorage</artifactId>
        </dependency></copy>
        
    
    ![agregar dependencia](images/add-dependency.png)
    
    > Asegúrese de mantener la sangría adecuada.
    
3.  Modificamos el archivo _~/oci-mp/server/src/main/java/ocw/hol/mp/oci/server/GreetingProvider.java_ para agregar acceso a **Object Storage**. En interés del tiempo, tenemos el código fuente para este cambio. Copie y pegue el siguiente código para actualizar este archivo con los cambios necesarios. Puede leer la _Nota_ a continuación, que describe los cambios que realizamos en este archivo.
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/source/GreetingProvider.java server/src/main/java/ocw/hol/mp/oci/server/</copy>
        
    
    ![almacenamiento de objetos agregado](images/os-added.png)
    
    > **Lea:-**
    
    *   En la sección de argumentos del constructor, agregue el parámetro _ObjectStorage objectStorageClient_. Como esto forma parte de la anotación _@Injected_, Helidon procesará y definirá automáticamente el parámetro para que contenga el cliente que se puede utilizar para comunicarse con el servicio Object Storage sin tener que agregar varias líneas de código **SDK de OCI** para ese fin.
    *   Desde la misma sección de argumentos del constructor, se agregó **ConfigProperty**, que extraerá el valor de una propiedad **oci.bucket.name** en la configuración. Esto se completó anteriormente en **microprofile-config.properties** durante la configuración inicial de la aplicación cuando se ejecutó una secuencia de comandos de utilidad denominada **`update_config_values.sh`** desde el directorio del repositorio **`devops_helidon_to_instance_ocw_hol`**.
    *   Mediante el método **getNamespace() del SDK de Object Storage**, recupere el espacio de nombres de Object Storage, ya que se utilizará más adelante para recuperar o almacenar un objeto:
    
        <copy>public GreetingProvider(@ConfigProperty(name = "app.greeting") String message,
                            ObjectStorage objectStorageClient,
                            @ConfigProperty(name = "oci.bucket.name") String bucketName) {
        try {
            this.bucketName = bucketName;
            GetNamespaceResponse namespaceResponse =
                    objectStorageClient.getNamespace(GetNamespaceRequest.builder().build());
            this.objectStorageClient = objectStorageClient;
            this.namespaceName = namespaceResponse.getValue();
            LOGGER.info("Object storage namespace: " + namespaceName);
            setMessage(message);
        } catch (Exception e) {
            LOGGER.warning("Error invoking getNamespace from Object Storage: " + e);
        }
        }     </copy>
        
    
    *   Justo debajo de la clase GreetingProvider, se ha eliminado la declaración de variable:
    
        <copy>private final AtomicReference<String> message = new AtomicReference<>();</copy>
        
    
    *   Se ha sustituido por variables globales locales como las siguientes. _LOGGER_ se utilizará para el registro, mientras que _objectStorageClient_, _namespaceName_, _bucketName_ y _objectName_ se utilizarán en las llamadas al _SDK de almacenamiento de objetos_.
    
        <copy>private static final Logger LOGGER = Logger.getLogger(GreetingProvider.class.getName());
        private ObjectStorage objectStorageClient;
        private String namespaceName;
        private String bucketName;
        private final String objectName = "hello.txt";</copy>
        
    
    *   Se ha sustituido el contenido de _getMessage()_ por el siguiente código. Se utilizará el método **getObject() SDK** para recuperar la palabra de saludo del objeto **hello.txt** recuperado del cubo.
    
        <copy>try {
        GetObjectResponse getResponse =
                objectStorageClient.getObject(
                        GetObjectRequest.builder()
                                .namespaceName(namespaceName)
                                .bucketName(bucketName)
                                .objectName(objectName)
                                .build());
        return new String(getResponse.getInputStream().readAllBytes());
        } catch (Exception e) {
            LOGGER.warning("Error invoking getObject from Object Storage: " + e);
            return "Hello-Error";
        }</copy>
        
    
    *   Se ha sustituido el contenido del método **setMessage(String message)** vacío por el siguiente código. Esto utilizará el método **putObject() SDK** para almacenar el objeto **hello.txt** que contiene la palabra de saludo en el cubo.
    
        <copy>try {
        byte[] contents = message.getBytes();
        PutObjectRequest putObjectRequest =
                PutObjectRequest.builder()
                        .namespaceName(namespaceName)
                        .bucketName(bucketName)
                        .objectName(objectName)
                        .putObjectBody(new ByteArrayInputStream(message.getBytes()))
                        .contentLength(Long.valueOf(contents.length))
                        .build();
        objectStorageClient.putObject(putObjectRequest);
        } catch (Exception e) {
            LOGGER.warning("Error invoking putObject from Object Storage: " + e);
        }</copy>
        
    
    *   Se ha eliminado la importación (**importar java.util.concurrent.atomic.AtomicReference;**) y se ha sustituido por estas nuevas importaciones para soportar todo el nuevo código que se ha agregado.
    
        <copy>import java.io.ByteArrayInputStream;
        import java.util.logging.Logger;
        
        import com.oracle.bmc.objectstorage.ObjectStorage;
        import com.oracle.bmc.objectstorage.requests.GetNamespaceRequest;
        import com.oracle.bmc.objectstorage.requests.GetObjectRequest;
        import com.oracle.bmc.objectstorage.requests.PutObjectRequest;
        import com.oracle.bmc.objectstorage.responses.GetNamespaceResponse;
        import com.oracle.bmc.objectstorage.responses.GetObjectResponse;</copy>
        

## Tarea 2: Transferencia del cambio de código de aplicación de Helidon y disparo del pipeline DevOps

1.  Copie y pegue el siguiente comando en el terminal **para confirmar y aplicar el cambio**.
    
        <copy>git add .
        git status
        git commit -m "Add support Object Storage integration"
        git push</copy>
        
    
    > El pipeline se activará mediante esta transferencia de Git.
    
2.  Espere hasta que el **ciclo de vida DevOps** se complete supervisando los logs de pipeline de compilación y despliegue. ![ejecución de despliegue](images/deployment-run.png)
    

## Tarea 3: Verificación de la integración correcta de Object Storage

Pruebe mediante curl y compruebe que se haya agregado un nuevo objeto **hello.txt** al **bloque**. Compruebe que el tamaño del objeto es el mismo que el tamaño de la palabra de saludo. Por ejemplo, si la palabra de saludo es _Hola_, el tamaño debe ser **5**. Si la palabra de saludo es _Hola_, el tamaño debe ser **4**.

1.  Configure el nodo de despliegue **`PUBLIC_IP`** como variable de entorno.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Copie y pegue el siguiente comando para colocar una solicitud **GET**. Tendrá una salida similar a la que se muestra a continuación.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  En **Consola en la nube**, haga clic en _Menú de hamburguesa_ -> _Almacenamiento_ -> _Cubos_, como se muestra. ![menú de cubo](images/bucket-menu.png)
    
4.  Seleccione el compartimento correcto y haga clic en **app-bucket-helidonocw-hol-string** para abrir el **bucket**. ![seleccionar compartimento](images/select-compartment.png)
    
5.  Compruebe que el cubo ahora contiene un objeto **hello.txt** y tiene un tamaño de **5 bytes** porque la palabra de saludo es **Hola**. También puede descargar el objeto y verificar que el contenido sea _Hola_. ![verificar tamaño](images/verify-size.png)
    
    > Deje esta página abierta, ya que actualizaremos esta página, una vez que cambiemos la palabra de saludo en el siguiente paso.
    
6.  Copie y pegue el siguiente comando para sustituir la palabra de saludo **Hola** por **Hola**.
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
7.  Compruebe que el objeto **hello.txt** del cubo tenga ahora un tamaño de **4 bytes** porque la palabra de saludo se sustituye por **Hola**. También puede descargar el objeto y verificar que el contenido se ha cambiado a **Hola**. ![tamaño hola](images/hola-size.png)
    
8.  Reinicie la aplicación mediante la herramienta **restart.sh** para demostrar que el valor de la palabra de saludo sobrevivirá a medida que se mantenga en **Object Storage**.
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/restart.sh</copy>
        Created private.key and can be used to ssh to the deployment instance by running this command: "ssh -i private.key opc@xx.xx.xx.xx"
        FIPS mode initialized
        The authenticity of host 'xx.xx.xx.xx (xx.xx.xx.xx)' can't be established.
        ECDSA key fingerprint is SHA256:hJl8axCNhFcILDo+AwxMkodxhY+UxRD40d1ans83GTg.
        ECDSA key fingerprint is SHA1:IBUhyn05DaIs60GAQsruVXajhym.
        Are you sure you want to continue connecting (yes/no)? yes
        Warning: Permanently added 'xx.xx.xx.xx' (ECDSA) to the list of known hosts.
        Starting oci-mp-server.jar
        Helidon app is now running with pid 264792!
        Cleaning up ssh private.key
        
    
    ![reiniciar aplicación](images/restart-application.png)
    
    > Introduzca _yes_, cuando se le pida **Are que desea continuar con la conexión (yes/no)?**.
    
9.  Llame a la solicitud Hello World predeterminada y **observe que la palabra de saludo sigue siendo Hola**.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
10.  Refresque la página Cubo en la **consola en la nube** y observará que el tamaño sigue siendo **4**, lo que confirma que la palabra de saludo sigue siendo **Hola**. ![verificar persistencia](images/verify-persistence.png)
    

**¡Felicidades!** Ha completado el taller. Si lo desea, puede continuar con el **Laboratorio 6**, que **suprime todos los recursos** creados durante este taller.

## Más información

*   [Integración de Helidon OCI](https://helidon.io/docs/v3/#/mp/integrations/oci)

## Reconocimientos

*   **Autor**: Keith Lustria
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, agosto de 2023