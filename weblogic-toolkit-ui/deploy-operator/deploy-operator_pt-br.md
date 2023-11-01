# Implantar o Operador WebLogic no Oracle Kubernetes Cluster (OKE)

## Introdução

Neste laboratório, autenticamos a CLI do OCI usando o browser, que criará o arquivo _.OCI/config_. Como usaremos o kubectl para gerenciar o cluster remotamente usando o _Acesso Local_. Ele precisa de um arquivo _kubeconfig_. Este arquivo kubeconfig será gerado usando a CLI do OCI. Em seguida, verificamos a conectividade com o cluster do Kubernetes na interface do usuário do Kit de Ferramentas do Kubernetes WebLogic. Finalmente, instalamos o Operador WebLogic do Kubernetes no cluster do Kubernetes (OKE).

Tempo Estimado: 10 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Implantar o Operador WebLogic no Cluster do OKE](videohub:1_0itbllhe)

### Objetivos

Neste laboratório, você vai:

*   Configure o kubectl (CLI do Cluster do Kubernetes) para estabelecer conexão com o Cluster do Kubernetes.
*   Verifique a Conectividade da IU do Kit de Ferramentas do Kubernetes WebLogic com o Cluster do Kubernetes.
*   Instale o Operador WebLogic do Kubernetes no Cluster do Kubernetes.

## Tarefa 1: Configurar o kubectl (CLI do Cluster do Kubernetes) para estabelecer conexão com o Cluster do Oracle Kubernetes

Nesta Tarefa, nós criamos o arquivo de configuração _.oci/config_ e _.kube/config_ no diretório _/home/opc_. Esse arquivo de configuração nos permite acessar o Oracle Kubernetes Cluster (OKE) nesta máquina virtual.

1.  Clique em _Atividades_ e digite _Firefox_ na caixa de pesquisa. Clique no ícone do _Firefox_. ![open firefox](images/open-firefox.png)
    
2.  Abra o url [https://cloud.oracle.com](https://cloud.oracle.com). Informe seu _Nome da Conta do Cloud_ e, em seguida, sua credencial para a Conta do Oracle Cloud e clique em _Acessar_.
    
3.  Na Console, selecione o _Menu do Hambúrguer_ -> _Serviços do Desenvolvedor_ -> _Clusters do Kubernetes (OKE)_, conforme mostrado. ![Ícone do OKE](images/oke-icon.png)
    
4.  Clique no nome do cluster criado no laboratório 3. Em seguida, clique em _Acessar Cluster_. ![Cluster de Acesso](images/access-cluster.png)
    
5.  Selecione _Acesso Local_ e clique em _Copiar_, conforme mostrado. ![Acesso Local](images/local-access.png)
    
6.  Clique em _Atividades_ e selecione o _Terminal_. ![Terminal](images/click-terminal.png)
    
7.  Cole o comando copiado no terminal. Para _Deseja criar um novo arquivo de configuração?_, digite _y_ e pressione _Enter_. Para _Deseja criar seu arquivo de configuração fazendo log-in em um browser?_, digite _y_ e pressione _Enter_. ![Configuração do OCI](images/oci-config.png)
    
8.  No Navegador Firefox, clique na sua sessão ativa.
    
    > Você verá _Autorização Concluída_ conforme mostrado. ![Autorização Concluída](images/authorization-complete.png)
    
9.  Em _Informe uma frase-senha para sua chave privada_, deixe-a vazia e pressione _Enter_. ![Senha Vazia](images/empty-passphrase.png)
    
10.  Use a tecla de seta superior para executar o comando _oce ce ..._ novamente e executá-lo novamente várias vezes, até que você veja a _Nova configuração gravada no arquivo Kubeconfig /home/opc/.kube/config_. ![Criar KubeConfig](images/create-kubeconfig.png)
    

## Tarefa 2: Verificar Conectividade da IU do Kit de Ferramentas do Kubernetes WebLogic com o Cluster do Oracle Kubernetes

Nesta tarefa, verificamos a conectividade com o _Cluster Oracle Kubernetes (OKE)_ no aplicativo `WebLogic Kubernetes Toolkit UI`.

1.  Volte para a interface do usuário do Kit de Ferramentas do Kubernetes WebLogic, clique em _Atividades_ e selecione a janela da interface do usuário do Kit de Ferramentas do Kubernetes WebLogic.
    
2.  Clique em _Kubernetes_ -> _Configuração do Cliente_ e, em seguida, clique em _Verificar Conectividade_. ![Verificar Conectividade](images/verify-connectivity.png)
    
3.  Depois que você vir a janela _Verificar Sucesso de Conectividade do Cliente Kubernetes_, clique em _Ok_. ![Conexão bem-sucedida](images/successfully-connected.png)
    

## Tarefa 3: Instalar o Operador WebLogic do Kubernetes no Cluster do Oracle Kubernetes

Esta seção fornece suporte para instalar o WebLogic Kubernetes Operator (o "operador") no cluster do Kubernetes de destino.

1.  Clique em _Operador WebLogic_. Especifique os detalhes da configuração a seguir e clique em _Instalar Operador_.
    
    **Namespace do Kubernetes** - O namespace do Kubernetes para o qual o operador será instalado. Deixe o valor padrão.  
    **Conta de Serviço do Kubernetes** - A conta de serviço do Kubernetes a ser usada pelo operador ao fazer solicitações de API do Kubernetes. Deixe o valor padrão.  
    **Helm Release Name to Use for Operator Installation** - O nome da versão Helm a ser usado para identificar essa instalação. Deixe o valor padrão.  
    
    ![WebLogic Operatotr](images/weblogic-operator.png)
    
    > **Somente para sua informação:**  
    > ! Por padrão, o campo _Tag de Imagem a Ser Usada_ do operador é definido como a tag de imagem correspondente à versão mais recente da release do operador no Registro de Contêiner GitHub.  
    > O operador precisa saber quais domínios WebLogic no cluster do Kubernetes ele gerenciará. Ele faz isso no nível de namespace do Kubernetes, de modo que qualquer domínio WebLogic em um namespace do Kubernetes que o operador está configurado para gerenciar será gerenciado pela instância do operador que está sendo instalada.  
    > Para o campo _Estratégia de Seleção de Namespace do Kubernetes_, selecionamos _Seletor de Label_, o que significa que qualquer namespace do Kubernetes com um label _weblogic-operator=enabled_ será gerenciado por esse operador.  
    > ![Imagem do Operador](images/operator-image.png)
    
    > Ativando a Ativação do Binding de Atribuição do Cluster, a instalação do operador criará um ClusterRole e ClusterRoleBinding do Kubernetes que o operador usará para todos os namespaces gerenciados.  
    > Por padrão, a API REST do operador não é exposta fora do cluster do Kubernetes. Para ativar a API REST a ser exposta, você pode ativar _Expor API REST Externamente_. [Vinculação de Função](images/role-binding.png)  
    
    > Este painel permite substituir a configuração de registro em log Java do operador, que pode ser útil ao depurar problemas com o operador.  
    > ![Log de Java](images/java-logging.png)  
    
    > Para obter mais informações sobre _WebLogic Imagem do Operador do Kubernetes_, _Estratégia de Seleção de Namespace do Kubernetes_, _WebLogic Bindings de Atribuição do Kubernetes_, _Acesso Externo à API REST_, _Integrações de Terceiros_ e _Registro em Log do Java_, consulte a documentação [WebLogic Operador do Kubernetes](https://oracle.github.io/weblogic-toolkit-ui/navigate/kubernetes/k8s-wko/).
    
2.  Quando vir _WebLogic Instalação do Operador do Kubernetes Concluída_, clique em _Ok_. ![Operador Instalado](images/operator-installed.png)
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023