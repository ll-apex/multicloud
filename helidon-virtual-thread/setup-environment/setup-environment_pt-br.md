# Configurar o Ambiente

## Introdução

Este laboratório orienta você pelas etapas necessárias para configurar o ambiente necessário para o Workshop.

[Passo a passo de Lab1](videohub:1_far2bboa)

Tempo Estimado: 10 minutos

### Sobre o Code Editor

O Code Editor permite editar e implantar código para vários serviços do OCI diretamente na Console do OCI. Agora você pode atualizar workflows e scripts de serviço sem precisar alternar entre a Console e seus ambientes de desenvolvimento locais. Isso facilita o protótipo de soluções em nuvem rapidamente, a exploração de novos serviços e a realização de tarefas de codificação rápida.

A integração direta do Editor de Código com o Cloud Shell permite que você acesse o GraalVM Enterprise Native Image e o JDK 17 (Java Development Kit) pré-instalados no Cloud Shell.

### Objetivos

*   Configurar o Code Editor
*   Faça download das versões necessárias do Maven e do JDK
*   Fazer download do código-fonte Helidon

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).
*   Você deve ter o Firefox como um navegador para abrir o Code Editor.

## Tarefa 1: Configurar o Code Editor

1.  Na Console do Cloud, clique no ícone Editor de Código conforme mostrado. ![Editor de Códigos](images/code-editor.png)
    
2.  No Code Editor, clique em Terminal -> New Terminal. ![Abrir Terminal](images/open-terminal.png)
    

## Tarefa 2: Fazer download das versões necessárias do Maven e do JDK

1.  Copie os comandos a seguir e cole no terminal. O download da versão necessária do JDK e do Maven.
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/archive/jdk-19.0.2_linux-x64_bin.tar.gz
        tar -xzvf jdk-19.0.2_linux-x64_bin.tar.gz</copy>
        

## Tarefa 3: Fazer download do código-fonte Helidon

1.  Copie os comandos a seguir e cole no terminal para fazer download do código-fonte do aplicativo helidon.
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  Copie e cole o comando a seguir para descompactar o arquivo _helidon-levelup-2023-main.zip_.
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

Agora você pode _continuar no laboratório 2_.

## Confirmação

*   **Autor** - Joe DiPol
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, Fev de 2023