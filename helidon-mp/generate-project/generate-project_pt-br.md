# Gerar um Projeto MP Helidon e Executar o projeto no Code Editor

## Introdução

Este laboratório o orienta pelas etapas para criar um aplicativo Helidon MP.

Tempo Estimado: 15 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Gerar um Projeto Helidon MP e Executar o projeto no Code Editor](videohub:1_22nv8v4q)

### Sobre Produto/Tecnologia

O Helidon foi projetado para ser simples de usar, com ferramentas e exemplos para que você comece rápido. Como o Helidon é apenas uma coleção de bibliotecas em execução em um núcleo Netty rápido, não há sobrecarga extra ou inchaço. O Helidon suporta MicroProfile e oferece APIs familiares como JAX-RS, CDI e JSON-P/B. Nossa implementação MicroProfile é executada em nosso Helidon Reactive WebServer rápido. O WebServer Reativo fornece um modelo de programação moderno, funcional e é executado em cima do Netty. Leve, flexível e reativo, o Helidon WebServer fornece uma base rápida e simples de usar para seus microsserviços.

Com suporte para verificações de integridade, métricas, rastreamento e tolerância a falhas, o Helidon tem o que você precisa para criar aplicativos prontos para a nuvem que se integram ao Prometheus, Jaeger/Zipkin e Kubernetes.

### Sobre o Helidon Project Starter

O Project Starter é uma nova interface da Web para a criação de projetos Helidon. É altamente personalizável, fornecendo várias opções que permitem aos usuários selecionar recursos do Helidon que desejam adicionar ao projeto. Os usuários finais poderão gerar projetos para suas necessidades específicas. Para obter mais informações, clique em [Helidon Starter](https://helidon.io/starter).

### Sobre o Code Editor

O Code Editor permite editar e implantar código para vários serviços do OCI diretamente na Console do OCI. Agora você pode atualizar workflows e scripts de serviço sem precisar alternar entre a Console e seus ambientes de desenvolvimento locais. Isso facilita o protótipo de soluções em nuvem rapidamente, a exploração de novos serviços e a realização de tarefas de codificação rápida.

A integração direta do Editor de Código com o Cloud Shell permite que você acesse o GraalVM Enterprise Native Image e o JDK 17 (Java Development Kit) pré-instalados no Cloud Shell.

### Sobre o OCI Cloud Shell

O [OCI Cloud Shell](https://docs.oracle.com/en-us/iaas/Content/API/Concepts/cloudshellintro.htm) é um terminal baseado em browser acessível na Console do Oracle Cloud. Ele oferece acesso a um shell Linux com uma interface de linha de comando (CLI) do OCI pré-autenticada e ferramentas de desenvolvedor pré-instaladas, e vem com 5 GB de armazenamento.

A partir da versão 22.2.0, o GraalVM Enterprise JDK 17 e o Native Image são pré-instalados no Cloud Shell.

> GraalVM O Enterprise está disponível no Oracle Cloud Infrastructure sem custo adicional.

### Objetivos

*   Crie um microsserviço suportado pelo MicroProfile chamado aplicativo Helidon Greeting
*   Abra o aplicativo Helidon no Code Editor
*   Altere o JDK padrão no shell da nuvem
*   Configurar o Maven necessário
*   Executar e exercer o aplicativo Helidon Greeting

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Tarefa 1: Gerar projeto Helidon usando o iniciador do projeto

1.  Copie o URL abaixo e cole-o no browser para abrir a página Projeto Helidon.
    
        <copy>https://helidon.io/starter/</copy>
        
2.  Em Gerar Seu Projeto, selecione _Helidon MP_ como Helidon Flavor e clique em _Próximo_.
    
3.  Para Tipo de Aplicativo, selecione _Início Rápido_ e clique em _Próximo_.
    
4.  Para Suporte de Mídia, selecione _JSON-B_ e clique em _Próximo_.
    
5.  Para Personalizar Projeto, selecione os valores padrão e clique em _Downloads_. Isso aparecerá em uma janela e salvará este _myproject.zip_ no local de sua escolha. No restante deste workshop, o nome do myproject será usado. Se você escolher um nome diferente, altere respectivamente.
    

## Tarefa 2: Criar e executar o projeto helidon localmente

1.  Na Console do Cloud, clique no ícone _Ferramentas do desenvolvedor_, conforme mostrado, e clique em _Editor de Código_. ![editor de código](images/code-editor.png)
    
2.  No Editor de Código, clique em _Terminal_ -> _Novo Terminal_. ![open terminal](images/open-terminal.png)
    
3.  Copie e cole o comando abaixo no terminal, para criar a pasta _myproject_. onde faremos download do arquivo myproject.zip padrão.
    
        <copy>mkdir -p ~/helidon-project/myproject</copy>
        
4.  No Code Editor, clique em _Arquivo_ -> _Abrir_. ![Abrir Pasta](images/open-folder.png)
    
5.  Selecione a pasta _helidon-project_ e clique em _Abrir_. ![Abrir Helidon](images/open-helidon.png)
    
6.  No Code Editor, na janela EXPLORER, você pode ver seu _HELIDON-PROJECT_. para ver a pasta _myproject_ aqui, clique nela. Agora clique em _Arquivo_ -> _Fazer Upload de Arquivos.._, especifique o local em que você fez download do projeto e selecione o arquivo zip e clique em _Abrir_. ![meu projeto](images/myproject.png) ![fazer upload de arquivos](images/upload-files.png)
    
7.  Copie e cole o comando a seguir para descompactar o arquivo _myproject.zip_.
    
        <copy>cd ~/helidon-project/myproject
        unzip myproject.zip
        </copy>
        
8.  Expanda a pasta _myproject_ para exibir a estrutura do projeto. ![Expandir Projeto](images/expand-project.png)
    
9.  Para executar este projeto, usaremos o Maven 3.8+ e o JDK 17+. Na nuvem Oracle, você tem vários JDK fornecidos. Aqui selecionaremos o JDK GraalVM. Copie e cole o comando a seguir no terminal, para saber seu JDK padrão.
    
        <copy>csruntimectl java list</copy>
        
    
    ![listar JDK](images/list-jdk.png)
    
    > O JDK com \* _asterisk_ no início é seu JDK padrão. Se você tiver qualquer outro JDK, então gravevmeejdk, altere a versão padrão do JDK executando o comando abaixo. Use a versão mostrada do graalvmeejdk, pois pode ser diferente do que é mostrado no comando.
    
        <copy>csruntimectl java set graalvmeejdk-17</copy>
        
10.  Para configurar o maven necessário, copie e cole o comando a seguir no terminal.
    
        <copy>cd ~/helidon-project/myproject/
        wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz
        tar -xzvf apache-maven-3.8.8-bin.tar.gz
        PATH=~/helidon-project/myproject/apache-maven-3.8.8/bin:$PATH</copy>
        
    
    ![configurar o maven](images/configure-maven.png)
    
11.  Para verificar se você tem a versão correta do JDK e do Maven, conforme mostrado abaixo, execute o comando a seguir no terminal.
    
        <copy>mvn -v</copy>
        
    
    ![verificar pré-requisito](images/verify-prerequisite.png)
    
12.  Na pasta _myproject_, execute o comando a seguir para criar o projeto.
    
        <copy>cd ~/helidon-project/myproject/myproject
        mvn clean package</copy>
        
    
    ![criar projeto](images/build-project.png)
    
    > Você deverá ver _BUILD SUCCESS_ no fim da execução desse comando.
    
13.  Copie e cole o comando a seguir no terminal para executar este aplicativo. Você verá a saída semelhante à mostrada na captura de tela abaixo.
    
        <copy>java -jar target/myproject.jar</copy>
        
    
    ![executar projeto](images/run-project.png)
    

> Observe que o horário de início é 4822 milissegundos. Vamos comparar esse horário com o executável de imagem nativa posteriormente.

14.  Abra um novo terminal/console no Code Editor e execute os seguintes comandos para verificar o aplicativo:
    
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
        
15.  _Interrompa o aplicativo **myproject** digitando `Ctrl + C` no terminal em que o comando "java -jar target/myproject.jar" está sendo executado_. É MUITO IMPORTANTE, OUTRAS QUAISQUER QUARIAS NA LABEÇA.
    

## Confirmação

*   **Autor** - Dmitry Aleksandrov
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, abril de 2023