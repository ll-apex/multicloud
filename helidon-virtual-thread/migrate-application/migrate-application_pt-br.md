# Crie um aplicativo Helidon 3 e migre-o para o Helidon 4

## Introdução

Neste laboratório, você começará com um aplicativo Helidon 3 em execução em nosso Servidor Web reativo original baseado em Netty. Em seguida, você migrará o aplicativo para o Helidon 4 em execução no novo Helidon Nima WebServer usando threads virtuais.

[Passo a passo de Lab4](videohub:1_zr1m00ba)

Tempo Estimado: 20 minutos

### Objetivos

*   Gere, crie e execute um aplicativo Helidon MicroProfile usando o Helidon Starter.
*   Migrar o aplicativo Helidon 3 Microprofile para o Helidon 4

### Pré-requisitos

*   Conta do Oracle Cloud

## Tarefa 1: Criar um aplicativo Helidon 3 e criar o aplicativo

1.  Copie o URL abaixo e cole-o no browser para abrir a página Projeto Helidon.
    
        <copy>https://helidon.io/starter/</copy>
        
2.  Em Gerar Seu Projeto, selecione _Helidon MP_ como Helidon Flavor e clique em _Próximo_.
    
3.  Para Tipo de Aplicativo, selecione _Início Rápido_ e clique em _Próximo_.
    
4.  Para Suporte de Mídia, selecione _Jackson_ e clique em _Próximo_.
    
5.  Para Personalizar Projeto, selecione os valores padrão e clique em _Downloads_. Isso aparecerá em uma janela e salvará este _myproject.zip_ no local de sua escolha. No restante deste workshop, o nome do _myproject_ será usado. Se você escolher outro nome, altere respectivamente.
    
6.  Volte para o Editor de Código, em HELIDON-LEVELUP-2023-MAIN e clique em _docs_. ![Selecionar documentos](images/select-docs.png)
    
7.  Clique em _Arquivo_ -> _Fazer Upload de Arquivos_ e selecione _myproject.zip_ no local em que você salvou esse arquivo anteriormente e clique em _Abrir_. ![Fazer Upload de Arquivos](images/upload-files.png)
    
8.  Você verá o arquivo _myproject.zip_ na pasta _docs_. ![Arquivo Zip](images/zip-file.png)
    
9.  copie e cole o comando a seguir para descompactar o arquivo.
    
        <copy>cd ~/helidon-levelup-2023-main/docs/
        unzip myproject.zip</copy>
        
10.  Na pasta myproject, execute o comando a seguir para criar o projeto. Use o terminal, onde você definiu as variáveis PATH e JAVA\_HOME.
    
        <copy>cd myproject
        mvn clean package</copy>
        
    
    > Você deverá ver _BUILD SUCCESS_ no fim da execução desse comando.
    
11.  Copie e cole o comando a seguir no terminal para executar este aplicativo. Você verá uma saída semelhante à mostrada na captura de tela abaixo.
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    Você verá uma saída semelhante à seguinte:
    
        $ java -jar target/myproject.jar
        2023.02.28 03:48:36 INFO io.helidon.common.LogConfig Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:48:39 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:48:40 INFO io.helidon.webserver.NettyWebServer Thread[#31,nioEventLoopGroup-2-1,10,main]: Channel '@default' started: [id: 0x5b66bd2a, L:/0.0.0.0:8080]
        2023.02.28 03:48:41 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 4816 milliseconds (since JVM startup).
        2023.02.28 03:48:41 INFO io.helidon.common.HelidonFeatures Thread[#32,features-thread,5,main]: Helidon MP 3.1.2 features: [CDI, Config, Health, JAX-RS, Metrics, Open API, Server]
        
12.  Volte ao terminal, onde você executa os comandos curl e executa os seguintes comandos para verificar o aplicativo:
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
13.  Interrompa o aplicativo _myproject_ digitando `Ctrl + C` no terminal em que o comando "java -jar target/myproject.jar" está sendo executado.
    
14.  Edite o arquivo _src/main/java/com/example/myproject/GreetResource.java_, Localize o método _createResponse(String who)_ e adicione a linha a seguir conforme mostrado na captura de tela.
    
        <copy>System.out.println("Running on thread " + Thread.currentThread());</copy>
        
    
    ![modificar aplicativo](images/modify-application.png)
    
15.  Recriar, executar e exercer o aplicativo conforme descrito nas etapas 10, 11 e 12.
    
16.  Verifique a saída do servidor no terminal em que você iniciou o servidor. Observe que o thread é chamado helidon-server-n. Este é um thread de plataforma tradicional em um pool de threads criado pelo Helidon para tratar solicitações JAX-RS. Você terá uma saída de servidor semelhante à seguinte:
    
        Running on thread Thread[#25,helidon-server-1,5,server]
        
17.  Interrompa o aplicativo _myproject_ digitando `Ctrl + C` no terminal em que o comando "java -jar target/myproject.jar" está sendo executado.
    

## Tarefa 2: Migrar o aplicativo Helidom MicroProfile para o Helidon 4

1.  Para myproject, abra o arquivo _pom.xml_ e altere o pom pai de _3.1.1_ para _4.0.0-ALPHA5_.
    
        <parent>
            <groupId>io.helidon.applications</groupId>
            <artifactId>helidon-mp</artifactId>
            <version>4.0.0-ALPHA5</version>
            <relativePath/>
        </parent>
        
    
    ![Versão Pom](images/pom-version.png)
    
2.  Edite src/main/resources/logging.properties e altere _io.helidon.common.HelidonConsoleHandler_ para _io.helidon.logging.jul.HelidonConsoleHandler_. ![Editar log](images/edit-logging.png)
    
3.  Seu aplicativo foi migrado para o Helidon 4! Copie e cole o comando a seguir para criar o aplicativo.
    
        <copy>mvn clean package -DskipTests</copy>
        
4.  Copie e cole o comando a seguir para executar o aplicativo.
    
        <copy>java --enable-preview  -jar target/myproject.jar</copy>
        
    
    Você terá uma saída semelhante à seguinte.
    
        $ java --enable-preview  -jar target/myproject.jar
        2023.02.28 03:56:17 INFO io.helidon.logging.jul.JulProvider Thread[#1,main,5,main]: Logging at initialization configured using classpath: /logging.properties
        2023.02.28 03:56:21 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Registering JAX-RS Application: HelidonMP
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] http://0.0.0.0:8080 bound for socket '@default'
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.ServerListener VirtualThread[#25,start @default (/0.0.0.0:8080)]/runnable@ForkJoinPool-1-worker-1: [0x613cc6f8] direct writes
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Helidon Níma 4.0.0-ALPHA5
        2023.02.28 03:56:22 INFO io.helidon.nima.webserver.LoomServer Thread[#1,main,5,main]: Started all channels in 19 milliseconds. 5131 milliseconds since JVM startup. Java 19.0.2+7-44
        2023.02.28 03:56:22 INFO io.helidon.microprofile.server.ServerCdiExtension Thread[#1,main,5,main]: Server started on http://localhost:8080 (and all other host addresses) in 5144 milliseconds (since JVM startup).
        2023.02.28 03:56:22 INFO io.helidon.common.features.HelidonFeatures Thread[#28,features-thread,5,main]: Helidon MP 4.0.0-ALPHA5 features: [CDI, Config, Health, Metrics, Open API, Server, WebServer]
        
5.  Observe a saída do servidor e observe que o thread agora é VirtualThread.
    
6.  Interrompa o aplicativo _myproject_ digitando `Ctrl + C` no terminal em que o comando "java --enable-preview -jar target/myproject.jar" está sendo executado.
    

Parabéns, você concluiu o workshop de thread virtual Helidon.

## Confirmação

*   **Autor** - Joe DiPol
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, Fev de 2023