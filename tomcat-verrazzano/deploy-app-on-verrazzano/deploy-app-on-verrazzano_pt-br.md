# Implante o Aplicativo Tomcat no Verrazzano

## Introdução

Este laboratório orienta você no processo de implantação da imagem do docker do aplicativo de amostra tomcat.

Tempo Estimado: 10 minutos

### Verrazzano e Implantação de Aplicativo

O Verrazzano suporta a definição de aplicativo usando o [Open Application Model (OAM)](https://oam.dev/). Os aplicativos Verrazzano são compostos de componentes e configurações de aplicativos.

Quando você implanta aplicativos com a Verrazzano, a plataforma configura conexões e políticas de rede, entra na malha de serviços e dispara uma pilha de monitoramento para capturar as métricas, logs e rastreamentos. O Verrazzano emprega componentes do OAM para definir as unidades funcionais de um sistema que, em seguida, são montadas e configuradas por meio da definição das configurações de aplicativos associadas.

### Componentes do Verrazzano

Um componente do Verrazzano OAM é um [Recurso Personalizado do Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) que descreve a composição geral e os requisitos de ambiente de um aplicativo.

O código a seguir mostra um componente de aplicativo simples tomcat para o aplicativo de amostra tomcat usado neste laboratório. Este recurso descreve um componente que é implementado por uma única imagem do Docker que contém um aplicativo tomcat que expõe um único ponto final.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: tomcat-component
    spec:
      workload:
        apiVersion: core.oam.dev/v1alpha2
        kind: ContainerizedWorkload
        metadata:
          name: tomcat-workload
          labels:
            app: tomcat
            version: v2
        spec:
          containers:
          - name: tomcat-container
            image: "ENDPOINT_OF_REGION/TENANCY_NAMESPACE/tomcat-example-your_firstname:v1"
            ports:
              - containerPort: 8080
                name: ecnj
    

Uma breve descrição de cada campo do componente:

*   **apiVersion** - Versão da definição de recurso personalizado do componente
*   **kind** - Nome padrão da definição de recurso personalizado do componente
*   **metadata.name** - O nome usado para criar o recurso personalizado do componente
*   **spec.workload.kind** - ContainerizedWorkload define uma carga de trabalho sem monitoramento de estado do Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - Portas expostas pelo contêiner

### Configuração do Aplicativo Verrazzano

Uma configuração de aplicativo Verrazzano é um Recurso Personalizado do Kubernetes que fornece personalizações específicas do ambiente. O código a seguir mostra a configuração do aplicativo para o exemplo de aplicativo tomcat usado neste laboratório. Este recurso especifica a implantação do aplicativo no namespace tomcat-ns.

Recursos adicionais de runtime são especificados usando características ou sobreposições de runtime que aumentam a carga de trabalho. Por exemplo, a característica de entrada especifica o host e o caminho de entrada, enquanto a característica de métricas fornece o raspador Prometheus usado para obter as métricas relacionadas ao aplicativo.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: tomcat-appconf
      annotations:
        version: v1.0.0
        description: "Verrazzano demo application"
    spec:
      components:
        - componentName: tomcat-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                  scraper: verrazzano-system/vmi-system-prometheus-0
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: tomcat-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
            - trait:
                apiVersion: core.oam.dev/v1alpha2
                kind: ManualScalerTrait
                spec:
                  replicaCount: 2
    

Uma breve descrição de cada campo na configuração do aplicativo:

*   **apiVersion** - Versão da definição de recurso personalizado ApplicationConfiguration
*   **kind** - Nome padrão da definição de recurso personalizado de configuração do aplicativo
*   **metadata.name** - O nome usado para criar este recurso de configuração do aplicativo
*   **spec.components** - Referência aos componentes do aplicativo utilizados para especificar a configuração de runtime
*   **spec.components\[\].traits** - As características especificadas para os componentes do aplicativo

Para explorar características, podemos examinar os campos de uma característica de entrada:

*   **apiVersion** - Versão da definição de recurso personalizado de característica do OAM
*   **kind** - IngressTrait é o nome da definição de recurso personalizada de característica de entrada do aplicativo OAM
*   **spec.rules.paths** - Os caminhos de contexto para acessar o aplicativo

### Objetivos

Neste laboratório, você vai:

*   Disponibilizar a aplicação de amostra tomcat.
*   Verifique a implantação do aplicativo de amostra tomcat.

### Pré-requisitos

Para executar este laboratório, você deve ter:

*   Cluster do Kubernetes (OKE) executado no Oracle Cloud Infrastructure.
*   A instalação do Verrazzano foi iniciada em um cluster do Kubernetes (OKE).
*   Aplicativo _`tomcat-example-your_first_name`_ empacotado em contêiner disponível em um registro de contêiner.

## Tarefa 1: Implantar o aplicativo de amostra Tomcat

1.  Crie um namespace `tomcat-ns` para o aplicativo de amostra tomcat. Manteremos todos os artefatos do Kubernetes em um namespace separado.
    
        <copy>kubectl create namespace tomcat-ns</copy>
        
    
    > Namespaces são uma forma de organizar clusters em subclusters virtuais. Podemos ter qualquer número de namespaces dentro de um cluster, cada um logicamente separado dos outros, mas com a capacidade de comunicação entre si.
    
2.  Precisamos deixar a Verrazzano ciente de que armazenamos nesse namespace artefatos da Verrazzano. Portanto, precisamos adicionar um label identificando o namespace `tomcat-ns`, conforme gerenciado pela Verrazzano. Os rótulos devem ser usados para especificar atributos de identificação de objetos significativos e relevantes para os usuários.
    
    Aqui, para o namespace `tomcat-ns`, estamos anexando um label a ele, que marca esse namespace conforme gerenciado pelo Verrazzano. O _istio-inject=enabled_, ativa um "sidecar" do Istio e, como tal, ajuda a estabelecer um proxy do Istio. Com um proxy Istio, podemos acessar outros serviços Istio, como um gateway Istio. Para adicionar o label ao namespace `tomcat-ns` com os atributos mencionados anteriormente, copie o seguinte comando e execute-o no Cloud Shell:
    
        <copy>kubectl label namespace tomcat-ns verrazzano-managed=true istio-injection=enabled</copy>
        
3.  Em _~/tomcat-examples/_, temos o arquivo de configuração para o aplicativo tomcatsample. Como parte do Laboratório 2, criamos uma nova imagem do Docker para o componente de aplicação. Além disso, enviamos essa imagem do Docker para o repositório do Oracle Cloud Container Registry. Agora, neste laboratório, modificaremos o arquivo _tomcat-comp.yaml_ para que ele extraia a nova imagem do Docker do repositório do Oracle Cloud Container Registry. Para modificar o arquivo _tomcat-comp.yaml_, copie o comando a seguir e cole-o no _Cloud Shell_.
    
        <copy>vi ~/tomcat-examples/tomcat-comp.yaml</copy>
        
4.  Como parte do Laboratório 3, você salvou seu nome completo de imagem do Docker. Você precisa copiar a linha a seguir e colá-la no seu editor de texto. Em seguida, substitua `docker image full name` pelo nome da imagem do Docker. Em seguida, copie a linha modificada e pressione _i_ para inserir o texto no arquivo `*tomcat-comp.yaml*`. Cole a saída no número da linha **19** (certifique-se de manter o recuo), pressione _Esc_ e digite _:wq_ para salvar o arquivo.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![Inserir linha](images/insert-line.png " ")
    
5.  Agora, queremos implantar o aplicativo conteinerizado tomcat em _cluster1_. Para isso, precisamos de uma configuração de implantação do Kubernetes. Esta implantação instrui o Kubernetes a criar e atualizar instâncias para o aplicativo tomcat. Aqui, temos o arquivo `tomcat-comp.yaml`, que instrui o Kubernetes.
    
    Para implantar o aplicativo de amostra tomcat, copie e cole os dois comandos a seguir, conforme mostrado. O arquivo `tomcat-comp.yaml` contém definições de vários componentes do OAM, em que um componente do OAM é um Recurso Personalizado do Kubernetes que descreve a composição geral e os requisitos de ambiente de um aplicativo.
    
        <copy>kubectl apply -f ~/tomcat-examples/tomcat-comp.yaml -n tomcat-ns</copy>
        
    
    O arquivo `tomcat-app.yaml` é um arquivo de configuração do aplicativo Verrazzano, que fornece personalizações específicas do ambiente.
    
        <copy>kubectl apply -f ~/tomcat-examples/tomcat-app.yaml -n tomcat-ns</copy>
        
6.  Aguarde que os pods estejam no status _Executando_. Use este comando _kubectl_ para aguardar que todos os pods estejam no estado _Executando_ dentro do namespace _tomcat-ns_. Demora cerca de 1 a 2 minutos.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n tomcat-ns --timeout=600s</copy>
        
    
    Quando os pods estiverem prontos, você poderá ver uma resposta semelhante:
    
        $ kubectl wait --for=condition=Ready pods --all -n tomcat-ns --timeout=600s
        pod/tomcat-workload-57d75d66d9-598lq condition met
        pod/tomcat-workload-57d75d66d9-b2m4l condition met
        
    
    Você também pode listar os pods diretamente para verificar seu status:
    
        <copy>kubectl  get po -n tomcat-ns</copy>
        
    
        $ kubectl  get po -n tomcat-ns
        NAME                               READY   STATUS    RESTARTS   AGE
        tomcat-workload-57d75d66d9-598lq   2/2     Running   0          57s
        tomcat-workload-57d75d66d9-b2m4l   2/2     Running   0          54s
        $
        

## Tarefa 2: Verificar a Implantação Bem-sucedida do Aplicativo Tomcat

1.  Verifique o ponto final `/sample-webapp/`. Para determinar o URL que foi construído com base no IP do balanceador de carga/externo e na configuração do aplicativo, execute o seguinte comando:
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io tomcat-ns-tomcat-appconf-gw -n tomcat-ns -o jsonpath={.spec.servers[0].hosts[0]})/sample-webapp/</copy>
        
    
    Isso imprimirá o URL adequado ao seu ponto final REST, por exemplo:
    
        https://tomcat-appconf.tomcat-ns.xx.xx.xx.xx.nip.io/sample-webapp/
        
2.  Cole o URL no browser e clique em _Clique para Chamar um Servlet_. ![URL do Aplicativo](images/application-url.png)
    
3.  Você verá o _Nome do Host_ e o _Endereço IP_. Abra o mesmo URL em outra _Janela Incógnita_. Você observará que essa solicitação será atendida por outra instância do servidor. ![Nome do Host](images/host-name.png) ![Mostrar balanceamento de carga](images/show-loadbalancing.png)
    

> Mostra o balanceamento de carga entre duas réplicas.

Agora você pode **acessar o próximo laboratório**.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023