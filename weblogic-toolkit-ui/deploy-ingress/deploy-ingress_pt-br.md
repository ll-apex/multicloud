# Implantar Controlador de Entrada no Cluster do Oracle Kubernetes (OKE)

## Introdução

Neste laboratório, instalamos o Controlador de entrada _Traefik_. Posteriormente, atualizamos as _Rotas de Entrada_ para acessar o aplicativo e o servidor de administração.

Tempo Estimado: 10 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Implantar o Controlador de Entrada para o Cluster do OKE](videohub:1_4kih00fi)

### Objetivos

Neste laboratório, você vai:

*   Instale o Controlador de Entrada no Cluster do Kubernetes.
*   Atualize as Rotas de Entrada.

## Tarefa 1: Instalação do Controlador de Entrada para o Cluster do Oracle Kubernetes

Nesta tarefa, instalamos o _Controlador de Entrada_.

1.  Clique em _Controlador de Entrada_. Você pode ver alguns valores pré-preenchidos; deixe-os permanecer iguais e clique em _Instalar Controlador de Entrada_. ![Instalar Controlador de Entrada](images/install-ingress-controller.png)
    
    > **Somente para suas informações:**  
    > Isso instala com sucesso o controlador de entrada _traefik-operator_ no namespace Kubernetes _traefik-ns_.
    
2.  Depois de ver a _Instalação do Controlador de Entrada Concluída_, clique em _Ok_. ![Controladora de Entrada Instalada](images/ingress-controller-installed.png)
    

## Tarefa 2: Atualizar as Rotas de Entrada para Acessar o Aplicativo

Nesta tarefa, adicionamos as rotas de Entrada para Acessar a Console de Administração, Aplicativo. No final, obtemos os pontos finais acessíveis.

1.  Role para baixo e clique em _+_ icone para adicionar a _Configuração da Rota de Entrada_. ![Adicionar Rotas de Entrada](images/add-ingress-routes.png)
    
2.  Clique no ícone Editar conforme mostrado para modificar os valores. ![Editar Entrada](images/edit-ingress.png)
    
3.  Informe os detalhes a seguir e clique em _OK_.  
    Nome: console  
    Expressão do Caminho: /console  
    Namespace do Serviço de Destino: test-domain-ns  
    Serviço de Destino: test-domain-admin-server  
    Porta de Destino: 7001  
    
    ![Entrada da Console](images/console-ingress.png)
    
4.  Da mesma forma, adicione as seguintes Rotas de Entrada _opdemo_ também:  
    Nome: opdemo  
    Expressão do Caminho: /opdemo  
    Namespace do Serviço de Destino: test-domain-ns  
    Serviço de Destino: test-domain-cluster-cluster-1  
    Porta de Destino: 8001  
    ![Entrada Opdemo](images/opdemo-ingress.png)
    
5.  Da mesma forma, adicione as seguintes Rotas de Entrada _remote-console_ também:  
    Nome: remote-console  
    Expressão do Caminho: /  
    Namespace do Serviço de Destino: test-domain-ns  
    Serviço de Destino: test-domain-admin-server  
    Porta de Destino: 7001  
    ![Entrada da Console Remota](images/remote-console-ingress.png)
    
6.  Para atualizar as Rotas de Entrada, clique em _Atualizar Rotas de Entrada_. ![Atualizar Rotas de Entrada](images/update-ingress-routes.png)
    
7.  Em _Atualizar Rotas de Entrada Existentes_, clique em _Sim_.
    
8.  Depois de ver a janela _Atualização de Rotas de Entrada Concluída_, clique em _Ok_. ![Atualização de Entrada Concluída](images/update-ingress-complete.png)
    
    > Você precisa anotar esse IP e salvá-lo em um arquivo de texto.
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023