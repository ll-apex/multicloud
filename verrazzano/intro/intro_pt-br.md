# Introdução

## Sobre este Workshop

Este laboratório mostra como instalar a plataforma Verrazzano em um único cluster do Kubernetes e implantar um aplicativo de amostra usando os conceitos do Verrazzano.

O aplicativo de amostra [Livros de Bobby](https://verrazzano.io/docs/samples/bobs-books/) consiste em três partes principais:

*   Um aplicativo de "processamento de pedido" de back-end, que é um aplicativo Java EE com serviços REST e uma UI JSP muito simples, que armazena dados em um banco de dados MySQL. Este aplicativo é executado no Servidor WebLogic.
*   Uma loja virtual de front-end "Robert's Books", que é um vendedor geral de livros. Isso é implementado como um microsserviço Helidon, que obtém dados de livro do Coherence, usa um armazenamento de cache do Coherence para persistir dados do gerente de ordens e tem uma interface de usuário Web React.
*   Uma loja virtual de front-end "Bobby's Books", que é uma loja de livros infantil especializada. Isso é implementado como um microsserviço Helidon, que obtém dados de livro de um armazenamento de cache do Coherence (diferente), interage diretamente com o gerenciador de pedidos e tem uma interface de usuário Web JSF em execução no Servidor WebLogic.

### Sobre Produto/Tecnologia

A Verrazzano é uma plataforma de contêiner empresarial completa para implantar aplicativos nativos da nuvem e tradicionais em ambientes híbridos e com várias nuvens. É composto por um conjunto curado de componentes de código aberto - muitos que você já pode usar e confiar, e alguns que foram escritos especificamente para reunir todas as peças que tornam o Verrazzano uma plataforma coesa e fácil de usar.

A Verrazzano inclui os seguintes recursos:

*   Gerenciamento de carga de trabalho híbrido e multicluster
*   Manuseio especial para aplicações WebLogic, Coherence e Helidon
*   Gerenciamento de infraestrutura multicluster
*   Monitoramento integrado e pré-com fio de aplicativos
*   Segurança integrada
*   Ativação DevOps e GitOps

![Verrazzano](images/verrazzano.png)

### Objetivos

1\. Configure uma instância do Oracle Kubernetes Engine no Oracle Cloud Infrastructure. 1\. Configure \`kubectl\` para interagir com a instância do Oracle Kubernetes Engine no Oracle Cloud Infrastructure. 2\. Instale e conheça a plataforma Verrazzano. 3. Implante o aplicativo de amostra \*Livros de Bobby\*. 4. Modifique e reimplante o componente de aplicativo \*Estoque\* baseado em Helidon.

## Saiba Mais

*   [Verrazzano](https://verrazzano.io/)

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023