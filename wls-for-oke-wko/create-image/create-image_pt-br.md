# Criar uma Imagem Auxiliar e Disponibilizá-la no Oracle Container Image Registry

## Introdução

**Imagem Principal** - A imagem que contém o software Oracle Fusion Middleware. Ele é usado como base de todos os contêineres que executam Servidores WebLogic para o domínio.

**Imagem Auxiliar** - A imagem que fornece o software Implantar Ferramentas WebLogic e os arquivos de modelo. No runtime, o conteúdo da imagem auxiliar é mesclado com o conteúdo da imagem principal. ![Estrutura da Imagem](images/image-structure.png)

Neste Laboratório, usamos a imagem 12.2.1.3.0-ol8 do servidor WebLogic como Imagem Primária. Além disso, criamos uma imagem auxiliar e a enviamos para o repositório do Oracle Container Image Registry usando o token de autenticação gerado.

Tempo Estimado: 10 minutos

### Objetivos

Neste laboratório, você vai:

*   Crie uma Imagem Auxiliar e envie a imagem ao Oracle Cloud Container Image Registry.

## Tarefa 1: Preparar Imagem Auxiliar e Enviar a Imagem Auxiliar

Nesta tarefa, estamos criando uma imagem Auxiliar, que enviaremos ao Oracle Cloud Container Registry.

1.  Clique em _Imagem_. Para a Imagem Principal, usaremos o _weblogic_ Image.So abaixo, deixe os valores padrão na seção _Imagem Principal_, conforme mostrado
    
        <copy>container-registry.oracle.com/middleware/weblogic:12.2.1.3-ol8</copy>
        
    
    ![Imagem Principal](images/primary-image.png)
    
    > **Somente para sua informação:**  
    > A imagem principal é a usada para executar o domínio. Uma imagem principal pode ser reutilizada para centenas de domínios. A imagem primária contém as instalações do software OS, JDK e FMW.
    
2.  Para criar a Tag de Imagem Auxiliar, precisamos das seguintes informações:
    
    *   Ponto final da Região
    *   Namespace da Tenancy
3.  Localize o _Ponto Final da sua Região_. Consulte a tabela documentada neste URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). No exemplo mostrado, o ponto final da região é _Sul do Reino Unido (Londres)_ (como o nome da região) e seu ponto final é _lhr.ocir.io_. Localize o ponto final do seu próprio _Nome da Região_ e salve-o no arquivo de texto. Você também precisará dele para o próximo laboratório.
    
    ![Pontos finais](images/end-point.png " ")
    
    > Agora você tem o namespace e o ponto final da tenancy para sua região.
    
4.  No laboratório 3, você já anotou o namespace da tenancy no seu arquivo de texto. Caso contrário, para localizar o Namespace da tenancy, selecione o _Menu do Hambúrguer_ -> _Serviços do Desenvolvedor_ -> _Registro de Contêiner_, conforme mostrado. Selecione o repositório criado. Você encontrará o Namespace conforme mostrado. ![Namespace da Tenancy](images/tenancy-namespace.png)
    
5.  Agora você tem o Namespace da Tenancy e o Ponto Final para sua região. Copie o comando a seguir e cole-o no seu arquivo de texto. Em seguida, substitua `END_POINT_OF_YOUR_REGION` pelo ponto final do nome da sua região, `NAMESPACE_OF_YOUR_TENANCY` pelo namespace da sua tenancy. Clique na guia _Imagem Auxiliar_ conforme mostrado. ![Guia Auxiliar](images/auxiliary-tab.png)
    
        <copy>END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/test-model-your_first_name:v1</copy>
        

> Por exemplo, no meu caso, a tag Imagem Auxiliar é `lhr.ocir.io/tenancynamespace/test-model-ankit:v1`.

6.  Na etapa 4, você também determinou o namespace da tenancy. Informe o Nome do Usuário de Envio do Registro de Imagem Auxiliar da seguinte forma: `NAMESPACE_OF_YOUR_TENANCY`/`YOUR_ORACLE_CLOUD_USERNAME`.  
    

*   Substitua `NAMESPACE_OF_YOUR_TENANCY` pelo namespace da sua tenancy
*   Substitua `YOUR_ORACLE_CLOUD_USERNAME` pelo nome de usuário da sua Conta do Oracle Cloud e copie o nome de usuário substituído do seu arquivo de texto e cole-o no _Nome de Usuário de Envio do Registro de Imagem Auxiliar_.

> Por exemplo, no meu caso, **Nome do Usuário de Envio do Registro de Imagem Auxiliar** é `tenancynamespace/lab.user@oracle.com`.

*   Para Senha, copie e cole o Token de Autenticação do seu arquivo de texto (ou onde quer que você o tenha salvo) e cole-o no **Nome do Usuário de Envio do Registro de Imagem Auxiliar**. ![Detalhes da Imagem Auxiliar](images/auxiliary-image-details.png)

7.  Clique em _Criar Imagem Auxiliar_. ![Criar Imagem Auxiliar](images/create-auxiliary-image.png)
    
8.  Como já preparamos o modelo no Laboratório 2, clique em _Não_. ![Preparar Modelo](images/prepare-model.png)
    
9.  Selecione a pasta _Downloads_ na qual queremos salvar o _WebLogic Deployer_ e clique em _Selecionar_ conforme mostrado. ![Local WDT](images/wdt-location.png)
    
10.  Depois que as imagens Auxiliares forem criadas com sucesso, na janela _Criar Imagem Auxiliar Concluída_, clique em _Ok_. ![Auxiliar Criado](images/auxiliary-created.png)
    
    > **Somente para sua informação:**  
    > Uma imagem auxiliar é específica do domínio. A imagem auxiliar contém os dados que definem o domínio.
    
11.  Clique em _Enviar Imagem Auxiliar_ para enviar a imagem no repositório dentro do Registro de Imagem do Oracle Cloud Container. ![Auxiliar de Envio](images/push-auxiliary.png)
    
12.  Depois que a imagem for enviada com sucesso, na janela _Enviar Imagem Concluída_, clique em _Ok_. ![Auxiliar Submetido](images/auxiliary-pushed.png)
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, outubro de 2023