# Implantar aplicativo de amostra de Livros de Bobby

## Apresentações

### Sobre a aplicação de livros de Bobby

![Aplicativo Bobby' Books](images/bobbyoverview.png " ")

[Livros de Bobby](https://verrazzano.io/docs/samples/bobs-books/) consiste em três partes principais:

*   Um aplicativo de _"processamento de ordem"_ de back-end, que é um aplicativo Java EE com serviços REST e uma IU JSP muito simples, que armazena dados em um banco de dados MySQL. Este aplicativo é executado no Servidor WebLogic.
*   Uma loja virtual de front-end _"Livros de Robert"_, que é um vendedor de livros gerais. Isso é implementado como um microsserviço Helidon, que obtém dados de livro do Coherence, usa um armazenamento de cache do Coherence para persistir dados do gerente de ordens e tem uma interface de usuário Web React.
*   Uma loja virtual de front-end _"Livros de Bobby"_, que é uma livraria infantil especializada. Isso é implementado como um microsserviço Helidon, que obtém dados de livro de um armazenamento de cache do Coherence (diferente), interage diretamente com o gerenciador de pedidos e tem uma interface de usuário Web JSF em execução no Servidor WebLogic.

Para obter mais informações e o código-fonte desse aplicativo, consulte os [Exemplos Verrazzano](https://github.com/verrazzano/examples).

### Verrazzano e Implantação de Aplicativo

O Verrazzano suporta a definição de aplicativo usando o [Open Application Model (OAM)](https://oam.dev/). As aplicações Verrrazzano são compostas por componentes e configurações de aplicações.

Quando você implanta aplicativos com a Verrazzano, a plataforma configura conexões, políticas de rede e entradas na malha de serviços e dispara uma pilha de monitoramento para capturar as métricas, logs e rastreamentos. O Verrazzano emprega componentes do OAM para definir as unidades funcionais de um sistema que, em seguida, são montadas e configuradas por meio da definição das configurações de aplicativos associadas.

### Componentes do Verrazzano

Um componente do Verrazzano OAM é um [Recurso Personalizado do Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) que descreve a composição geral e os requisitos de ambiente de um aplicativo.

O código a seguir mostra um componente para o aplicativo de exemplo Livros de Bobby usado neste laboratório. Este recurso descreve um componente que é implementado por uma única imagem do Docker que contém um aplicativo Helidon expondo um único ponto final.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: bobby-helidon
      namespace: bobs-books
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: bobbys-helidon-stock-application
          labels:
            app: bobbys-helidon-stock-application
        spec:
          deploymentTemplate:
            metadata:
              name: bobbys-helidon-stock-application
            podSpec:
              containers:
                - name: bobbys-helidon-stock-application
                  image: container-registry.oracle.com/verrazzano/example-bobbys-helidon-stock-application:0.1.12-1-20210624160519-017d358
                  imagePullPolicy: IfNotPresent
                  ports:
                    - containerPort: 8080
                      name: http
                  env:
                    - name: BACKEND_PORT
                      value: "8001"
                    - name: BACKEND_HOSTNAME
                      value: bobs-bookstore-cluster-cluster-1.bobs-books.svc.cluster.local
                    - name: COH_CLUSTER
                      value: bobbys-coherence
                    - name: COH_CACHE_CONFIG
                      value: coherence-cache-config.xml
                    - name: COH_POF_CONFIG
                      value: pof-config.xml
              imagePullSecrets:
                - name: bobs-books-repo-credentials
    

Uma breve descrição de cada campo do componente:

*   **apiVersion** - Versão da definição de recurso personalizado do componente
*   **kind** - Nome padrão da definição de recurso personalizado do componente
*   **metadata.name** - O nome usado para criar o recurso personalizado do componente
*   **metadata.namespace** - O namespace usado para criar o recurso personalizado deste componente
*   **spec.workload.kind** - VerrazzanoHelidonWorkload define uma carga de trabalho sem monitoramento de estado do Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name** - O nome usado para criar a carga de trabalho sem monitoramento de estado do Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - Os contêineres de implementação
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - Portas expostas pelo contêiner

Você pode encontrar a descrição completa do componente para o aplicativo de livros da Bobbys no arquivo [bobs-books-comp.yaml](https://github.com/verrazzano/verrazzano/blob/master/examples/bobs-books/bobs-books-comp.yaml).

### Configuração do Aplicativo Verrazzano

Uma configuração de aplicativo Verrazzano é um Recurso Personalizado do Kubernetes que fornece personalizações específicas do ambiente. O código a seguir mostra a configuração do aplicativo para o exemplo de Livros de Bob usado neste laboratório. Este recurso especifica a implantação do aplicativo para o namespace bobs-books.

Recursos adicionais de runtime são especificados usando características ou sobreposições de runtime que aumentam a carga de trabalho. Por exemplo, a característica de entrada especifica o host e o caminho de entrada, enquanto a característica de métricas fornece o raspador Prometheus usado para obter as métricas relacionadas ao aplicativo.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: bobs-books
      namespace: bobs-books
      annotations:
        version: v1.0.0
        description: "Bob's Books"
    spec:
      components:
        - componentName: robert-helidon
          traits:
            - trait:
                apiVersion: core.oam.dev/v1alpha2
                kind: ManualScalerTrait
                spec:
                  replicaCount: 2
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
        - componentName: robert-coh
        - componentName: bobby-coh
        - componentName: bobby-helidon
        - componentName: bobby-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobbys-front-end"
                          pathType: Prefix
        - componentName: bobs-orders-wls
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                spec:
                  rules:
                    - paths:
                        - path: "/bobs-bookstore-order-manager/orders"
                          pathType: Prefix
        - componentName: bobs-orders-configmap
        - componentName: bobs-mysql-deployment
        - componentName: bobs-mysql-service
        - componentName: bobs-mysql-configmap
    

Uma breve descrição de cada campo na configuração do aplicativo:

*   **apiVersion** - Versão da definição de recurso personalizado ApplicationConfiguration
*   **kind** - Nome padrão da definição de recurso personalizado de configuração do aplicativo
*   **metadata.name** - O nome usado para criar este recurso de configuração do aplicativo
*   **metadata.namespace** - O namespace usado para este recurso personalizado de configuração do aplicativo
*   **spec.components** - Referência aos componentes do aplicativo utilizados para especificar a configuração de runtime
*   **spec.components\[\].traits** - As características especificadas para os componentes do aplicativo

Para explorar características, podemos examinar os campos de uma característica de entrada:

*   **apiVersion** - Versão da definição de recurso personalizado de característica do OAM
*   **kind** - IngressTrait é o nome da definição de recurso personalizada de característica de entrada do aplicativo OAM
*   **spec.rules.paths** - Os caminhos de contexto para acessar o aplicativo

Este laboratório orienta você no processo de implantação do aplicativo de amostra Livros de Bobby.

Tempo estimado: 10 minutos

### Objetivos

Neste laboratório, você vai:

*   Aceite a licença para fazer download das imagens dos repositórios no Oracle Container Registry.
*   Implantar o aplicativo de amostra Livros de Bobby.
*   Verifique a implantação bem-sucedida do aplicativo de amostra Livros de Bobby.

### Pré-requisitos

Para executar o Laboratório 3, você deve ter:

*   Execute o Laboratório 1, que cria um cluster do OKE no Oracle Cloud Infrastructure.

\* Execute o Laboratório 1, que configura o kubectl para acessar um cluster do OKE no Oracle Cloud Infrastructure.

*   Execute o Laboratório 2, que instala o Verrazzano no cluster do Kubernetes.
*   Um editor de texto, no qual você pode colar os comandos e URLs e modificá-los, de acordo com seu ambiente. Em seguida, você pode copiar e colar os comandos modificados para executá-los no _Cloud Shell_.

## Tarefa 1: Aceitar o contrato de licença para fazer download das imagens dos repositórios no Oracle Container Registry

Para a implantação do aplicativo de amostra _Livros de Bobby_, usaremos as imagens de exemplo. Como essas imagens contêm produtos Oracle, você precisará aceitar o contrato de licença antes de usar essas imagens no arquivo bobs-books, `bobs-books-comp.yaml`.

1.  Clique no link do Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) e acesse o sistema. Para isso, você precisa de uma Conta Oracle.
    
    ![Acessar](images/registrysignin.png " ")
    
2.  Informe suas _Credenciais da Conta Oracle_ nos campos Nome de Usuário e Senha e clique em _Acessar_. Posteriormente, usaremos essas credenciais para criar o segredo no Kubernetes.
    
    ![Oracle SSO](images/oraclesso.png " ")
    
3.  Na Página Inicial, selecione _Verrazzano_.
    
    ![Página inicial do registro](images/registryhomepage.png " ")
    
4.  Por exemplo, o repositório example-bobbys-coherence, example-bobbys-front-end, example-bobs-books-order-manager e example-roberts-coherence , selecione _Inglês_ como idioma e clique em _Continuar_.
    
    ![Continuar](images/continue.png " ")
    
5.  Clique em _Aceitar_ para aceitar o contrato de licença.
    
    ![Aceitar Acordo](images/acceptagreement.png " ")
    
6.  Verifique se você aceitou o contrato de licença dos repositórios relacionados ao Verrazzano, conforme mostrado na imagem a seguir.
    
    ![Verificar Acordo de Licença](images/verifyaggrement.png " ")
    
7.  Na Home page do Oracle Container Registry, Procure _weblogic_
    
    ![Pesquisar WebLogic](images/searchweblogic.png " ")
    
8.  Clique em _weblogic_ conforme mostrado e aceite a licença como fez para Verrazzano imagaes. ![clique em weblogic](images/clickweblogic.png) ![aceitar weblogic](images/acceptagreement.png " ")
    

## Tarefa 2: Implantar o aplicativo Bobby's Books

Precisamos fazer download do código-fonte, no qual temos arquivos de configuração, `bobs-books-app.yaml` e `bobs-books-comp.yaml`.

1.  Faça download do arquivo yaml do componente Verrazzano OAM e dos arquivos de Configuração do Aplicativo Verrazzano do exemplo do Livro de Bobby. Clique em _Copiar_ e cole o comando no Cloud Shell, conforme mostrado:
    
        <copy>
        curl -LSs  https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-app.yaml >~/bobs-books-app.yaml
        curl -LSs https://raw.githubusercontent.com/verrazzano/verrazzano/v1.6.2/examples/bobs-books/bobs-books-comp.yaml >~/bobs-books-comp.yaml
        cd ~
        </copy>
        
2.  Manteremos todos os artefatos do Kubernetes no namespace separado. Crie um namespace para o aplicativo de exemplo dos Livros de Bob. Namespaces são uma forma de organizar clusters em subclusters virtuais. Podemos ter qualquer número de namespaces dentro de um cluster, cada um logicamente separado dos outros, mas com a capacidade de comunicação entre si. Também precisamos deixar a Verrazzano ciente de que armazenamos nesse namespace artefatos Verrazzano. Então, precisamos adicionar um label identificando o namespace bobs-books conforme gerenciado pela Verrazzano. Os rótulos devem ser usados para especificar atributos de identificação de objetos significativos e relevantes para os usuários. Aqui, para o namespace bobs-book, estamos anexando um label a ele, o que marca esse namespace como gerenciado pelo Verrazzano. O _istio-inject=enabled_, ativa um "sidecar" do Istio e, como tal, ajuda a estabelecer um proxy do Istio. Com um proxy Istio, podemos acessar outros serviços Istio, como um gateway Istio e assim por diante. Para adicionar o label ao namespace bobs-books com os atributos mencionados anteriormente, copie o comando a seguir e execute-o no _Cloud Shell_
    
        <copy>
        kubectl create namespace bobs-books
        kubectl label namespace bobs-books verrazzano-managed=true istio-injection=enabled
        </copy>
        
    
    ![Criar Namespace](images/createnamespace.png " ")
    
3.  Copie o comando a seguir para fazer download do script. Este script autentica o usuário do Oracle Container Registry. Se a autenticação for bem-sucedida, ele criará o segredo do registro do docker. O registro do Docker é uma maneira de armazenar e controlar a versão de imagens, como GitHub para código normal, mas para contêineres (que o Kubernetes pode extrair). Aqui, vamos criar um segredo de registro do docker para permitir a extração da imagem de exemplo dos Livros de Bobby do Oracle Container Registry. Clique em _Copiar_ no comando a seguir e cole-o em qualquer editor de texto de sua escolha e substitua o nome de usuário e a senha pelo ID de e-mail e senha, respectivamente, usados na Tarefa 1, para aceitar o contrato de licença para fazer download de imagens do Oracle Container Registry. Em seguida, no Cloud Shell, cole o comando modificado, conforme mostrado:
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/deploy-bobsbook/create_secret.sh >~/create_secret.sh
        chmod 777 create_secret.sh
        ./create_secret.sh username 'password'    
        </copy>
        
    
    ![Criar Segredo](images/createsecret.png " ")
    
    > Informe a senha entre aspas simples.
    
4.  Precisamos criar vários segredos do Kubernetes com credenciais. No aplicativo Livros de Bobby, temos dois domínios WebLogic _bobby-front-end_ e _bobs-bookstore_. As credenciais do domínio WebLogic são mantidas em um Segredo do Kubernetes em que o nome do segredo é especificado usando _webLogicCredentialsSecret_ no recurso Domínio WebLogic. Além disso, o segredo das credenciais do domínio deve ser criado no namespace em que o domínio estará em execução. Precisamos criar os segredos chamados _bobbys-front-end-weblogic-credentials_ e _bobs-bookstore-weblogic-credentials_ usados pelos domínios do Servidor WebLogic, com um valor de nome de usuário `weblogic` e uma senha que é gerada aleatoriamente no namespace bobs-books. Nosso aplicativo Livros de Bobby usa um banco de dados _mysql_. Portanto, criamos um novo segredo chamado _mysql-credentials_, com o valor de nome de usuário `weblogic`, uma senha que é gerada aleatoriamente e o URL JDBC, _jdbc:mysql://mysql.bobs-books.svc.cluster.local:3306/books_ no namespace bobs-books. Usaremos esses valores na string de conexão JDBC no objeto WebLogic DataSource. Copie e cole o bloco de comandos no _Cloud Shell_.
    
        <copy>
        export WLS_USERNAME=weblogic
        export WLS_PASSWORD=$((< /dev/urandom tr -dc 'A-Za-z0-9"'\''/<=>?\^_`|~' | head -c10);(date +%S))
        echo $WLS_PASSWORD
        kubectl create secret generic bobbys-front-end-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic bobs-bookstore-weblogic-credentials --from-literal=password=$WLS_PASSWORD --from-literal=username=$WLS_USERNAME -n bobs-books
        kubectl create secret generic mysql-credentials \
            --from-literal=username=$WLS_USERNAME \
            --from-literal=password=$WLS_PASSWORD \
            --from-literal=url=jdbc:mysql://mysql.bobs-books.svc.cluster.local:3306/books \
            -n bobs-books
        cd ~
        </copy>
        
    
    ![Criar Recurso](images/createresource.png " ")
    
5.  Temos um cluster para governador, _cluster1__verrazzano_, com três nós. Agora, queremos implantar o aplicativo em contêineres Bobby's Books em _cluster1__verrazzano_. Para isso, precisamos de uma configuração de implantação do Kubernetes. Esta implantação instrui o Kubernetes a criar e atualizar instâncias para o aplicativo Livros de Bobby. Aqui, temos o arquivo `bobs-books-comp.yaml`, que instrui o Kubernetes a implantar o aplicativo Livros do Bobby. Copie e cole os dois comandos a seguir, conforme mostrado. O arquivo `bobs-books-comp.yaml` contém definições de vários componentes do OAM, em que um componente do OAM é um Recurso Personalizado do Kubernetes que descreve a composição geral e os requisitos de ambiente de um aplicativo. Para saber mais sobre o arquivo `bobs-books-comp.yaml`, revise os Componentes do Verrazzano na seção Introdução deste Laboratório 3.
    
        <copy>kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh created
        component.core.oam.dev/robert-helidon created
        component.core.oam.dev/bobby-coh created
        component.core.oam.dev/bobby-helidon created
        component.core.oam.dev/bobby-wls created
        component.core.oam.dev/bobs-mysql-configmap created
        component.core.oam.dev/bobs-mysql-service created
        component.core.oam.dev/bobs-mysql-deployment created
        component.core.oam.dev/bobs-orders-configmap created
        component.core.oam.dev/bobs-orders-wls created
        $
        
6.  O arquivo `bobs-books-app.yaml` é um arquivo de configuração do aplicativo Verrazzano, que fornece personalizações específicas do ambiente. Para saber mais sobre o arquivo `bobs-books-app.yaml`, verifique a Configuração do Aplicativo Verrazzano na seção Introdução deste Laboratório 3.
    
        <copy>kubectl apply -f ~/bobs-books-app.yaml -n bobs-books</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        $ kubectl apply -f ~/bobs-books-app.yaml -n bobs-books
        applicationconfiguration.core.oam.dev/bobs-books created
        $
        
7.  Aguarde que todos os pods do aplicativo de exemplo Livros de Bobby estejam no estado _Executando_. Talvez seja necessário repetir este comando várias vezes antes de ele ser bem-sucedido. Os pods do Servidor WebLogic e do Coherence podem levar algum tempo para serem criados e Prontos. Este comando _kubectl_ aguardará que todos os pods estejam no estado _Executando_ dentro do namespace bobs-books. Demora cerca de 4 a 5 minutos. Se você estiver esperando por mais de 5 minutos, poderá executar novamente o comando.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
          $ kubectl wait --for=condition=Ready pods --all -n bobs-books --timeout=600s
          pod/bobbys-coherence-0 condition met
          pod/bobbys-front-end-adminserver condition met
          pod/bobbys-front-end-managed-server1 condition met
          pod/bobbys-helidon-stock-application-5f74cbcc8b-cw4x4 condition met
          pod/bobs-bookstore-adminserver condition met
          pod/bobs-bookstore-managed-server1 condition met
          pod/mysql-6bc8f9f785-n4qjh condition met
          pod/robert-helidon-65b8874988-7x5vj condition met
          pod/robert-helidon-65b8874988-vnntp condition met
          pod/roberts-coherence-0 condition met
          pod/roberts-coherence-1 condition met
          $
        

## Tarefa 3: Verificar a implantação bem-sucedida do aplicativo Livro de Bobby

Verifique se a configuração do aplicativo, os domínios, os recursos do Coherence e a característica de entrada existem todos.

1.  Para verificar se o aplicativo _Livros de Bobby_ foi implantado com sucesso no namespace bobs-books.
    
        <copy>kubectl get ApplicationConfiguration -n bobs-books</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        $ kubectl get ApplicationConfiguration -n bobs-books
          NAME         AGE
          bobs-books   72m
        $
        
2.  Para verificar se os dois domínios WebLogic foram criados no namespace bobs-books com sucesso.
    
        <copy>kubectl get Domain -n bobs-books</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        $ kubectl get Domain -n bobs-books
          NAME               AGE
          bobbys-front-end   73m
          bobs-orders-wls    73m
        $
        
3.  Para verificar se ambos os clusters do Coherence foram criados no namespace bobs-books com sucesso.
    
        <copy>kubectl get Coherence -n bobs-books</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        $ kubectl get Coherence -n bobs-books
        NAME              CLUSTER           ROLE           REPLICAS  READY PHASE
        bobbys-coherence  bobbys-coherence  bobbys-coherence  1       1    Ready
        roberts-coherence roberts-coherence roberts-coherence 2       2    Ready
        $
        
4.  Para obter o IngressTrait para o aplicativo Livro de Bobby, execute o comando a seguir no _Cloud Shell_.
    
        <copy>kubectl get IngressTrait -n bobs-books</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        $ kubectl get IngressTrait -n bobs-books
          NAME                               AGE
          bobby-wls-trait-79b67d9d88         76m
          bobs-orders-wls-trait-57b4d8cb4b   76m
          robert-helidon-trait-54d7bcd54b    76m
        $
        
5.  Verifique se os pods de serviço foram criados com sucesso e faça a transição para o estado _Executando_. Observe que isso pode levar alguns minutos e que talvez você veja alguns dos serviços serem encerrados e reiniciados. Por fim, você observará todos os pods associados ao namespace bobs-books no Status _Executando_. Copie os detalhes dos pods para o _bobbys-helidon-stock-application_.
    
        <copy>kubectl get pods -n bobs-books</copy>
        
    
        $ kubectl get pods -n bobs-books
        NAME                                          READY STATUS    RESTARTS   AGE
        bobbys-coherence-0                             2/2  Running   0          7m51s
        bobbys-front-end-adminserver                   4/4  Running   0          5m28s
        bobbys-front-end-managed-server1               4/4  Running   0          4m30s
        bobbys-helidon-stock-application-5f74cbc-cw4x4 2/2  Running   0          7m54s
        bobs-bookstore-adminserver                     4/4  Running   0          4m31s
        bobs-bookstore-managed-server1                 4/4  Running   0          3m41s
        mysql-6bc8f9f785-n4qjh                         2/2  Running   0          5m52s
        robert-helidon-65b8874988-7x5vj                2/2  Running   0          7m53s
        robert-helidon-65b8874988-vnntp                2/2  Running   0          7m54s
        roberts-coherence-0                            2/2  Running   0          7m52s
        roberts-coherence-1                            2/2  Running   0          7m51s
        $
        
    
    > Observe o nome do pod para **bobbys-helidon-stock-application**. Quando reimplantarmos esse componente, você observará que esse pod entrará em um status _Encerrando_ e o novo pod começará e entrará no estado _Em Execução_ no Laboratório 7.
    
    Deixe o _Cloud Shell_ aberto; nós o usaremos também para os próximos laboratórios.
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023