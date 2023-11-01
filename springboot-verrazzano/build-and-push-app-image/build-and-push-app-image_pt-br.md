# Envie a Imagem do Aplicativo Springboot para o Oracle Cloud Container Registry

## Introdução

Neste laboratório, você criará uma imagem do Docker com seu aplicativo springboot e enviará essa imagem para um repositório dentro do Oracle Cloud Container Registry.

Tempo Estimado: 10 minutos

### Objetivos

*   Crie e empacote seu aplicativo usando o Docker.
*   Gere um Token de Autenticação para fazer log-in no Oracle Cloud Container Registry.
*   Envie a imagem do Docker do aplicativo springboot para seu repositório do Oracle Cloud Container Registry.

### Pré-requisitos

*   Docker
*   Conta do Oracle Cloud

## Tarefa 1: Fazer download do código-fonte do aplicativo e do JDK necessário

1.  Copie e cole o comando a seguir para fazer download do código-fonte deste workshop.
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/EeNNvCVLObhXjHIwu5M-J_zbSXJsbLop6wcsFFnDfneY3zqbAFfdKikZe-Q0GT3I/n/lrv4zdykjqrj/b/ankit-bucket/o/springboot-app.zip >~/springboot-app.zip</copy>
        
2.  Copie e cole o comando a seguir para descompactar o código-fonte e alterar o diretório atual para a pasta do aplicativo.
    
        <copy>unzip ~/springboot-app.zip
        cd ~/springboot-app/</copy>
        
3.  Vamos criar uma imagem do Docker para o aplicativo springboot, mas esse aplicativo usa uma versão específica do JDK e não queremos alterar os arquivos do Docker que criam a nova imagem. Então, fazemos download do JDK necessário. Para fazer download da versão do JDK necessária, copie o comando abaixo e cole-o no Cloud Shell.
    
        <copy>wget https://download.java.net/java/ga/jdk11/openjdk-11_linux-x64_bin.tar.gz</copy>
        
4.  Copie e cole o comando a seguir para criar o aplicativo springboot.
    
        <copy>mvn clean package</copy>
        

## Tarefa 2: Criar a Imagem do Docker do Aplicativo Springboot

Começaremos preparando a imagem do Docker que você usará para implantar na Verrazzano.

Estamos criando uma imagem do Docker, que você fará upload para o Oracle Cloud Container Registry que pertence à sua conta do OCI. Para isso, você precisa criar um nome de imagem que reflita suas coordenadas de registro.

É necessário ter as seguintes informações:

*   Namespace da Tenancy
    
*   Ponto final da Região
    
    > Copie essas informações para um arquivo de texto para que você possa consultá-las em todo o laboratório.
    

1.  Para localizar o Namespace da tenancy, clique no Ícone _Usuário_ -> _Tenancy_, conforme mostrado. Nas **Definições de armazenamento de objetos**, você encontrará o Namespace. Copie e salve-o em seu arquivo de texto, pois também o usaremos posteriormente.
    
    ![Copiar Tenancynamespace](images/copy-tenancynamespace.png " ")
    
2.  Localize o _Ponto Final da sua Região_.  
    Consulte a tabela documentada neste URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). No exemplo mostrado, o ponto final da região é _Sul do Reino Unido (Londres)_ (como o nome da região) e seu ponto final é _lhr.ocir.io_. Localize o ponto final do seu próprio _Nome da Região_ e salve-o no arquivo de texto.
    
    ![Pontos finais](images/end-points.png)
    
    > Agora você tem o namespace e o ponto final da tenancy para sua região.
    
3.  Copie o comando a seguir e cole-o no seu arquivo de texto. Em seguida, substitua _`ENDPOINT_OF_YOUR_REGION`_ pelo ponto final do nome da sua região, _`NAMESPACE_OF_YOUR_TENANCY`_ pelo namespace da sua tenancy e _`your_first_name`_ pelo nome da sua tenancy.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-your_first_name:v1 .</copy>
        
    
    Quando o comando estiver pronto, execute no Cloud Shell no diretório `~/springboot-app/`. O build produzirá o seguinte resultado:
    
        $ cd ~/springboot-app/
        $ docker build -t lhr.ocir.io/tenancy-namespace/springboot-ankit:v1 .
        Sending build context to Docker daemon  206.7MB
        Step 1/14 : FROM ghcr.io/oracle/oraclelinux:7-slim AS build_base
        Trying to pull repository ghcr.io/oracle/oraclelinux ... 
        7-slim: Pulling from ghcr.io/oracle/oraclelinux
        6cb086706000: Pull complete 
        Digest: sha256:4353fdc8664c386c0a443eb40b10a7662b4eb8d6eb5d6dcefe218e9783132c71
        Status: Downloaded newer image for ghcr.io/oracle/oraclelinux:7-slim
        ---> 1d56b1a0fd84
        Step 2/14 : RUN yum update -y && yum-config-manager --save --setopt=ol7_ociyum_config.skip_if_unavailable=true     && yum install -y tar unzip gzip     && yum clean all; rm -rf /var/cache/yum     && mkdir -p /license
        ---> Running in 92c013a8e84f
        Loaded plugins: ovl
        No packages marked for update
        Loaded plugins: ovl
        ===================================== main =====================================
        [main]
        alwaysprompt = True
        assumeno = False
        assumeyes = False
        autocheck_running_kernel = True
        autosavets = True
        bandwidth = 0
        bugtracker_url = https://linux.oracle.com
        cache = 0
        cachedir = /var/cache/yum/x86_64/7Server
        check_config_file_age = True
        clean_requirements_on_remove = False
        ----------------------------------------------------------------------
        Step 3/14 : ENV JAVA_HOME=/usr/java
        ---> Running in 96b99fba7e50
        Removing intermediate container 96b99fba7e50
        ---> 1cc6c7a63b89
        Step 4/14 : ENV PATH $JAVA_HOME/bin:$PATH
        ---> Running in 4a88eb052547
        Removing intermediate container 4a88eb052547
        ---> 48e7fa9b7b0c
        Step 5/14 : ARG JDK_BINARY="${JDK_BINARY:-openjdk-11_linux-x64_bin.tar.gz}"
        ---> Running in e922e8b35bfd
        Removing intermediate container e922e8b35bfd
        ---> 6888b690a4b0
        Step 6/14 : COPY ${JDK_BINARY} jdk.tar.gz
        ---> e9c5ffd0f2a5
        Step 7/14 : ENV JDK_DOWNLOAD_SHA256=3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e
        ---> Running in a7a89b057f25
        Removing intermediate container a7a89b057f25
        ---> 12ecfaa002bf
        Step 8/14 : RUN set -eux     echo "Checking JDK hash";     echo "${JDK_DOWNLOAD_SHA256} *jdk.tar.gz" | sha256sum --check -;     echo "Installing JDK";     mkdir -p "$JAVA_HOME";     tar xzf jdk.tar.gz --directory "${JAVA_HOME}" --strip-components=1;     rm -f jdk.tar.gz;
        ---> Running in a6a590adeee6
        + echo '3784cfc4670f0d4c5482604c7c513beb1a92b005f569df9bf100e8bef6610f2e *jdk.tar.gz'
        + sha256sum --check -
        jdk.tar.gz: OK
        + echo 'Installing JDK'
        + mkdir -p /usr/java
        Installing JDK
        + tar xzf jdk.tar.gz --directory /usr/java --strip-components=1
        + rm -f jdk.tar.gz
        Removing intermediate container a6a590adeee6
        ---> 1f37a6cb044d
        Step 9/14 : COPY LICENSE.txt /license/
        ---> dc69f24f5be6
        Step 10/14 : COPY THIRD_PARTY_LICENSES.txt /license/
        ---> 5ef683dbda22
        Step 11/14 : ARG JAR_FILE=target/*.jar
        ---> Running in 3b80032f8310
        Removing intermediate container 3b80032f8310
        ---> 8eca16289bd7
        Step 12/14 : COPY ${JAR_FILE} app.jar
        ---> dcb7e3ed0871
        Step 13/14 : ENTRYPOINT ["java","-jar","/app.jar"]
        ---> Running in 2191623bf524
        Removing intermediate container 2191623bf524
        ---> 11e59e19cfb4
        Step 14/14 : USER 1000
        ---> Running in 16446779b92b
        Removing intermediate container 16446779b92b
        ---> a701fa912f2e
        Successfully built a701fa912f2e
        Successfully tagged lhr.ocir.io/tenancynamespace/springboot-ankit:v1
        $
        
4.  Isso cria a imagem do Docker, que você pode verificar no seu repositório local.
    
        $ docker images
        REPOSITORY                            TAG     IMAGE ID    CREATED      SIZE
        lhr.ocir.io/namespace/springboot-ankit v1    a701fa912f2 3 minutes ago 664MB
        ghcr.io/oracle/oraclelinux            7-slim 1d56b1a0fd8 3 weeks ago   138MB
        $ 
        
    
    Copie para seu arquivo de texto o nome completo substituído `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1` porque você precisará dele posteriormente.
    

## Tarefa 3: Gerar um Token de Autenticação para fazer log-in no Oracle Cloud Container Registry

Nesta etapa, vamos gerar um _Token de Autenticação_, que usaremos para fazer log-in no Oracle Cloud Container Registry.

1.  Selecione o Ícone do Usuário no canto superior direito e selecione _Meu Perfil_.
    
    ![Meu Perfil](images/my-profile.png " ")
    
2.  Role para baixo e selecione _Tokens de Autenticação_.
    
    ![Tokens de autenticação](images/auth-token.png " ")
    
3.  Clique em _Gerar Token_.
    
    ![Gerar Token](images/generate-token.png " ")
    
4.  Copie _`springboot-your_first_name`_ e cole-o na caixa _Descrição_ e clique em _Gerar Token_.
    
    ![Criação de token](images/token-create.png " ")
    
5.  Selecione _Copiar_ em Token Gerado e cole-o no arquivo de texto. Não podemos copiá-lo mais tarde. Em seguida, clique em _Fechar_.
    
    ![Copiar Token](images/copy-token.png " ")
    

## Tarefa 4: Enviar a Imagem do Docker do Aplicativo Springboot para seu Repositório de Registro de Contêiner

1.  Na Tarefa 1 deste laboratório, você abriu um URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) e determinou o ponto final do nome da Região e copiou-o em um arquivo de texto. Em nosso exemplo, o Nome da Região é Sul do Reino Unido (Londres). Você precisará dessas informações para esta tarefa. ![Ponto Final](images/end-points.png)
    
2.  Copie o comando a seguir e cole-o no editor de texto e, em seguida, substitua `ENDPOINT_OF_REGION_NAME` pelo ponto final da sua região.
    
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
    
5.  Selecione o compartimento e clique em **Criar**. ![Criação de Repositório](images/repository-create.png)
    
6.  Selecione o compartimento e informe _`springboot-your_first_name`_ como o Nome do Repositório; em seguida, escolha Acessar como **Público** e clique em **Criar Repositório**.
    
    ![Descrição do Repositório](images/describe-repository.png)
    
7.  Para enviar sua imagem do Docker ao seu repositório dentro do Oracle Cloud Container Registry, copie e cole o seguinte comando no seu arquivo de texto e substitua _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/springboot-your\_firstname:1.0_ pelo nome completo da imagem do Docker, que você salvou anteriormente.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/springboot-ankit:v1</copy>
        
    
    O resultado deve ficar como este:
    
        $ docker push lhr.ocir.io/namespace/springboot-ankit:v1
        The push refers to repository [lhr.ocir.io/namespace/springboot-ankit]
        31118271414e: Pushed 
        e6144652ec48: Pushed 
        a5ac4d4576aa: Pushed 
        2f93ab3a0c42: Pushed 
        3ed60ad88e51: Pushed 
        f47db30f116a: Pushed 
        f50ba2e0b2f9: Pushed 
        v1: digest: sha256:96aacff31cb255ea815213aba837f16f40d73b14d67449d4744ed811c7a864c8 size: 1795
        $ 
        
8.  Depois que o comando _docker push_ for executado com sucesso, expanda o repositório _`springboot-ankit:v1`_ e você observará que foi feito upload de uma nova imagem para esse repositório.
    

Agora você pode **acessar o próximo laboratório**.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023