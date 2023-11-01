# Configurar uma instância do Oracle Kubernetes Engine no Oracle Cloud Infrastructure

## Introdução

Este laboratório o orienta pelas etapas para criar um ambiente Kubernetes gerenciado no Oracle Cloud Infrastructure.

Tempo estimado: 15 minutos

### Sobre Produto/Tecnologia

O Oracle Cloud Infrastructure Container Engine for Kubernetes é um serviço totalmente gerenciado, escalável e altamente disponível que você pode usar para implantar seus aplicativos de contêiner na nuvem. Use o Container Engine para Kubernetes (às vezes abreviado como OKE) quando sua equipe de desenvolvimento quiser criar, implantar e gerenciar de forma confiável aplicativos nativos da nuvem. Você especifica os recursos de computação que seus aplicativos exigem, e o OKE os provisiona no Oracle Cloud Infrastructure em uma tenancy existente do OCI.

### Objetivos

Neste laboratório, você vai:

*   Crie uma instância do OKE (Oracle Kubernetes Engine).
*   Abra o OCI Cloud Shell e configure `kubectl` para interagir com o cluster do Kubernetes.

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

Para criar o Container Engine para Kubernetes (OKE), conclua as seguintes etapas:

*   Crie os recursos de rede (VCN, sub-redes, listas de segurança etc.).
*   Crie um cluster.
*   Crie um `NodePool`.

Este laboratório mostra como o recurso _Início Rápido_ cria e configura todos os recursos necessários para um cluster do Kubernetes de 3 nós. Todos os nós serão implantados em diferentes domínios de disponibilidade para garantir alta disponibilidade.

Para obter mais informações sobre o OKE e a implantação de cluster personalizado, consulte a documentação do [Oracle Container Engine](https://docs.cloud.oracle.com/iaas/Content/ContEng/Concepts/contengoverview.htm).

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
    

## Tarefa 2: Configurar `kubectl` (CLI do Cluster do Kubernetes)

O Oracle Cloud Infrastructure (OCI) Cloud Shell é um terminal baseado em web browser, acessível na Console do Oracle Cloud. O Cloud Shell oferece acesso a um shell Linux, com uma CLI do Oracle Cloud Infrastructure pré-autenticada e outras ferramentas úteis (_Git, kubectl, helm, CLI do OCI_) para concluir os tutoriais Verrazzano. O Cloud Shell pode ser acessado na Console. Seu Cloud Shell aparecerá na Console do Oracle Cloud como um quadro persistente da Console e permanecerá ativo enquanto você navega para diferentes páginas da Console.

Você usará o _Cloud Shell_ para concluir este workshop.

Usaremos `kubectl` para gerenciar o cluster remotamente usando o Cloud Shell. Ele precisa de um arquivo `kubeconfig`. Isso será gerado usando a CLI do OCI que é pré-autenticada; portanto, não há configuração para fazer antes de começar a usá-la.

1.  Clique em _Acessar Cluster_ na página de detalhes do cluster.
    
    > Se você se afastar dessa página, abra o menu de navegação e, em _Serviços do Desenvolvedor_, selecione _Clusters do Kubernetes (OKE)_. Selecione seu cluster e vá para a página de detalhes.
    
    ![Cluster de Acesso](images/access-cluster.png " ")
    
    > Uma caixa de diálogo é exibida com base na qual você pode abrir o Cloud Shell e conter o comando personalizado do OCI que precisa ser executado, para criar um arquivo de configuração do Kubernetes.
    
2.  Deixe o _Acesso do Cloud Shell_ padrão e selecione primeiro o link _Copiar_ para copiar o comando `oci ce...` para o Cloud Shell.
    
    ![Copiar Configuração do kubectl](images/copy-config.png " ")
    
3.  Agora, clique em _Iniciar Cloud Shell_ para abrir a console integrada. Em seguida, feche a caixa de diálogo de configuração antes de colar o comando no _Cloud Shell_.
    
    ![Iniciar Cloud Shell](images/launch-cloudshell.png " ")
    
4.  Copie o comando da área de transferência (Ctrl+V ou clique com o botão direito do mouse e copie) para o Cloud Shell e execute o comando.
    
    Por exemplo, o comando se parece com o seguinte:
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![Configuração do kubectl](images/kube-config.png " ")
    
5.  Agora verifique se `kubectl` está funcionando, por exemplo, usando o comando `get node`. Talvez você precise executar esse comando várias vezes até ver a saída semelhante à seguinte.
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.101   Ready    node    11m   v1.26.2
        10.0.10.29    Ready    node    11m   v1.26.2
        10.0.10.37    Ready    node    11m   v1.26.2
        
    
    > Se você vir as informações do nó, a configuração foi bem-sucedida.
    
6.  Você pode minimizar e restaurar o tamanho do terminal a qualquer momento usando os controles no canto superior direito do Cloud Shell.
    
    ![shell na nuvem](images/cloudshell.png " ")
    

Deixe este _Cloud Shell_ aberto; nós o usaremos para outros laboratórios.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023