# Modificar um Projeto de UI WKT e Criar um arquivo de Modelo

## Introdução

Neste laboratório, exploramos o domínio WebLogic local. Nós navegamos pela console de administração para ver o aplicativo implantado, as origens de dados e os servidores no _test-domain_. Também abrimos a seção _`base_project.wktproj`_ pré-criada, que já tem valores pré-preenchidos para _Definições do Projeto_. Em seguida, criamos o arquivo de modelo, fazendo uma introspecção de um domínio local off-line. Por fim, validamos o modelo e preparamos o modelo a ser implantado no Oracle Kubernetes Cluster (OKE).

Tempo Estimado: 15 minutos

### Objetivos

Neste laboratório, você vai:

*   Explore o domínio _test-domain_ local WebLogic.
*   Abra o projeto WKT base.
*   Introspecção de um domínio local off-line.
*   Valide e prepare o modelo.

### Pré-requisitos

Para executar este laboratório, você deve ter:

*   Acesso ao noVNC Remote Desktop criado no laboratório 2.

## Tarefa 1: Explorar domínio local

Nesta tarefa, navegamos pelos recursos no _test-domain_ local usando a console de Administração WebLogic.

1.  No lado esquerdo, clique em _Ícone de Seta_. ![Área de Transferência](images/clipboard.png)

> **Importante**\- Você pode ver o _Clipboard_, para copiar e colar entre a máquina host e a área de trabalho remota, usamos o _Clipboard_. Por exemplo, se quiser copiar da máquina host e quiser colá-la dentro da área de trabalho remota, primeiro cole-a na área de transferência, primeiro você poderá colá-la na área de trabalho remota. Novamente, clique em _Ícone de Seta_ para ocultar a opção _Configurações_.

2.  Clique na guia _Oracle WebLogic Server_ e informe _weblogic/Welcome1%_ como `Username/Password`; em seguida, clique em _Log-in_. Você pode ver que temos o Servidor WebLogic versão _12.2.1.3.0_.  
    ![Console de Admin de Log-in](images/login-admin-console.png)
    
3.  Para exibir servidores disponíveis, expanda _Ambiente_ e clique em _Servidores_. Você pode ver, temos um cluster dinâmico com 5 servidores gerenciados. ![Exibir Servidores](images/view-servers.png)
    
4.  Para exibir as origens de dados, expanda _Serviços_ e clique em _Origens de Dados_. ![Exibir Origens de Dados](images/view-datasources.png)
    
5.  Para exibir o aplicativo implantado, clique em _Implantação_. Você pode ver, temos _opdemo_ como aplicativo implantado. ![Exibir Implementações](images/view-deployments.png)
    

## Tarefa 2: Abrindo o Projeto base da UI WKT

Para simplificar o laboratório, criamos _`base_project.wktproj`_, que predefiniu a localização do docker, Java, Oracle Home, Tag de Imagem Principal. Nesta tarefa, abrimos o projeto _`base_project.wktproj`_.

1.  Clique em _Atividades_ e digite **WebLogic** na caixa de pesquisa. Clique no ícone da _WebLogic IU do Kit de Ferramentas do Kubernetes_. ![Abrir WKTUI](images/open-wktui.png)
    
2.  Para abrir o projeto _base\_project.wktproj_, clique em _File_ -> _Open Project_. ![Abrir Projeto](images/open-project.png)
    
3.  Clique em _Downloads_ no lado esquerdo, escolha _base\_project.wktproj_ e clique em _Abrir Projeto_. ![Localização do Projeto](images/project-location.png)
    
    > **For your information only:**  
    > As _Credential Story Policy_, we select **Store in Native OS Credentials Store**. It means the credentials (like for WebLogic Server and datasources) are only stored on the local machine.  
    > For _Where would you like the target Oracle Fusion Middleware domain to live?_, we select **Created in the container from the model in the image**. In this case, the set of model-related files are added to the image. So when the WebLogic Kubernetes Operator domain object is deployed, its inspector process runs and creates the WebLogic Server domain inside a running container on-the-fly.  
    > ![Definições do Projeto](images/project-settings.png) As _Kubernetes Environment Target Type_, we select **WebLogic Kubernetes Operator**. This means, you want this domain to be deployed in Kubernetes managed by the WebLogic Kubernetes Operator. This settings also determine what sections and their associated actions within the application, to display.  
    > we also specify the location for _JAVA HOME_ and _ORACLE\_HOME_. WebLogic Kubernetes Toolkit UI uses this directory when invoking the WebLogic Deployer Tooling and WebLogic Image Tool.  
    > To build new images, inspect images and interact with image repositories, the WKT UI application uses an image build tool, which defaults to docker.  
    > ![Tipo de Cluster Kubernetes](images/kubernetes-cluster-type.png)
    
4.  Digite _welcome1_ como **Senha** e clique em _Desbloquear_. ![destravar](images/unlock.png)
    

## Tarefa 3: Introspecção de um domínio off-line no local

Nessa tarefa, executamos a introspecção de um domínio local, que cria um arquivo de modelo que consiste na configuração do domínio.

1.  Na interface do usuário do Kit de Ferramentas do Kubernetes WebLogic, clique em _Modelo_. ![Modelo](images/click-model.png)
    
2.  Clique em _Arquivo_ -> _Adicionar Modelo_ -> _Descobrir Modelo (off-line)_. ![Descobrir Modelo](images/discover-model.png)
    
3.  Clique em _ícone_ Abrir pasta para abrir _Home do Domínio_. ![Abrir Hom do Domínio](images/open-domain-home.png)
    
4.  Na pasta Home, navegue até o diretório _`/home/opc/Oracle/Middleware/Oracle_Home/user_projects/domains/`_ e selecione a pasta _test-domain_ e clique em _Selecionar_. Clique em _OK_. ![Local de Navegação](images/navigate-location.png) ![Especificar Local](images/specify-location.png)
    
    > Se você olhar na console, verá que isso chama a Ferramenta do Implantador WebLogic para introspecionar a configuração do domínio no modo off-line.
    
5.  Você pode ver a janela conforme mostrado abaixo. No final, você terá o modelo pronto para você. ![Exibir Modelo](images/view-model.png)
    
    > O resultado dessa introspecção WDT é um modelo (uma representação de metadados da sua configuração de domínio), um espaço reservado, no qual você pode especificar os valores (como senha para origem de dados) e o aplicativo no arquivo compactado do aplicativo.
    

## Tarefa 4: Validar e Preparar Modelo

Nesta tarefa, validamos o modelo e preparamos o modelo a ser implantado no Oracle Kubernetes Cluster (OKE).

1.  Clique em _View de Código_ e, para validar o modelo, clique em _Validar Modelo_. ![Validar Modelo](images/validate-model.png)
    
    > **Somente para suas informações:**  
    > O modelo de validação chama a [Ferramenta de Validação de Modelo](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/validate/) WDT, que valida se o modelo e seus artefatos relacionados estão bem formados e fornece ajuda nos atributos e subpasta válidos para um local de modelo específico.
    
2.  Depois de ver a janela _Validar Modelo Completo_, clique em _Ok_. ![Validação Concluída](images/validate-complete.png)
    
3.  Para preparar o modelo, a ser implantado no cluster do Kubernetes, clique em _Preparar Modelo_ ![Preparar Modelo](images/prepare-model.png)
    
    > **Somente para suas informações:**  
    > O modelo de preparação chama a [Ferramenta de Preparação de Modelo](https://oracle.github.io/weblogic-deploy-tooling/userguide/tools/prepare/) WDT para modificar o modelo para funcionar em um cluster do Kubernetes com o Operador Kubernetes WebLogic ou Verrazzano instalado.  
    > A opção Preparar Modelo faz o seguinte:
    
    *   Remove seções de modelo e campos que não são compatíveis com o ambiente de destino.
    *   Substitui valores de ponto final por tokens de modelo que fazem referência a variáveis.
    *   Substitui valores de credenciais por tokens de modelo que fazem referência a um campo em um segredo do Kubernetes ou a uma variável.
    *   Fornece valores padrão para campos exibidos na variável do aplicativo, substituições de variável e editores secretos.
    *   Extrai informações de topologia para o aplicativo que ele usa para gerar o arquivo de recursos usado para implantar o domínio.
4.  Depois de ver a janela _Preparar Modelo Concluído_, clique em _Ok_. ![Preparação Concluída](images/prepare-complete.png)
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, outubro de 2023