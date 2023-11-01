# Instalar Verrazzano

## Introdução

Este laboratório o orienta pelas etapas para instalar o Verrazzano em um cluster do Kubernetes no Oracle Cloud Infrastructure.

Tempo estimado: 20 minutos

### Sobre Produto/Tecnologia

A Verrazzano é uma plataforma de contêiner empresarial completa para implantar aplicativos nativos da nuvem e tradicionais em ambientes híbridos e com várias nuvens. É composto por um conjunto curado de componentes de código aberto - muitos que você já pode usar e confiar, e alguns que foram escritos especificamente para reunir todas as peças que tornam o Verrazzano uma plataforma coesa e fácil de usar.

A Verrazzano inclui os seguintes recursos:

*   Gerenciamento de carga de trabalho híbrido e multicluster
*   Manuseio especial para aplicações WebLogic, Coherence e Helidon
*   Gerenciamento de infraestrutura multicluster
*   Monitoramento integrado e pré-com fio de aplicativos
*   Segurança integrada
*   Ativação DevOps e GitOps

### Objetivos

Neste laboratório, você vai:

*   Instale a ferramenta de linha de comando Verrazzano.
*   Instale o perfil de desenvolvimento (`dev`) do Verrazzano.
*   Verifique a instalação bem-sucedida do Verrazzano.

### Pré-requisitos

O Verrazzano requer o seguinte:

*   Um cluster do Kubernetes e um `kubectl` compatível.
*   Pelo menos 2 CPUs, 100 GB de armazenamento em disco e 16 GB de RAM disponíveis nos nós de trabalho do Kubernetes. Isso é suficiente para instalar o perfil de desenvolvimento do Verrazzano. Dependendo dos requisitos de recursos dos aplicativos implantados, isso pode ou não ser suficiente para implantar seus aplicativos.

No Laboratório 1, você criou um cluster do Kubernetes no Oracle Cloud Infrastructure. Você usará esse cluster do Kubernetes, \*cluster1\*, para instalar o perfil de desenvolvimento do Verrazzano. No Laboratório 1, você criou um arquivo de configuração para acessar o cluster do Kubernetes no Oracle Cloud Infrastructure. Você usará esse cluster do Kubernetes, \*cluster1\*, para instalar o perfil de desenvolvimento do Verrazzano.

## Tarefa 1: Instalar a CLI vz

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
        

## Tarefa 2: Instalação do perfil de desenvolvimento do Verrazzano

Um perfil de instalação é uma configuração bem conhecida de configurações Verrazzano que pode ser referenciada pelo nome, que pode então ser personalizada conforme necessário.

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
        \> Demora cerca de 15 a 20 minutos para concluir a instalação. Este comando instala o operador da plataforma Verrazzano e aplica o recurso personalizado Verrazzano. \> Demora cerca de 8 a 10 minutos para concluir a instalação. Este comando instala o operador da plataforma Verrazzano e aplica o recurso personalizado Verrazzano.
2.  Aguarde a conclusão da instalação. Os logs de instalação serão transmitidos para a janela de comando até que a instalação seja concluída ou até que o tempo limite padrão (30m) seja atingido.
    

## Tarefa 3: Verificação de uma instalação bem-sucedida do Verrazzano

O Verrazzano instala vários objetos em vários namespaces. Os componentes Verrazzano são instalados no namespace _verrazzano-system_.

1.  Verifique se todos os pods associados aos vários objetos têm um status _Em Execução_.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        $   kubectl get pods -n verrazzano-system
        NAME                                    READY   STATUS  RESTARTS AGE
        coherence-operator-b5dc669c6-rk2sm      1/1     Running   1       23m
        fluentd-54f5x                           2/2     Running   1       11m
        fluentd-h7mgh                           2/2     Running   1       11m
        fluentd-xcdfz                           2/2     Running   0       11m
        oam-kubernetes-runtime-5b48f944b-cx7b9  1/1     Running   0       24m
        verrazzano-application-operator-665c5   1/1     Running   0       22m
        verrazzano-application-operator-webhook 1/1     Running   0       22m
        verrazzano-authproxy-67776ff58b-8l      3/3     Running   0       21m
        verrazzano-cluster-operator-67dc5695    1/1     Running   0       14m
        verrazzano-cluster-operator-webhook     1/1     Running   0       14m
        verrazzano-console-7d95c98cb9-9ql2      2/2     Running   0       20m
        verrazzano-monitoring-operator-59f      2/2     Running   0       21m
        vmi-system-es-master-0                  2/2     Running   0       19m
        vmi-system-grafana-7fd956b585-2tbgl     3/3     Running   0       19m
        vmi-system-kiali-dd87546d6-ddxss        2/2     Running   0       22m
        vmi-system-osd-7687d6fccf-nm7kt         2/2     Running   0       14m
        weblogic-operator-54979449f4-njgrq      2/2     Running   0       22m
        weblogic-operator-webhook-f7ff8c8cf     1/1     Running   0       22m
        
    
    Deixe o _Cloud Shell_ aberto; precisamos dele para o Laboratório 4.
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023