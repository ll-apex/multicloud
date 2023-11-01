# Implante o Aplicativo Helidon na Verrazzano

## Introdução

Este laboratório orienta você no processo de implantação do aplicativo Helidon quickstart-mp.

Tempo Estimado: 10 minutos

### Verrazzano e Implantação de Aplicativo

O Verrazzano suporta a definição de aplicativo usando o [Open Application Model (OAM)](https://oam.dev/). Os aplicativos Verrazzano são compostos de componentes e configurações de aplicativos.

Quando você implanta aplicativos com a Verrazzano, a plataforma configura conexões e políticas de rede, entra na malha de serviços e dispara uma pilha de monitoramento para capturar as métricas, logs e rastreamentos. O Verrazzano emprega componentes do OAM para definir as unidades funcionais de um sistema que, em seguida, são montadas e configuradas por meio da definição das configurações de aplicativos associadas.

### Componentes do Verrazzano

Um componente do Verrazzano OAM é um [Recurso Personalizado do Kubernetes](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) que descreve a composição geral e os requisitos de ambiente de um aplicativo.

O código a seguir mostra um componente de aplicativo Helidon simples para o aplicativo _quickstart-mp_ Helidon usado neste laboratório. Este recurso descreve um componente que é implementado por uma única imagem do Docker que contém um aplicativo Helidon expondo um único ponto final.

    apiVersion: core.oam.dev/v1alpha2
    kind: Component
    metadata:
      name: hello-helidon-component
      namespace: hello-helidon
    spec:
      workload:
        apiVersion: oam.verrazzano.io/v1alpha1
        kind: VerrazzanoHelidonWorkload
        metadata:
          name: hello-helidon-workload
          labels:
            app: hello-helidon
        spec:
          deploymentTemplate:
            metadata:
              name: hello-helidon-deployment
            podSpec:
              containers:
                - name: hello-helidon-container
                  image: "END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp:1.0"
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

### Configuração do Aplicativo Verrazzano

Uma configuração de aplicativo Verrazzano é um Recurso Personalizado do Kubernetes que fornece personalizações específicas do ambiente. O código a seguir mostra a configuração do aplicativo para o exemplo de _quickstart-mp_ Helidon usado neste laboratório. Este recurso especifica a implantação do aplicativo no namespace hello-helidon.

Recursos adicionais de runtime são especificados usando características ou sobreposições de runtime que aumentam a carga de trabalho. Por exemplo, a característica de entrada especifica o host e o caminho de entrada, enquanto a característica de métricas fornece o raspador Prometheus usado para obter as métricas relacionadas ao aplicativo.

    apiVersion: core.oam.dev/v1alpha2
    kind: ApplicationConfiguration
    metadata:
      name: hello-helidon-appconf
      namespace: hello-helidon
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
                    port: 8080
                    scraper: verrazzano-system/vmi-system-prometheus-0
            - trait:
                apiVersion: oam.verrazzano.io/v1alpha1
                kind: IngressTrait
                metadata:
                  name: hello-helidon-ingress
                spec:
                  rules:
                    - paths:
                        - path: "/help/allGreetings"
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

### Objetivos

Neste laboratório, você vai:

*   Verifique a instalação bem-sucedida do ambiente Verrazzano.
*   Implante o aplicativo Helidon _quickstart-mp_.
*   Verifique a implantação do aplicativo Helidon _quickstart-mp_.

### Pré-requisitos

Para executar este laboratório, você deve ter:

*   Cluster do Kubernetes (OKE) executado no Oracle Cloud Infrastructure.
*   A instalação do Verrazzano foi iniciada em um cluster do Kubernetes (OKE).
*   Aplicativo Helidon _quickstart-mp_ empacotado em contêiner disponível em um registro de contêiner.

## Tarefa 1: Verificação de uma instalação bem-sucedida do Verrazzano

O Verrazzano instala vários objetos em vários namespaces. Os componentes Verrazzano são instalados no namespace _verrazzano-system_.

1.  Verifique se todos os pods associados aos vários objetos têm um status _Em Execução_. Você terá pods no estado _Executando_ conforme mostrado abaixo.
    
        <copy>kubectl get pods -n verrazzano-system</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        $   kubectl get pods -n verrazzano-system
        NAME                                     READY   STATUS    RESTARTS  AGE
        coherence-operator-b5dc669c6-rk2sm       1/1     Running     4       46m
        fluentd-54f5x                            2/2     Running     2       46m
        fluentd-h7mgh                            2/2     Running     1       46m
        fluentd-xcdfz                            2/2     Running     0       46m
        oam-kubernetes-runtime-5b48f944b-cx7b9   1/1     Running     0       46m
        verrazzano-application-operator-665c5c94 1/1     Running     0       46m
        verrazzano-application-operator-webhook  1/1     Running     0       46m
        verrazzano-authproxy-67776ff58b-8lkzs    3/3     Running     0       46m
        verrazzano-cluster-operator-67dc569555   1/1     Running     0       46m
        verrazzano-cluster-operator-webhook-57f  1/1     Running     0       46m
        verrazzano-console-7d95c98cb9-9ql2x      2/2     Running     0       46m
        verrazzano-monitoring-operator-59ff9576  2/2     Running     0       46m
        vmi-system-es-master-0                   2/2     Running     0       46m
        vmi-system-grafana-7fd956b585-2tbgl      3/3     Running     0       46m
        vmi-system-kiali-dd87546d6-ddxss         2/2     Running     0       46m
        vmi-system-osd-7687d6fccf-nm7kt          2/2     Running     0       46m
        weblogic-operator-54979449f4-njgrq       2/2     Running     0       46m
        weblogic-operator-webhook-f7ff8c8cf      1/1     Running     0       46m
        

## Tarefa 2: Implantar o aplicativo Helidon quickstart-mp

1.  Faça download do arquivo yaml do componente do Verrazzano OAM e dos arquivos de Configuração do Aplicativo Verrazzano no ambiente do Cloud Shell:
    
        <copy>
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-app.yaml >~/hello-helidon-app.yaml
        curl -LSs https://oracle-livelabs.github.io/multicloud/helidon-verrazzano/hello-helidon-comp.yaml >~/hello-helidon-comp.yaml
        cd ~
        </copy>
        
2.  Modifique o nome da imagem em _hello-helidon-comp.yaml_. Você pode usar o editor `vi`:
    
        <copy>vi ~/hello-helidon-comp.yaml</copy>
        
3.  Use `i` para alterar o modo de inserção e modificar o nome da imagem para refletir o caminho do repositório na linha 23:
    
        image: "END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/quickstart-mp-your_first_name:1.0"
        
    
    Por exemplo:
    
        image: "ocir.io/tenancynamespace/quickstart-mp-your_first_name:1.0"
        
4.  Use `Esc` o modo de inserção de encerramento e digite `:wq` para salvar as alterações e fechar o editor.
    
5.  Crie um namespace `hello-helidon` para o aplicativo quickstart-mp Helidon. Manteremos todos os artefatos do Kubernetes em um namespace separado.
    
        <copy>
        kubectl create namespace hello-helidon
        </copy>
        
    
    > Namespaces são uma forma de organizar clusters em subclusters virtuais. Podemos ter qualquer número de namespaces dentro de um cluster, cada um logicamente separado dos outros, mas com a capacidade de comunicação entre si.
    
6.  Precisamos deixar a Verrazzano ciente de que armazenamos nesse namespace artefatos da Verrazzano. Portanto, precisamos adicionar um label identificando o namespace `hello-helidon`, conforme gerenciado pela Verrazzano. Os rótulos devem ser usados para especificar atributos de identificação de objetos significativos e relevantes para os usuários.
    
    Aqui, para o namespace `hello-helidon`, estamos anexando um label a ele, que marca esse namespace conforme gerenciado pelo Verrazzano. O _istio-inject=enabled_, ativa um "sidecar" do Istio e, como tal, ajuda a estabelecer um proxy do Istio. Com um proxy Istio, podemos acessar outros serviços Istio, como um gateway Istio. Para adicionar o label ao namespace `hello-helidon` com os atributos mencionados anteriormente, copie o seguinte comando e execute-o no Cloud Shell:
    
        <copy>
        kubectl label namespace hello-helidon verrazzano-managed=true istio-injection=enabled
        </copy>
        
7.  Agora, queremos implantar o aplicativo em contêiner _quickstart-mp_ Helidon em _cluster1_. Para isso, precisamos de uma configuração de implantação do Kubernetes. Essa implantação instrui o Kubernetes a criar e atualizar instâncias para o aplicativo _quickstart-mp_ Helidon. Aqui, temos o arquivo `hello-helidon-comp.yaml`, que instrui o Kubernetes.
    
    Para implantar o aplicativo Helidon _quickstart-mp_, copie e cole os dois comandos a seguir, conforme mostrado. O arquivo `hello-helidon-comp.yaml` contém definições de vários componentes do OAM, em que um componente do OAM é um Recurso Personalizado do Kubernetes que descreve a composição geral e os requisitos de ambiente de um aplicativo.
    
        <copy>kubectl apply -f ~/hello-helidon-comp.yaml -n hello-helidon</copy>
        
    
    O arquivo `hello-helidon-app.yaml` é um arquivo de configuração do aplicativo Verrazzano, que fornece personalizações específicas do ambiente.
    
        <copy>kubectl apply -f ~/hello-helidon-app.yaml -n hello-helidon</copy>
        
8.  Aguarde que os pods estejam no status _Executando_. Use este comando _kubectl_ para aguardar que todos os pods estejam no estado _Executando_ dentro do namespace hello-helidon. Demora cerca de 1 a 2 minutos.
    
        <copy>kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s</copy>
        
    
    Quando os pods estiverem prontos, você poderá ver uma resposta semelhante:
    
        $ kubectl wait --for=condition=Ready pods --all -n hello-helidon --timeout=600s
        pod/hello-helidon-deployment-58fdd5cd4-94wjf condition met
        
    
    Você também pode listar os pods diretamente para verificar seu status:
    
        $ kubectl  get po -n hello-helidon
        NAME                                       READY   STATUS    RESTARTS   AGE
        hello-helidon-deployment-58fdd5cd4-94wjf   2/2     Running   0          34m
        

## Tarefa 3: Verificar a Implantação Bem-sucedida do Aplicativo Helidon quickstart-mp

1.  Verifique o ponto final `help/allGreetings`. Para determinar o URL que foi construído com base no IP do balanceador de carga/externo e na configuração do aplicativo, execute o seguinte comando:
    
        <copy>echo https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings</copy>
        
    
    Isso imprimirá o URL adequado ao seu ponto final REST, por exemplo:
    
        https://hello-helidon-appconf.hello-helidon.xx.xx.xx.xx.nip.io/help/allGreetings
        
2.  Use este link para testar no seu navegador. No entanto, devido a certificados autoassinados, você precisa aceitar riscos e permitir que o navegador continue o processamento de solicitações.
    
    Talvez seja mais fácil usar `curl` porque a resposta é apenas uma string:
    
        <copy>curl -k https://$(kubectl get gateway hello-helidon-hello-helidon-appconf-gw -n hello-helidon -o jsonpath={.spec.servers[0].hosts[0]})/help/allGreetings; echo</copy>
        
    
    Você deverá ver o mesmo resultado recebido durante o desenvolvimento:
    
        [Hello, Привет, Hola, Hallo, Ciao, Nǐ hǎo, Marhaba, Olá]
        
3.  Deixe o _Cloud Shell_ aberto; nós o usaremos para o próximo laboratório.
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023