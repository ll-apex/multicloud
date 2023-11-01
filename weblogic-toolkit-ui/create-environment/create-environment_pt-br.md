# Configurar o Ambiente de Laboratório

## Introdução

Neste laboratório, você criará um cluster Kubernetes de 3 nós configurado com todos os recursos de rede necessários. Além disso, você criará um repositório no Oracle Cloud Container Image Registry. Em seguida, você gerará um token de autenticação. Além disso, você aceitará o contrato de licença para imagens do Servidor WebLogic no Oracle Container Registry.

Tempo Estimado: 15 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Configurar o Ambiente de Laboratório](videohub:1_zhvohpqq)

### Objetivos

Neste laboratório, você vai:

*   Crie um Cluster do Oracle Kubernetes.
*   Crie um repositório no Oracle Cloud Container Image Registry.
*   Gere um token de autenticação.
*   Aceite a licença para Imagens do Servidor WebLogic no Oracle Container Registry.

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).
*   Você deve ter uma Conta Oracle.
*   Você deve ter um editor de texto.

## Tarefa 1: Criar um Cluster do Oracle Kubernetes

O recurso _Criação Rápida_ usa as definições padrão para criar um _cluster rápido_ com novos recursos de rede, conforme necessário. Essa abordagem é a maneira mais rápida de criar um novo cluster. Se você aceitar todos os valores padrão, poderá criar um novo cluster com apenas alguns cliques. Novos recursos de rede para o cluster são criados automaticamente, juntamente com um pool de nós e três nós de trabalho.

1.  Na Console, selecione o _Menu do Hambúrguer -> Serviços do Desenvolvedor -> Clusters do Kubernetes (OKE)_, conforme mostrado.
    
    ![Menu Hambúrguer](images/hamburger-menu.png " ")
    
2.  Na página Lista de Clusters, selecione o Compartimento e clique em _Criar Cluster_.
    
    > Você precisa selecionar um compartimento no qual tenha permissão para criar um cluster e também um repositório dentro do Oracle Container Registry.
    
    ![Selecionar Compartimento](images/select-compartment.png " ")
    
3.  Na caixa de diálogo Criar Solução de Cluster, selecione _Criação Rápida_ e clique em _Submeter_.
    
    ![Iniciar workflow](images/launch-workflow.png " ")
    
    A _Criação Rápida_ criará um novo cluster com as definições padrão, juntamente com novos recursos de rede para o novo cluster.
    
    Especifique os seguintes detalhes de configuração na página Criação de Cluster (preste atenção ao valor que você coloca no campo _Forma_):
    
    *   **Nome**: O nome do cluster. Deixe o valor padrão.
    *   **Compartimento**: O nome do compartimento. Selecione o compartimento no qual você tem permissão para criar recursos.
    *   **Versão do Kubernetes**: A versão do Kubernetes. Selecione _1.26.2_ como versão do Kubernetes.
    *   **Ponto Final da API do Kubernetes**: Os nós principais do cluster serão ou não roteáveis. Selecione o valor do _Ponto Final Público_.
    *   **Tipo de nó**: Selecione _Gerenciado_ como tipo de nó.
    *   **Nós de Trabalho do Kubernetes**: Os nós de trabalho do cluster serão roteáveis ou não. Deixe o valor padrão _Trabalhadores Privados_.
    *   **Forma**: A forma a ser usada para cada nó no pool de nós. A forma determina o número de CPUs e a quantidade de memória alocada para cada nó. A lista mostra apenas as formas disponíveis em sua tenancy que são suportadas pelo OKE. Selecione _VM.Standard.E4. Flex_ (que geralmente está disponível na Conta Oracle Free Tier). Selecione as 1 OCPUs e 16 GB como Quantidade de Memória.
    *   **Imagem**: Selecione a imagem padrão com a versão _1.26.2_ do Kubernetes.
    *   **Número de nós**: O número de nós de trabalho a serem criados. Deixe o valor padrão, _3_.
    
    ![Cluster Rápido](images/quick-cluster1.png " ") ![Informar Dados](images/enter-data.png " ")
    
4.  Clique em _Próximo_ para revisar os detalhes informados para o novo cluster.
    
5.  Na página _Revisar_, marque a caixa de seleção _Criar um cluster Básico_ e clique em _Criar Cluster_ para criar os novos recursos de rede e o novo cluster.
    
    ![Revisar Cluster](images/review-cluster.png " ")
    
    > Você vê os recursos de rede que estão sendo criados para você. Aguarde até que a solicitação para criar o pool de nós seja iniciada e clique em _Fechar_.
    
    ![Recurso de Rede](images/network-resource.png " ")
    
    > Em seguida, o novo cluster é mostrado na página _Detalhes do Cluster_. Quando os nós principais são criados, o novo cluster obtém um status _Ativo_ (aproximadamente 7 minutos). Não é necessário aguardar, vá para a próxima tarefa.
    
    ![acesso ao cluster](images/cluster-access.png " ")
    

## Tarefa 2: Criação de um Repositório

Nesta tarefa, você cria um repositório público. No laboratório 5, enviaremos a Imagem Auxiliar neste repositório.

1.  Na Console, selecione o _Menu do Hambúrguer_ -> _Serviços do Desenvolvedor_ -> _Registro de Contêiner_, conforme mostrado. ![Registro de contêiner](images/container-registry.png)
    
2.  Selecione seu compartimento, no qual você tem permissão para criar o repositório. Clique em _Criar repositório_. ![Criar Repositório](images/create-repository.png)
    
3.  Informe _`test-model-your_firstname`_ como nome do Repositório e Acesso como _Público_ e clique em _Criar_. ![Detalhes do Repositório](images/repository-details.png)
    
4.  Quando seu repositório estiver pronto. Anote o namespace da tenancy no seu arquivo de texto dentro do editor de texto. ![Observação da Tenancy NameSpace](images/tenancy-namespace.png)
    

## Tarefa 3: Gerar um Token de Autenticação

Nesta tarefa, geraremos um _Token de Autenticação_. No laboratório 5, usaremos esse token de autenticação para enviar imagem auxiliar para o Repositório de Registro do Oracle Cloud Container.

1.  Selecione o Ícone do Usuário no canto superior direito e selecione _MyProfile_.
    
    ![Meu Perfil](images/my-profile.png)
    
2.  Role para baixo e selecione _Tokens de Autenticação_ e clique em _Gerar Token_.
    
    ![Token de autenticação](images/auth-token.png)
    
3.  Copie _`test-model-your_first_name`_ e cole-o na caixa _Descrição_ e clique em _Gerar Token_.
    
    ![Criar Token](images/create-token.png)
    
4.  Selecione _Copiar_ em Token Gerado e cole-o no editor de texto. Não podemos copiá-lo mais tarde. Clique em _Fechar_.
    
    ![Copiar Token](images/copy-token.png)
    

## Tarefa 4: Aceitando a licença para Imagens do Servidor WebLogic

Nesta tarefa, aceitamos o contrato de licença para imagens do Servidor WebLogic reside no Oracle Container Registry. Como no Laboratório 3, usaremos a imagem do Servidor WebLogic 12.2.1.3.0 como nossa Imagem Principal. Portanto, para obter acesso às Imagens do Servidor WebLogic, aceitamos o contrato de licença.

1.  Clique no link do Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) e acesse o sistema. Para isso, você precisa de uma Conta Oracle. ![Acesso ao Registro de Contêiner](images/container-registry-sign-in.png)
    
2.  Informe suas _Credenciais da Conta Oracle_ nos campos Nome de Usuário e Senha e clique em _Acessar_. ![Log-in no Container Registry](images/login-container-registry.png)
    
3.  Na Home page do Oracle Container Registry, Procure _weblogic_. ![Pesquisar WebLogic](images/search-weblogic.png)
    
4.  Clique em _weblogic_ conforme mostrado e selecione _Inglês_ como idioma. Em seguida, clique em _Continuar_. ![Clique em WebLogic](images/click-weblogic.png) ![Selecionar Idioma](images/select-language.png)
    
5.  Clique em _Aceitar_ para aceitar o contrato de licença. ![Aceitar Licença](images/accept-license.png)
    

## Saiba Mais

_Sobre o Oracle Cloud Infrastructure Container Engine for Kubernetes_

O Oracle Cloud Infrastructure Container Engine for Kubernetes é um serviço totalmente gerenciado, escalável e altamente disponível que você pode usar para implantar seus aplicativos de contêiner na nuvem. Use o Container Engine para Kubernetes (às vezes abreviado como OKE) quando sua equipe de desenvolvimento quiser criar, implantar e gerenciar de forma confiável aplicativos nativos da nuvem. Você especifica os recursos de computação que seus aplicativos exigem, e o OKE os provisiona no Oracle Cloud Infrastructure em uma tenancy existente do OCI.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023