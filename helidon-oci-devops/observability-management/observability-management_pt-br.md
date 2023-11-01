# Explorar Logs e Metrics Explorer

## Introdução

Neste laboratório, você verificará a implantação bem-sucedida do aplicativo Helidon. Além disso, você explorará o serviço de explorador de logs e métricas.

Tempo estimado: 5 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Explore logs e explorador de métricas](videohub:1_7a0qaaif)

### Objetivos

Neste laboratório, você vai:

*   Verifique a implantação bem-sucedida do aplicativo Helidon.
*   Explore o OCI Metrics Explorer
*   Explore o serviço OCI Logging
*   Validar a integridade e a prontidão do aplicativo Helidon

### Pré-requisitos

*   Uma Conta do Oracle Free Tier(Trial), Paga ou LiveLabs Cloud
*   Familiaridade com a console do OCI

## Tarefa 1: Acessar o aplicativo Helidon

Nesta tarefa, acessaremos o aplicativo usando curl para fazer as solicitações GET e PUT HTTP.

1.  Volte para o **Editor de Código**, copie e cole o seguinte comando no terminal para configurar o nó de implantação **`PUBLIC_IP`** como uma variável de ambiente.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Copie e cole o comando a seguir para colocar uma **solicitação GET**. Você terá uma saída semelhante, conforme mostrado abaixo.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  Olá para um nome, ou seja, para **Juntar**. Você terá uma saída semelhante, conforme mostrado abaixo.
    
        <copy>curl http://$PUBLIC_IP:8080/greet/Joe</copy>
        {"message":"Hello Joe!","date":[2023,5,23]}
        
4.  Substitua Hello por outra palavra de saudação, isto é, **Hola**. Você terá uma saída semelhante, conforme mostrado abaixo.
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,23]}
        

## Tarefa 2: Explorar o OCI Metrics Explorer

Nesta tarefa, você validará se as **métricas do Helidon** são enviadas para o **Serviço de Monitoramento do OCI**.

1.  Na Console do Cloud, clique em _Menu do hambúrguer_ -> _Observabilidade e Gerenciamento_ -> _Explorador de Métricas_ em **Monitoramento**, conforme mostrado abaixo. ![explorador de métricas](images/metrics-explorer.png)
    
2.  Role para baixo para ir até a seção **Consulta**, selecione o compartimento correto e, em seguida, selecione **`helidon_metrics`** como namespace de Métricas e **`request.count_counter`** como nome de Métrica para criar a consulta. Em seguida, clique em _Atualizar Gráfico_ conforme mostrado abaixo. ![criar consulta](images/create-query.png)
    
3.  Role para cima e você verá uma saída semelhante, conforme mostrado abaixo. Isso fornece dados no formato Gráfico para o contador de solicitações. ![editor de consulta](images/query-editor.png)
    
4.  Para mostrar os dados na Tabela de Dados, alterne o botão para **Mostrar Tabela de Dados**, conforme mostrado abaixo. À medida que você alterna o botão, _Mostrar tabela de Dados_ substitui por **Mostrar Gráfico**. ![tabela de dados](images/data-table.png)
    

## Tarefa 3: Explorar serviço OCI Logging

Nesta tarefa, validamos a integração do **OCI logging SDK** para enviar mensagens ao serviço de Log explorando o log **app-log-helidon-ocw-hol** no grupo de logs **app-log-group-helidon-ocw-hol**.

1.  Na console da Nuvem, clique em _Menu do hambúrguer_ -> _Observabilidade e Gerenciamento_ -> _Logs_. ![menu de logs](images/logs-menu.png)
    
2.  Selecione o compartimento correto e, em seguida, selecione _`app-log-group-helidon-ocw-hol`_ como **Grupo de Logs** e clique em **app-log-helidon-ocw-hol**, conforme mostrado abaixo. ![selecionar compartimento](images/select-compartment.png)
    
3.  Em **Filtrar por tempo**, selecione **Hoje** na lista drop-down e você poderá ver os logs do aplicativo. ![exibir logs](images/view-logs.png)
    

## Tarefa 4: Verificação de integridade do aplicativo Helidon para validar a vida útil e a prontidão

O aplicativo Helidon **oci-mp** adiciona um recurso de **Verificação de Integridade** para validar a **atividade** e/ou **preparação**. Você pode verificar os arquivos de classe _GreetLivenessCheck_ e _GreetReadinessCheck_, respectivamente, no projeto, para ver como eles são concluídos. Isso será particularmente útil ao executar o aplicativo como um microsserviço em um ambiente de orquestrador, como o Kubernetes, para determinar se o microsserviço precisa ser reiniciado se não estiver íntegro. Específico a este laboratório, a verificação de prontidão é utilizada na especificação do pipeline de implantação DevOps depois que o aplicativo é iniciado _para determinar se o aplicativo Helidon foi iniciado com sucesso_. Confira o código na _linha no 79_ em **deployment\_spec.yaml** para vê-lo em ação.

1.  Volte para o **Editor de Código**, copie e cole o seguinte comando no terminal para configurar o nó de implantação **`PUBLIC_IP`** como uma variável de ambiente.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Copie e cole o comando a seguir para verificar o **liveness**. Você terá uma saída semelhante, conforme mostrado abaixo.
    
        <copy>curl http://$PUBLIC_IP:8080/health/live</copy>
        {"status":"UP","checks":[{"name":"CustomLivenessCheck","status":"UP","data":{"time":1684391639448}}]}
        
3.  Copie e cole o comando a seguir para verificar a **preparação**. Você terá uma saída semelhante, conforme mostrado abaixo.
    
        <copy>curl http://$PUBLIC_IP:8080/health/ready</copy>
        {"status":"UP","checks":[{"name":"CustomReadinessCheck","status":"UP","data":{"time":1684391438298}}]}
        

Agora você pode **acessar o próximo laboratório.**

## Saiba Mais

*   [Aplicativo OCI com Helidon](https://medium.com/helidon/oci-application-with-helidon-caa78cacaee5)
*   [Métricas MP do Helidon](https://helidon.io/docs/v3/#/mp/metrics/metrics)
*   [Guia de Métricas do Helidon MP](https://helidon.io/docs/v3/#/mp/guides/metrics)
*   [Integridade do MP do Helidon](https://helidon.io/docs/v3/#/mp/health)
*   [Guia de Verificação de Integridade do Helidon MP](https://helidon.io/docs/v3/#/mp/guides/health)

## Confirmação

*   **Autor** - Keith Lustria
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023