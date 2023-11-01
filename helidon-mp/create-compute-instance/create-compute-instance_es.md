# Ejecutar imagen de docker nativo de la aplicación Helidon en instancia informática

## Introducción

Este laboratorio muestra el proceso de extracción de una imagen de docker de Oracle Container Image Registry y su ejecución en una máquina virtual dentro de Oracle Cloud Infrastructure.

Tiempo estimado: 15 minutos

Vea el siguiente vídeo para una breve introducción al laboratorio. [Ejecución de una imagen de docker nativa de la aplicación Helidon en una instancia informática](videohub:1_dsfd22u5)

### Objetivos

En esta práctica de prácticas, aprenderá a:

*   Crear una instancia informática
*   Instalar Docker en la instancia informática
*   Ejecute el contenedor de docker de imágenes nativo de la aplicación en la instancia informática.

### Requisitos

Para ejecutar este laboratorio, debe tener:

*   Cuenta de Oracle Cloud
*   Que tiene un recurso para crear una instancia informática
*   La aplicación Helidon _myproject-your\_first\_name_ de contenedor empaquetada está disponible en un registro de contenedor.

## Tarea 1: Crear una instancia informática

1.  En la consola de Cloud, haga clic en _Recursos informáticos_ -> _Instancias_. ![instancias informáticas](images/compute-instance.png)
    
2.  Seleccione el compartimento correcto y, a continuación, haga clic en _Crear instancia_. ![crear instancia](images/create-instance.png)
    
3.  Select the following values and click _Create_.  
    **Name:** Leave default.  
    **Create in compatment** Select your own compartment. **Image:** Select _Oracle Linux 8_ image.  
    **Shape:** Select the shape **VM.Standard.E4.Flex** and then select 1 OCPU with 16GB memory.  
    **Primary network:** select _Create new virtual cloud network_ and leave default values.  
    **Subnet:** select _Create new public subnet_ and leave default values.  
    **Public IP address:** select _Assign a public IPv4 address_.  
    **Add SSH keys:** select _Generate a key pair for me_ and click _Save private key_ and _Save public key_ to save the key pair in your local machine. you will need to copy the private key to Code Editor later.
    
4.  Una vez creada la instancia, haga clic en _Copiar_ para copiar la IP pública de la instancia. ![copiar ip](images/copy-ip.png)
    

## Tarea 2: Instalación de docker en la instancia informática

1.  En el terminal del editor de códigos, ejecute el siguiente comando para crear una clave privada.
    
        <copy>vi ~/opc.key</copy>
        
2.  Pulse _i_ para entrar en modo de inserción y pegar el contenido de la clave privada, que descargó en la tarea 1 de este laboratorio. Pulse la tecla _escape_ y, a continuación, introduzca _:wq_ para guardar el contenido del archivo.
    
3.  Cambie el permiso del archivo ejecutando el siguiente comando en el terminal.
    
        <copy>chmod 600 ~/opc.key</copy>
        
4.  Ejecute el siguiente comando con su propia _IP PUBLIC_ para conectarse a la instancia informática que acaba de crear.
    
        <copy>ssh -i ~/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        
    
    ![opc de ssh](images/ssh-opc.png)
    
    > **En la cuenta de nivel gratuito, no tenemos la carpeta _.ssh_ por defecto, por lo que tendrá una salida diferente a la captura de pantalla.**
    
5.  Ejecute el siguiente comando para instalar docker como usuario raíz en la instancia informática y proporcionar al usuario _opc_ capacidad para ejecutar docker.
    
        <copy>sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
        sudo dnf remove -y runc
        sudo dnf install -y docker-ce --nobest
        sudo systemctl enable docker.service
        sudo systemctl start docker.service
        sudo usermod -aG docker opc
        sudo reboot</copy>
        
6.  Espere entre 1 y 2 minutos hasta que la máquina se reinicie y se vuelva a conectar a la instancia informática ejecutando el siguiente comando:
    
        <copy>ssh -i ~/.ssh/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        

## Tarea 3: Extracción y ejecución de la aplicación Saludo en la instancia informática

1.  Ejecute el siguiente comando para extraer la imagen de docker de Oracle Container Image Registry.
    
        <copy>docker pull ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    tendrá una salida similar a la que se muestra a continuación. ![extraer imagen](images/docker-pull.png)
    
2.  Ejecute el siguiente comando para ejecutar la aplicación:
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
3.  Puede abrir un nuevo terminal y ssh para calcular una instancia y ejecutar los siguientes comandos para ejercer la aplicación.
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        
    
    Enhorabuena por haber completado el despliegue de la aplicación Helidon en la instancia informática de Oracle Cloud Infrastructure.
    

## Reconocimientos

*   **Autor**: Dmitry Aleksandrov
*   **Contribuyentes**: Ankit Pandey, Maciej Gruszka
*   **Última actualización por/Fecha**: Ankit Pandey, abril de 2023