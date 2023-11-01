# Introdução

## Sobre este Workshop

Este workshop mostra um workshop de ponta a ponta da migração de um Domínio de Servidor WebLogic local para os contêineres e o torna executável no OCI com a pilha do **Oracle WebLogic Suite for OKE BYOL**. Nós demonstramos a interface gráfica da interface do usuário do Kit de Ferramentas do Kubernetes WebLogic, bem como a Ferramenta do Implantador WebLogic e o Operador do Weblogic Kubernetes. Demonstramos como o processo de migração pode ser simplificado e acelerado usando um conjunto de ferramentas orientado a DevOps.

![Fluxo de laboratório](images/lab-flow.png)

Tempo Estimado: 120 minutos

### Sobre Produto/Tecnologia

O WKT (Kubernetes Toolkit) WebLogic é um conjunto de ferramentas de código aberto que ajudam a provisionar aplicativos baseados em WebLogic para serem executados em contêineres Linux em um cluster do Kubernetes. O WKT inclui as seguintes ferramentas:  

*   [WebLogic Deploy Tooling (WDT)](https://github.com/oracle/weblogic-deploy-tooling) - Um conjunto de ferramentas de ciclo de vida de uma única finalidade que operam fora de uma única representação de modelo de metadados de um domínio WebLogic.
*   [WebLogic Image Tool (WIT)](https://github.com/oracle/weblogic-image-tool) - Uma ferramenta para criar imagens de contêiner Linux para executar domínios WebLogic.
*   [WebLogic WKO (Kubernetes Operator)](https://github.com/oracle/weblogic-kubernetes-operator) - Um operador Kubernetes que permite que os domínios WebLogic sejam executados de forma nativa em um cluster do Kubernetes.

A IU do WKT fornece uma interface gráfica do usuário que envolve as ferramentas do WKT, o Docker, o Helm e o cliente Kubernetes (kubectl) e ajuda a orientá-lo no processo de criação e modificação de um modelo. do seu domínio WebLogic, criando uma imagem de contêiner do Linux a ser usada para executar o domínio e configurando e implantando o software e a configuração necessários para implantar e acessar o domínio no cluster do Kubernetes.

### Objetivos

*   Criação de Máquina Virtual a partir de imagem do Marketplace
*   Configurar uma Instância do Oracle Kubernetes Engine no Oracle Cloud Infrastructure
*   Modificando um Projeto de UI WKT e Criação de arquivo de Modelo
*   Criação de Imagem Auxiliar e Envio no Registro de Imagem do Oracle Container
*   Implantar o Operador WebLogic no Oracle Kubernetes Cluster (OKE)
*   Implantar Domínio WebLogic no Cluster do Oracle Kubernetes (OKE)
*   Implantar Controlador de Entrada no Cluster do Oracle Kubernetes (OKE)
*   Mostrando balanceamento de carga entre pods do Servidor Gerenciado e explorando a Console Remota do WebLogic
*   Dimensionar um Cluster WebLogic
*   Atualize um aplicativo implantado por meio de uma reinicialização contínua da nova imagem

## Saiba Mais

*   [WebLogic IU do Kit de Ferramentas do Kubernetes](https://oracle.github.io/weblogic-toolkit-ui/)
    
*   [Oracle WebLogic Suite para OKE](https://docs.oracle.com/en/cloud/paas/weblogic-container/user/oracle-weblogic-server-oke.html)
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, outubro de 2023