# Introdução

## Sobre este Workshop

Neste workshop, vamos explorar como implantar aplicativos Spring Boot em um cluster do Oracle Kubernetes Engine (OKE) usando o Verrazzano.

Na era digital atual, as empresas estão buscando maneiras de desenvolver e implantar aplicativos com mais rapidez e eficiência. O Kubernetes se tornou o padrão de fato para orquestração de contêineres, permitindo que as organizações implantem, gerenciem e escalem aplicativos em contêineres. O Oracle Kubernetes Engine (OKE) fornece um serviço Kubernetes gerenciado que simplifica a implantação e o gerenciamento de aplicativos conteinerizados na Oracle Cloud Infrastructure (OCI).

Spring Boot é uma estrutura popular baseada em Java usada para criar aplicativos web e microsserviços. Ele oferece uma experiência de desenvolvimento simplificada com uma abordagem de convenção sobre configuração, tornando-a uma excelente opção para criar e implantar aplicativos em contêineres no Kubernetes.

A Verrazzano é uma plataforma nativa do Kubernetes de código aberto que fornece uma solução completa de ponta a ponta para implantar e gerenciar aplicativos nativos da nuvem. Ele simplifica a implantação de aplicativos complexos, com vários componentes, fornecendo uma visão unificada da topologia de aplicativos, bem como um conjunto de ferramentas integradas para implantação, monitoramento e gerenciamento.

Este workshop é projetado para ser o mais autoexplicativo possível, mas sinta-se livre para pedir esclarecimentos ou assistência ao longo do caminho.

Tempo Estimado: 90 minutos

### Objetivos

*   Configure sua conta do Oracle Cloud Free Tier (se você ainda não tiver feito isso).
*   Configurando um cluster do OKE no OCI
*   Instalando e configurando o Verrazzano no cluster
*   Criando uma imagem de contêiner para um aplicativo Spring Boot
*   Implantando o aplicativo no cluster do OKE usando o Verrazzano
*   Monitorando e gerenciando o aplicativo usando as ferramentas integradas da Verrazzano

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Saiba Mais

**Sobre o Springboot**

Spring Boot é uma estrutura popular baseada em Java usada para criar aplicativos web e microsserviços. Ele oferece uma experiência de desenvolvimento simplificada com uma abordagem de convenção sobre configuração, tornando-a uma excelente opção para criar e implantar aplicativos em contêineres no Kubernetes.

O Spring Boot oferece uma variedade de recursos que facilitam a criação e a implantação de aplicativos rapidamente, como servidores incorporados, configuração automática e uma ampla gama de bibliotecas e ferramentas para a criação de aplicativos web, sistemas de mensagens e aplicativos orientados a dados. Ele também oferece um valioso conjunto de ferramentas para testar e depurar, facilitando a gravação de um código robusto e confiável.

Um dos principais benefícios do Spring Boot é sua capacidade de integração fácil com outras tecnologias e estruturas, incluindo sistemas de banco de dados, filas de mensagens e sistemas de armazenamento em cache. Isso permite que os desenvolvedores criem sistemas complexos e distribuídos que podem manipular cargas de trabalho em larga escala e atender às demandas de aplicativos modernos baseados na nuvem.

Com seu foco na simplicidade e facilidade de uso, o Spring Boot tornou-se uma escolha popular entre desenvolvedores e organizações que buscam criar aplicativos modernos nativos da nuvem. Ele tem uma comunidade vibrante de colaboradores e usuários, e está evoluindo continuamente para atender às necessidades em constante mudança do setor.

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

*   [https://spring.io/](https://spring.io/)
*   [https://verrazzano.io/](https://verrazzano.io/)

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, abril de 2023