# Introdução

## Sobre este Workshop

Durante este laboratório, você usará o Helidon Project Starter para desenvolver o aplicativo de microsserviço HTTP. É altamente personalizável, fornecendo várias opções que permitem aos usuários selecionar recursos do Helidon que desejam adicionar ao projeto.

Em seguida, você explorará o Editor de Código do OCI e abrirá o aplicativo Helidon nele. Você também aprenderá a usar o GraalVM Enterprise Edition no Oracle Cloud Infrastructure (OCI) Cloud Shell.

Você criará uma imagem nativa GraalVM para um aplicativo MP Helidon usando o maven. Em seguida, você modificará o aplicativo adicionando um ponto final personalizado na Classe Java existente. Posteriormente, você criará uma imagem nativa do docker deste aplicativo e a enviará para o Oracle Cloud Container Image Registry. Você também criará uma instância de computação e extrairá a imagem do docker aqui e executará o contêiner do docker para este aplicativo.

Este workshop é projetado para ser o mais autoexplicativo possível, mas sinta-se livre para pedir esclarecimentos ou assistência ao longo do caminho.

Tempo Estimado: 90 minutos

### Objetivos

*   Gerar um aplicativo MP Helidon
*   Explore o OCI Code Editor
*   Criar uma imagem nativa para o aplicativo MP Helidon
*   Criar imagem nativa do docker GraalVM para o aplicativo Helidon
*   Criar uma Máquina Virtual
*   Executar contêiner docker para aplicativo Helidon MP

### Pré-requisitos

Este laboratório pressupõe que você tenha:

*   Conta do Oracle Cloud

## Sobre o Helidon

O Helidon é uma estrutura de microsserviços de código-fonte aberto introduzida pela Oracle que fornece uma coleção de bibliotecas Java projetadas para criar aplicativos leves e rápidos baseados em microsserviços. A estrutura suporta dois modelos de programação para gravar microsserviços: Helidon SE e Helidon MP.

Embora o Helidon SE seja projetado para ser uma microframework que suporta o modelo de programação reativa, o Helidon MP é uma implementação da especificação MicroProfile. Como o MicroProfile tem suas raízes no Java EE, as APIs MicroProfile seguem uma abordagem familiar declarativa com uso pesado de anotações. Isso o torna uma boa opção para desenvolvedores Java EE.

Os recursos MicroProfile visam à implementação de microsserviços. Você pode encontrar APIs para definir Clientes REST, monitorar o aplicativo, ler estatísticas técnicas e funcionais e configurar o aplicativo. A Helidon também adicionou APIs adicionais ao conjunto básico de APIs Microprofile, o que oferece todos os recursos necessários para criar aplicativos modernos nativos da nuvem.

> O padrão [MicroProfile](https://microprofile.io/) é baseado em Jakarta EE. Assim como Jakarta EE, MicroProfile é de código aberto e é desenvolvido pela Eclipse Foundation. A implementação com MicroProfile ocorre nas bibliotecas ou servidores de aplicativos que implementam o padrão, assim como Jakarta EE.

## Saiba Mais

*   [https://helidon.io](https://helidon.io)

## Confirmação

*   **Autor** - Dmitry Aleksandrov
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, abril de 2023