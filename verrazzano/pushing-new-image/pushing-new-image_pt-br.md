# Envie a Imagem do Docker para o aplicativo bobbys-helidon-stock-application ao Oracle Cloud Container Registry

## Introdução

No Laboratório 5, modificamos o aplicativo bobbys-helidon-stock e criamos uma nova imagem do Docker. Neste Laboratório, enviaremos essa imagem para um repositório dentro do Oracle Cloud Container Registry.

Tempo estimado: 10 minutos

### Objetivos

Neste laboratório, você vai:

*   Gere um Token de Autenticação para fazer log-in no Oracle Cloud Container Registry.
*   Envie a imagem do Docker da aplicação bobbys-helidon-stock para o repositório do Oracle Cloud Container Registry.

### Pré-requisitos

Você deve ter um editor de texto, no qual pode colar os comandos e URLs e modificá-los, conforme seu ambiente. Em seguida, você pode copiar e colar o comando modificado para executá-los no _Cloud Shell_.

## Tarefa 1: Gerar um Token de Autenticação para Fazer Log-in no Oracle Cloud Container Registry

Nesta etapa, vamos gerar um _Token de Autenticação_, que usaremos para fazer log-in no Oracle Cloud Container Registry.

1.  Selecione o Ícone do Usuário no canto superior direito e selecione _Meu Perfil_.
    
    ![Definições de Usuário](images/user-settings.png " ")
    
2.  Role para baixo e selecione _Tokens de Autenticação_.
    
    ![Tokens de autenticação](images/auth-token.png " ")
    
3.  Clique em _Gerar Token_.
    
    ![Gerar Token](images/generate-token.png " ")
    
4.  Copie _`helidon-stock-application-your_first_name`_ e cole-o na caixa _Descrição_ e clique em _Gerar Token_.
    
    ![Criação de token](images/token-create.png " ")
    
5.  Selecione _Copiar_ em Token Gerado e cole-o no arquivo de texto. Não podemos copiá-lo mais tarde.
    
    ![Copiar Token](images/copy-token.png " ")
    

## Tarefa 2: Envie a imagem do Docker de aplicação bobbys-helidon-stock para seu Repositório de Registro do Oracle Cloud Container

1.  Na Tarefa 1 deste laboratório, você abriu um URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab) e determinou o ponto final do nome da sua Região e o copiou para um editor de texto. Em nosso exemplo, o Nome da Região é Sul do Reino Unido (Londres). Você precisará dessas informações para esta tarefa. ![Ponto final](images/end-point.png)
    
2.  Copie o comando a seguir e cole-o no editor de texto e substitua **`END_POINT_OF_REGION_NAME`** pelo ponto final da sua região.
    
        <copy> docker login `END_POINT_OF_REGION_NAME`</copy>
        

3\. No laboratório anterior, você determinou o Namespace da Tenancy. Faça o seguinte nome de usuário: \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`/oracleidentitycloudservice/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\*. Aqui, substitua \*\*\`NAMESPACE\_OF\_YOUR\_TENANCY\`\*\* pelo namespace da sua tenancy e \*\*\`YOUR\_ORACLE\_CLOUD\_USERNAME\`\*\* pelo seu nome de usuário da Conta do Oracle Cloud e copie o nome de usuário substituído do seu arquivo de texto e cole-o no \*Cloud Shell\*. Para Senha , cole o Token de Autenticação no editor de texto ou onde você o salvou. !\[Login Registry\](images/login-registry.png " ") 3\. No laboratório anterior, você determinou o Namespace da Tenancy. Faça o nome de usuário da seguinte forma: \`NAMESPACE\_OF\_YOUR\_TENANCY\`/\`YOUR\_ORACLE\_CLOUD\_USERNAME\`. Você precisa usar o namespace e o nome de usuário da tenancy em letras minúsculas ao executar o log-in do docker. Aqui, substitua \`NAMESPACE\_OF\_YOUR\_TENANCY\` pelo namespace da sua tenancy e \`YOUR\_ORACLE\_CLOUD\_USERNAME\` pelo seu nome de usuário da Conta do Oracle Cloud e copie o nome de usuário substituído do editor de texto e cole-o no \*Cloud Shell\*. Para Senha , cole o Token de Autenticação no editor de texto ou onde você o salvou.

4.  Volte para o Registro de Contêiner, selecione _Menu do Hambúrguer -> Serviços do Desenvolvedor -> Registro de Contêiner_.
    
    ![Registro de contêiner](images/container-registry.png " ")
    
5.  Selecione o compartimento e clique em _Criar Repositório_.
    
    ![Criação de Repositório](images/repository-create.png " ")
    
6.  Selecione o compartimento e informe _`helidon-stock-application-your_first_name`_ como o Nome do Repositório; em seguida, escolha Acessar como _Público_ e clique em _Criar_.
    
    ![Dados do Repositório](images/repository-data.png " ")
    
    Depois que o repositório _`helidon-stock-application-your_first_name`_ tiver sido criado, você poderá verificar o Namespace e ele deverá ser igual ao namespace da tenancy.
    
    !\[Verificar Namespace\](imagens/verificar-namespace.png" ")
    
7.  Como parte do Laboratório 5, você copiou o nome completo da imagem do Docker no editor de texto. Para enviar sua imagem do Docker ao seu repositório dentro do Oracle Cloud Container Registry, copie e cole o seguinte comando no seu editor de texto e substitua _`ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/`helidon-stock-application-your_first_name`:1.0_ pelo nome completo da imagem do Docker, que você salvou no seu editor de texto.
    
        <copy>docker push `ENDPOINT_OF_YOUR_REGION_NAME`/`NAMESPACE_OF_YOUR_TENANCY`/helidon-stock-application-your_first_name:1.0</copy>
        
    
    ![Push do Docker](images/docker-push.png " ")
    
    Depois que o comando _docker push_ for executado com sucesso, expanda o repositório _`helidon-stock-application-your_first_name`_ e você observará que foi feito upload de uma nova imagem neste repositório.
    
    Deixe a página do repositório do _Cloud Shell_ e do Container Registry aberta; precisaremos deles para os próximos laboratórios.
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, março de 2023