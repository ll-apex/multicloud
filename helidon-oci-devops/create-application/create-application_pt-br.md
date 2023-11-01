# Gerar Aplicativo MP Helidon

## Introdução

Este laboratório o orienta pelas etapas para criar um aplicativo **Helidon MP** usando a **CLI Helidon**. Além disso, você modificará o aplicativo Helidon para mostrar sua integração com os serviços **OCI Logging and Monitoring**.

Tempo estimado: 15 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Gerar aplicativo Helidon MP](videohub:1_i4j33ed4)

### Objetivos

Neste laboratório, você vai:

*   Fazer Download da CLI do Helidon
*   Gerar aplicativo MP Helidon usando a CLI Helidon
*   Executar integração com o serviço OCI Logging and Metrics
*   Enviar o código do aplicativo para o repositório de Código do OCI
*   Acione o pipeline DevOps

### Pré-requisitos

*   Uma Conta do Oracle Free Tier(Trial), Paga ou LiveLabs Cloud
*   Familiaridade com comandos git

## Tarefa 1: Fazer Download da CLI Helidon

1.  Abra um novo terminal no **Editor de Código do OCI** e cole o comando a seguir para navegar até a pasta home.
    
        <copy>cd ~</copy>
        
2.  Copie e cole o comando a seguir para fazer download da **CLI do Helidon** e altere as **permissões** para torná-la executável.
    
        <copy>curl -L -O https://helidon.io/cli/latest/linux/helidon
        chmod +x ./helidon</copy>
        

## Tarefa 2: Gerar um aplicativo Helidon Microprofile usando a CLI Helidon

1.  Execute a CLI para **gerar um projeto de aplicativo Helidon Microprofile**.
    
        <copy>./helidon init</copy>
        
2.  Quando solicitar a _versão do Helidon_, digite _4_ para ver todas as versões e, em seguida, digite _35_ para selecionar a versão _4.0.0-ALPHA6_, conforme mostrado abaixo.
    
        Helidon versions
        (1) 3.2.2 
        (2) 2.6.2 
        (3) 4.0.0-M1 
        (4) Show all versions 
        Enter selection (default: 1): 4
        -----------------------------
        -----------------------------
        (33) 2.0.1 
        (34) 2.0.0 
        (35) 4.0.0-ALPHA6 
        (36) 4.0.0-M1 
        Enter selection (default: 1): 35 
        
3.  Quando solicitado a _Selecionar um Sabor_, copie e cole o valor abaixo no terminal.
    
            | Helidon Flavor
        
            Select a Flavor
            (1) se   | Helidon SE
            (2) mp   | Helidon MP
            (3) nima | Helidon Níma
            Enter selection (default: 1): <copy>2</copy>
        
4.  Quando solicitado a _Selecionar um Tipo de Aplicativo_, copie e cole o valor abaixo no terminal.
    
            Select an Application Type
        (1) quickstart | Quickstart
        (2) database   | Database
        (3) custom     | Custom
        (4) oci        | OCI
        Enter selection (default:1):<copy>4</copy>
        
5.  Quando solicitado para _Project groupId_, _Project artifactId_ e _Project version_, basta **aceitar os valores padrão**.
    
6.  Quando solicitado para o _nome do pacote Java_, copie e cole o valor abaixo no terminal.
    
        Java package name (default: me.username.mp.oci): <copy>ocw.hol.mp.oci</copy>
        
7.  Quando solicitado para _Iniciar loop de desenvolvimento? (padrão: n):_, pressione _Enter_ para selecionar o valor padrão.
    
    > Uma vez concluído, isso gerará um projeto **oci-mp**.
    

## Tarefa 3: Modificar o aplicativo Helidon para registro em log e explorador de métricas

1.  Para abrir o projeto _oci-mp_ no **Editor de Códigos**, clique em _Arquivo_ -> _Abrir_. ![abrir projeto](images/open-project.png)
    
2.  Clique na _Seta para Cima_ para navegar até a pasta pai e selecione a pasta _oci-mp_ e clique em _Abrir_. ![abrir oci mp](images/open-ocimp.png)
    
    > Isso abrirá o aplicativo _oci-mp_ no Explorer.
    
3.  Abra um novo terminal, Copie e cole o comando a seguir para **copiar as especificações do pipeline de build e implantação** na pasta _`devops_helidon_to_instance_ocw_hol`_.
    
        <copy>cd ~/oci-mp/
        cp ~/devops_helidon_to_instance_ocw_hol/pipeline_specs/* .</copy>
        
4.  Adicione **.gitignore** para que os arquivos e diretórios que não forem necessários para fazer parte do repositório sejam ignorados pelo git.
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/.gitignore .</copy>
        
5.  Copie e cole o comando a seguir para executar o script do utilitário na pasta principal _`devops_helidon_to_instance_ocw_hol`_ para atualizar os parâmetros **config**. quando ele solicitar **Informe o diretório raiz do projeto Helidon MP**, pressione Enter para selecionar o valor **default**.
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/update_config_values.sh</copy>
        
    
    Você terá uma saída semelhante à mostrada a seguir: ![configuração de atualização](images/update-config.png)
    
    > **Por favor, leia:-**
    
    *   Chamar este script executará o seguinte:
    *   Atualizações no arquivo de configuração _~/oci-mp/server/src/main/resources/application.yaml_ para configurar um recurso Helidon que envia métricas geradas pelo Helidon para o serviço de monitoramento do OCI.
        *   **compartmentId** - Octid de compartimento usado para esta demonstração
        *   **namespace** - Pode ser qualquer string, mas para esta demonstração, isso será definido como helidon\_metrics.
    *   Atualizações no arquivo de configuração _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ para configurar parâmetros de configuração usados pelo código do aplicativo Helidon para executar a integração com o serviço OCI Logging and Metrics.
        *   **oci.monitoring.compartmentId** - Octid de compartimento usado para esta demonstração
        *   **oci.monitoring.namespace** - Pode ser qualquer string, mas para esta demonstração, ela será definida como helidon\_application.
        *   **oci.logging.id** - ID de log do aplicativo provisionado pelos scripts do Terraform.
    *   Atualize no arquivo de configuração _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ para configurar a propriedade _oci.bucket.name_ a fim de conter o nome do bucket do Object Storage que foi provisionado pelos scripts terraform que serão usados em um exercício posterior para demonstrar suporte do Object Storage de um aplicativo Helidon.
6.  Abra o arquivo _~/oci-mp/server/src/main/resources/application.yaml_ no Code editor para verificar se o valor de **compartmentId** e **namespace** foi atualizado. ![yaml do aplicativo](images/application-yaml.png)
    
7.  Abra o arquivo _~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties_ no Code editor para verificar se o valor de **oci.monitoring.compartmentId**, **oci.monitoring.namespace**, **oci.logging.id** e **oci.bucket.name** foi atualizado. _oci.bucket.name_ será usado posteriormente no Laboratório 5.
    

## Tarefa 4: Gerar um Token de Autenticação para enviar o código ao repositório de Código do OCI

Nesta etapa, vamos gerar um _Token de Autenticação_, que usaremos para enviar o código do aplicativo Helidon ao _repositório do OCI Code_.

1.  Selecione o _Ícone do Usuário_ no canto superior direito e selecione _Meu Perfil_. ![ícone usuário](images/user-icon.png)
    
2.  Role para baixo e selecione _Tokens de Autenticação_. ![tokens de autenticação](images/auth-tokens.png)
    
3.  Clique em _Gerar Token_. ![gerar token](images/generate-token.png)
    
4.  Copie _oci-mp_ e cole-o na caixa Descrição e clique em _Gerar Token_. ![descrição do token](images/token-description.png)
    
5.  Selecione **Copiar** em Token Gerado e cole/salve-o em um arquivo de texto usando um editor de sua escolha. Lembre-se de que o token não pode ser recuperado posteriormente; portanto, é importante manter uma cópia disso agora. Quando terminar, clique em **Fechar**. ![copiar token](images/copy-token.png)
    

## Tarefa 5: Sincronize o projeto do aplicativo Helidon com o repositório do OCI Code em seu projeto DevOps

1.  Abra um novo terminal, Copie e cole o comando a seguir para navegar até o diretório _oci-mp_.
    
        <copy>cd ~/oci-mp</copy>
        
2.  Inicialize o diretório de projeto _oci-mp_ para se tornar um **repositório git**.
    
        <copy>git init</copy>
        
3.  Defina a ramificação como **principal** para corresponder à ramificação remota correspondente.
    
        <copy>git checkout -b main</copy>
        
4.  Defina o **repositório remoto**. Use o url https do Repositório de Código do OCI exibido na última saída do terraform ou use a ferramenta get.sh de `devops_helidon_to_instance_ocw_hol` para recuperar esse valor.
    
        <copy>git remote add origin $(~/devops_helidon_to_instance_ocw_hol/main/get.sh code_repo_https_url)
        git remote -v</copy>
        
    
    > **git remote -v** é verificar se a origem foi definida.
    
5.  Configure o git para usar o armazenamento **auxiliar de credenciais** para que o nome de usuário e a senha do repositório do OCI sejam informados apenas uma vez nos comandos git que os exigem. Além disso, defina **user.name** e **user.email**, que é exigido pelo commit git.
    
        <copy>git config credential.helper store
        git config --global user.email "my.name@example.com"
        git config --global user.name "FIRST_NAME LAST_NAME"</copy>
        
6.  Sincronize o log git do repositório do oci com o repositório local usando **git pull**.
    
        <copy>git pull origin main</copy>
        
7.  Será solicitado um nome de usuário e uma senha. Use o **nome da tenancy**/**nome do usuário** para o nome do usuário e o **token de autenticação** do OCI que foi gerado na **tarefa 4** para a senha. ![nome de usuário git](images/git-username.png) ![senha do git](images/git-password.png)
    
8.  Você terá uma saída semelhante à seguinte. Caso contrário, verifique se executou cada comando git corretamente. ![sincronização de extração de git](images/git-pull-sync.png)
    

## Tarefa 6: Pressione o código do aplicativo Helidon e acione o pipeline DevOps

1.  **Prepare** todos os arquivos para o commit git.
    
        <copy>git add .
        git status</copy>
        
    
    > O status git irá gerar todos os arquivos no repositório.
    
2.  Execute o primeiro **commit**.
    
        <copy>git commit -m "Helidon oci-mp first commit"</copy>
        
3.  **Enviar** as alterações para o repositório remoto.
    
        <copy>git push -u origin main</copy>
        
    
    > Isso acionará DevOps para iniciar o pipeline de build.
    
4.  Abra a [Console da Nuvem](https://cloud.oracle.com/) na nova guia, clique em _menu Hambúrguer_ -> _Serviços do Desenvolvedor_ -> _Projetos_ em **DevOps**. ![projeto de devops](images/devops-project.png)
    
5.  Selecione o compartimento, que você criou no **Lab 1** e clique em _devops-project-helidon-ocw-hol-string_ para abrir o **DevOps Project**. ![selecionar compartimento](images/select-compartment.png)
    
    > Atualize o browser, se você não vir o novo compartimento.
    
6.  Em _Histórico de builds mais recente_, você verá _Execuções_ e Status como _Aceito/Em Andamento_. Clique nas execuções mais recentes conforme mostrado abaixo. ![histórico de builds](images/build-history.png)
    
7.  Assim que o pipeline de build concluir todos os três estágios, você verá a saída conforme mostrado abaixo. Você pode clicar na seta logo antes dos estágios, para exibir qual ação eles estão executando. Esta ação, definimos no arquivo _`build_spec.yaml`_ na pasta _oci-mp_. ![execução de build primeiro](images/build-run-first.png)
    
8.  No andamento da execução do Build, no terceiro estágio, clique em **Três pontos** e, em seguida, clique em **Exibir implantação**, conforme mostrado abaixo. Isso abrirá o **pipeline de implantação**. ![exibir implantação](images/view-deployment.png)
    
9.  Aqui você pode ver _Progresso da Implantação_. Depois que o pipeline de implantação for concluído, você verá a saída conforme mostrado abaixo. Você pode clicar na seta logo antes do estágio, para exibir qual ação eles estão executando. Esta ação, definimos no arquivo _`deployment_spec.yaml`_ na pasta _oci-mp_. ![execução de implantação](images/deployment-run.png)
    
    > Isso implanta com sucesso o aplicativo Helidon em **instâncias do Compute** no OCI.
    
10.  Para exibir os logs do pipeline de implantação, clique em **Três pontos** próximo ao estágio de implantação e clique em **Exibir detalhes**, conforme mostrado abaixo. ![exibir logs](images/view-logs.png)
    
11.  Role os logs para baixo e verifique se a sabor do JDK é **Abrir JDK**. Observe também os logs que o aplicativo Helidon utiliza o novo recurso **Threads Virtuais** no Java, conforme mostrado abaixo. ![jdk aberto](images/open-jdk.png)
    
    > Como parte do **Lab 4**, substituiremos o **Open JDK** pelo **Oracle JDK**.
    

Agora você pode **acessar o próximo laboratório.**

## Saiba Mais

*   [CLI do Helidon](https://helidon.io/docs/v3/#/about/cli)
*   [Guia de Início Rápido do Helidon MP](https://helidon.io/docs/v3/#/mp/guides/quickstart)
*   [Origens de Configuração do MP do Helidon](https://helidon.io/docs/v3/#/mp/config/advanced-configuration)
*   [Origens de Configuração do MP do Helidon](https://helidon.io/docs/v3/#/mp/guides/config)

## Confirmação

*   **Autor** - Keith Lustria
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023