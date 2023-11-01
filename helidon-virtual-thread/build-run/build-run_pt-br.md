# Criar e Executar o aplicativo Helidon Nima e Reativo

## Introdução

Este laboratório orienta você no processo de criação e execução dos aplicativos Nima e Reactive Helidon no Oracle Code Editor dentro do Oracle Cloud Infrastructure.

[Passo a passo de Lab2](videohub:1_e88ydqwt)

Tempo Estimado: 15 minutos

### Objetivos

Neste laboratório, você vai:

*   Criar, executar e testar o aplicativo Helidon Nima
*   Crie, execute e teste o aplicativo Helidon Reactive
*   Analisar a simplicidade do aplicativo Nima em comparação com o aplicativo Reativo

### Pré-requisitos

Para executar este laboratório, você deve ter:

*   Conta do Oracle Cloud
*   Concluído o Laboratório 1, que instala o JDK e o maven necessários.

## Tarefa 1: Criar e Executar o aplicativo Nima

1.  Clique em _File_ -> _Open_ no Code Editor. ![Abrir Projeto](images/open-project.png)
    
2.  Selecione a pasta _helidon-levelup-2023-main_ e clique em _Abrir_. ![Projeto Helidon](images/helidon-project.png)
    
    > Ignore os avisos/problemas, você percebe no Code Editor ao abrir esta pasta.
    
3.  Abra um novo terminal e, em seguida, copie e cole o comando a seguir para definir a variável PATH e JAVA\_HOME.
    
        <copy>PATH=~/jdk-19.0.2/bin:~/apache-maven-3.8.3/bin:$PATH
        JAVA_HOME=~/jdk-19.0.2</copy>
        
4.  Copie o comando a seguir e execute no terminal para verificar se as versões JDK e Maven necessárias estão configuradas corretamente.
    
        <copy>mvn -v</copy>
        
    
    você terá a saída semelhante à seguinte:
    
        $ mvn -v 
        Apache Maven 3.8.3 (ff8e977a158738155dc465c6a97ffaf31982d739)
        Maven home: /home/ankit_x_pa/apache-maven-3.8.3
        Java version: 19.0.2, vendor: Oracle Corporation, runtime: /home/ankit_x_pa/jdk-19.0.2
        Default locale: en_US, platform encoding: UTF-8
        OS name: "linux", version: "4.14.35-2047.520.3.1.el7uek.x86_64", arch: "amd64", family: "unix"
        $ 
        
    
    > _Você usará somente este terminal para criar e executar o aplicativo, pois ele tem as versões necessárias do JDK e do Maven._
    
5.  Copie e cole o comando a seguir para criar o aplicativo nima.
    
        <copy>cd ~/helidon-levelup-2023-main/nima/
        mvn clean package -DskipTests</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-blocking ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/nima/target/example-nima-blocking.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  7.562 s
        [INFO] Finished at: 2023-02-27T11:42:52Z
        [INFO] ------------------------------------------------------------------------
        
6.  Copie e cole o seguinte comando no terminal para executar a versão nima de bloqueio do aplicativo:
    
        <copy>java --enable-preview -jar target/example-nima-blocking.jar</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        $ java --enable-preview -jar target/example-nima-blocking.jar
        2023.02.27 11:43:18.589 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:43:18.902 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:43:19.700 [0x314d2373] http://127.0.0.1:33193 bound for socket '@default'
        2023.02.27 11:43:19.703 [0x314d2373] direct writes
        2023.02.27 11:43:19.722 Helidon Níma 4.0.0-ALPHA5
        
7.  Anote o número da porta em que o servidor é executado (consulte a entrada de log para @default). Por exemplo, em nossa saída, o número da porta é 33193. Da mesma forma, descubra o número da porta do servidor.
    
8.  Para abrir um novo terminal, clique em _Terminal_ -> _Novo Terminal_. Usaremos esse terminal para executar comandos _curl_. ![Novo Terminal](images/new-terminal.png)
    
9.  Copie e cole o comando a seguir no novo terminal. Não se esqueça de substituir _`<port>`_ pela porta do servidor.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        curl http://localhost:33193/one
        remote_1
        $
        
10.  Copie e cole o comando a seguir no novo terminal. Não se esqueça de substituir por _`<port>`_ pela porta do servidor.
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        $ curl http://localhost:33193/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > Observe a ordem dos resultados. Conforme sugerido pelo nome, essa solicitação chama um recurso remoto várias vezes em sequência.
    
11.  Copie e cole o comando a seguir no novo terminal. Não se esqueça de substituir por _`<port>`_ pela porta do servidor.
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        $ curl http://localhost:33193/parallel
        Combined results: [remote_21, remote_18, remote_12, remote_13, remote_14, remote_15, remote_16, remote_17, remote_19, remote_20]
        $
        
    
    > Observe a ordem dos resultados. Conforme sugerido pelo nome, essa solicitação chama um recurso remoto várias vezes em paralelo.
    
12.  Pressione _Ctrl + C_ no terminal em que o comando \*java -jar \* está sendo executado para interromper o servidor.
    

## Tarefa 2: Criar e Executar o aplicativo Reativo

1.  Copie e cole o comando a seguir para criar o aplicativo nima.
    
        <copy>cd ~/helidon-levelup-2023-main/reactive/
        mvn clean package -DskipTests</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        [INFO] --- maven-jar-plugin:3.0.2:jar (default-jar) @ example-nima-reactive ---
        [INFO] Building jar: /home/ankit_x_pa/helidon-levelup-2023-main/reactive/target/example-nima-reactive.jar
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  6.196 s
        [INFO] Finished at: 2023-02-27T11:51:17Z
        [INFO] ------------------------------------------------------------------------
        $
        
2.  Copie e cole o seguinte comando no terminal para executar a versão reativa do aplicativo:
    
        <copy>java --enable-preview -jar target/example-nima-reactive.jar</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        $ java --enable-preview -jar target/example-nima-reactive.jar
        2023.02.27 11:51:26.227 Logging at initialization configured using classpath: /logging.properties
        2023.02.27 11:51:26.723 Configuration file application.yaml is on classpath, yet there is no parser configured for it
        2023.02.27 11:51:27.286 Helidon SE 4.0.0-ALPHA5 features: [Config, Fault Tolerance, Tracing, Web Client, WebServer]
        2023.02.27 11:51:27.618 Channel '@default' started: [id: 0x53fadd43, L:/0.0.0.0:45765]
        
3.  Anote o número da porta em que o servidor é executado (consulte a entrada de log para @default). Por exemplo, em nossa saída, o número da porta é 45765. Da mesma forma, descubra o número da porta do servidor.
    
4.  Volte ao terminal, que abrimos na Tarefa 1 para executar o comando curl.
    
5.  Copie e cole o comando a seguir no novo terminal. Não se esqueça de substituir por _`<port>`_ pela porta do servidor.
    
        <copy>curl http://localhost:<port>/one</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        $ curl http://localhost:45765/one
        remote_1
        $
        
6.  Copie e cole o comando a seguir no novo terminal. Não se esqueça de substituir por _`<port>`_ pela porta do servidor.
    
        <copy>curl http://localhost:<port>/sequence</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        $ curl http://localhost:45765/sequence
        Combined results: [remote_2, remote_3, remote_4, remote_5, remote_6, remote_7, remote_8, remote_9, remote_10, remote_11]
        $
        
    
    > Observe a ordem dos resultados. Conforme sugerido pelo nome, essa solicitação chama um recurso remoto várias vezes em sequência.
    
7.  Copie e cole o comando a seguir no novo terminal. Não se esqueça de substituir por _`<port>`_ pela porta do servidor.
    
        <copy>curl http://localhost:<port>/parallel</copy>
        
    
    Você terá uma saída semelhante à seguinte:
    
        $ curl http://localhost:45765/parallel
        Combined results: [remote_17, remote_16, remote_13, remote_20, remote_12, remote_19, remote_18, remote_14, remote_21, remote_15]
        $
        
    
    > Observe a ordem dos resultados. Conforme sugerido pelo nome, essa solicitação chama um recurso remoto várias vezes em paralelo.
    
8.  Pressione _Ctrl + C_ no terminal em que o comando \*java -jar \* está sendo executado para interromper o servidor.
    

## Tarefa 3: Analisar a simplicidade da aplicação Nima

**Bloqueio versus reativo**

Vamos comparar as implementações entre Níma (bloqueio) e Helidon SE (reativo) para a mesma tarefa.

*   Ambas as implementações executam chamadas REST usando o Helidon WebClient
*   A implementação de bloqueio é simples de seguir:
    *   O fluxo de execução é refletido pela ordem da instrução no código
    *   Não há chamadas para restringir ou enriquecer semanticamente as chamadas de biblioteca
    *   A depuração é simples
*   As versões reativas exigem um bom entendimento das bibliotecas reativas
    *   Incluindo Multi, flatMap, tratamento de erros etc.
    *   O fluxo de controle não é mais óbvio devido ao uso de handlers reativos
    *   É necessário compreender a capacidade do flatMap de ativar/forbecer a simultaneidade
    *   Depuração é mais difícil

1.  Abra o arquivo _nima/src/main/java/io/examples/helidon/nima/BlockingService.java_ para ver como os pontos finais são implementados na versão nima do aplicativo. ![Nima Block](images/nima-block.png)
    
2.  Abra o arquivo _reactive/src/main/java/io/examples/helidon/reactive/ReactiveService.java_ para ver como os pontos finais são implementados na versão reativa do aplicativo. ![Serviço Reativo](images/reactive-service.png)
    
3.  Na seção _EDITORES OPEN_, clique com o botão direito do mouse no arquivo _BlockingService.java_ e selecione _Selecionar para Comparação_. ![Selecionar Comparação](images/select-compare.png)
    
4.  Clique com o botão direito do mouse no arquivo _ReactiveService.java_ e selecione _Comparar com Selecionado_. ![Comparar selecionados](images/compare-selected.png)
    
5.  Veja que o código reativo é mais complicado do que bloquear (Thread Virtual) ![Comparar código](images/compare-code.png)
    
    > Verifique os métodos um, sequência e paralelo em BlockingService e ReactiveService, respectivamente. Veja se você entende como eles funcionam!
    

Agora você pode _acessar o próximo laboratório_.

## Confirmação

*   **Autor** - Joe DiPol
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, Fev de 2023