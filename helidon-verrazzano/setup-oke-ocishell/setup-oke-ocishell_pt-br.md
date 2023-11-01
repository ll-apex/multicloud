# Configurar uma instância do Oracle Kubernetes Engine no Oracle Cloud Infrastructure

## Introdução

Neste laboratório, você criará um cluster Kubernetes de 3 nós configurado com todos os recursos de rede necessários. Os nós serão implantados em diferentes domínios de falha para garantir alta disponibilidade.

Para obter mais informações sobre o OKE e a implantação de cluster personalizado, consulte a documentação do [Oracle Kubernetes Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm).

Tempo Estimado: 15 minutos

### Sobre Produto/Tecnologia

O Oracle Cloud Infrastructure Container Engine for Kubernetes é um serviço totalmente gerenciado, escalável e altamente disponível que você pode usar para implantar seus aplicativos de contêiner na nuvem. Use o Container Engine para Kubernetes (às vezes abreviado como OKE) quando sua equipe de desenvolvimento quiser criar, implantar e gerenciar de forma confiável aplicativos nativos da nuvem. Você especifica os recursos de computação que seus aplicativos exigem, e o OKE os provisiona no Oracle Cloud Infrastructure em uma tenancy existente do OCI.

### Objetivos

Você usará o recurso de cluster _Criação Rápida_ para criar uma instância do OKE (Oracle Kubernetes Engine) com os recursos de rede necessários, um pool de nós e três nós de trabalho. A abordagem _Criação Rápida_ é a maneira mais rápida de criar um novo cluster. Se você aceitar todos os valores padrão, poderá criar um novo cluster com apenas alguns cliques.

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Tarefa 1: Criar um cluster do OKE

O recurso _Criação Rápida_ usa as definições padrão para criar um _cluster rápido_ com novos recursos de rede, conforme necessário. Essa abordagem é a maneira mais rápida de criar um novo cluster. Se você aceitar todos os valores padrão, poderá criar um novo cluster com apenas alguns cliques. Novos recursos de rede para o cluster são criados automaticamente, juntamente com um pool de nós e três nós de trabalho.

1.  Na Console, selecione o _Menu do Hambúrguer -> Serviços do Desenvolvedor -> Clusters do Kubernetes (OKE)_, conforme mostrado.
    
    ![Menu Hambúrguer](images/hamburger-menu.png " ")
    
2.  Na página Lista de Clusters, selecione o Compartimento de sua escolha, no qual você tem permissão para criar um cluster e clique em _Criar Cluster_.
    
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
    *   **Tipo de Nó**: Selecione _Gerenciado_ como Tipo de Nó.
    *   **Nós de Trabalho do Kubernetes**: Os nós de trabalho do cluster serão roteáveis ou não. Deixe o valor padrão _Trabalhadores Privados_.
    *   **Forma**: A forma a ser usada para cada nó no pool de nós. A forma determina o número de CPUs e a quantidade de memória alocada para cada nó. A lista mostra apenas as formas disponíveis em sua tenancy que são suportadas pelo OKE. Selecione _VM.Standard.E4. Flex_ (que geralmente está disponível na Conta Oracle Free Tier). Selecione as 2 OCPUs e 32 GB como Quantidade de Memória.
    *   **Imagem**: Selecione a imagem padrão com a versão _1.26.2_ do Kubernetes.
    *   **Número de nós**: O número de nós de trabalho a serem criados. Deixe o valor padrão, _3_.
    
    > _POR FAVOR, TENHA MUITO CUIDADO PARA NÃO DEIXAR A FORMA PADRÃO; A FORMA PADRÃO É MUITO PEQUENA PARA CABER TODOS OS COMPONENTES DE VERRAZZANO_
    
    ![Cluster Rápido](images/quick-cluster.png " ") ![Número do nó](images/node-number.png " ")
    
4.  Clique em _Próximo_ para revisar os detalhes informados para o novo cluster.
    
5.  Na página _Revisar_, marque a caixa de seleção _Criar um cluster Básico_ e clique em _Criar Cluster_ para criar os novos recursos de rede e o novo cluster.
    
    ![Revisar Cluster](images/review-cluster.png " ")
    
    > Você vê os recursos de rede que estão sendo criados para você. Aguarde até que a solicitação para criar o pool de nós seja iniciada e clique em _Fechar_.
    
    ![Recurso de Rede](images/network-resource.png " ")
    
    > Em seguida, o novo cluster é mostrado na página _Detalhes do Cluster_. Quando os nós principais são criados, o novo cluster obtém um status _Ativo_ (aproximadamente 7 minutos). Então, você pode continuar seus laboratórios.
    
    ![provisionamento de cluster](images/cluster-provision.png " ")
    
    ![acesso ao cluster](images/cluster-access.png " ")
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023