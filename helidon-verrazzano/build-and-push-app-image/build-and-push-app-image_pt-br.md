# Envie a Imagem do Aplicativo Helidon para o Oracle Cloud Container Registry

## Introdução

Neste laboratório, você criará uma imagem do Docker com seu aplicativo Helidon e enviará essa imagem para um repositório dentro do Oracle Cloud Container Registry.

Tempo Estimado: 10 minutos

### Objetivos

*   Crie e empacote seu aplicativo usando o Docker.
*   Gere um Token de Autenticação para fazer log-in no Oracle Cloud Container Registry.
*   Envie a imagem do Docker do aplicativo Helidon para seu repositório do Oracle Cloud Container Registry.

### Pré-requisitos

*   O aplicativo Helidon que você criou no laboratório anterior
*   Docker
*   Conta do Oracle Cloud

## Tarefa 1: Criar a Imagem do Docker do Aplicativo Helidon

Começaremos preparando a imagem do Docker que você usará para implantar na Verrazzano.

Estamos criando uma imagem do Docker, que você fará upload para o Oracle Cloud Container Registry que pertence à sua conta do OCI. Para isso, você precisa criar um nome de imagem que reflita suas coordenadas de registro.

É necessário ter as seguintes informações:

*   Namespace da Tenancy
*   Ponto final da Região

1.  Para localizar o Namespace da tenancy, clique no Ícone _Usuário_ -> _Tenancy_, conforme mostrado. Nas **Definições de armazenamento de objetos**, você encontrará o Namespace. Copie e salve-o em seu arquivo de texto, pois também o usaremos posteriormente.
    
    ![Copiar Tenancynamespace](images/copy-tenancynamespace.png " ")
    
2.  Localize o _Ponto Final da sua Região_. Consulte a tabela documentada neste URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). No exemplo mostrado, o ponto final da região é _Sul do Reino Unido (Londres)_ (como o nome da região) e seu ponto final é _lhr.ocir.io_. Localize o ponto final do seu próprio _Nome da Região_ e salve-o no arquivo de texto. Você também precisará dele para o próximo laboratório.
    
    ![Pontos finais](images/end-point.png " ")
    
    > Agora você tem o namespace e o ponto final da tenancy para sua região.
    
3.  Copie o comando a seguir e cole-o no seu editor de texto. Em seguida, substitua _`ENDPOINT_OF_YOUR_REGION`_ pelo ponto final do nome da sua região, _`NAMESPACE_OF_YOUR_TENANCY`_ pelo namespace da sua tenancy e _`your_first_name`_ pelo nome da sua tenancy.
    
        <copy>docker build -t ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0 .</copy>
        
    
    Quando o comando estiver pronto, execute no Cloud Shell no diretório _`~/quickstart-mp/`_. O build produzirá o seguinte resultado:
    
        $ cd ~/quickstart-mp/
        $ docker build lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0 .
        > docker pull lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        [+] Building 107.5s (19/19) FINISHED                                                                                                            
        => [internal] load build definition from Dockerfile                                                                                       0.1s
        => => transferring dockerfile: 785B                                                                                                       0.1s
        => [internal] load .dockerignore                                                                                                          0.1s
        => => transferring context: 48B                                                                                                           0.0s
        => [internal] load metadata for docker.io/library/openjdk:11-jre-slim                                                                     3.7s
        => [internal] load metadata for docker.io/library/maven:3.6-jdk-11                                                                        2.8s
        => [auth] library/openjdk:pull token for registry-1.docker.io                                                                             0.0s
        => [auth] library/maven:pull token for registry-1.docker.io                                                                               0.0s
        => [stage-1 1/4] FROM docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527      33.3s
        => => resolve docker.io/library/openjdk:11-jre-slim@sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527               0.0s
        => => sha256:eb6b779c0a429efee5eb4bf45a3bad058ea028ac9434a24ff323b1eb73735527 549B / 549B                                                 0.0s
        => => sha256:f3cdb8fd164057f4ef3e60674fca986f3cd7b3081d55875c7ce75b7a214fca6d 1.16kB / 1.16kB                                             0.0s
        => => sha256:9c9e40a31d4fa290f933d76f3b0a4183ba02a7298a309f6bfa892d618e7196ef 7.56kB / 7.56kB                                             0.0s
        => => sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717 31.36MB / 31.36MB                                          18.6s
        => => sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251 1.58MB / 1.58MB                                             1.6s
        => => sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c 211B / 211B                                                 0.7s
        => => sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358 47.13MB / 47.13MB                                          24.4s
        => => extracting sha256:99046ad9247f8a1cbd1048d9099d026191ad9cda63c08aadeb704b7000a51717                                                  7.7s
        => => extracting sha256:e97c10298fea9915576e7ff72c6eca3d7c589aaafd6bc6481a5814c4271d2251                                                  0.3s
        => => extracting sha256:1ad67722a7508cc8ca694c9e75efebc7c37f65d179dc2d6f36a9a1615dc5206c                                                  0.0s
        => => extracting sha256:cbeb29d69a7c2580a75906597d7cd4d59a9eff12b482810794fa01b105155358                                                  5.7s
        => [build 1/7] FROM docker.io/library/maven:3.6-jdk-11@sha256:1d29ccf46ef2a5e64f7de3d79a63f9bcffb4dc56be0ae3daed5ca5542b38aa2d            0.0s
        => [internal] load build context                                                                                                          0.1s
        => => transferring context: 13.99kB                                                                                                       0.1s
        => CACHED [build 2/7] WORKDIR /helidon                                                                                                    0.0s
        => [build 3/7] ADD pom.xml .                                                                                                              0.1s
        => [build 4/7] RUN mvn package -Dmaven.test.skip -Declipselink.weave.skip                                                                91.7s
        => [stage-1 2/4] WORKDIR /helidon                                                                                                         0.8s
        => [build 5/7] ADD src src                                                                                                                0.1s
        => [build 6/7] RUN mvn package -DskipTests                                                                                               10.8s
        => [build 7/7] RUN echo "done!"                                                                                                           0.5s
        => [stage-1 3/4] COPY --from=build /helidon/target/quickstart-mp-ankit.jar ./                                                                   0.1s
        => [stage-1 4/4] COPY --from=build /helidon/target/libs ./libs                                                                            0.1s
        => exporting to image                                                                                                                     0.1s
        => => exporting layers                                                                                                                    0.1s
        => => writing image sha256:587a079ad854fc79e768acda11fc05dd87d37013261249e778e80749798c2837                                               0.0s
        => => naming to lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0                                                                           0.0s
        
4.  Isso cria a imagem do Docker, que você pode verificar no seu repositório local.
    
        $ docker images
        
        REPOSITORY                                                                           TAG                               IMAGE ID       CREATED         SIZE
        lhr.ocir.io/tenancynamespace/quickstart-mp-ankit                                               1.0                               587a079ad854   5 minutes ago   243MB
        
    
    Copie para seu arquivo de texto o nome completo substituído `ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-ankit:1.0` porque você precisará dele posteriormente.
    

## Tarefa 2: Gerar um Token de Autenticação para fazer log-in no Oracle Cloud Container Registry

Nesta etapa, vamos gerar um _Token de Autenticação_, que usaremos para fazer log-in no Oracle Cloud Container Registry.

1.  Selecione o Ícone do Usuário no canto superior direito e selecione _Meu Perfil_.
    
    ![Meu Perfil](images/my-profile.png)
    
2.  Role para baixo e selecione _Tokens de Autenticação_.
    
    ![Token de Autenticação](images/auth-token.png)
    
3.  Clique em _Gerar Token_.
    
    ![Gerar tokens](images/generate-token.png)
    
4.  Copie _`quickstart-mp-your_first_name`_ e cole-o na caixa Descrição e clique em _Gerar Token_.
    
    ![Criar Token](images/create-token.png)
    
5.  Clique em _Copiar_ em Token Gerado e cole-o no editor de texto. Você não pode copiá-lo posteriormente; portanto, certifique-se de ter uma cópia desse token salva. Em seguida, clique em _Fechar_.
    
    ![Copiar token](images/copy-token.png)
    

## Tarefa 3: Enviar a Imagem do Docker do Aplicativo Helidon (quickstart-mp) para seu Repositório de Registro de Contêiner

1.  Na Tarefa 1 deste laboratório, você abriu um URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) e determinou o ponto final do nome da sua Região e o copiou para um editor de texto. Em nosso exemplo, o Nome da Região é Sul do Reino Unido (Londres). Você precisará dessas informações para esta tarefa. ![Ponto final](images/end-point.png)
    
2.  Copie o comando a seguir e cole-o no editor de texto e, em seguida, substitua `ENDPOINT_OF_REGION_NAME` pelo ponto final da sua região.
    
    > Em nosso exemplo, o Nome da Região é _Sul do Reino Unido (Londres)_ e o ponto final é _lhr.ocir.io_. Você precisará de suas informações específicas para esta tarefa.
    
        <copy>docker login ENDPOINT_OF_REGION_NAME</copy>
        
3.  Na etapa anterior, você também determinou o namespace da tenancy. Informe o Nome de Usuário da seguinte forma: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    
    *   Substitua _`NAMESPACE_OF_YOUR_TENANCY`_ pelo namespace da sua tenancy
    *   Substitua _`YOUR_ORACLE_CLOUD_USERNAME`_ pelo nome de usuário da sua Conta do Oracle Cloud e copie o nome de usuário substituído do editor de texto e cole-o no _Cloud Shell_.
    *   Para Senha, copie e cole o Token de Autenticação no editor de texto (ou onde quer que você o salve).
    
        $ docker login lhr.ocir.io
        Username: NAMESPACE_OF_YOUR_TENANCY/YOUR_ORACLE_CLOUD_USERNAME
        Password:
        Login Succeeded
        
4.  Navegue de volta para o Container Registry. Na Console, abra o menu de navegação e clique em **Serviços ao Desenvolvedor**. Em **Containers & Artifacts**, clique em **Container Registry**. ![Registro de contêiner](images/container-registry.png)
    
5.  Selecione o compartimento e clique em **Criar**. ![Criação de Repositório](images/repository-create.png)
    
6.  Selecione o compartimento e informe _`quickstart-mp-your_first_name`_ como o Nome do Repositório; em seguida, escolha Acessar como **Público** e clique em **Criar Repositório**.
    
    ![Descrição do Repositório](images/describe-repository.png)
    
7.  Depois que o repositório _`quickstart-mp-your_first_name`_ tiver sido criado, você poderá verificar na lista de repositórios e suas definições.
    
    ![Verificar Namespace](images/verify-namespace.png)
    
8.  Para enviar sua imagem do Docker ao seu repositório dentro do Oracle Cloud Container Registry, copie e cole o seguinte comando no seu editor de texto e substitua `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`quickstar-mp-your_first_name`:1.0 pelo nome completo da imagem do Docker, que você salvou anteriormente.
    
        <copy>docker push ENDPOINT_OF_YOUR_REGION_NAME/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0</copy>
        
    
    O resultado deve ficar como este:
    
        $ docker push lhr.ocir.io/tenancynamespace/quickstart-mp-ankit:1.0
        The push refers to a repository [lhr.ocir.io/tenancynamespace/quickstart-mp-ankit]
        0795b8384c47: Pushed
        131452972f9d: Pushed
        93c53f2e9519: Pushed
        3b78b65a4be9: Pushed
        e1434e7d0308: Pushed
        17679d5f39bd: Pushed
        300b011056d9: Pushed
        1.0: digest: sha256:355fa56eab185535a58c5038186381b6d02fd8e0bcb534872107fc249f98256a size: 1786
        
9.  Depois que o comando _docker push_ for executado com sucesso, expanda o repositório _`quickstart-mp-your_first_name`_ e você observará que foi feito upload de uma nova imagem para esse repositório.
    
10.  Deixe a página do repositório do _Cloud Shell_ e do Container Registry aberta; você precisará delas para os próximos laboratórios.
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023