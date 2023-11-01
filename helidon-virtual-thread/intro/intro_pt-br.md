# Introdução

## Sobre este Workshop

Neste laboratório, você usará o Oracle Code Editor para criar, executar e modificar microsserviços usando o Helidon Nima WebServer disponível no Helidon 4. Nima é escrito do zero para aproveitar os threads virtuais do Java 19 e fornece um modelo de programação de bloqueio simples.

Você também verá como ele se compara à programação reativa mais complexa.

Durante este laboratório, você usará o Helidon Project Starter para desenvolver um aplicativo Helidon Microprofile. É altamente personalizável, fornecendo várias opções que permitem aos usuários selecionar recursos do Helidon que desejam adicionar ao projeto. Em seguida, você migrará o aplicativo para o Helidon 4 em execução no novo Helidon Nima WebServer usando threads virtuais.

Este workshop é projetado para ser o mais autoexplicativo possível, mas sinta-se livre para pedir esclarecimentos ou assistência ao longo do caminho.

Tempo Estimado: 90 minutos

### Objetivos

*   Explore o OCI Code Editor
*   Criar e executar o aplicativo Nima
*   Criar e executar o aplicativo Reativo
*   Analisar a simplicidade do aplicativo Nima
*   Comparar rastreamento de pilha para o aplicativo Nima e Reativo
*   Gerar um aplicativo MP Helidon
*   Migrar o aplicativo Helidon 3 para o Helidon 4

**Sobre o Helidon Nima**

O Helidon é uma estrutura de microsserviços de código-fonte aberto introduzida pela Oracle que fornece uma coleção de bibliotecas Java projetadas para criar um aplicativo leve e rápido baseado em microsserviços.

Em versões anteriores de desenvolvedores Helidon poderia escolher entre dois modelos de programação. [Helidon MP](https://helidon.io/docs/v3/#/mp/introduction) uma implementação do padrão Eclipse MicroProfile com APIs familiarizadas com desenvolvedores Jakarta EE. E o [Helidon SE](https://helidon.io/docs/v3/#/se/introduction) é uma estrutura enxuta e reativa.

No Helidon 4, a Oracle está introduzindo o Níma, um servidor Web gravado do zero para usar threads virtuais do JEP 425. Nima fornece um modelo de programação fácil de usar com excelente desempenho. Com threads virtuais, os threads não são mais um recurso escasso a ser cuidadosamente agrupado e gerenciado. Em vez disso, eles são um recurso abundante que pode ser criado conforme necessário para tratar solicitações simultâneas quase ilimitadas. Como cada solicitação é executada em seu thread dedicado, é livre executar operações de bloqueio -- como chamar um banco de dados ou outro serviço. E isso pode ser feito de uma forma simples e síncrona, sem medo de bloquear um thread de plataforma e fome de outras solicitações. Você não precisa mais recorrer a um código assíncrono complicado para implementar um serviço altamente concorrente e de baixo custo indireto.

O servidor Web Helidon Níma pretende substituir o Netty no ecossistema Helidon. Ele também pode ser usado por outras estruturas como um componente de servidor Web incorporado.

### Pré-requisitos

Este laboratório pressupõe que você tenha:

*   Conta do Oracle Cloud

## Saiba Mais

*   [https://helidon.io](https://helidon.io)

## Confirmação

*   **Autor** - Joe DiPol
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, Fev de 2023