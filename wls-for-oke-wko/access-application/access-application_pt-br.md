# Testar a Implantação do Aplicativo

## Introdução

### Sobre a Console Remota do WebLogic

A Console Remota do WebLogic é uma console leve e de código-fonte aberto que você pode usar para gerenciar seu domínio do Servidor WebLogic em execução em qualquer lugar, como em uma máquina física ou virtual, em um contêiner, Kubernetes ou no Oracle Cloud. A Console Remota WebLogic não precisa ser co-localizada com o domínio do Servidor WebLogic.

Você pode instalar e executar a Console Remota WebLogic em qualquer lugar e se conectar ao seu domínio usando APIs REST WebLogic. Você simplesmente inicia o aplicativo de desktop e se conecta ao Servidor de Administração do seu domínio. Ou você pode iniciar o servidor de console, iniciar a console em um browser e, em seguida, estabelecer conexão com o Servidor de Administração.

A Console Remota do WebLogic é totalmente suportada com o Servidor WebLogic 12.2.1.3, 12.2.1.4 e 14.1.1.0.

**Principais Recursos da Console Remota WebLogic**

*   Configurar instâncias e clusters do WebLogic Server
*   Criar ou modificar modelos de metadados WDT
*   Configurar serviços do Servidor WebLogic, como conectividade do banco de dados (JDBC) e mensagens (JMS)
*   Implantar e cancelar a implantação de aplicativos
*   Iniciar e parar servidores e aplicativos
*   Monitorar desempenho do servidor e da aplicação

Neste laboratório, acessamos o aplicativo _opdemo_ e verificamos a migração bem-sucedida de um domínio local off-line. Também verificamos o balanceamento de carga entre pods de servidor gerenciado. Posteriormente, usamos a Console Remota WebLogic para verificar a implantação bem-sucedida de recursos do domínio de teste no ambiente kubernetes.

Tempo Estimado: 10 minutos

### Objetivos

Neste laboratório, você vai:

*   Acesse o Aplicativo por meio do Browser.
*   Explore o Domínio WebLogic usando a Console Remota WebLogic.

## Tarefa 1: Acessar o Aplicativo por meio do Browser

Nesta tarefa, acessamos o aplicativo _opdemo_. Clicamos no ícone de atualização para fazer várias solicitações ao aplicativo, para verificar o balanceamento de carga entre dois pods de servidor gerenciado.

1.  Copie o URL abaixo e substitua _XX.XX.XX.XX_ pelo seu IP, que você anotou no último laboratório. Você pode ver a saída abaixo.
    
        <copy>http://XX.XX.XX.XX/opdemo/?dsname=testDatasource</copy>
        
    
    ![Abrir Aplicativo](images/open-application.png)
    
2.  Se você clicar no ícone Atualizar, poderá ver o balanceamento de carga entre dois pods de servidor gerenciado. ![Mostrar Balanceamento de Carga](images/show-load-balancing.png)
    

## Tarefa 2: Explorar Domínio WebLogic no Cluster Kubernetes usando a Console Remota WebLogic

Nesta tarefa, exploramos a Console Remota WebLogic. Criamos conexão com o _Servidor Admin_ na Console Remota e verificamos os recursos no Domínio WebLogic. Isso verifica a migração bem-sucedida de um domínio local para o Cluster do Oracle Kubernetes.

1.  To open WebLogic Remote Console, Click on _Activities_, type _WebLogic_ in search box and click on the _WebLogic Remote Console_ Icon.
    
2.  Clique em `Three dots` em _Kiosk_ e selecione _Adicionar Provedor de Conexão do Servidor Admin_ e clique em _Escolher_. ![Conexão do Servidor de Administração](images/adminserver-connection.png)
    
3.  Informe os dados a seguir e clique em _OK_.  
    Nome do Provedor de Conexão: AdminServer  
    Nome do Usuário: weblogic  
    Senha: welcome1  
    URL: `Copy_IP_From_TextFile`  
    ![Detalhes da Conexão](images/connection-details.png)
    
4.  Clique no ícone _Editar Árvore_ e selecione _Serviços_ -> _Origens de Dados_. Você pode observar o mesmo Datasouce, que vimos no domínio local. ![Verificar Origens de Dados](images/verify-datasources.png)
    
5.  Para exibir quais servidores estão sendo executados no seu domínio. Clique no Ícone **Árvore de Monitoramento**, conforme mostrado, selecione **Ambiente** -> **Servidores**. Você pode ver que os pods **Admin Server** e 2 Managed Server estão em execução. ![Servidores em Execução](images/running-server-status.png)
    
6.  Clique em **admin-server**. Você pode ver a Versão WebLogic como **12.2.1.3.0**. ![Status do Servidor](images/wls-version.png)
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, outubro de 2023