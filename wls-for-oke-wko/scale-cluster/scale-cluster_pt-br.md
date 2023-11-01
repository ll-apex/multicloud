# Dimensionar um Cluster WebLogic (Opcional)

## Introdução

Neste laboratório, dimensionamos um Cluster WebLogic. Aqui, modificamos o valor para _3_ e reimplantamos o domínio.

Tempo Estimado: 5 minutos

### Objetivos

Neste laboratório, você vai:

*   Dimensione um Cluster WebLogic.

## Tarefa 1: Dimensionando um Cluster WebLogic usando a interface de usuário do Kubernetes Toolkit WebLogic

Nesta tarefa, você só precisa modificar o valor _Réplica_ de 2 para 3 e reimplantar o domínio novamente.

1.  Volte para a interface do usuário do Kit de Ferramentas do Kubernetes WebLogic, clique em _WebLogic Domain_. Vá para a seção _Clusters_ e clique no ícone _Editar_.  
    ![Redimensionamento de Cluster](images/cluster-resize.png)
    
2.  Altere as réplicas de _2_ para _3_ e clique em _OK_. ![Alterar réplicas](images/change-replicas.png)
    
3.  Para Reimplantar o domínio, clique em _Implantar Domínio_. ![Reimplantar Domínio](images/redeploy-domain.png)
    
4.  Depois que você vir a janela _WebLogic Implantação de Domínio no Kubernetes Concluída_, clique em _Ok_. ![Implantação Concluída](images/deployment-complete.png)
    
5.  Volte para a janela _Terminal_, clique em _Atividades_ e selecione a janela _Terminal_. Copie o comando a seguir e cole no terminal.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Exibir Escala](images/view-scaling.png)
    
    > Você pode ver, a reimplantação do domínio, inicia o job do introspector, que inicia o processo de criação do pod para test-domain-managed-server3 e, em algum momento, esse pod entra no status _Executando_.
    
6.  Volte ao navegador, onde você tem a página do aplicativo aberta. Clique no botão Atualizar. Você verá o balanceamento de carga entre três servidores gerenciados agora. ![novo servidor](images/new-server.png)
    
7.  Volte para a Console Remota WebLogic, clique em _Árvore de Monitoramento_ -> _Ambiente_ -> _Servidores_. Você notará gerenciado - server3 aqui também. ![console remota](images/remote-console.png)
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, outubro de 2023