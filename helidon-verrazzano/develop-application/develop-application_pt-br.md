# Desenvolver Aplicativo Helidon

## Introdução

Este laboratório o orienta pelas etapas para criar um aplicativo Helidon MP.

Tempo Estimado: 15 minutos

### Sobre Produto/Tecnologia

O Helidon foi projetado para ser simples de usar, com ferramentas e exemplos para que você comece rápido. Como o Helidon é apenas uma coleção de bibliotecas em execução em um núcleo Netty rápido, não há sobrecarga extra ou inchaço. O Helidon suporta MicroProfile e oferece APIs familiares como JAX-RS, CDI e JSON-P/B. Nossa implementação MicroProfile é executada em nosso Helidon Reactive WebServer rápido. O WebServer Reativo fornece um modelo de programação moderno, funcional e é executado em cima do Netty. Leve, flexível e reativo, o Helidon WebServer fornece uma base rápida e simples de usar para seus microsserviços.

Com suporte para verificações de integridade, métricas, rastreamento e tolerância a falhas, o Helidon tem o que você precisa para criar aplicativos prontos para a nuvem que se integram ao Prometheus, Jaeger/Zipkin e Kubernetes.

### Sobre a CLI do Helidon

A CLI do Helidon permite que você crie facilmente um projeto Helidon escolhendo um conjunto de arquétipos. Ele também suporta um loop de desenvolvedor que executa compilação contínua e reinicialização do aplicativo, para que você possa iterar facilmente as alterações do código-fonte.

A CLI é distribuída como um executável independente (compilado usando GraalVM) para facilitar a instalação. Atualmente está disponível como um download para Linux, Mac e Windows. Basta baixar o binário, instalá-lo em um local acessível a partir de seu PATH e você está pronto para ir.

### Objetivos

*   Instalar a CLI do Helidon
*   Crie um microsserviço suportado pelo MicroProfile chamado Saudação do Helidon
*   Executar e exercer o aplicativo Helidon Greeting
*   Exibir dados de integridade e métricas
*   Adicionar nova funcionalidade ao aplicativo

### Pré-requisitos

*   O Helidon exige Java 11+
*   Maven 3.6.x

> **Cuidado: Não use a versão 3.8.x devido a um problema conhecido com a criação do aplicativo!**

*   Java e `mvn` estão no seu caminho.
*   Os usuários do Windows também precisarão do Visual C++ Redistributable Runtime.  
    Consulte [Helidon no Windows](https://helidon.io/docs/v2/#/about/04_windows) para obter mais informações.

## Tarefa 1: Instalar a CLI Helidon

1.  Instalar CLI do Helidon
    
    Para macOS:
    
        <copy>
        curl -O https://helidon.io/cli/latest/darwin/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Para Linux:
    
        <copy>
        curl -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon
        sudo mv ./helidon /usr/local/bin/
        </copy>
        
    
    Para Windows:
    
        <copy>
        PowerShell -Command Invoke-WebRequest -Uri "https://helidon.io/cli/latest/windows/helidon.exe" -OutFile "C:\Windows\system32\helidon.exe"
        </copy>
        

## Tarefa 2: Criar Aplicativo de Saudação para Helidon

1.  Na console, informe:
    
        <copy>helidon init --version 2.4.0 </copy>
        
    
    > Para evitar possíveis problemas, defina a versão específica do Helidon que foi testada para o ambiente deste laboratório.
    
2.  Para este demo criaremos um microsserviço suportado MicroProfile, então escolha a opção **(2)** para **Helidon MP Flavor**:
    
        Helidon flavor
        (1) SE 
        (2) MP 
        Enter selection (Default: 1): 2
        
3.  Para obter a maior parte da funcionalidade, escolha a opção **(2) quickstart** e, em seguida, **Enter** para as respostas padrão. Observe que você pode ter diferentes nomes de pacote e grupo de projetos padrão porque ele usa o nome de usuário do SO. Anote o nome do pacote, você precisará usá-lo ao criar uma nova classe Java.
    
        Select archetype
        (1) bare | Minimal Helidon MP project suitable to start from scratch 
        (2) quickstart | Sample Helidon MP project that includes multiple REST operations 
        (3) database | Helidon MP application that uses JPA with an in-memory H2 database 
        Enter selection (Default: 1): 2
        Project name (Default: quickstart-mp): 
        Project groupId (Default: me.user-helidon): 
        Project artifactId (Default: quickstart-mp): 
        Project version (Default: 1.0-SNAPSHOT): 
        Java package name (Default: me.user_name.mp.quickstart): 
        Switch directory to /home/user/quickstart-mp to use CLI
        
        Start development loop? (Default: n):
        $
        
    
    > Para o **loop de desenvolvimento**, aceite o padrão (**n**) por enquanto. Você iniciará o loop de desenvolvimento posteriormente neste laboratório.
    
    Agora você tem um Projeto Maven de Microsserviços totalmente funcional:
    

        quickstart-mp
        ├── Dockerfile
        ├── Dockerfile.jlink
        ├── Dockerfile.native
        ├── README.md
        ├── app.yaml
        ├── pom.xml
        └── src
            ├── main
            │   ├── java
            │   │   └── me
            │   │       └── buzz
            │   │           └── mp
            │   │               └── quickstart
            │   │                   ├── GreetResource.java
            │   │                   ├── GreetingProvider.java
            │   │                   └── package-info.java
            │   └── resources
            │       ├── META-INF
            │       │   ├── beans.xml
            │       │   ├── microprofile-config.properties
            │       │   └── native-image
            │       │       └── reflect-config.json
            │       └── logging.properties
            └── test
                └── java
                    └── me
                        └── buzz
                            └── mp
                                └── quickstart
                                    └── MainTest.java
    
    

## Tarefa 3: Executar o Aplicativo Helidon Greeting

1.  No mesmo console/terminal, navegue até o diretório quickstart-mp e execute os seguintes comandos:
    
        <copy> cd quickstart-mp
        </copy>
        
2.  Com JDK11+
    
        <copy>
        mvn package
        java -jar target/quickstart-mp.jar
        </copy>
        

### Exercitar o Aplicativo

1.  Abra um novo terminal/console e execute os seguintes comandos para verificar o aplicativo:
    
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
        

### Revisar Dados de Integridade e Métricas

1.  No mesmo terminal/console, execute os seguintes comandos para verificar a integridade e as métricas:
    
        <copy>
        curl -s -X GET http://localhost:8080/health
        </copy>
        {"outcome":"UP",...
        . . .
        
    
        # Prometheus Format
        <copy>
        curl -s -X GET http://localhost:8080/metrics
        </copy>
        # TYPE base:gc_g1_young_generation_count gauge
        . . .
        
    
        # JSON Format
        <copy>
        curl -H 'Accept: application/json' -X GET http://localhost:8080/metrics
        </copy>
        {"base":...
        . . .
        
2.  Interrompa o aplicativo _quickstart-mp_ digitando `Ctrl + C` no terminal em que o comando "java -jar target/quickstart-mp.jar" está sendo executado.
    

## Tarefa 4: Modificar o Aplicativo

1.  Abra o IDE favorito e navegue até o arquivo **microprofile-config.properties**.
    
    ![Arquivo de Configuração](images/config.jpg)
    
2.  No console/terminal, navegue até a pasta do projeto e digite:
    
        <copy>helidon dev</copy>
        
    
    > Isso iniciará o **Loop de desenvolvimento** mencionado na tarefa anterior.
    
3.  Altere a propriedade _app.greeting_ para "Hello Oracle" e salve o arquivo.
    
        <copy>app.greeting=Hello Oracle</copy>
        
    
    ![HelidonDev](images/properties.jpg)
    
    > Você verá que sempre que alterar um arquivo, a **CLI Helidon** reconhece que há uma alteração, recompila o aplicativo e o executa novamente. Como o Helidon é pequeno, tudo acontece rapidamente.
    
4.  No console/terminal, informe o seguinte:
    
        <copy>curl -X GET http://localhost:8080/greet</copy>
        
    
    Espera-se que o resultado seja:
    
        {"message":"Hello Oracle World!"}
        
    
    > Certifique-se de interromper o loop de desenvolvimento com `CTRL+C`
    
5.  Volte para a pasta do projeto e abra o arquivo **GreetResource.java**.
    
    > Você pode ver que o código compatível com MicroProfile é puro:
    
    ![ModifyJava](images/greet-resource.jpg)
    
6.  Crie um novo ponto final que forneça ajuda para diferentes cumprimentos em diferentes idiomas. Para criar essa nova funcionalidade, crie uma nova classe chamada **GreetHelpResource** com o seguinte código:
    
        <copy>
        package me.user_name.me.quickstart;
        import java.util.Arrays;
        import java.util.List;
        import java.util.logging.Logger;
        
        import javax.enterprise.context.ApplicationScoped;
        import javax.ws.rs.GET;
        import javax.ws.rs.Path;
        
        import org.eclipse.microprofile.metrics.annotation.Counted;
        
        @ApplicationScoped
        @Path("/help")
        public class GreetHelpResource {
        
            Logger LOGGER = Logger.getLogger(GreetHelpResource.class.getName());
        
            @GET
            @Path("/allGreetings")
            @Counted(name = "helpCalled", description = "How many time help was called")
            public String getAllGreetings(){
                LOGGER.info("Help requested!");
                return Arrays.toString(List.of("Hello","Привет","Hola","Hallo","Ciao","Nǐ hǎo", "Marhaba","Olá").toArray());
            }
        }
        </copy>
        
    
    > A classe tem apenas um método _getAllGreetings_ que retorna uma lista de saudações em diferentes idiomas. Ao copiar o código, certifique-se de adicionar o nome do pacote necessário na parte superior da classe.
    
7.  Crie e execute o aplicativo:
    
        <copy>
        mvn package -DskipTests
        java -jar target/quickstart-mp.jar
        </copy>
        
8.  Execute o comando a seguir e observe os resultados:
    
        <copy>curl http://localhost:8080/help/allGreetings</copy>
        
    
    O resultado esperado:
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
9.  Veja as métricas e você verá que um novo contador apareceu. Sempre que esse ponto final for chamado, o valor será incrementado:
    
        curl http://localhost:8080/metrics
        
        ...
        # TYPE application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total counter
        # HELP application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total How many time help was called
        application_me_buzz_mp_quickstart_GreetHelpResource_helpCalled_total 1
        ...
        
10.  No console, você verá a linha de log INFO sobre essa chamada:
    
        INFO me.buzz.mp.quickstart.GreetHelpResource Thread[helidon-4,5,server]: Help requested!
        
    
    E o novo ponto final foi adicionado.
    
    ![NewEndpoint](images/logs-output.jpg)
    
    > Trabalhar com o Helidon e suas ferramentas é fácil e rápido!
    
11.  Deixe seu terminal/console aberto e continue com o laboratório de instalação do Verrazzano.
    

## Confirmação

*   **Autor** - Dmitry Aleksandrov
*   **Contribuidores** - Maciej Gruszka, Ankit Pandey
*   **Última Atualização por/Data** - Ankit Pandey, março de 2023