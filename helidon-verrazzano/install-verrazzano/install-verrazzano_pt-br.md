# Instalar Verrazzano

## Introdução

Este laboratório o orienta pelas etapas para instalar o Verrazzano em um cluster do Kubernetes no Oracle Cloud Infrastructure.

Tempo Estimado: 15 minutos

### Sobre Produto/Tecnologia

A Verrazzano é uma plataforma de contêiner empresarial completa para implantar aplicativos nativos da nuvem e tradicionais em ambientes híbridos e com várias nuvens. É composto por um conjunto curado de componentes de código aberto - muitos que você já pode usar e confiar, e alguns que foram escritos especificamente para reunir todas as peças que tornam o Verrazzano uma plataforma coesa e fácil de usar.

A Verrazzano inclui os seguintes recursos:

*   Gerenciamento de carga de trabalho híbrido e multicluster
*   Manuseio especial para aplicações WebLogic, Coherence e Helidon
*   Gerenciamento de infraestrutura multicluster
*   Monitoramento integrado e pré-com fio de aplicativos
*   Segurança integrada
*   Ativação DevOps e GitOps

O Verrazzano suporta os seguintes perfis de instalação: desenvolvimento (`dev`), produção (`prod`) e cluster gerenciado (`managed-cluster`).

A imagem a seguir descreve os perfis de instalação do Verrazzano. ![Instalar perfil](images/installprofile.png)

Para alterar perfis em qualquer um dos comandos a seguir, defina a variável de ambiente _VZ\_PROFILE_ como o nome do perfil que deseja instalar.

Para obter uma descrição completa das opções de configuração do Verrazzano, consulte a [Definição de Recursos Personalizados do Verrazzano](https://verrazzano.io/docs/reference/api/verrazzano/verrazzano/).

Neste laboratório, vamos instalar o _perfil de desenvolvimento do Verrazzano_, que tem as seguintes características:

*   DNS curinga (nip.io)
*   Certificados autoassinados
*   Pilha de observabilidade compartilhada usada pelos componentes do sistema e todos os aplicativos
*   Armazenamento efêmero para a pilha de observabilidade (se os pods forem reiniciados, você perderá todos os logs e métricas)
*   Tem uma instalação leve.
*   É para fins de avaliação.
*   Topologia de cluster Opensearch de nó único.

O Verrazzano instala um conjunto selecionado de componentes de código-fonte aberto. A tabela a seguir lista cada componente, sua versão e uma breve descrição.

![Perfil do Verrazzano](images/verrazzano-profile.png " ") ![Perfis do Verrazzano](images/verrazzano-profiles.png " ")

De acordo com nossa opção de DNS, podemos usar nip.io (wildcard DNS) ou [Oracle OCI DNS](https://docs.cloud.oracle.com/en-us/iaas/Content/DNS/Concepts/dnszonemanagement.htm). Neste laboratório, vamos instalar usando nip.io (wildcard DNS).

Um controlador de entrada é algo que ajuda a fornecer acesso a contêineres do Docker para o mundo externo (com um endereço IP). A entrada roteia o endereço IP para diferentes clusters.

### Objetivos

Neste laboratório, você vai:

*   Configurar `kubectl` para usar o cluster do Oracle Kubernetes Engine
*   Instale a CLI do Verrazzano vz.
*   Instale o perfil de desenvolvimento (`dev`) do Verrazzano.

### Pré-requisitos

O Verrazzano requer o seguinte:

*   Um cluster do Kubernetes e um `kubectl` compatível.
*   Pelo menos 2 CPUs, 100 GB de armazenamento em disco e 16 GB de RAM disponíveis nos nós de trabalho do Kubernetes. Isso é suficiente para instalar o perfil de desenvolvimento do Verrazzano. Dependendo dos requisitos de recursos dos aplicativos implantados, isso pode ou não ser suficiente para implantar seus aplicativos.
*   No Laboratório 1, você criou um cluster do Kubernetes no Oracle Cloud Infrastructure. Você usará esse cluster do Kubernetes, _cluster1_, para instalar o perfil de desenvolvimento do Verrazzano.

## Tarefa 1: Configurar `kubectl` (CLI do Cluster do Kubernetes)

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
    

## Tarefa 2: Instalar a CLI do Verrazzano

1.  Faça download da CLI vz mais recente.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
        

0 0 0 0 0 0 0 0 --:--:-- --:--:-- --:--:-- 0 100 39.7M 100 39.7M 0 0 23.6M 0 0:00:01 0:00:01 --:--:-- 32.7M \`\`\`

2.  Faça download do arquivo de soma de verificação.
    
        <copy>curl -LO https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        

A saída deve ser semelhante à seguinte:

    ```bash
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
    

0 0 0 0 0 0 0 0 --:--:-- --:--:-- --:--:-- 0 100 102 100 102 0 0 218 0 --:--:-- --:-- --:--:--:-- 218 \`\`\`\`

3.  Valide o binário em relação ao arquivo de soma de verificação.
    
        <copy>sha256sum -c verrazzano-1.6.2-linux-amd64.tar.gz.sha256</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        verrazzano-1.6.2-linux-amd64.tar.gz: OK
        
4.  Desembalar e mover para o diretório binário de vz,
    
        <copy>tar xvf verrazzano-1.6.2-linux-amd64.tar.gz
        cd ~/verrazzano-1.6.2/bin/</copy>
        
5.  Teste para garantir que a versão instalada esteja atualizada.
    
        <copy>./vz version</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        Version: v1.6.2
        BuildDate: 2023-07-21T12:06:02Z
        GitCommit: 5cd8a2f8670000d2ef0b02404e3169699d87f26b
        

## Tarefa 3: Instalação do perfil de desenvolvimento do Verrazzano

Um perfil de instalação é uma configuração bem conhecida de configurações Verrazzano que pode ser referenciada pelo nome, que pode então ser personalizada conforme necessário.

O Verrazzano suporta os seguintes perfis de instalação: desenvolvimento (`dev`), produção (`prod`) e cluster gerenciado (`managed-cluster`).

1.  Instale usando o Método DNS nip.io. Copie o comando a seguir e cole-o no _Cloud Shell_ para instalar o Verrazzano.
    
        <copy>./vz install -f - <<EOF
        apiVersion: install.verrazzano.io/v1beta1
        kind: Verrazzano
        metadata:
          name: example-verrazzano
        spec:
          profile: dev
        EOF
        </copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        Installing Verrazzano version v1.6.2
        Applying the file https://github.com/verrazzano/verrazzano/releases/download/v1.6.2/verrazzano-platform-operator.yaml
        customresourcedefinition.apiextensions.k8s.io/verrazzanos.install.verrazzano.io created
        namespace/verrazzano-install created
        serviceaccount/verrazzano-platform-operator created
        clusterrole.rbac.authorization.k8s.io/verrazzano-managed-cluster created
        clusterrolebinding.rbac.authorization.k8s.io/verrazzano-platform-operator created
        service/verrazzano-platform-operator created
        service/verrazzano-platform-operator-webhook created
        deployment.apps/verrazzano-platform-operator created
        deployment.apps/verrazzano-platform-operator-webhook created
        mutatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-mysql-backup created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-operator-webhook created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-mysqlinstalloverrides created
        validatingwebhookconfiguration.admissionregistration.k8s.io/verrazzano-platform-requirements-validator created
        Waiting for verrazzano-platform-operator to be ready before starting install - 23 seconds
        
    
    > Demora cerca de 15 a 20 minutos para concluir a instalação. Este comando instala o operador da plataforma Verrazzano e aplica o recurso personalizado Verrazzano. Os logs de instalação serão transmitidos para a janela de comando até que a instalação seja concluída ou até que o tempo limite padrão (30m) seja atingido.
    
2.  Deixe o _Cloud Shell_ aberto e deixe a instalação ser executada.
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023