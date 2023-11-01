# Configurar o KUBECTL para interagir com o Oracle Container Engine for Kubernetes (OKE) no OCI (Oracle Cloud Infrastructure)

## Introdução

Este laboratório orienta você pelas etapas para criar um arquivo de configuração, que permite acesso ao ambiente do Kubernetes no Oracle Cloud Infrastructure.

### Sobre Produto/Tecnologia

O Oracle Cloud Infrastructure Container Engine for Kubernetes é um serviço totalmente gerenciado, escalável e altamente disponível que você pode usar para implantar seus aplicativos de contêiner na nuvem. Use o Container Engine para Kubernetes (às vezes abreviado como OKE) quando sua equipe de desenvolvimento quiser criar, implantar e gerenciar de forma confiável aplicativos nativos da nuvem. Você especifica os recursos de computação que seus aplicativos exigem, e o OKE os provisiona no Oracle Cloud Infrastructure em uma tenancy existente do OCI.

### Objetivos

Neste laboratório, você vai:

*   Abra o OCI Cloud Shell e configure `kubectl` para interagir com o cluster do Kubernetes.

### Pré-requisitos

*   Este workshop pressupõe que você tenha reserva deste workshop em LiveLabs.

## Tarefa 1: Configurar `kubectl` (CLI do Cluster do Kubernetes)

O Oracle Cloud Infrastructure (OCI) Cloud Shell é um terminal baseado em web browser, acessível na Console do Oracle Cloud. O Cloud Shell oferece acesso a um shell Linux, com uma CLI do Oracle Cloud Infrastructure pré-autenticada e outras ferramentas úteis (_Git, kubectl, helm, CLI do OCI_) para concluir os tutoriais Verrazzano. O Cloud Shell pode ser acessado na Console. Seu Cloud Shell aparecerá na Console do Oracle Cloud como um quadro persistente da Console e permanecerá ativo enquanto você navega para diferentes páginas da Console.

Você usará o _Cloud Shell_ para concluir este workshop.

Usaremos `kubectl` para gerenciar o cluster remotamente usando o Cloud Shell. Ele precisa de um arquivo `kubeconfig`. Isso será gerado usando a CLI do OCI que é pré-autenticada; portanto, não há configuração para fazer antes de começar a usá-la.

1.  Na Console, selecione o _Menu do Hambúrguer -> Serviços do Desenvolvedor -> Clusters do Kubernetes (OKE)_, conforme mostrado.
    
    ![Menu Hambúrguer](../setup-oke-ocishell/images/hamburgermenu.png " ")
    
2.  Selecione o compartimento que está associado a você. Em seguida, clique no cluster _cluster1_.
    
3.  Clique em _Acessar Cluster_ na página de detalhes do cluster.
    
    ![Cluster de Acesso](../setup-oke-ocishell/images/accesscluster.png " ")
    
    > Uma caixa de diálogo é exibida com base na qual você pode abrir o Cloud Shell e conter o comando personalizado do OCI que precisa ser executado, para criar um arquivo de configuração do Kubernetes.
    
4.  Deixe o _Acesso do Cloud Shell_ padrão e selecione primeiro o link _Copiar_ para copiar o comando `oci ce...` para o Cloud Shell.
    
    ![Copiar Configuração do kubectl](../setup-oke-ocishell/images/copyconfig.png " ")
    
5.  Agora, clique em _Iniciar Cloud Shell_ para abrir a console integrada. Em seguida, feche a caixa de diálogo de configuração antes de colar o comando no _Cloud Shell_.
    
    ![Iniciar Cloud Shell](../setup-oke-ocishell/images/launchcloudshell.png " ")
    
6.  Copie o comando da área de transferência (Ctrl+V ou clique com o botão direito do mouse e copie) para o Cloud Shell e execute o comando.
    
    Por exemplo, o comando se parece com o seguinte:
    
        oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaaezwen..................zjwgm2tqnjvgc2dey3emnsd --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0
        
    
    ![Configuração do kubectl](../setup-oke-ocishell/images/kubeconfig.png " ")
    
7.  Agora verifique se `kubectl` está funcionando, por exemplo, usando o comando `get node`. Talvez você precise executar esse comando várias vezes até ver a saída semelhante à seguinte.
    
        <copy>kubectl get node</copy>
        
    
        $ kubectl get node
        NAME          STATUS   ROLES   AGE   VERSION
        10.0.10.17    Ready    node    11m   v1.21.5
        10.0.10.184   Ready    node    11m   v1.21.5
        10.0.10.31    Ready    node    11m   v1.21.5
        
    
    > Se você vir as informações do nó, a configuração foi bem-sucedida.
    
8.  Você pode minimizar e restaurar o tamanho do terminal a qualquer momento usando os controles no canto superior direito do Cloud Shell.
    
    ![shell na nuvem](../setup-oke-ocishell/images/cloudshell.png " ")
    

Deixe este _Cloud Shell_ aberto; nós o usaremos para outros laboratórios.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, fevereiro de 2023