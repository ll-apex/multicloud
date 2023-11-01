# Usar o Verrazzano para Acessar e Monitorar o Aplicativo

## Introdução

Você implantou o aplicativo tomcat. Neste laboratório, vamos acessar o aplicativo e verificar o uso de várias ferramentas de gerenciamento fornecidas pela Verrazzano.

Tempo Estimado: 15 minutos

**Grafana**

O Grafana é uma solução de código aberto para executar análises de dados, obtendo métricas que fazem sentido da enorme quantidade de dados e para monitorar nossos aplicativos com a ajuda de bons painéis personalizáveis. A ferramenta ajuda a estudar, analisar e monitorar dados ao longo de algum tempo, tecnicamente chamados de análises de séries temporais. Útil para rastrear o comportamento do usuário, o comportamento do aplicativo, a frequência de erros que aparecem na produção ou em um ambiente de pré-prod, o tipo de erros que aparecem e os cenários contextuais, fornecendo dados relativos.

[https://grafana.com/grafana/](https://grafana.com/grafana/)

**OpenSearch Painéis de Controle**

OpenSearch Painéis de Controle é um painel de visualização para o conteúdo indexado em um cluster OpenSearch. A Verrazzano cria uma implantação de Painéis de Controle OpenSearch para fornecer uma interface de usuário para consultar e visualizar os dados de log coletados em OpenSearch.

Para acessar a console de Painéis de Controle OpenSearch, leia [Acessar Verrazzano](https://verrazzano.io/latest/docs/access/).

Para ver os registros de um índice OpenSearch por meio de Painéis OpenSearch, crie um padrão de índice para filtrar os registros sob o índice desejado.

Na Tarefa 3, exploraremos os Painéis OpenSearch.

[https://opensearch.org/docs/latest/dashboards/index/](https://opensearch.org/docs/latest/dashboards/index/)

**Prometheus**

Prometheus é um kit de ferramentas de monitoramento e alerta. A Prometheus coleta e armazena suas métricas como dados de séries temporais, ou seja, as informações de métricas são armazenadas com o timestamp no qual foram registradas, juntamente com pares de chave/valor opcionais chamados labels.

[https://prometheus.io/](https://prometheus.io/)

**Rancher**

Rancher é uma plataforma que permite que a Verrazzano execute contêineres em vários clusters do Kubernetes em produção. Ele permite um híbrido, o que significa um único painel de gerenciamento de clusters locais e hospedados em serviços de nuvem.

[https://rancher.com/](https://rancher.com/)

**Kiali**

Kiali é a interface do usuário para Istio, e é super simples de usar. No console Verrazzano, você pode acessar diretamente o Kiali. A partir daí, você pode ver os fluxos de tráfego e quaisquer pontos quentes ou áreas problemáticas. Você pode então fazer drill-down para ver os detalhes de cada um. A Kiali usa dados de métricas do Envoy armazenado na Prometheus para criar o modelo gráfico

### Objetivos

Neste laboratório, você vai:

*   Explore o console Rancher.
*   Explore o console Prometheus.
*   Explore a console do Grafana.
*   Explore os Painéis de Controle OpenSearch.
*   Explore o console Kiali.
*   Explore a console Keycloak.

### Pré-requisitos

*   Cluster do Kubernetes (OKE) executado no Oracle Cloud Infrastructure.
*   Verrazzano instalado em um cluster do Kubernetes (OKE).
*   Aplicativo de amostra de tomcat implantado.

## Tarefa 1: Explorar o Rancher Console

A Verrazzano instala vários consoles. Os pontos finais de uma instalação são armazenados no campo `Status` do Recurso Personalizado Verrazzano instalado.

1.  Você pode obter os pontos finais para essas consoles usando o seguinte comando:
    
        <copy>kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        $ kubectl get vz -o jsonpath="{.items[].status.instance}" | jq .
        {
        "consoleUrl": "https://verrazzano.default.xx.xx.xx.xx.nip.io",
        "grafanaUrl": "https://grafana.vmi.system.default.xx.xx.xx.xx.nip.io",
        "keyCloakUrl": "https://keycloak.default.xx.xx.xx.xx.nip.io",
        "kialiUrl": "https://kiali.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchDashboardsUrl": "https://osd.vmi.system.default.xx.xx.xx.xx.nip.io",
        "openSearchUrl": "https://opensearch.vmi.system.default.xx.xx.xx.xx.nip.io",
        "prometheusUrl": "https://prometheus.vmi.system.default.xx.xx.xx.xx.nip.io",
        "rancherUrl": "https://rancher.default.xx.xx.xx.xx.nip.io"
        }
        $
        
2.  Use `https://rancher.default.YOUR_UNIQUE_IP.nip.io` para abrir a console do Rancher.
    
3.  O perfil Verrazzano _dev_ usa certificados autoassinados; portanto, você precisa clicar em **Avançado** para aceitar riscos e ignorar o aviso.
    
    ![Avançado](images/rancher-advanced.png)
    
4.  Clique em **Prosseguir para o rancheiro padrão XX.XX.XX.XX.nip.io(não seguro)**. Se você não está obtendo essa opção para continuar, digite _thisisunsafe_ sem espaço em qualquer lugar dentro dessa janela do navegador Chrome. Como você está digitando na janela do navegador Chrome, não pode vê-la, mas assim que terminar de digitar _thisisunsafe_, você pode ver a próxima página imediatamente. Você pode encontrar mais detalhes [aqui](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Continuar](images/rancher-proceed.png)
    
5.  Clique em _Login com Keycloak_. ![Log-in do Rancher](images/keycloak-login.png)
    
6.  Como ele redireciona para o URL da console Keycloak para autenticação, clique em **Avançado**.
    
    ![Autenticação Keycloak](images/keycloak-advanced.png)
    
7.  Clique em **Prosseguir para Keycloak padrão XX.XX.XX.XX.nip.io(não seguro)**. Se você não está obtendo essa opção para continuar, digite _thisisunsafe_ sem espaço em qualquer lugar dentro dessa janela do navegador Chrome. Como você está digitando na janela do navegador Chrome, não pode vê-la, mas assim que terminar de digitar _thisisunsafe_, você pode ver a próxima página imediatamente. Você pode encontrar mais detalhes [aqui](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Continuar](images/keycloak-proceed.png)
    
8.  Agora precisamos do nome de usuário e da senha para o console Verrazzano. _O nome de usuário_ é _verrazzano_ e, para descobrir a senha, volte ao _Cloud Shell_ e cole o comando a seguir para descobrir a senha da _Console do Ramcher_.
    
        <copy>kubectl get secret --namespace verrazzano-system verrazzano -o jsonpath={.data.password} | base64 --decode; echo</copy>
        
9.  Copie a senha e volte ao navegador, onde a _Console do Ramcher_ está aberta. Cole a senha no campo _Senha_ e digite _verrazzano_ como _Nome do Usuário_ e clique em **Efetuar Sign-in**.
    
    ![SignIn](images/verrazzano-sign-in.png)
    
10.  Na home page do console do rancheiro, você pode exibir os Links do Verrazzano. Você pode acessar qualquer um desses consoles do rancheiro console.Click _menu do hambúrguer_ -> _Verrazzano_.
    
    ![Rancher Home](images/rancher-home.png)
    
11.  Clique em _Aplicativos_. Esta seção mostra todos os aplicativos com seu namespace e é gerenciada pela Verrazzano. Clique no aplicativo _tomcat-appconf_ no namespace _tomcat-ns_. ![Aplicativo Tomcat](images/tomcat-application.png)
    
12.  Você pode exibir os pods associados ao aplicativo. O nome do pod contém uma string exclusiva gerada automaticamente para identificar essa réplica específica. Para exibir os logs de pods, clique em _Três pontos_ -> _Exibir Logs_. ![Componentes Tomcat](images/view-pod-logs.png)
    
13.  Você pode exibir os logs do aplicativo. Se você não puder ver o log do aplicativo, clique nas **Definições** (botão azul com o ícone de engrenagem) e altere o filtro de tempo para mostrar todas as entradas de log do início do contêiner. Para exibir o Componente associado ao aplicativo, clique em _Componentes_. ![Logs de aplicativo](images/view-pod-log.png)
    
14.  Este aplicativo tem apenas uma view component.To que são os recursos relacionados, clique em _Recursos Relacionados_. ![Recurso Tomcat](images/tomcat-resource.png) ![Recursos do Tomcat](images/tomcat-resources.png)
    
15.  Clique em _Menu Hambúrgar_ -> _local_ para abrir o _Cluster Explorer_. O _Cluster Explorer_ permite exibir e manipular todos os recursos personalizados e CRDs em um cluster do Kubernetes na IU do Rancher. ![Cluster Verrazzano](images/verrazzano-cluster.png)
    
16.  O painel fornece uma visão geral do cluster e dos aplicativos implantados. O número de recursos pertence aos _Namespaces do Usuário_, que são praticamente todos os recursos, inclusive o sistema. Você pode filtrar por namespace na parte superior do painel de controle, mas isso não é necessário agora. Clique no item **Nós** no menu esquerdo para obter uma visão geral do carregamento atual dos nós.
    
    ![Explorador do Cluster](images/cluster-dashboard.png)
    
17.  A implantação inteira não tem impacto no cluster do OKE. Agora clique no item **Implantação** no menu do lado esquerdo para verificar o aplicativo implantado.
    
    ![Nós](images/cluster-nodes.png)
    
18.  Você pode ver várias implantações.
    
    ![Implementações](images/deployment.png)
    

## Tarefa 2: Explorar a Console do Grafana

1.  Clique em _Menu Hambúrgar_ -> _Home_. ![Início](images/ranchar-menu.png)
    
2.  Na home page, você verá o link para abrir a _console do Grafana_. Clique no link da **Console do Grafana** conforme mostrado:
    
    ![Home do Grafana](images/grafana-link.png)
    
3.  Clique em **Avançado**.
    
    ![Avançado](images/grafana-advanced.png)
    
4.  Clique em **Prosseguir para grafana.vmi.system.default.XX.XX.XX.XX.nip.io(não seguro)**. Se você não está obtendo essa opção para continuar, digite _thisisunsafe_ sem espaço em qualquer lugar dentro dessa janela do navegador Chrome. Como você está digitando na janela do navegador Chrome, não pode vê-la, mas assim que terminar de digitar _thisisunsafe_, você pode ver a próxima página imediatamente. Você pode encontrar mais detalhes [aqui](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![continuar](images/grafana-proceed.png)
    
5.  Clique no ícone _+_ e depois clique em _Importar_, conforme mostrado abaixo. ![importar](images/import.png)
    
6.  Informe _6340_ como valor para _Importar via grafana.com_ e clique em _Carregar_. ![ID do carregamento](images/load-id.png)
    
7.  Selecione _Prometheus_ como Origem de Dados padrão e clique em _Importar_. ![Importar Origem de Dados](images/import-datasource.png)
    
8.  Selecione _tomcat-ns_ como namespace e _tomcat-container_ como contêiner. Em seguida, você poderá ver o _Heap Usado_, o _Mem Confirmado_ e outros dados, conforme mostrado abaixo. ![selecionar namespace](images/select-namespace.png)
    
    > Em Grafana.com, você pode obter mais id para importar na console do grafana.
    

## Tarefa 3: Explorar os Painéis de Controle OpenSearch

1.  Volte para a home page do Verrazzano e clique na console **OpenSearch Dashboards**.
    
    ![Link do Kibana](images/opensearch-link.png)
    
2.  Clique em _Prosseguir até ... padrão XX.XX.XX.XX.nip.io(não seguro)_, se necessário. A primeira vez que _OpenSearch Dashboards_ mostra a página de boas-vindas. Ele oferece dados de amostra incorporados para tentar OpenSearch, mas você pode selecionar a opção **Explorar sozinho** porque a Verrazzano concluiu a configuração necessária e os dados do aplicativo já estão disponíveis.
    
    ![Página de boas-vindas do Kibana](images/opensearch-proceed.png)
    
3.  Na home page OpenSearch, clique em **Home** -> **Descobrir**.
    
    ![Clique no painel de controle Kibana](images/discover-1.png)
    
4.  Chame a solicitação HTTP a seguir no Cloud Shell em relação ao seu ponto final. Você pode executar solicitações várias vezes.
    
        <copy>curl -k https://$(kubectl get gateways.networking.istio.io tomcat-ns-tomcat-appconf-gw -n tomcat-ns -o jsonpath={.spec.servers[0].hosts[0]})/sample-webapp/log; echo</copy>
        
5.  Selecione o namespace _`verrazzano-application*`_ conforme mostrado e digite o valor da entrada de log personalizada que você criou no aplicativo tomcat: `Logs` na caixa de texto do filtro. Pressione **Enter** ou clique em **Atualizar**. Você deve obter pelo menos um resultado. ![Resultado do log](images/log-result.png)
    

## Tarefa 4: Explorar a Console Prometheus

1.  Volte para a home page do Verrazzano e clique no console **Prometheus**.
    
    ![Link Prometheus](images/prometheus-link.png)
    
2.  Clique em **Prosseguir até ... padrão XX.XX.XX.XX.nip.io(não seguro)**, se solicitado.
    
3.  No tipo de página do painel Prometheus _tomcat_ no campo de pesquisa e clique na métrica _tomcat\_requestcount\_total_ e, em seguida, clique em _Executar_.
    
    ![Execução do Prometheus](images/prometheus-query.png)
    
4.  Clique em **Executar** e verifique o resultado abaixo. Você deverá ver o valor atual da métrica, o que significa quantas solicitações foram concluídas pelo ponto final. Você também pode alternar para a view _Gráfico_ em vez do modo _Console_.
    
    ![Valor de Prometheus](images/execute-query.png)
    
    > Você também pode adicionar outra métrica ao painel. Descubra as métricas padrão disponíveis na lista.
    

## Tarefa 5: Explorar a Console Kiali

1.  Volte para a home page do Verrazzano e clique no console **Kiali**.
    
    ![Link Rancher](images/kiali-link.png)
    
2.  Clique em **Prosseguir até ... padrão XX.XX.XX.XX.nip.io(não seguro)**, se solicitado.
    
3.  No lado esquerdo, clique em Gráfico.
    
    ![Painel Kiali](images/kiali-dashboard.png " ")
    
4.  No menu suspenso Namespace, marque a caixa de seleção _verrazzano-system_ e faça com que o curser se mova para fora do menu suspenso. ![Namespace Bobs](images/verrazzano-namespace.png " ")
    
5.  Você pode exibir a view gráfica do namespace _verrazzano-system_. Clique em _Legenda_ para exibir a exibição Legenda.
    
    ![View Gráfica](images/graphical-view.png " ")
    
6.  Aqui você pode exibir o que cada forma representa, como círculo representa as _Cargas de Trabalho_.
    
    ![Exibição de Legenda](images/legend-view.png " ")
    
7.  No lado esquerdo, clique em _Aplicativos_ para exibir os aplicativos implantados.
    
    ![Aplicativos](images/applications.png " ")
    

## Tarefa 6: Explorar a Console do Keycloak

1.  Volte para a home page do Verrazzano e clique no console **Keycloak**.
    
    ![Link Keycloak](images/keycloak-link.png)
    
2.  Clique em **Prosseguir até ... padrão XX.XX.XX.XX.nip.io(não seguro)**, se solicitado.
    
3.  Na página Bem-vindo ao Keycloak, clique em _Console de Administração_. ![Home do Keycloak](images/keycloak-home.png)
    
4.  Agora precisamos do nome de usuário e da senha para o console Keycloak. _O nome de usuário_ é _keycloakadmin_ e para descobrir a senha, volte ao _Cloud Shell_ e cole o comando a seguir para descobrir a senha da _Console do Keycloak_.
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  Copie a senha e volte ao browser, onde a _Console do Keycloak_ está aberta. Cole a senha no campo _Senha_ e digite _keycloakadmin_ como _Nome do Usuário_ e clique em **Efetuar Sign-in**.
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  Aqui você pode exibir a configuração padrão feita pelo Verrazzano. ![Home do Keycloak](images/keycloak-realms.png)
    

Parabéns, você concluiu a implantação do aplicativo tomcat no laboratório Verrazzano.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023