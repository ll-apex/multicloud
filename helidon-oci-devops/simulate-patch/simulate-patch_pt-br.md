# Simular um Cenário de Patch

## Apresentações

Este laboratório tentará simular um cenário de aplicação de patches. Tome um caso em que o ambiente inicial use _Open JDK_ para testar e eventualmente use o _Oracle JDK_ quando ele estiver pronto para produção. O pipeline anterior de build e implantação já define _Open JDK 20_ como a versão Java a ser usada. Neste laboratório, ele será substituído pelo _Oracle JDK 20_.

Tempo estimado: 10 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Simular um cenário de patch](videohub:1_4pecvhmf)

### Objetivos

Neste laboratório, você vai:

*   Modificar a configuração do JDK
*   Verifique a nova configuração do JDK no pipeline de implantação

### Pré-requisitos

*   Uma Conta do Oracle Free Tier(Trial), Paga ou LiveLabs Cloud

## Tarefa 1: Alterar o Instalador do JDK

1.  Como vimos no **Lab 2**, no **log do pipeline de implantação** concluído anteriormente e observamos que perto da parte inferior do log ele estava usando o **Open JDK**. ![jdk aberto](images/open-jdk.png)
    
2.  No **Editor de Códigos**, clique no nome de arquivo **`build_spec.yaml`** para abri-lo. Comente a variável de ambiente **`JDK20_TAR_GZ_INSTALLER`** que define o caminho do URL para um instalador **Open JDK** na linha número 15 e o comentário **`JDK20_TAR_GZ_INSTALLER`** que define o caminho do URL para um instalador do **Oracle JDK** na linha número 16, conforme mostrado abaixo. ![modificar jdk](images/modify-jdk.png)
    

## Tarefa 2: Pressione a alteração e acione o pipeline DevOps

1.  Copie e cole o comando a seguir no terminal **para confirmar e enviar** a alteração.
    
        <copy>git add .
        git status
        git commit -m "Replace OpenJDK 20 with OracleJDK 20"
        git push</copy>
        
    
    > O pipeline será acionado por esse git push.
    

## Tarefa 3: Verificar o novo sabor do JDK no pipeline de Implantação

1.  Abra a [Console da Nuvem](https://cloud.oracle.com/) na nova guia, clique em _menu Hambúrguer_ -> _Serviços do Desenvolvedor_ -> _Projetos_ em **DevOps**. ![projeto de devops](images/devops-project.png)
    
2.  Selecione o compartimento, que você criou no **Lab 1** e clique em _devops-project-helidon-ocw-hol-string_ para abrir o **DevOps Project**. ![selecionar compartimento](images/select-compartment.png)
    
3.  Em **Histórico de builds mais recente**, você verá **Execuções** e Status como **Aceito/Em Andamento**. Clique nas Execuções mais recentes conforme mostrado abaixo. ![histórico de builds](images/build-history.png)
    
4.  Assim que o pipeline de build **concluído todos os três estágios**, você verá a saída conforme mostrado abaixo. ![execução de build primeiro](images/build-run-first.png)
    
5.  No andamento da execução do Build, no terceiro estágio, clique em **Três pontos** e, em seguida, clique em **Exibir implantação**, conforme mostrado abaixo. Isso abrirá o pipeline de implantação. ![exibir implantação](images/view-deployment.png)
    
6.  Aqui você pode ver **Progresso da Implantação**. Depois que o pipeline de implantação for concluído, você verá a saída conforme mostrado abaixo. ![execução de implantação](images/deployment-run.png)
    
    > Isso implanta com sucesso o aplicativo Helidon em instâncias do serviço Compute no OCI.
    
7.  Para exibir os logs do pipeline de implantação, clique em **Três pontos** próximo ao estágio de implantação e clique em **Exibir detalhes**, conforme mostrado abaixo. ![exibir logs](images/view-logs.png)
    
8.  Role os logs para baixo e verifique a versão do JDK. Deve ser o **Oracle JDK**, conforme mostrado abaixo. ![jdk da oracle](images/oracle-jdk.png)
    

Agora você pode **acessar o próximo laboratório.**

## Confirmação

*   **Autor** - Keith Lustria
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023