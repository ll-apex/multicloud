# Implantar o Operador WebLogic no Oracle Kubernetes Cluster (OKE)

## Introdução

Neste laboratório, autenticamos a CLI do OCI usando o browser, que criará o arquivo _.OCI/config_. Como usaremos o kubectl para gerenciar o cluster remotamente usando o _Acesso Local_. Ele precisa de um arquivo _kubeconfig_. Este arquivo kubeconfig será gerado usando a CLI do OCI. Em seguida, verificamos a conectividade com o cluster do Kubernetes na interface do usuário do Kit de Ferramentas do Kubernetes WebLogic. Finalmente, instalamos o Operador WebLogic do Kubernetes no cluster do Kubernetes (OKE).

Tempo Estimado: 10 minutos

### Objetivos

Neste laboratório, você vai:

*   Configure o kubectl (CLI do Cluster do Kubernetes) para estabelecer conexão com o Cluster do Kubernetes.
*   Verifique a Conectividade da IU do Kit de Ferramentas do Kubernetes WebLogic com o Cluster do Kubernetes.
*   Instale o Operador WebLogic do Kubernetes no Cluster do Kubernetes.

## Tarefa 1: Configurar o kubectl (CLI do Cluster do Kubernetes) para estabelecer conexão com o Cluster do Oracle Kubernetes

Nesta Tarefa, nós criamos o arquivo de configuração _.oci/config_ e _.kube/config_ no diretório _/home/opc_. Esse arquivo de configuração nos permite acessar o Oracle Kubernetes Cluster (OKE) nesta máquina virtual.

1.  No Firefox, abra a console da nuvem, selecione o **Menu do Hambúrguer** -> **Serviços do Desenvolvedor** -> **Clusters do Kubernetes (OKE)**, conforme mostrado. ![Ícone do OKE](images/oke-icon.png)
    
2.  Clique no nome do cluster criado no laboratório 3. Em seguida, clique em **Acessar Cluster**. ![Cluster de Acesso](images/access-cluster.png)
    
3.  Selecione **Acesso Local** e clique em **Copiar**, conforme mostrado. ![Acesso Local](images/local-access.png)
    
4.  Clique em **Atividades** e selecione o **Terminal**. ![Terminal](images/click-terminal.png)
    
5.  Cole o comando copiado no terminal. Para **Deseja criar um novo arquivo de configuração?**, digite **y** e pressione **Enter**. Para **Deseja criar seu arquivo de configuração fazendo log-in em um browser?**, digite **y** e pressione **Enter**. ![Configuração do OCI](images/oci-config.png)
    
6.  No Navegador Firefox, clique na sua sessão ativa.
    
    > Você verá _Autorização Concluída_ conforme mostrado. ![Autorização Concluída](images/authorization-complete.png)
    
7.  Em **Informe uma frase-senha para sua chave privada**, deixe-a vazia e pressione **Enter**. ![Senha Vazia](images/empty-passphrase.png)
    
8.  Use a tecla de seta superior para executar o comando **oce ce ...** novamente e executá-lo novamente várias vezes, até que você veja a **Nova configuração gravada no arquivo Kubeconfig /home/opc/.kube/config**. ![Criar KubeConfig](images/create-kubeconfig.png)
    
9.  Copie e cole o comando a seguir para alterar as permissões do arquivo **~/.kube/config**.
    
        <copy>chmod 600 ~/.kube/config</copy>
        

## Tarefa 2: Exibir os Recursos de Nuvem do Oracle WebLogic Server para Pilha do OKE

1.  Na Console do OCI, clique no **menu de navegação** e selecione **Serviços do Desenvolvedor**. No grupo Gerenciador de Recursos, clique em **Pilhas**.
    
2.  Selecione o **Compartimento** que contém sua pilha.
    
3.  Clique no nome da pilha.
    
4.  Navegue até a guia **Informações do Aplicativo**. Você pode exibir os recursos criados por sua pilha. Copie o **IP Público da Instância do Bastion** e cole-o no arquivo de texto. ![copiar bastion](images/copy-bastion.png)
    

## Tarefa 3: Ativar a configuração de proxy no terminal

Nesta tarefa, ativamos o **túnel ssh** da área de trabalho remota para nós do Cluster do Oracle Kubernetes.

1.  Abra o terminal, copie e cole o comando a seguir para criar o arquivo de chave privada na área de trabalho remota.
    
        <copy>vi ~/.ssh/opc.key</copy>
        
2.  Pressione **i** para entrar no modo de inserção e cole o conteúdo da chave privada nesse arquivo e pressione escape e, em seguida, digite **:wq** para salvar esse arquivo.
    
3.  Copie e cole o comando a seguir para alterar as permissões do arquivo de chave privada.
    
        <copy>chmod 600 ~/.ssh/opc.key</copy>
        
4.  Copie o comando a seguir e cole-o no terminal e, em seguida, substitua **bastion-public-ip** pelo ip público que você copiou na última tarefa.
    
        <copy>ssh -D 1088 -fCqN -i ~/.ssh/opc.key opc@bastion-public-ip</copy>
        
5.  Abra o arquivo **~/.kube/config** e copie e cole o seguinte nesse arquivo, conforme mostrado abaixo.
    
        <copy>proxy-url: socks5://localhost:1088</copy>
        
    
    ![configuração de proxy](images/proxy-config.png)
    

## Tarefa 4: Verificar Conectividade da IU do Kit de Ferramentas do Kubernetes WebLogic com o Cluster do Oracle Kubernetes

Nesta tarefa, verificamos a conectividade com o _Cluster Oracle Kubernetes (OKE)_ no aplicativo `WebLogic Kubernetes Toolkit UI`.

1.  Volte para a interface do usuário do Kit de Ferramentas do Kubernetes WebLogic, clique em _Atividades_ e selecione a janela da interface do usuário do Kit de Ferramentas do Kubernetes WebLogic.
    
2.  Clique em _Kubernetes_ -> _Configuração do Cliente_ e, em seguida, clique em _Verificar Conectividade_. ![Verificar Conectividade](images/verify-connectivity.png)
    
3.  Depois que você vir a janela _Verificar Sucesso de Conectividade do Cliente Kubernetes_, clique em _Ok_. ![Conexão bem-sucedida](images/successfully-connected.png)
    

## Tarefa 5: Atualizar o Operador WebLogic do Kubernetes para o Cluster do Oracle Kubernetes

Esta seção fornece suporte para instalar o WebLogic Kubernetes Operator (o "operador") no cluster do Kubernetes de destino.

1.  Clique em **Operador WebLogic**. Especifique os detalhes da configuração a seguir e clique em **Atualizar Operador**.
    
    **Namespace do Kubernetes** - O namespace do Kubernetes para o qual o operador será instalado. Digite o valor **wko-operator-ns** conforme mostrado na captura de tela abaixo.
    
    **Conta de Serviço do Kubernetes** - A conta de serviço do Kubernetes para o operador usar ao fazer solicitações de API do Kubernetes. Digite o valor **wko-operator-sa** conforme mostrado na captura de tela abaixo.
    
    **Nome da Release do Helm a Ser Usada para Instalação do Operador** - O nome da release do Helm a ser usado para identificar essa instalação. Digite o vale **wko-weblogic-operator**, conforme mostrado na captura de tela abaixo.
    
    ![WebLogic Operatotr](images/weblogic-operator.png)
    
    > **Somente para sua informação:**  
    > ! Por padrão, o campo _Tag de Imagem a Ser Usada_ do operador é definido como a tag de imagem correspondente à versão mais recente da release do operador no Registro de Contêiner GitHub.  
    > O operador precisa saber quais domínios WebLogic no cluster do Kubernetes ele gerenciará. Ele faz isso no nível de namespace do Kubernetes, de modo que qualquer domínio WebLogic em um namespace do Kubernetes que o operador está configurado para gerenciar será gerenciado pela instância do operador que está sendo instalada.  
    > Para o campo _Estratégia de Seleção de Namespace do Kubernetes_, selecionamos _Seletor de Label_, o que significa que qualquer namespace do Kubernetes com um label _weblogic-operator=enabled_ será gerenciado por esse operador.  
    > ![Imagem do Operador](images/operator-image.png)
    
    > Ativando a Ativação do Binding de Atribuição do Cluster, a instalação do operador criará um ClusterRole e ClusterRoleBinding do Kubernetes que o operador usará para todos os namespaces gerenciados.  
    > Por padrão, a API REST do operador não é exposta fora do cluster do Kubernetes. Para ativar a API REST a ser exposta, você pode ativar _Expor API REST Externamente_. ![Associação de Atribuições](images/role-binding.png)  
    
    > Este painel permite substituir a configuração de registro em log Java do operador, que pode ser útil ao depurar problemas com o operador.  
    > ![Log de Java](images/java-logging.png)  
    
    > Para obter mais informações sobre _WebLogic Imagem do Operador do Kubernetes_, _Estratégia de Seleção de Namespace do Kubernetes_, _WebLogic Bindings de Atribuição do Kubernetes_, _Acesso Externo à API REST_, _Integrações de Terceiros_ e _Registro em Log do Java_, consulte a documentação [WebLogic Operador do Kubernetes](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/).
    
2.  Quando vir _WebLogic Instalação do Operador do Kubernetes Concluída_, clique em _Ok_. ![Operador Instalado](images/operator-installed.png)
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, outubro de 2023