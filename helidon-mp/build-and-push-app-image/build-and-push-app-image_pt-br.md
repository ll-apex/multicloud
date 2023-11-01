# Crie e Envie a Imagem do Aplicativo Helidon para o Oracle Cloud Container Registry

## Introdução

Neste laboratório, você criará uma imagem _nativa_ do Docker com seu aplicativo Helidon e enviará essa imagem para um repositório dentro do Oracle Cloud Container Registry.

Tempo Estimado: 15 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Crie e Envie a Imagem do Aplicativo Helidon para o Oracle Cloud Container Registry](videohub:1_mh1brw5t)

### Objetivos

*   Crie e empacote seu aplicativo usando o Docker.
*   Gere um Token de Autenticação para fazer log-in no Oracle Cloud Container Registry.
*   Envie a imagem do Docker do aplicativo Helidon para seu repositório do Oracle Cloud Container Registry.

### Pré-requisitos

*   O aplicativo Helidon que você criou no laboratório anterior
*   Docker
*   Conta do Oracle Cloud

## Tarefa 1: Criar a Imagem do Docker do Aplicativo Helidon

Estamos criando uma imagem do Docker, que você fará upload para o Oracle Cloud Container Registry que pertence à sua conta do OCI. Para isso, você precisa criar um nome de imagem que reflita suas coordenadas de registro.

É necessário ter as seguintes informações:

*   Nome da Região
*   Namespace da Tenancy
*   Ponto final da Região
    
    > Copie essas informações para um arquivo de texto para que você possa consultá-las em todo o laboratório.
    

1.  Localize seu _Nome da Região_.  
    Seu _Nome da Região_ está localizado no canto superior direito da Console do Oracle Cloud, neste exemplo, ele é mostrado como _Sul do Reino Unido (Londres)_. Sua pode ser diferente.
    
    ![Registro de contêiner](images/region-name.png)
    
2.  Localize o _Namespace da Tenancy_.  
    Na Console, abra o menu de navegação e clique em **Serviços ao Desenvolvedor**. Em **Contêineres e Artefatos**, clique em **Container Registry**.
    
    ![Namespace da Tenancy](images/container-registry.png)
    
    > O namespace da tenancy é listado no compartimento. Copie e salve-o em um arquivo de texto. Você também usará essas informações no próximo laboratório. ![Namespace da Tenancy](images/name-space.png)
    
3.  Localize o _Ponto Final da sua Região_.  
    Consulte a tabela documentada neste URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). No exemplo mostrado, o ponto final da região é _Sul do Reino Unido (Londres)_ (como o nome da região) e seu ponto final é _lhr.ocir.io_. Localize o ponto final do seu próprio _Nome da Região_ e salve-o no arquivo de texto. Você também precisará dele para o próximo laboratório.
    
    ![Pontos finais](images/end-points.png)
    
    > Agora você tem o namespace e o ponto final da tenancy para sua região.
    
4.  Copie o comando a seguir e cole-o no seu arquivo de texto. Em seguida, substitua _`ENDPOINT_OF_YOUR_REGION`_ pelo ponto final do nome da sua região, _`NAMESPACE_OF_YOUR_TENANCY`_ pelo namespace da sua tenancy e _`your_first_name`_ pelo nome da sua tenancy.
    
    > Isso faz um build completo dentro do contêiner do Docker. Na primeira vez que você executá-lo, levará algum tempo porque ele está fazendo download de todas as dependências do Maven e armazenando-as em cache em uma camada do Docker. Os builds subsequentes serão muito mais rápidos, desde que você não altere o arquivo pom.xml. Se o pom for modificado, as dependências serão submetidas a download novamente.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0 -f Dockerfile.native .</copy>
        
    
    Quando o comando estiver pronto, execute no terminal dentro do Code Editor a partir do diretório _`~/helidon-project/myproject/myproject`_. O build produzirá o seguinte resultado no final:
    
        $ docker build -t lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 -f Dockerfile.native .
        
        [1/7] Initializing...                                                                                   (15.7s @ 0.14GB)
        Version info: 'GraalVM 22.3.0 Java 17 CE'
        Java version info: '17.0.5+8-jvmci-22.3-b08'
        C compiler: gcc (redhat, x86_64, 11.3.1)
        Garbage collector: Serial GC
        2 user-specific feature(s)
        - io.helidon.integrations.graal.mp.nativeimage.extension.WeldFeature
        - io.helidon.integrations.graal.nativeimage.extension.HelidonReflectionFeature
        2023.04.06 05:41:01 INFO io.helidon.common.LogConfig Thread[main,5,main]: Logging at initialization configured using classpath: /logging.properties
        [2/7] Performing analysis...  [**********]                                                             (202.8s @ 1.92GB)
        18,812 (92.77%) of 20,278 classes reachable
        27,564 (63.52%) of 43,392 fields reachable
        87,900 (62.22%) of 141,268 methods reachable
        1,068 classes,   565 fields, and 6,864 methods registered for reflection
            65 classes,    70 fields, and    58 methods registered for JNI access
            6 native libraries: dl, m, pthread, rt, stdc++, z
        [3/7] Building universe...                                                                              (27.6s @ 3.15GB)
        [4/7] Parsing methods...      [*****]                                                                   (22.5s @ 3.00GB)
        [5/7] Inlining methods...     [***]                                                                     (11.9s @ 1.84GB)
        [6/7] Compiling methods...    [************]                                                           (156.5s @ 3.05GB)
        [7/7] Creating image...                                                                                 (15.6s @ 2.44GB)
        35.03MB (45.80%) for code area:    57,947 compilation units
        39.02MB (51.01%) for image heap:  477,987 objects and 128 resources
        2.44MB ( 3.19%) for other data
        76.49MB in total
        ------------------------------------------------------------------------------------------------------------------------
        Top 10 packages in code area:                               Top 10 object types in image heap:
        1.63MB sun.security.ssl                                     7.71MB byte[] for code metadata
        1.20MB com.sun.media.sound                                  4.60MB java.lang.Class
        1.17MB java.util                                            3.93MB java.lang.String
        822.87KB java.lang.invoke                                     3.41MB byte[] for java.lang.String
        717.54KB com.sun.crypto.provider                              3.22MB byte[] for general heap data
        517.57KB io.helidon.config                                    1.58MB com.oracle.svm.core.hub.DynamicHubCompanion
        510.02KB java.util.concurrent                                 1.13MB byte[] for reflection metadata
        481.49KB jdk.proxy4                                           1.03MB byte[] for embedded resources
        474.98KB java.lang                                          915.61KB java.util.HashMap$Node
        468.42KB com.sun.org.apache.xerces.internal.impl            781.21KB java.lang.String[]
        26.70MB for 671 more packages                                9.83MB for 4584 more object types
        ------------------------------------------------------------------------------------------------------------------------
                                31.0s (6.6% of total time) in 59 GCs | Peak RSS: 4.80GB | CPU load: 1.60
        ------------------------------------------------------------------------------------------------------------------------
        Produced artifacts:
        /helidon/target/myproject (executable)
        /helidon/target/myproject.build_artifacts.txt (txt)
        ========================================================================================================================
        Finished generating 'myproject' in 7m 48s.
        [INFO] ------------------------------------------------------------------------
        [INFO] BUILD SUCCESS
        [INFO] ------------------------------------------------------------------------
        [INFO] Total time:  08:04 min
        [INFO] Finished at: 2023-04-06T05:48:33Z
        [INFO] ------------------------------------------------------------------------
        Removing intermediate container e400c5c6897b
        ---> 20099e4619d6
        Step 10/15 : RUN echo "done!"
        ---> Running in a8eddd448e48
        done!
        Removing intermediate container a8eddd448e48
        ---> ebfd3064dc68
        Step 11/15 : FROM scratch
        ---> 
        Step 12/15 : WORKDIR /helidon
        ---> Running in 46be56a98462
        Removing intermediate container 46be56a98462
        ---> eaf15b746a1c
        Step 13/15 : COPY --from=build /helidon/target/myproject .
        ---> a69ac5933048
        Step 14/15 : ENTRYPOINT ["./myproject"]
        ---> Running in 71633a601e7f
        Removing intermediate container 71633a601e7f
        ---> cd9f22bfa4b3
        Step 15/15 : EXPOSE 8080
        ---> Running in 4b9763eb49fa
        Removing intermediate container 4b9763eb49fa
        ---> aa8b6e7b04c0
        Successfully built aa8b6e7b04c0
        Successfully tagged lhr.ocir.io/lrv4zdykjqrj/myproject-ankit:1.0
        
5.  Isso cria a imagem do Docker, que você pode verificar no seu repositório local.
    
        $ docker images
        REPOSITORY                     TAG IMAGE ID        CREATED           SIZE
        lhr.ocir.io/tn/myproject-ankit 1.0 aa8b6e7b04c0 About a minute ago   80.2MB
        
    
    Copie para o editor de texto o nome completo substituído `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0` porque você precisará dele posteriormente.
    
6.  Copie e cole o comando a seguir no terminal para executar a imagem do docker no Cloud Shell do Editor de Código.
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    ![execução do docker](images/docker-run.png)
    
7.  Abra um novo terminal/console e execute os seguintes comandos para verificar o aplicativo:
    
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
        

## Tarefa 2: Gerar um Token de Autenticação para fazer log-in no Oracle Cloud Container Registry

Nesta etapa, vamos gerar um _Token de Autenticação_, que usaremos para fazer log-in no Oracle Cloud Container Registry.

1.  Selecione o Ícone do Usuário no canto superior direito e selecione _Meu Perfil_.
    
    ![Meu Perfil](images/my-profile.png " ")
    
2.  Role para baixo e selecione _Tokens de Autenticação_.
    
    ![Tokens de autenticação](images/auth-token.png " ")
    
3.  Clique em _Gerar Token_.
    
    ![Gerar Token](images/generate-token.png " ")
    
4.  Copie _`myproject-your_first_name`_ e cole-o na caixa _Descrição_ e clique em _Gerar Token_.
    
    ![Criação de token](images/token-create.png " ")
    
5.  Selecione _Copiar_ em Token Gerado e cole-o no editor de texto. Não podemos copiá-lo mais tarde. Em seguida, clique em _Fechar_.
    
    ![Copiar Token](images/copy-token.png " ")
    

## Tarefa 3: Enviar a Imagem do Docker do Aplicativo Helidon (myproject) para seu Repositório de Registro de Contêiner

1.  Na Tarefa 1 deste laboratório, você abriu um URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) e determinou o ponto final do nome da Região e copiou-o em um arquivo de texto. Em nosso exemplo, o Nome da Região é Sul do Reino Unido (Londres). Você precisará dessas informações para esta tarefa. ![Ponto Final](images/end-points.png)
    
2.  Copie o comando a seguir e cole-o no seu arquivo de texto e, em seguida, substitua `ENDPOINT_OF_REGION_NAME` pelo ponto final da sua região.
    
    > Em nosso exemplo, o Nome da Região é _Sul do Reino Unido (Londres)_ e o ponto final é _lhr.ocir.io_. Você precisará de suas informações específicas para esta tarefa.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  Na etapa anterior, você também determinou o namespace da tenancy. Informe o Nome de Usuário da seguinte forma: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Substitua `NAMESPACE_OF_YOUR_TENANCY` pelo namespace da sua tenancy
    *   Substitua `YOUR_ORACLE_CLOUD_USERNAME` pelo nome de usuário da sua Conta do Oracle Cloud e copie o nome de usuário substituído do seu arquivo de texto e cole-o no _Cloud Shell_.
    *   Para Senha, copie e cole o Token de Autenticação do seu arquivo de texto (ou onde quer que você o salve).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Navegue de volta para o Container Registry. Na Console, abra o menu de navegação e clique em **Serviços ao Desenvolvedor**. Em **Containers & Artifacts**, clique em **Container Registry**. ![Registro de contêiner](images/container-registry.png)
    
5.  Selecione o compartimento e clique em **Criar Repositório**. ![Criação de Repositório](images/repository-create.png)
    
6.  Selecione o compartimento e informe _`myproject-your_first_name`_ como o Nome do Repositório; em seguida, escolha Acessar como **Público** e clique em **Criar Repositório**.
    
    ![Descrição do Repositório](images/describe-repository.png)
    
7.  Depois que o repositório _`myproject-your_first_name`_ tiver sido criado, você poderá verificar na lista de repositórios e suas definições.
    
    ![Verificar Namespace](images/verify-namespace.png)
    
8.  Para enviar sua imagem do Docker ao seu repositório dentro do Oracle Cloud Container Registry, copie e cole o seguinte comando no seu arquivo de texto e substitua `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/myproject-your\_first\_name:1.0 pelo nome completo da imagem do Docker, que você salvou anteriormente.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    O resultado deve ficar como este:
    
        $ docker push lhr.ocir.io/tenancynamespace/myproject-ankit:1.0 The push refers to repository [lhr.ocir.io/tenancynamespace/myproject-ankit]
        0bf9e88b7284: Pushed 
        0acc08535a89: Pushed 
        1.0: digest: sha256:3e8cc0ff23d256499dff8d907150b639925687aed0c41008cbe1794bc02433e2 size: 735
        $
        
9.  Depois que o comando _docker push_ for executado com sucesso, expanda o repositório _`myproject-your_first_name`_ e você observará que foi feito upload de uma nova imagem para esse repositório.
    
    ![Imagem submetida a upload](images/verify-push.png)
    

## Confirmação

*   **Autor** - Dmitry Aleksandrov
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, abril de 2023