# Introdução

## Sobre este Workshop

A implantação de um aplicativo Tomcat em um cluster do Kubernetes pode ser um processo complexo que envolve o gerenciamento de vários contêineres, a configuração de redes e a garantia de alta disponibilidade e escalabilidade. Para simplificar esse processo, muitas organizações estão se voltando para tecnologias de conteinerização, como Docker e Kubernetes.

O Docker permite a criação de imagens de contêiner portáteis que podem ser implantadas em qualquer plataforma, enquanto o Kubernetes fornece um avançado mecanismo de orquestração que pode gerenciar a implantação e o dimensionamento de contêineres.

O Oracle Verrazzano é uma plataforma nativa da nuvem que fornece uma experiência de gerenciamento unificada para implantar e operar aplicativos em contêineres em clusters do Kubernetes. Ele simplifica a implantação de aplicativos complexos, fornecendo um conjunto consistente de ferramentas e APIs que podem ser usadas para gerenciar e monitorar aplicativos em vários clusters.

Neste workshop, vamos explorar o processo de implantação de uma imagem do Docker do aplicativo Tomcat em um cluster do Oracle Kubernetes usando o Verrazzano. Abordaremos as etapas envolvidas na criação de uma imagem do Docker, na configuração do Verrazzano, na implantação do aplicativo e no monitoramento de seu desempenho.

No final deste workshop, você terá uma compreensão clara de como implantar aplicativos em contêineres em um cluster do Oracle Kubernetes usando o Verrazzano.

Este workshop é projetado para ser o mais autoexplicativo possível, mas sinta-se livre para pedir esclarecimentos ou assistência ao longo do caminho.

Tempo Estimado: 90 minutos

### Objetivos

*   Configure sua conta do Oracle Cloud Free Tier (se você ainda não tiver feito isso).
*   Configure uma instância do Oracle Kubernetes Engine no Oracle Cloud Infrastructure.
*   Instale o perfil de produção da Verrazzano.
*   Crie uma imagem do docker do aplicativo tomcat de amostra.
*   Implante o aplicativo tomcat no OKE usando o Verrazzano.
*   Explore a console do Painel de Controle do Grafana, Prometheus e OpenSearch.

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Saiba Mais

**Sobre o Tomcat**

O Apache Tomcat é um servidor Web de código aberto e um contêiner de servlet amplamente usado para implantar aplicativos Web baseados em Java. O Tomcat foi projetado para ser leve, eficiente e fácil de usar e fornece um rico conjunto de recursos para gerenciar e implantar aplicativos web.

**Sobre a Verrazzano**

A Verrazzano é uma plataforma de contêiner empresarial completa para implantar aplicativos nativos da nuvem e tradicionais em ambientes híbridos e com várias nuvens. É composto por um conjunto curado de componentes de código aberto - muitos que você já pode usar e confiar, e alguns que foram escritos especificamente para reunir todas as peças que tornam o Verrazzano uma plataforma coesa e fácil de usar.

A Verrazzano inclui os seguintes recursos:

*   Gerenciamento de carga de trabalho híbrido e multicluster
*   Manuseio especial para aplicações WebLogic, Coherence e Helidon
*   Gerenciamento de infraestrutura multicluster
*   Monitoramento integrado e pré-com fio de aplicativos
*   Segurança integrada
*   Ativação DevOps e GitOps

![Verrazzano](images/verrazzano.png)

*   [https://tomcat.apache.org/](https://tomcat.apache.org/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023