# Configurar instância de computação

## Introdução

Este laboratório mostrará a você como configurar uma pilha do **Oracle WebLogic Suite for OKE BYOL** que gerará os objetos do Oracle Cloud necessários para executar seu workshop.

Tempo Estimado: 15 minutos

### Objetivos

Neste laboratório, você vai:

*   Gere um token de autenticação.
*   Criar um segredo em um Vault
*   Criar uma Pilha: Oracle WebLogic Suite para OKE BYOL
*   Conecte-se com a instância de computação

### Pré-requisitos

Este laboratório pressupõe que você tenha:

*   Uma conta no Oracle Cloud
*   Você gerou o par de chaves SSH.
*   Você concluiu: **Lab: Preparar Configuração**

## Tarefa 1: Gerar um Token de Autenticação

Nesta tarefa, geraremos um _Token de Autenticação_. No laboratório 5, usaremos esse token de autenticação para enviar imagem auxiliar para o Repositório de Registro do Oracle Cloud Container. Além disso, usamos esse token de autenticação para extrair imagens do docker WebLogic para o Oracle Cloud Infrastructure Registry (também conhecido como Container Registry).

1.  Selecione o Ícone do Usuário no canto superior direito e selecione _MyProfile_.
    
    ![Meu Perfil](images/my-profile.png)
    
2.  Role para baixo e selecione _Tokens de Autenticação_ e clique em _Gerar Token_.
    
    ![Token de autenticação](images/auth-token.png)
    
3.  Copie _`test-model-your_first_name`_ e cole-o na caixa _Descrição_ e clique em _Gerar Token_.
    
    ![Criar Token](images/create-token.png)
    
4.  Selecione _Copiar_ em Token Gerado e cole-o em seu arquivo de texto. Não podemos copiá-lo mais tarde. Clique em _Fechar_.
    
    ![Copiar Token](images/copy-token.png)
    
    > Na próxima tarefa, armazenaremos esse token de autenticação em **Secret**.
    

## Tarefa 2: Criar um segredo em um vault

Segredos são credenciais como senhas, certificados, chaves SSH ou **token de autenticação** que você usa com os serviços do Oracle Cloud Infrastructure.

1.  Na Console do OCI, clique em **Menu do hambúrguer** -> **Identidade e Segurança** -> **Vault**. ![vault de menu](images/menu-vault.png)
    
2.  Selecione o compartimento em **Escopo da lista** e clique em **Criar Vault**.
    
3.  Informe o nome do Vault e clique em **Criar Vault**. ![criar vault](images/create-vault.png)
    
4.  Depois de ver o Vault criado no Estado **Ativo**, clique no nome do Vault conforme mostrado abaixo. ![nome do vault](images/vault-name.png)
    
5.  Dentro do vault, clique em **Chave de Criptografia Principais** e, em seguida, clique em **Criar Chave**. ![mek de menu](images/menu-mek.png)
    
6.  Digite o nome da **chave** e clique em **Criar Chave**, conforme mostrado abaixo. ![criar mek](images/create-mek.png)
    
7.  Depois que você vir o estado da nova chave de Criptografia Principal mudar para **Ativado**, clique em **Segredos** em **Recursos**, conforme mostrado abaixo. ![segredos do menu](images/menu-secret.png)
    
8.  Clique em **Create Secret**.
    
9.  Informe o nome do Segredo e escolha a nova chave de Criptografia e informe o token de autenticação como **Conteúdo do Segredo**, conforme mostrado abaixo. Clique em **Criar Segredo**. ![criar segredo](images/create-secret.png)
    

## Tarefa 3: Criar Pilha: Oracle WebLogic Suite para OKE BYOL

1.  Na Console do OCI, clique em **Menu de hambúrguer** -> **Marketplace** -> **Todos os Aplicativos**. ![Marketplace de menu](images/menu-marketplace.png)
    
2.  Digite **WebLogic OKE** na caixa de pesquisa e clique em **Oracle WebLogic Suite para OKE BYOL**. ![pilha de menus](images/menu-stack.png)
    
3.  Selecione a versão mais recente disponível e escolha seu compartimento e marque a caixa para aceitar os termos e a condição. Clique em **Iniciar Pilha**. ![iniciar pilha](images/launch-stack.png)
    
4.  Na seção Informações da Pilha, deixe tudo padrão e clique em **Próximo**. ![informações da pilha](images/stack-info.png)
    
5.  Digite **wko** como Prefixo de Nome de Recurso e clique em Procurar para escolher a chave Pública SSH. ![prefixo-ssh](images/prefix-ssh.png)
    
    > Certifique-se de usar **wko** como prefixo, pois usaremos isso em outros laboratórios.
    
6.  Na Integração com Verrazzano, deixe a caixa desmarcada padrão para **Ativar Verrazzano**. ![desmarcar verrazzano](images/uncheck-verrazzano.png)
    
7.  Na Rede, Selecione **Criar uma Nova VCN** como Estratégia de Rede Virtual na Nuvem e deixe tudo o padrão. ![rede](images/network.png)
    
8.  Na Configuração do Cluster de Contêineres (OKE), selecione o seguinte.
    
    **Versão do Kubernetes:** - Deixe o padrão **v1.26.2**.
    
    **Forma do Pool de Nós Não WebLogic (Obrigatória)** - Selecione **VM.Standard.E4. Flex** como forma e seleciona **1** como Número de OCPUs e **16** como Quantidade de Memória.
    
    **Nó no NodePool para pods não WebLogic** - Deixe o **1** padrão.
    
    ![não WebLogic](images/non-weblogic.png)
    
    **Criar Forma do Pool de Nós WebLogic** - Deixe a caixa padrão como marcada.
    
    **WebLogic Forma do Pool de Nós (Obrigatório)** - Selecione **VM.Standard.E4. Flex** como forma e seleciona **1** como Número de OCPUs e **16** como Quantidade de Memória.
    
    **Node in the NodePool for WebLogic pods** - Deixe o padrão **1**.
    
    ![WebLogic](images/weblogic-pool.png)
    
9.  Em Instâncias de Administração, informe ou selecione o seguinte, conforme mostrado. **Domínio de Disponibilidade para instâncias de computação** - Selecione na lista drop-down o domínio de disponibilidade.
    
    **Forma de Computação da Instância de Administração (Obrigatória)** - Selecione **VM.Standard.E4. Flex** como forma e seleciona **1** como Número de OCPUs e **16** como Quantidade de Memória.
    
    ![instância de administrador](images/admin-instance.png)
    
    **Forma da Instância do Bastion (Obrigatória)** - Selecione **VM.Standard.E4. Flex** como forma e seleciona **1** como Número de OCPUs e **16** como Quantidade de Memória.
    
    ![bastion](images/bastion.png)
    
10.  Na seção Sistema de arquivos, selecione o seguinte, conforme mostrado. **Domínio de Disponibilidade do Sistema de Arquivos** - Selecione o domínio de disponibilidade na lista drop-down. ![avd do arquivo](images/file-avd.png)
    
11.  Na seção Registro (OCIR), informe o seguinte, conforme mostrado.
    
    **Nome do Usuário do Registro** - O nome do usuário que o Kubernetes usa para acessar o registro de imagem do contêiner, que tem o formato {identity domain name}/{username}. Se sua tenancy estiver usando o Oracle Identity Cloud Service, use o formato oracleidentitycloudservice/{username}.
    
    **Compartimento do Token de Autenticação do OCIR** - Selecione o compartimento no qual você tem o token de autenticação do OCIR.
    
    **Segredo Validado para Token de Autenticação OCIR** - O segredo que contém o token de autenticação OCIR que você gerou para o usuário acessar o registro de imagem.
    
    ![segredo do ocir](images/ocir-secret.png)
    
12.  Nas Políticas do OCI, marque a caixa para **Políticas do OCI**. Ele cria políticas para ler Segredos do Vault e gerenciar o Banco de Dados Autonomous Transaction Processing (se aplicável). Clique em **Próximo**.
    
13.  Na seção Revisão, marque a caixa **Executar aplicação** e clique em **Criar**. ![executar aplicação](images/run-apply.png)
    
    > Isso criará um Job, que criará os recursos necessários na pilha. Não é necessário aguardar. Vá para a próxima tarefa.
    

## Tarefa 4: Acessar a Área de Trabalho Remota Gráfica

Para facilitar a execução deste workshop, sua instância de VM foi pré-configurada com uma área de trabalho gráfica remota acessível usando qualquer navegador moderno em seu laptop ou estação de trabalho. Prossiga conforme detalhado abaixo para fazer log-in.

1.  Abra o menu de hambúrguer no canto superior esquerdo. Clique em **Serviços ao Desenvolvedor** e escolha **Gerenciador de Recursos** > **Pilhas**.
    
2.  Clique no nome da pilha que você criou no laboratório 1. ![clique na pilha](images/click-stack.png)
    
3.  Navegue até a guia **Informações do Aplicativo** e copie o **URL da Área de Trabalho Remota** e cole-o na nova guia do browser. ![url do desktop](images/desktop-url.png)
    
    > Agora você precisa seguir todas as instruções dentro desta área de trabalho remota.
    

Agora você pode ir para o próximo laboratório.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, outubro de 2023