# Envie a Imagem do Aplicativo Tomcat para o Oracle Cloud Container Registry

## Introdução

Neste laboratório, você criará uma imagem do Docker com seu aplicativo tomcat e enviará essa imagem para um repositório dentro do Oracle Cloud Container Registry.

Tempo Estimado: 10 minutos

### Objetivos

*   Crie e empacote seu aplicativo usando o Docker.
*   Gere um Token de Autenticação para fazer log-in no Oracle Cloud Container Registry.
*   Envie a imagem do Docker do aplicativo tomcat para o repositório do Oracle Cloud Container Registry.

### Pré-requisitos

*   Docker
*   Conta do Oracle Cloud

## Tarefa 1: Fazer download do código-fonte do aplicativo

1.  Copie e cole o comando a seguir para fazer download do código-fonte deste workshop.
    
        <copy>curl -LSs https://objectstorage.uk-london-1.oraclecloud.com/p/5DhoxZ22MseWaoSjG3EoBRf6DF5JbQ1QyR7tHa2qem-7IHb7nNEymF1ZSm2eA1ix/n/lrv4zdykjqrj/b/ankit-bucket/o/tomcat-examples.zip >~/tomcat-examples.zip</copy>
        
2.  Copie e cole o comando a seguir para descompactar o código-fonte e alterar o diretório atual para a pasta do aplicativo.
    
        <copy>unzip ~/tomcat-examples.zip
        cd ~/tomcat-examples/</copy>
        

## Tarefa 2: Criar a Imagem do Docker do Aplicativo Tomcat

Começaremos preparando a imagem do Docker que você usará para implantar na Verrazzano.

Estamos criando uma imagem do Docker, que você fará upload para o Oracle Cloud Container Registry que pertence à sua conta do OCI. Para isso, você precisa criar um nome de imagem que reflita suas coordenadas de registro.

É necessário ter as seguintes informações:

*   Namespace da Tenancy
    
*   Ponto final da Região
    
    > Copie essas informações para um arquivo de texto para que você possa consultá-las em todo o laboratório.
    

1.  Para localizar o Namespace da tenancy, clique no Ícone _Usuário_ -> _Tenancy_, conforme mostrado. Nas **Definições de armazenamento de objetos**, você encontrará o Namespace. Copie e salve-o em seu arquivo de texto, pois também o usaremos posteriormente.
    
    ![Copiar Tenancynamespace](images/copy-tenancynamespace.png " ")
    
2.  Abra um URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) e determine o ponto final do nome da sua Região e copie-o para um arquivo de texto. Em nosso exemplo, o Nome da Região é Sul do Reino Unido (Londres). Você precisará dessas informações para esta tarefa. ![Ponto final](images/end-point.png)
    
3.  Copie o comando a seguir e cole-o no seu editor de texto. Em seguida, substitua _`ENDPOINT_OF_YOUR_REGION`_ pelo ponto final do nome da sua região, _`NAMESPACE_OF_YOUR_TENANCY`_ pelo namespace da sua tenancy e _`your_first_name`_ pelo nome da sua tenancy.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-your_first_name:v1 .</copy>
        
    
    Quando o comando estiver pronto, execute no Cloud Shell no diretório `~/tomcat-examples/`. O build produzirá o seguinte resultado:
    
        $ cd ~/tomcat-examples/
        $ $ docker build -t lhr.ocir.io/tenancy-namespace/tomcat-example-ankit:v1 .
        Sending build context to Docker daemon  1.097MB
        Step 1/10 : FROM tomcat:8.0-alpine
        Trying to pull repository docker.io/library/tomcat ... 
        8.0-alpine: Pulling from docker.io/library/tomcat
        4fe2ade4980c: Pull complete 
        6fc58a8d4ae4: Pull complete 
        7d9bd64c803b: Pull complete 
        a22aedc5ac11: Pull complete 
        5bde63ae3587: Pull complete 
        69cb0c9b940a: Pull complete 
        Digest: sha256:d02a16c0147fcae13d812fa670a4b3c9944f5328b10a5a463ad697d2aa5bb063
        Status: Downloaded newer image for tomcat:8.0-alpine
        ---> 624fb61775c3
        Step 2/10 : LABEL maintainer="ankit.x.pandey@oracle.com"
        ---> Running in 20cc23726499
        Removing intermediate container 20cc23726499
        ---> 50245c696fb6
        Step 3/10 : ADD sample-webapp.war /usr/local/tomcat/webapps/
        ---> 727c55f91bb5
        Step 4/10 : RUN mkdir /data
        ---> Running in f3129a859e11
        Removing intermediate container f3129a859e11
        ---> 9ce0f5674f51
        Step 5/10 : ADD jmx_prometheus_javaagent-0.17.0.jar /data/jmx_prometheus_javaagent-0.17.0.jar
        ---> f03cc9ee1bee
        Step 6/10 : ADD prometheus-jmx-config.yaml /data/prometheus-jmx-config.yaml
        ---> 50c51ae6a148
        Step 7/10 : ENV JAVA_OPTS="-javaagent:/data/jmx_prometheus_javaagent-0.17.0.jar=8088:/data/prometheus-jmx-config.yaml"
        ---> Running in 5e9effd5d494
        Removing intermediate container 5e9effd5d494
        ---> 85ca06fcd965
        Step 8/10 : EXPOSE 8088
        ---> Running in 795325f82526
        Removing intermediate container 795325f82526
        ---> 19dfc6fd903c
        Step 9/10 : EXPOSE 8080
        ---> Running in 43be96f20275
        Removing intermediate container 43be96f20275
        ---> 7d9bcaa7a271
        Step 10/10 : CMD ["catalina.sh", "run"]
        ---> Running in 3e25cd78ab88
        Removing intermediate container 3e25cd78ab88
        ---> 516065fe1bf5
        Successfully built 516065fe1bf5
        Successfully tagged lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        $
        
4.  Isso cria a imagem do Docker, que você pode verificar no seu repositório local.
    
        $ docker images
        REPOSITORY                           TAG IMAGE ID      CREATED       SIZE
        lhr.ocir.io/name/tomcat-example-ankit v1 516065fe1bf5 2 minutes ago  147MB
        tomcat                       8.0-alpine  624fb61775c3 4 years ago    147MB
        
    
    Copie para o editor de texto o nome completo substituído `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1` porque você precisará dele posteriormente.
    

## Tarefa 3: Gerar um Token de Autenticação para fazer log-in no Oracle Cloud Container Registry

Nesta etapa, vamos gerar um _Token de Autenticação_, que usaremos para fazer log-in no Oracle Cloud Container Registry.

1.  Selecione o Ícone do Usuário no canto superior direito e selecione _Meu Perfil_.
    
    ![Meu Perfil](images/my-profile.png " ")
    
2.  Role para baixo e selecione _Tokens de Autenticação_.
    
    ![Tokens de autenticação](images/auth-token.png " ")
    
3.  Clique em _Gerar Token_.
    
    ![Gerar Token](images/generate-token.png " ")
    
4.  Copie _`tomcat-example-your_first_name`_ e cole-o na caixa _Descrição_ e clique em _Gerar Token_.
    
    ![Criação de token](images/token-create.png " ")
    
5.  Selecione _Copiar_ em Token Gerado e cole-o no editor de texto. Não podemos copiá-lo mais tarde. Em seguida, clique em _Fechar_.
    
    ![Copiar Token](images/copy-token.png " ")
    

## Tarefa 4: Enviar a Imagem do Docker do Aplicativo tomcat para seu Repositório de Registro de Contêiner

1.  Na Tarefa 2 deste laboratório, você abriu um URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) e determinou o ponto final do nome da Região e copiou-o em um arquivo de texto. Em nosso exemplo, o Nome da Região é Sul do Reino Unido (Londres). Você precisará dessas informações para esta tarefa. ![Ponto Final](images/end-point.png)
    
2.  Copie o comando a seguir e cole-o no editor de texto e, em seguida, substitua `ENDPOINT_OF_REGION_NAME` pelo ponto final da sua região.
    
    > Em nosso exemplo, o Nome da Região é _Sul do Reino Unido (Londres)_ e o ponto final é _lhr.ocir.io_. Você precisará de suas informações específicas para esta tarefa.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  Na etapa anterior, você também determinou o namespace da tenancy. Informe o Nome de Usuário da seguinte forma: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Substitua `NAMESPACE_OF_YOUR_TENANCY` pelo namespace da sua tenancy
    *   Substitua `YOUR_ORACLE_CLOUD_USERNAME` pelo nome de usuário da sua Conta do Oracle Cloud e copie o nome de usuário substituído do editor de texto e cole-o no _Cloud Shell_.
    *   Para Senha, copie e cole o Token de Autenticação no editor de texto (ou onde quer que você o salve).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Navegue de volta para o Container Registry. Na Console, abra o menu de navegação e clique em **Serviços ao Desenvolvedor**. Em **Containers & Artifacts**, clique em **Container Registry**. ![Registro de contêiner](images/container-registry.png)
    
5.  Selecione o compartimento e clique em **Criar Repositório**. ![Criação de Repositório](images/repository-create.png)
    
6.  Selecione o compartimento e informe _`tomcat-example-your_first_name`_ como o Nome do Repositório; em seguida, escolha Acessar como **Público** e clique em **Criar**.
    
    ![Descrição do Repositório](images/describe-repository.png)
    
7.  Para enviar sua imagem do Docker ao seu repositório dentro do Oracle Cloud Container Registry, copie e cole o seguinte comando no seu editor de texto e substitua `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`tomcat-example-your_first_name`:1.0 pelo nome completo da imagem do Docker, que você salvou anteriormente.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/tomcat-example-ankit:v1</copy>
        
    
    O resultado deve ficar como este:
    
        $ docker push lhr.ocir.io/tenancynamespace/tomcat-example-ankit:v1
        The push refers to repository [lhr.ocir.io/tenancynamespace/tomcat-example-ankit]
        4b193f4c616d: Pushed 
        0469528628db: Pushed 
        cce8193c4190: Pushed 
        ca36c0db4673: Pushed 
        0136a6a85859: Pushed 
        98a0db77a14c: Pushed 
        9072514c7af0: Pushed 
        f6146a44a7d3: Pushed 
        0c3170905795: Pushed 
        df64d3292fd6: Pushed 
        v1: digest: sha256:65b562a7117870540f1807e0d796fe964e6428bda0ae290b8a6389bf637d1aba size: 2405
        $ 
        
8.  Depois que o comando _docker push_ for executado com sucesso, expanda o repositório _`tomcat-example-ankit:v1`_ e você observará que foi feito upload de uma nova imagem para esse repositório.
    

Agora você pode **acessar o próximo laboratório**.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023