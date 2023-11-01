# Acesse o aplicativo de amostra Bobby's Books Explore Verrazzano, Grafana e Kiali Console

## Introdução

No Laboratório 3, disponibilizamos o aplicativo Bobby's Book. Neste laboratório, acessaremos o aplicativo e verificaremos se ele está funcionando. Mais tarde, exploraremos os consoles Verrazzano e Grafana.

Tempo estimado: 10 minutos

### Objetivos

Neste laboratório, você vai:

*   Acesse o aplicativo Livro de Bobby.
*   Explore o console Verrazzano.
*   Explore a console do Grafana.

### Pré-requisitos

\* Execute o Laboratório 1, que cria um cluster do OKE no Oracle Cloud Infrastructure. \* Run Lab 1, que configura o kubectl para acessar um cluster do OKE no Oracle Cloud Infrastructure.

*   Execute o Laboratório 2, que instala o Verrazzano em um cluster do Kubernetes.
*   Execute o Laboratório 3, que implementa o aplicativo Livro de Bobby.
*   Você deve ter um editor de texto, no qual pode colar os comandos e URLs e modificá-los, conforme seu ambiente. Em seguida, você pode copiar e colar os comandos modificados para executá-los no _Cloud Shell_.
*   Recomendamos usar o Firefox para abrir os URLs da aplicação Bobby's Books, Verrazano, Grafana e Kiali Console. No entanto, se você quiser usar o Chrome, precisará usar a solução alternativa 'thisisunsafe' não documentada para permitir que o Chrome aceite o certificado.

## Tarefa 1: Acessar a aplicação do Livro de Bobby

1.  Precisamos de um endereço `EXTERNAL_IP` através do qual podemos acessar o aplicativo Livro de Bobby. Para obter o endereço `EXTERNAL_IP` do serviço istio-ingressgateway, copie o comando a seguir e cole-o no _Cloud Shell_.
    
        <copy> kubectl get service \
        -n "istio-system" "istio-ingressgateway" \
        -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
           $ kubectl get service \
           > -n "istio-system" "istio-ingressgateway" \
           > -o jsonpath={.status.loadBalancer.ingress[0].ip}; echo
           XX.XX.XX.XX
        
2.  Para abrir a Home Page da Loja de Livros do Robert, copie o URL a seguir e substitua _XX.XX.XX.XX_ pelo endereço _EXTERNAL\_IP_ que obtivemos na última etapa, conforme mostrado na imagem a seguir.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/</copy>
        
    
    ![Página de Livros Roberts](images/robertbooks.png " ")
    
3.  Clique em _Avançado_, conforme mostrado:
    
    ![Robert Advanced](images/robertadvanced.png " ")
    
4.  Selecione _Prosseguir para bobs-books.bobs-books. EXTERNAL\_IP .nip.io(não seguro)_ para acessar o aplicativo. Se você não está obtendo essa opção para continuar, digite _thisisunsafe_ sem espaço em qualquer lugar dentro dessa janela do navegador Chrome. Como você está digitando na janela do navegador Chrome, não pode vê-la, mas assim que terminar de digitar _thisisunsafe_, você pode ver a página do aplicativo imediatamente. Você pode encontrar mais detalhes [aqui](https://verrazzano.io/latest/docs/faq/faq/#enable-google-chrome-to-accept-self-signed-verrazzano-certificates).
    
    ![Robert Unsafe](images/robertunsafe.png " ") ![Robert Page](images/robertpage.png " ")
    
5.  Para abrir a Home page da Loja de Livros do Bobby, abra uma nova guia e copie o URL a seguir e substitua _XX.XX.XX.XX_ pelo endereço `EXTERNAL_IP`, conforme mostrado na imagem a seguir.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![URL do Bobbys](images/bobbysurl.png " ")
    
    ![Bobby livraria](images/bobbysbookstore.png " ")
    
    > Deixe esta página aberta porque a usaremos no Laboratório 8.
    
6.  Para abrir a IU do Gerenciador de Ordem de Livro de Bobby, abra uma nova guia e copie o URL a seguir e substitua _XX.XX.XX.XX_ pelo endereço _EXTERNAL\_IP_ conforme mostrado na imagem a seguir.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobs-bookstore-order-manager/orders</copy>
        
    
    ![gerente de pedidos](images/ordermanager.png " ")
    
7.  Volte para a página _Livros de Bobby_ e vamos comprar um livro. Clique em _Livros_ conforme mostrado na imagem a seguir.
    
    ![Reservar ordem](images/checkoutorder.png " ")
    
8.  Selecione a imagem do Livro _Twilight_, conforme mostrado na imagem a seguir.
    
    ![Livro de compras](images/purchasebook.png " ")
    
9.  Primeiro, clique em _Adicionar ao carrinho_ e depois em _Fazer Check-out_, conforme mostrado na imagem a seguir.
    
    ![Clique em addcart](images/clickaddcart.png " ")
    
10.  Informe os detalhes para a compra do livro. Para _Seu Estado_, informe seu código de estado de dois dígitos e clique em _Enviar Pedido_.
    

![Enviar Ordem](images/submitorder.png " ") 11. Volte para a página _Gerenciador de Pedidos_ e selecione o botão _Atualizar_ para verificar se seu pedido foi registrado com êxito no gerenciador de pedidos.

![Verificar Ordem](images/verify-order.png " ")

## Tarefa 2: Explorar o Rancher Console

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
    
10.  Na home page do console rancher, você pode visualizar os Links Verrazzano. Clique em _Menu do hambúrguer_ -> _Verrazzano_.
    
    ![Rancher Home](images/rancher-home.png)
    
11.  Clique em _Aplicativos_. Esta seção mostra todos os aplicativos com seu namespace e é gerenciada pela Verrazzano. Clique no aplicativo _bobs-books_ no namespace _bobs-books_. ![Aplicativo Bobs](images/bobs-application.png)
    
12.  Você pode exibir os pods associados ao aplicativo. O nome do pod contém uma string exclusiva gerada automaticamente para identificar essa réplica específica. Para exibir os logs dos pods _bobbys-helidon-stok-application_, clique em _Três pontos_ -> _Exibir Logs_. ![Logs do Bobbys](images/bobs-logs.png)
    
13.  Você pode exibir os logs do aplicativo. Se você não puder ver o log do aplicativo, clique nas **Definições** (botão azul com o ícone de engrenagem) e altere o filtro de tempo para mostrar todas as entradas de log do início do contêiner. Para exibir o Componente associado ao aplicativo, clique em _Componentes_. ![Bobbys Log](images/bobs-log.png)
    
14.  Você pode exibir todos os componentes do aplicativo _bobs-books_ aqui. Para exibir quais são os recursos relacionados, clique em _Recursos Relacionados_. ![Bobs Resource](images/bobs-resource.png) ![Bobs Resources](images/bobs-resources.png)
    
15.  Clique em _Menu Hambúrgar_ -> _local_ para abrir o _Cluster Explorer_. O _Cluster Explorer_ permite exibir e manipular todos os recursos personalizados e CRDs em um cluster do Kubernetes na IU do Rancher. ![Cluster Verrazzano](images/verrazzano-cluster.png)
    
16.  O painel fornece uma visão geral do cluster e dos aplicativos implantados. O número de recursos pertence aos _Namespaces do Usuário_, que são praticamente todos os recursos, inclusive o sistema. Você pode filtrar por namespace na parte superior do painel de controle, mas isso não é necessário agora. Clique no item **Nós** no menu esquerdo para obter uma visão geral do carregamento atual dos nós.
    
    ![Explorador do Cluster](images/cluster-dashboard.png)
    
17.  A implantação inteira não tem impacto no cluster do OKE. Agora clique no item **Implantação** no menu do lado esquerdo para verificar os aplicativos implantados.
    
    ![nós clustr](images/cluster-nodes.png)
    
18.  Você pode ver várias implantações.
    
    ![Implementações](images/cluster-deployments.png)
    

## Tarefa 3: Explorar a Console do Grafana

1.  Clique em _Menu Hambúrgar_ -> _Home_ para abrir a home page do Rancher. ![Início](images/ranchar-menu.png)
    
2.  Na home page, você verá o link para abrir a _console do Grafana_. Clique no link da **Console do Grafana** conforme mostrado:
    
    ![Home do Grafana](images/grafana-link.png)
    
3.  Clique em _Avançado_.
    
    ![Grafana Avançado](images/grafanaadvanced.png " ")
    
4.  Selecione _grafana.vmi.system.default.XX.XX.XX.XX.nip.io(não seguro)_. Se você não está obtendo esta opção para continuar, digite _thisisunsafe_ sem espaço.
    
    ![Grafana continuar](images/grafanaproceed.png " ")
    
5.  A Home Page do Grafana é aberta. Selecione _Home_ na parte superior esquerda.
    
    ![Página inicial do Grafana](images/grafana-homepage.png " ")
    
6.  Digite _WebLogic_ e você verá _WebLogic Painel de Controle do Servidor_ em _Geral_. Selecione _WebLogic Painel de Controle do Servidor_.
    
    !\[Pesquisar WebLogic\](imagens/pesquisa-weblogic.png" ")
    
    Aqui você pode observar os dois domínios em _Domínio_ e Servidores em Execução, Aplicativos Implantados, Nome do Servidor e seus Status, Uso de Heap, Tempo de Execução, Heap da JVM. Se seu aplicativo tiver recursos como JDBC e JMS, você também poderá obter detalhes sobre ele aqui.
    
    ![WebLogic Painel de Controle](images/weblogic-dashboard.png " ")
    
7.  Agora, selecione WebLogic Painel de Controle do Servidor e digite _Helidon_ e você verá _Painel de Monitoramento do Helidon_. Selecione _Painel de Controle de Monitoramento do Helidon_.
    
    ![Helidon](images/search-helidon.png " ")
    
    Aqui você pode ver vários detalhes como o _Status_ do seu aplicativo e seu _Tempo de Atividade_, Coletor de Lixo e Marcar Total de Varredura e seu Tempo, Contagem de Threads.
    
    ![Painel de Controle do Helidon](images/helidon-dashboard.png " ")
    
8.  Agora, selecione Painel de Controle de Monitoramento Helidon e digite _Coherence_. Você verá _Painel Principal do Coherence_. Selecione _Painel Principal do Coherence_.
    
    ![Coerência](images/search-coherence.png " ")
    
9.  Aqui você pode ver os detalhes do _Cluster do Coherence_. Para a aplicação dos Livros de Bobby, temos dois clusters do Coherence, um para Livros de Bobby e outro para Livros de Robert. Você precisa selecionar o menu drop-down para _Nome do Cluster_ para exibir ambos os clusters.
    
    ![Bobby Cluster](images/bobby-cluster.png " ")
    
    ![Robert Cluster](images/robert-cluster.png " ")
    

## Tarefa 4: Explorar a Console Kiali

1.  Volte para a home page do Verrazzano e clique no console **Kiali**.
    
    ![Link Rancher](images/kiali-link.png)
    
2.  No lado esquerdo, clique em Gráfico.
    
    ![Painel Kiali](images/kiali-dashboard.png " ")
    
3.  No menu suspenso Namespace, marque a caixa para _bobs-books_ e faça com que o curser se mova para fora do menu suspenso. ![Namespace Bobs](images/bobs-namespace.png " ")
    
4.  Você pode exibir a exibição gráfica do aplicativo _bobs-books_. Clique em _Legenda_ para exibir a exibição Legenda.
    
    ![View Gráfica](images/graphical-view.png " ")
    
5.  Aqui você pode exibir o que cada forma representa, como círculo representa as _Cargas de Trabalho_.
    
    ![Exibição de Legenda](images/legend-view.png " ")
    
6.  No lado esquerdo, clique em _Aplicativos_ para exibir todos os aplicativos implantados.
    
    ![Aplicativos](images/kiali-applications.png " ")
    

## Tarefa 5: Explorar os Painéis de Controle OpenSearch

1.  Volte para a home page do Verrazzano e clique na console **OpenSearch Dashboards**.
    
    ![Link do Kibana](images/opensearch-link.png)
    
2.  Clique em _Prosseguir até ... padrão XX.XX.XX.XX.nip.io(não seguro)_, se necessário.
    
3.  Na home page OpenSearch, clique em **Home** -> **Descobrir**.
    
    ![Clique no painel de controle Kibana](images/discover-1.png)
    
4.  Selecione o namespace _`verrazzano-applications*`_ conforme mostrado, em seguida, procure _livros_ e pressione **Enter** ou clique em **Atualizar**. Você deverá obter os logs que contêm _livros_. ![Resultado do log](images/search-books.png)
    

## Tarefa 6: Explorar a Console do Prometheus

1.  Volte para a home page do Verrazzano e clique no console **Prometheus**.
    
    ![Link Prometheus](images/prometheus-link.png)
    
2.  Clique em **Prosseguir até ... padrão XX.XX.XX.XX.nip.io(não seguro)**, se solicitado.
    
3.  No tipo de página do painel Prometheus, _obtenha_ no campo de pesquisa e clique em sua métrica personalizada _`application_org_books_bobby_BookResource_getBook_total`_.
    
    ![Execução do Prometheus](images/prometheus-query.png)
    
4.  Clique em **Executar** e verifique o resultado abaixo. Você deverá ver o valor atual da métrica, o que significa quantas solicitações foram concluídas pelo ponto final. Você também pode alternar para a view _Gráfico_ em vez do modo _Console_.
    
    ![Valor de Prometheus](images/execute-query.png)
    
    > Você também pode adicionar outra métrica ao painel. Descubra as métricas padrão disponíveis na lista.
    

## Tarefa 7: Explorar a Console do Keycloak

1.  Volte para a home page do Verrazzano e clique no console **Keycloak**.
    
    ![Link Keycloak](images/keycloak-link.png)
    
2.  Clique em **Prosseguir até ... padrão XX.XX.XX.XX.nip.io(não seguro)**, se solicitado.
    
3.  Na página Bem-vindo ao Keycloak, clique em _Console de Administração_. ![Home do Keycloak](images/keycloak-home.png)
    
4.  Agora precisamos do nome de usuário e da senha para o console Keycloak. _O nome de usuário_ é _keycloakadmin_ e para descobrir a senha, volte ao _Cloud Shell_ e cole o comando a seguir para descobrir a senha da _Console do Keycloak_.
    
        <copy>kubectl get secret --namespace keycloak keycloak-http -o jsonpath={.data.password} | base64  --decode; echo</copy>
        
5.  Copie a senha e volte ao browser, onde a _Console do Keycloak_ está aberta. Cole a senha no campo _Senha_ e digite _keycloakadmin_ como _Nome do Usuário_ e clique em **Efetuar Sign-in**.
    
    ![SignIn](images/keycloak-sign-in.png)
    
6.  Aqui você pode exibir a configuração padrão feita pelo Verrazzano. ![Home do Keycloak](images/keycloak-realms.png)
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023