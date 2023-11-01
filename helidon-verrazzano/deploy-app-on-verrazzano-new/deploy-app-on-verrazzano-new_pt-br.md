# Implante o Aplicativo Helidon na Verrazzano

## Introdução

Este laboratório orienta você no processo de implantação do aplicativo Hello Helidon.

Tempo Estimado: 10 minutos

### Verrazzano e Implantação de Aplicativo

O Verrazzano suporta a definição de aplicativo usando o [Open Application Model (OAM)](https://oam.dev/). Os aplicativos Verrazzano são compostos de componentes e configurações de aplicativos.

Quando você implanta aplicativos com a Verrazzano, a Verrazzano executa o gerenciamento inteligente da carga de trabalho. A plataforma configura conexões e políticas de rede, entra na malha de serviços e dispara uma pilha de monitoramento para capturar as métricas, logs e rastreamentos. O Verrazzano emprega componentes do OAM para definir as unidades funcionais de um sistema que, em seguida, são montadas e configuradas por meio da definição das configurações de aplicativos associadas.

![Verrazzano](images/Verrazzano_IntelligentWorkload.png)

### Objetivos

Neste laboratório, você vai:

*   Verifique a instalação bem-sucedida do ambiente Verrazzano.
*   Implante o aplicativo Hello Helidon.
*   Verifique a implantação do aplicativo Hello Helidon.

### Pré-requisitos

Para executar este laboratório, você deve ter:

*   Cluster do Kubernetes (OKE) executado no Oracle Cloud Infrastructure.
*   A instalação do Verrazzano foi iniciada em um cluster do Kubernetes (OKE).

## Tarefa 1: Verificação de uma instalação bem-sucedida do Verrazzano

O Verrazzano instala vários objetos em vários namespaces. Os componentes Verrazzano são instalados no namespace _verrazzano-system_.

1.  Verifique se todos os pods associados aos vários objetos têm um status _Em Execução_.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        $   kubectl get pods -n verrazzano-system
        NAME                                         READY   STATUS RESTARTS AGE
        coherence-operator-b5dc669c6-zm4dw           1/1     Running   1     153m
        fluentd-5s7hh                                2/2     Running   1     144m
        fluentd-bt4t4                                2/2     Running   1     144m
        fluentd-ghmkx                                2/2     Running   1     144m
        oam-kubernetes-runtime-5b48f944b-9bv2v       1/1     Running   0     154m
        verrazzano-application-operator-f976cf96f    1/1     Running   0     152m
        verrazzano-application-operator-webhook      1/1     Running   0     152m
        verrazzano-authproxy-58f8975c5d-jhx72        3/3     Running   0     151m
        verrazzano-cluster-operator-544d494f49       1/1     Running   0     144m
        verrazzano-cluster-operator-webhook-79       1/1     Running   0     144m
        verrazzano-console-6f59f7485d-xrdt8          2/2     Running   0     147m
        verrazzano-monitoring-operator-d88cdd66c     2/2     Running   0     151m
        vmi-system-es-master-0                       2/2     Running   0     151m
        vmi-system-grafana-78cd975dd9-vfjv5          3/3     Running   0     151m
        vmi-system-kiali-57d859dc4b-bnvjh            2/2     Running   0     151m
        vmi-system-osd-7687d6fccf-cjwqr              2/2     Running   0     147m
        weblogic-operator-54979449f4-t9q26           2/2     Running   0     152m
        weblogic-operator-webhook-f7ff8c8cf-nc4r     1/1     Running   0     152m
        

## Tarefa 2: Implantar o aplicativo Hello Helidon

1.  Crie um namespace `hello-helidon` para o aplicativo quickstart-mp Helidon. Manteremos todos os artefatos do Kubernetes em um namespace separado.
    
        <copy>kubectl create namespace hello-helidon</copy>
        
    
    > Namespaces são uma forma de organizar clusters em subclusters virtuais. Podemos ter qualquer número de namespaces dentro de um cluster, cada um logicamente separado dos outros, mas com a capacidade de comunicação entre si.
    
2.  Precisamos deixar a Verrazzano ciente de que armazenamos nesse namespace artefatos da Verrazzano. Portanto, precisamos adicionar um label identificando o namespace `hello-helidon`, conforme gerenciado pela Verrazzano. Os rótulos devem ser usados para especificar atributos de identificação de objetos significativos e relevantes para os usuários.
    
    Aqui, para o namespace `hello-helidon`, estamos anexando um label a ele, que marca esse namespace conforme gerenciado pelo Verrazzano. O _istio-inject=enabled_, ativa um "sidecar" do Istio e, como tal, ajuda a estabelecer um proxy do Istio. Com um proxy Istio, podemos acessar outros serviços Istio, como um gateway Istio. Para adicionar o label ao namespace `hello-helidon` com os atributos mencionados anteriormente, copie o seguinte comando e execute-o no Cloud Shell:
    
        <copy>kubectl label namespace hello-helidon verrazzano-managed=true istio-injection=enabled</copy>
        
3.  Agora, queremos implantar o aplicativo conteinerizado Hello Helidon em _cluster1_. Para isso, precisamos de uma configuração de implantação do Kubernetes. Esta implantação instrui o Kubernetes a criar e atualizar instâncias para o aplicativo Hello Helidon. Aqui, temos o arquivo `hello-helidon-comp.yaml`, que instrui o Kubernetes.
    
    Para implantar o aplicativo Hello Helidon, copie e cole os dois comandos a seguir, conforme mostrado. O arquivo `hello-helidon-comp.yaml` contém definições de vários componentes do OAM, em que um componente do OAM é um Recurso Personalizado do Kubernetes que descreve a composição geral e os requisitos de ambiente de um aplicativo.
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/verrazzano/verrazzano/v1.5.2/examples/hello-helidon/hello-helidon-comp.yaml -n hello-helidon</copy>
        
    
    O arquivo `hello-helidon-app.yaml` é um arquivo de configuração do aplicativo Verrazzano, que fornece personalizações específicas do ambiente.
    
        <copy>kubectl apply -f https://raw.githubusercontent.com/verrazzano/verrazzano/v1.5.2/examples/hello-helidon/hello-helidon-app.yaml -n hello-helidon</copy>
        
    
    > A ação de implantar o aplicativo com verrazzano executa esta tarefa inteligente de gerenciamento de carga de trabalho.
    
    *   Criando contas de Serviço e política de autorização, se necessário
    *   Implantar o aplicativo
    *   Configure o gateway e os serviços virtuais conforme necessário.
    *   Definir certificados, segredos e políticas de rede conforme necessário com o Istio.
    *   Kick de gerenciamento de observabilidade automatizado
        *   Sucateamento de logs em OpenSearch
        *   Colocar todas as informações de monitoramento em Prometheus e Grafana
4.  Aguarde que os pods estejam no status _Executando_. Use este comando _kubectl_ para aguardar que todos os pods estejam no estado _Executando_ dentro do namespace hello-helidon. Demora cerca de 1 a 2 minutos.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s</copy>
        
    
    Quando os pods estiverem prontos, você poderá ver uma resposta semelhante:
    
        $ kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s
        pod/hello-helidon-deployment-58fdd5cd4-94wjf condition met
        
    
    Você também pode listar os pods diretamente para verificar seu status:
    
        $ kubectl  get po -n hello-helidon
        NAME                                       READY   STATUS    RESTARTS   AGE
        hello-helidon-deployment-58fdd5cd4-94wjf   2/2     Running   0          34m
        

## Tarefa 3: Verificar a Implantação Bem-sucedida do Aplicativo Hello Helidon

1.  Verifique o ponto final `/greet`. Para determinar o URL que foi construído com base no IP do balanceador de carga/externo e na configuração do aplicativo, execute o seguinte comando:
    
        <copy>echo https://$(kubectl get gateways.networking.istio.io hello-helidon-hello-helidon-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/greet</copy>
        
    
    Isso imprimirá o URL adequado ao seu ponto final REST, por exemplo:
    
        https://hello-helidon.hello-helidon.xx.xx.xx.xx.nip.io/greet
        
2.  Use este link para testar no seu navegador. No entanto, devido a certificados autoassinados, você precisa aceitar riscos e permitir que o navegador continue o processamento de solicitações.
    
    Talvez seja mais fácil usar `curl` porque a resposta é apenas uma string:
    
        <copy>curl -k https://$(kubectl get gateways.networking.istio.io hello-helidon-hello-helidon-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/greet; echo</copy>
        
    
    Você deverá ver o mesmo resultado recebido durante o desenvolvimento:
    
        {"message":"Hello World!"}
        
3.  Deixe o _Cloud Shell_ aberto; nós o usaremos para o próximo laboratório.
    

## Saiba Mais sobre o Open Application Model

**Componentes do Verrazzano**

Um componente do Verrazzano OAM é um [Recurso Personalizado do Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) que descreve a composição geral e os requisitos de ambiente de um aplicativo.

O código a seguir mostra um componente de aplicativo Helidon simples para o aplicativo Hello Helidon usado neste laboratório. Este recurso descreve um componente que é implementado por uma única imagem do Docker que contém um aplicativo Helidon expondo um único ponto final.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: hello-helidon-component
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: hello-helidon-workload
          labels:
            app: hello-helidon
            version: v1
        spec:
          deploymentTemplate:
            metadata:
              name: hello-helidon-deployment
            podSpec:
              containers:
                - name: hello-helidon-container
                  image: "ghcr.io/verrazzano/example-helidon-greet-app-v1:1.0.0-1-20230126194830-31cd41f"
                  ports:
                    - containerPort: 8080
                      name: http
    

Uma breve descrição de cada campo do componente:

*   **apiVersion** - Versão da definição de recurso personalizado do componente
*   **kind** - Nome padrão da definição de recurso personalizado do componente
*   **metadata.name** - O nome usado para criar o recurso personalizado do componente
*   **metadata.namespace** - O namespace usado para criar o recurso personalizado deste componente
*   **spec.workload.kind** - VerrazzanoHelidonWorkload define uma carga de trabalho sem monitoramento de estado do Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.metadata.name** - O nome usado para criar a carga de trabalho sem monitoramento de estado do Kubernetes
*   **spec.workload.spec.deploymentTemplate.podSpec.containers** - Os contêineres de implementação
*   **spec.workload.spec.deploymentTemplate.podSpec.containers.ports** - Portas expostas pelo contêiner

**Configuração do Aplicativo Verrazzano**

Uma configuração de aplicativo Verrazzano é um Recurso Personalizado do Kubernetes que fornece personalizações específicas do ambiente. O código a seguir mostra a configuração do aplicativo para o exemplo de _quickstart-mp_ Helidon usado neste laboratório. Este recurso especifica a implantação do aplicativo no namespace hello-helidon.

Recursos adicionais de runtime são especificados usando características ou sobreposições de runtime que aumentam a carga de trabalho. Por exemplo, a característica de entrada especifica o host e o caminho de entrada, enquanto a característica de métricas fornece o raspador Prometheus usado para obter as métricas relacionadas ao aplicativo.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: hello-helidon
      annotations:
        version: v1.0.0
        description: "Hello Helidon application"
    spec:
      components:
        - componentName: hello-helidon-component
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
                  name: hello-helidon-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/greet"
                          pathType: Prefix
    

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

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023