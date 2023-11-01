# Implantar Aplicativo Springboot na Verrazzano

## Introdução

Este laboratório orienta você no processo de implantação da imagem do docker do aplicativo springboot no OKE.

Tempo Estimado: 10 minutos

### Verrazzano e Implantação de Aplicativo

O Verrazzano suporta a definição de aplicativo usando o [Open Application Model (OAM)](https://oam.dev/). Os aplicativos Verrazzano são compostos de componentes e configurações de aplicativos.

Quando você implanta aplicativos com a Verrazzano, a plataforma configura conexões e políticas de rede, entra na malha de serviços e dispara uma pilha de monitoramento para capturar as métricas, logs e rastreamentos. O Verrazzano emprega componentes do OAM para definir as unidades funcionais de um sistema que, em seguida, são montadas e configuradas por meio da definição das configurações de aplicativos associadas.

### Componentes do Verrazzano

Um componente do Verrazzano OAM é um [Recurso Personalizado do Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) que descreve a composição geral e os requisitos de ambiente de um aplicativo.

O código a seguir mostra um componente simples do aplicativo springboot para o aplicativo de amostra tomcat usado neste laboratório. Este recurso descreve um componente que é implementado por uma única imagem do Docker contendo um aplicativo springboot expondo um único ponto final.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: springboot-component
    spec:
      workload:
        apiVersion: core.oam.dev/v1alpha2
        kind: ContainerizedWorkload
        metadata:
          name: springboot-workload
          labels:
            app: springboot
            version: v1
        spec:
          containers:
          - name: springboot-container
            image: "EndpointOfYourRegion/TenancyNamespace/springboot-your_firstname:v1"
            ports:
              - containerPort: 8080
                name: springboot
    

Uma breve descrição de cada campo do componente:

*   **apiVersion** - Versão da definição de recurso personalizado do componente
*   **kind** - Nome padrão da definição de recurso personalizado do componente
*   **metadata.name** - O nome usado para criar o recurso personalizado do componente
*   **spec.workload.kind** - ContainerizedWorkload define uma carga de trabalho sem monitoramento de estado do Kubernetes
*   **spec.workload.spec.containers.ports.containerPort** - Portas expostas pelo contêiner

### Configuração do Aplicativo Verrazzano

Uma configuração de aplicativo Verrazzano é um Recurso Personalizado do Kubernetes que fornece personalizações específicas do ambiente. O código a seguir mostra a configuração do aplicativo para o exemplo de aplicativo Springboot usado neste laboratório. Este recurso especifica a implantação do aplicativo no namespace springboot.

Recursos adicionais de runtime são especificados usando características ou sobreposições de runtime que aumentam a carga de trabalho. Por exemplo, a característica de entrada especifica o host e o caminho de entrada, enquanto a característica de métricas fornece o raspador Prometheus usado para obter as métricas relacionadas ao aplicativo.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: springboot-appconf
      annotations:
        version: v1.0.0
        description: "Spring Boot application"
    spec:
      components:
        - componentName: springboot-component
          traits:
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: MetricsTrait
                spec:
                  scraper: verrazzano-system/vmi-system-prometheus-0
                  path: "/actuator/prometheus"
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: springboot-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/"
                          pathType: Prefix
    

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

*   Implante o aplicativo springboot.
*   Verifique a implantação do aplicativo springboot.

### Pré-requisitos

Para executar este laboratório, você deve ter:

*   Cluster do Kubernetes (OKE) executado no Oracle Cloud Infrastructure.
*   A instalação do Verrazzano foi iniciada em um cluster do Kubernetes (OKE).
*   Aplicativo _`springboot-your_firstname`_ empacotado em contêiner disponível em um registro de contêiner.

## Tarefa 1: Implantar o aplicativo de inicialização Spring

1.  Crie um namespace `springboot` para o aplicativo springboot. Manteremos todos os artefatos do Kubernetes em um namespace separado.
    
        <copy>kubectl create namespace springboot</copy>
        
    
    > Namespaces são uma forma de organizar clusters em subclusters virtuais. Podemos ter qualquer número de namespaces dentro de um cluster, cada um logicamente separado dos outros, mas com a capacidade de comunicação entre si.
    
2.  Precisamos deixar a Verrazzano ciente de que armazenamos nesse namespace artefatos da Verrazzano. Portanto, precisamos adicionar um label identificando o namespace `springboot`, conforme gerenciado pela Verrazzano. Os rótulos devem ser usados para especificar atributos de identificação de objetos significativos e relevantes para os usuários.
    
    Aqui, para o namespace `springboot`, estamos anexando um label a ele, que marca esse namespace conforme gerenciado pelo Verrazzano. O _istio-inject=enabled_, ativa um "sidecar" do Istio e, como tal, ajuda a estabelecer um proxy do Istio. Com um proxy Istio, podemos acessar outros serviços Istio, como um gateway Istio. Para adicionar o label ao namespace `springboot` com os atributos mencionados anteriormente, copie o seguinte comando e execute-o no Cloud Shell:
    
        <copy>kubectl label namespace springboot verrazzano-managed=true istio-injection=enabled</copy>
        
3.  Em _~/springboot-app_, temos o arquivo de configuração para o aplicativo springboot. Como parte do Laboratório 2, criamos uma nova imagem do Docker para o componente do aplicativo. Além disso, enviamos essa imagem do Docker para o repositório do Oracle Cloud Container Registry. Agora, neste laboratório, modificaremos o arquivo _springboot-comp.yaml_ para que ele extraia a nova imagem do Docker do repositório do Oracle Cloud Container Registry. Para modificar o arquivo _springboot-comp.yaml_, copie o comando a seguir e cole-o no _Cloud Shell_.
    
        <copy>vi ~/springboot-app/springboot-comp.yaml</copy>
        
4.  Como parte do Laboratório 2, você salvou seu nome completo de imagem do Docker. Você precisa copiar a linha a seguir e colá-la em seu arquivo de texto. Em seguida, substitua `docker image full name` pelo nome da imagem do Docker. Em seguida, copie a linha modificada e pressione _i_ para inserir o texto no arquivo `*springboot-comp.yaml*`. Cole a saída no número da linha **19** (certifique-se de manter o recuo), pressione _Esc_ e digite _:wq_ para salvar o arquivo.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![Inserir linha](images/insert-line.png " ")
    
5.  Agora, queremos implantar o aplicativo em contêineres springboot em _cluster1_. Para isso, precisamos de uma configuração de implantação do Kubernetes. Esta implantação instrui o Kubernetes a criar e atualizar instâncias para o aplicativo springboot. Aqui, temos o arquivo `springboot-comp.yaml`, que instrui o Kubernetes.
    
    Para implantar o aplicativo springboot, copie e cole os dois comandos a seguir, conforme mostrado. O arquivo `springboot-comp.yaml` contém definições de vários componentes do OAM, em que um componente do OAM é um Recurso Personalizado do Kubernetes que descreve a composição geral e os requisitos de ambiente de um aplicativo.
    
        <copy>kubectl apply -f ~/springboot-app/springboot-comp.yaml -n springboot</copy>
        
    
    O arquivo `springboot-app.yaml` é um arquivo de configuração do aplicativo Verrazzano, que fornece personalizações específicas do ambiente.
    
        <copy>kubectl apply -f ~/springboot-app/springboot-app.yaml -n springboot</copy>
        
6.  Aguarde que os pods estejam no status _Executando_. Use este comando _kubectl_ para aguardar que todos os pods estejam no estado _Executando_ dentro do namespace _springboot_. Demora cerca de 1 a 2 minutos.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s</copy>
        
    
    Quando os pods estiverem prontos, você poderá ver uma resposta semelhante:
    
        $ kubectl wait --for=condition=Ready pods --all -n kubectl wait --for=condition=Ready pods --all -n springboot --timeout=600s --timeout=600s
        pod/springboot-workload-5c4ffc8954-mbgbb condition met
        pod/springboot-workload-bfb9d487-dwp7q condition met
        
    
    Você também pode listar os pods diretamente para verificar seu status:
    
        <copy>kubectl  get po -n springboot</copy>
        
    
        $ kubectl  get po -n springboot
        NAME                                 READY   STATUS    RESTARTS   AGE
        springboot-workload-bfb9d487-dwp7q   2/2     Running   0          68s
        $
        

## Tarefa 2: Verificar a Implantação Bem-sucedida do Aplicativo Springboot

1.  Verifique o ponto final `/facts`. Para determinar o URL que foi construído com base no IP do balanceador de carga/externo e na configuração do aplicativo, execute o seguinte comando:
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io springboot-springboot-appconf-gw -n springboot -o jsonpath={.spec.servers[0].hosts[0]})/facts</copy>
        
    
    Isso imprimirá o URL adequado ao seu ponto final REST, por exemplo:
    
        https://springboot-appconf.springboot.xx.xx.xx.xx.nip.io/facts
        
2.  Cole o URL no browser e clique no botão _Atualizar_ no browser. Toda vez que você clica no botão Atualizar, ele fornece um fato aleatório sobre Verrazzano. ![URL do Aplicativo](images/application-url.png) ![Outro fato](images/another-fact.png)
    

Agora você pode **acessar o próximo laboratório**.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023