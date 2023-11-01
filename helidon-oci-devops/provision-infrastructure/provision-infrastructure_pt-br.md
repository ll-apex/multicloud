# Provisionamento da Infraestrutura

## Introdução

Neste laboratório, você criará um **compartimento**, **grupos dinâmicos**, **grupo de usuários e políticas**. Em seguida, você criará um **projeto DevOps** e seus recursos relacionados usando o serviço Terraform no **OCI Code Editor**.

Tempo estimado: 10 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Provisione a infraestrutura](videohub:1_tnhjvsxr)

### Objetivos

Neste laboratório, você vai:

*   Abra o Code Editor para fazer download do script Terraform.
*   Provisione o compartimento, os grupos dinâmicos, o grupo de usuários e as políticas.
*   Provisione os recursos necessários para o projeto DevOps

### Pré-requisitos

*   Uma Conta do Oracle Free Tier(Trial), Paga ou LiveLabs Cloud
*   Familiaridade com compartimentos, grupos dinâmicos, grupos de usuários e políticas

## Tarefa 1: Abrir Editor de Código e fazer download do código-fonte

1.  Na Console do Cloud, clique no ícone _Ferramentas do desenvolvedor_, conforme mostrado, e clique em _Editor de Código_. ![editor de código aberto](images/open-codeeditor.png)
    
2.  Clique em _Terminal_\-> _Novo Terminal_ para abrir o terminal. Durante o workshop, você será solicitado a abrir um novo terminal. Dessa forma, você pode abrir um novo terminal no Code Editor. ![open terminal](images/open-terminal.png)
    
3.  Copie e cole o comando a seguir no terminal para fazer download do código-fonte. Este código-fonte contém os scripts terraform que criam os recursos do OCI necessários para este workshop.
    
        <copy>curl -O https://objectstorage.uk-london-1.oraclecloud.com/p/IxV3cO7Uf5VnbniLEmx8zOBhr6liix40QWNTnTy0TTBcGdrLaRNSt2IJYxBPHqdw/n/lrv4zdykjqrj/b/ankit-bucket/o/devops_helidon_to_instance_ocw_hol.zip
        unzip ~/devops_helidon_to_instance_ocw_hol.zip</copy>
        

![fazer download do código-fonte](images/download-sourcecode.png)

4.  Para abrir o código-fonte _`devops_helidon_to_instance_ocw_hol`_ no **Editor de Códigos**, clique em _Arquivo_\-> _Abrir_. ![código aberto](images/open-sourcecode.png)
    
5.  Selecione _`devops_helidon_to_instance_ocw_hol`_ no diretório home e clique em _Abrir_. ![devops abertos](images/open-devops.png)
    
6.  Clique no nome de arquivo _terraform.tfvars_ dentro da pasta _`devops_helidon_to_instance_ocw_hol`_, conforme mostrado. Você pode ver que temos variáveis **`tenancy_ocid`**, **region**, **`compartment_ocid`**, **`user_ocid`** que personalizaremos para sua conta de teste no Cloud. ![tfvars abertos](images/open-tfvars.png)
    

> Se você não vir o projeto. Talvez seja necessário clicar no ícone _Explorador_ no Code Editor. ![exlorador](images/explorer.png)

7.  No browser, abra uma **nova guia** para a [Console do Cloud](https://cloud.oracle.com/). Usaremos essa guia para obter o valor das variáveis acima.
    
8.  Para obter o **`tenancy_ocid`**, clique no _ícone Usuário_ e, em seguida, clique em _Tenancy_, conforme mostrado. ![obter ocid da tenancy](images/get-tenancyocid.png)
    
9.  Clique em _Copiar_ para copiar o **OCID** da tenancy e colá-lo no arquivo _terraform.tfvars_ como o valor de _`tenancy_ocid`_. ![copiar ocid da tenancy](images/copy-tenancyocid.png)
    
10.  Para obter o **`user_ocid`**, clique no _ícone Usuário_ e, em seguida, clique em _Meu perfil_, conforme mostrado. ![meu perfil](images/my-profile.png)
    
11.  Clique em _Copiar_ para copiar o **OCID** do usuário e colá-lo no arquivo _terraform.tfvars_ como o valor de _`user_ocid`_. ![copiar ocid do usuário](images/copy-userocid.png)
    
12.  Para localizar o nome da região, clique em **Gerenciar regiões** conforme mostrado abaixo. Em seguida, copie o **Identificador de Região** da sua região home e cole-o no arquivo _terraform.tfvars_ como o valor da _região_. ![gerenciar região](images/manage-region.png) ![nome da região](images/region-name.png)
    
13.  Por fim, seu _terraform.tfvars_ deve ter esta aparência. Deixe o valor de _`compartment_ocid`_ como está. Substituiremos o valor depois que o compartimento for criado como parte da Tarefa 2. ![tfvars de inicialização](images/init-tfvars.png)
    

## Tarefa 2: Criar um Compartimento, Grupos Dinâmicos, Grupo de Usuários e Políticas

O objetivo desta tarefa é preparar o ambiente para a configuração DevOps criando um Compartimento, um Grupo Dinâmico, um Grupo de Usuários e políticas. Esta seção requer um usuário com privilégio de administrador. Se você não tiver, certifique-se de solicitar a outro usuário com esse privilégio para executá-lo para você.

1.  Abra um terminal, copie e cole o comando a seguir para navegar até a pasta _init_.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/init/</copy>
        
2.  No Code Editor, você pode exibir vários arquivos na pasta _init_. Esses são os scripts terraform que criam o compartimento, os grupos dinâmicos, o grupo de usuários e as políticas. ![arquivos de inicialização](images/init-files.png)
    
3.  Copie e cole os comandos a seguir para provisionar o compartimento, grupos dinâmicos, grupo de usuários e políticas.
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    você verá uma saída semelhante à mostrada a seguir. **observe a saída** para saber o que o script terraform cria. Além disso, você pode consultar o código para ver sua implementação. ![inicialização criada](images/init-created.png)
    
    > Se houver erros, verifique se definiu corretamente os valores no arquivo **terraform.tfvars**.
    

## Tarefa 3: Criar um projeto DevOps e seus recursos

1.  No terminal, copie e cole o comando a seguir para navegar até a pasta _main_.
    
        <copy>cd ~/devops_helidon_to_instance_ocw_hol/main/</copy>
        
2.  Copie e cole o comando a seguir para atualizar o **compartment\_ocid** de um compartimento recém-criado em _terraform.tfvars_ na pasta _`devops_helidon_to_instance_ocw_hol`_.
    
        <copy>../utils/update_compartment.sh</copy>
        
3.  Copie e cole o comando a seguir no terminal para provisionar todos os recursos DevOps.
    
        <copy>terraform init
        terraform plan
        terraform apply -auto-approve</copy>
        
    
    > **Leia:-** Isso fornecerá os seguintes recursos necessários para DevOps:
    
    *   **Serviço OCI DevOps**
        *   **Projeto DevOps do OCI** que conterá todos os componentes DevOps necessários para este projeto.
        *   **OCI Code Repository** que hospedará o projeto do código-fonte do Aplicativo.
        *   **DevOps Pipeline de Build** com os seguintes estágios:
            *   **Gerenciar Build** - executa etapas para fazer download de JDK20, maven e criar o aplicativo Helidon
            *   **Entregar Artefatos** - Faz upload do aplicativo Helidon criado e da Implantação do Repositório de Artefatos
            *   **Acionar Implantação** - Aciona o Pipeline de Implantação
        *   **DevOps Pipeline de Implantação** que executará o seguinte no ambiente de destino:
            *   Faça download do JDK20
            *   Instale a CLI do OCI e use-a para fazer download do Material para Distribuição do Aplicativo
            *   Executar o Aplicativo
        *   **DevOps Ambiente de Grupo de Instâncias** que será usado pelo Pipeline de Implantação para identificar a Instância de Computação do OCI criada como alvo de implantação.
        *   **DevOps Trigger** que chamará o ciclo de vida do pipeline do início ao fim quando um evento de envio ocorrer no Repositório de Código do OCI.
    *   **Registro de Artefato do OCI**
        *   **OCI Artifact Repository** que hospedará os Binários de Aplicativos Helidon e o Manifesto de Implantação criados como artefatos com controle de versão.
    *   **Plataforma OCI**
        *   **OCI Compute Instance** que abre a porta 8080 com base no firewall. É aqui que o aplicativo será eventualmente implantado.
    *   **VCN (OCI Virtual Cloud Network) com Lista de Segurança** que contém uma Entrada que abre a porta 8080. A porta 8080 é onde o aplicativo Helidon será acessado. A VCN do OCI será usada pela Instância de Computação do OCI para suas necessidades de rede.
4.  O diagrama abaixo ilustra como a configuração do DevOps funcionará: ![diagrama de devops](images/devops-diagram.png)
    
5.  Você obterá uma saída semelhante à mostrada a seguir. ![saída de tf](images/tf-output.png)
    

Agora você pode **acessar o próximo laboratório.**

## Confirmação

*   **Autor** - Keith Lustria
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023