# Implantar Domínio WebLogic no Cluster do Oracle Kubernetes (OKE)

## Introdução

Neste laboratório, implantamos o Domínio WebLogic no cluster do kubernetes. Na seção de imagem principal, especificamos a credencial da conta oracle. Na seção de imagem auxiliar, especificamos a credencial da conta do oracle cloud. Aqui também especificamos a réplica do cluster.

Tempo Estimado: 10 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Implantar o Domínio WebLogic no Cluster do OKE](videohub:1_wz94de1l)

### Objetivos

Neste laboratório, você vai:

*   Implante o Domínio WebLogic no Cluster de Kubernetes.

## Tarefa 1: Implantar o Domínio WebLogic no Cluster do Oracle Kubernetes

Nesta tarefa, implantamos o recurso personalizado do Kubernetes do domínio WebLogic no Cluster do Kubernetes.

1.  Role para baixo, informe o seguinte na seção Imagem Principal, Digite _domain-secret_ como _Nome do Segredo de Extração de Imagem_ e Use o nome de usuário e a senha da conta da Oracle em _Nome do Usuário de Extração do Registro de Imagem_ e _Senha de Extração do Registro de Imagem_. Informe seu id de e-mail Oracle em _Endereço de E-mail de Baixa Automática do Registro de Imagem_. Estas são as mesmas credenciais usadas para aceitar a licença para imagens do _weblogic_ no Oracle Container Registry. ![Detalhes da Imagem Principal](images/primary-image-details.png)
    
    > **Somente para suas informações:**  
    > Estamos extraindo a imagem do Oracle Container Registry; portanto, estamos especificando a credencial, que usamos para aceitar o contrato de licença para Imagens do Servidor WebLogic.
    
2.  Role para baixo, na seção Imagem Auxiliar, desmarque a caixa de seleção **Especificar Credenciais de Extração de Imagem Auxiliar**.
    
3.  Na seção _Clusters_, clique no ícone _Editar_, conforme mostrado. ![Redimensionamento de Cluster](images/cluster-resize.png)
    
4.  Informe _2_ como _Replicas_ e clique em _OK_. O tamanho da réplica decide o número de servidor gerenciado no estado _Executando_ após a implantação bem-sucedida do Domínio WebLogic para o cluster do Kubernetes. ![Replicas do Cluster](images/cluster-replicas.png)
    
5.  Na seção Origens de Dados, clique duas vezes para editar _senhas_ para duas origens de dados. Você pode fornecer o _tiger_ como senha nas duas origens de dados. Quando terminar, clique em _Implantar Domínio_. ![Senha do Datasoure](images/datasource-password.png)
    
    > Esta implantação do domínio de teste WebLogic no namespace _test-domain-ns_ do Kubernetes.
    
6.  Depois que você vir a janela _WebLogic Implantação de Domínio no Kubernetes Concluída_, clique em _OK_. ![Implantação Concluída](images/deployment-complete.png)
    
7.  Volte ao terminal, clique em _Atividades_ e selecione a janela _Terminal_. Copie o comando a seguir e cole o terminal. Você deverá ver a saída semelhante, em que o pod do introspector é executado primeiro e depois para o Servidor Admin e os pods posteriores do servidor gerenciado vão no estado _Executando_.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Status do Pod](images/pod-status.png)
    
8.  Você também pode obter o status do domínio por meio da _WebLogic IU do Kit de Ferramentas do Kubernetes_. Volte para a _WebLogic IU do Kit de Ferramentas do Kubernetes_ e clique em _Obter Status do Domínio_. ![Status do Domínio](images/domain-status.png)
    
9.  Na janela Status do Domínio, Role para baixo para ver o status de todos os pods do servidor e clique em _OK_. ![Status do Servidor](images/server-status.png)
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023