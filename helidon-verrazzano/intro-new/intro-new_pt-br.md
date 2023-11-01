# Introdução

## Sobre este Workshop

Este laboratório oferece aos participantes uma sessão prática de nível introdutório com o Oracle Kubernetes Engine, Verrazzano e Helidon. Você aprenderá a implantar uma amostra de aplicativo hello helidon no OKE usando o Verrazzano.

Durante este laboratório, você configurará o Oracle Kubernetes Engine (OKE) no OCI (Oracle Cloud Infrastructure) usando a conta do Oracle Cloud Free Tier. A conta do Free Tier é suficiente para explorar e aprender a executar e operar aplicativos de microsserviços em um nível empresarial.

O objetivo deste workshop é que você aprenda os conceitos básicos do uso do OKE e do Verrazzano e entenda como eles podem ajudá-lo em sua empresa. Se você quiser saber mais sobre os recursos desses projetos, continue a explorar usando sua conta na nuvem Oracle Free Tier e o Oracle Cloud Infrastructure.

Este workshop é projetado para ser o mais autoexplicativo possível, mas sinta-se livre para pedir esclarecimentos ou assistência ao longo do caminho.

Tempo Estimado: 60 minutos

### Objetivos

*   Configure sua conta do Oracle Cloud Free Tier (se você ainda não tiver feito isso).
*   Configure uma instância do Oracle Kubernetes Engine no Oracle Cloud Infrastructure.
*   Instale a plataforma Verrazzano.
*   Implante o aplicativo _Simple Hello Helidon_.
*   Monitore o aplicativo _Simple Hello Helidon_ usando ferramentas Verrazzano, incluindo Rancher, Opensearch, Grafana e Prometheus.

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Sobre o Helidon

O Helidon é uma estrutura de microsserviços de código-fonte aberto introduzida pela Oracle que fornece uma coleção de bibliotecas Java projetadas para criar aplicativos leves e rápidos baseados em microsserviços. A estrutura suporta dois modelos de programação para gravar microsserviços: Helidon SE e Helidon MP.

Embora o Helidon SE seja projetado para ser uma microframework que suporta o modelo de programação reativa, o Helidon MP é uma implementação da especificação MicroProfile. Como o MicroProfile tem suas raízes no Java EE, as APIs MicroProfile seguem uma abordagem familiar declarativa com uso pesado de anotações. Isso o torna uma boa opção para desenvolvedores Java EE.

Os recursos MicroProfile visam à implementação de microsserviços. Você pode encontrar APIs para definir Clientes REST, monitorar o aplicativo, ler estatísticas técnicas e funcionais e configurar o aplicativo. A Helidon também adicionou APIs adicionais ao conjunto básico de APIs Microprofile, o que oferece todos os recursos necessários para criar aplicativos modernos nativos da nuvem.

> O padrão [MicroProfile](https://microprofile.io/) é baseado em Jakarta EE. Assim como Jakarta EE, MicroProfile é de código aberto e é desenvolvido pela Eclipse Foundation. A implementação com MicroProfile ocorre nas bibliotecas ou servidores de aplicativos que implementam o padrão, assim como Jakarta EE.

## Sobre a Verrazzano

A Verrazzano é uma plataforma de contêiner empresarial completa para implantar aplicativos nativos da nuvem e tradicionais em ambientes híbridos e com várias nuvens. É composto por um conjunto curado de componentes de código aberto - muitos que você já pode usar e confiar, e alguns que foram escritos especificamente para reunir todas as peças que tornam o Verrazzano uma plataforma coesa e fácil de usar.

A Verrazzano inclui os seguintes recursos:

*   Gerenciamento de carga de trabalho híbrido e multicluster
*   Manuseio especial para aplicações WebLogic, Coherence e Helidon
*   Gerenciamento de infraestrutura multicluster
*   Monitoramento integrado e pré-com fio de aplicativos
*   Segurança integrada
*   Ativação DevOps e GitOps

![Verrazzano](images/verrazzano.png)

## Saiba Mais

*   [https://helidon.io](https://helidon.io)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, março de 2023