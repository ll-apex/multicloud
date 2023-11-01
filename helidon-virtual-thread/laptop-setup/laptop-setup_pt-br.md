# Criar imagem nativa do Aplicativo MP Helidon

## Introdução

Neste laboratório, você vai aprender a configurar o ambiente na sua máquina linux local.

Tempo Estimado: 10 minutos

### Objetivos

*   Configure o JDK e o Maven necessários.
*   Faça download do código-fonte do aplicativo.

### Pré-requisitos

*   É necessário ter um IDE para modificar, criar e executar o projeto.

## Tarefa 1: Fazer download das versões necessárias do Maven e do JDK

1.  No IDE, abra um terminal/console.
    
2.  Copie os comandos a seguir e cole no terminal. Ele faz download da versão necessária do JDK e do Maven e define a variável PATH para usar o Maven e o JDK necessários.
    
        <copy>wget https://archive.apache.org/dist/maven/maven-3/3.8.3/binaries/apache-maven-3.8.3-bin.tar.gz
        tar -xvf apache-maven-3.8.3-bin.tar.gz
        wget https://download.oracle.com/java/19/latest/jdk-19_linux-x64_bin.tar.gz
        tar -xzvf jdk-19_linux-x64_bin.tar.gz</copy>
        

## Tarefa 2: Fazer download do código-fonte Helidon

1.  Copie os comandos a seguir e cole no terminal para fazer download do código-fonte do aplicativo helidon.
    
        <copy>curl -LSs https://github.com/oracle-livelabs/multicloud/blob/main/helidon-virtual-thread/setup-environment/code/helidon-levelup-2023-main.zip?raw=true >~/helidon-levelup-2023-main.zip </copy>
        
2.  Copie e cole o comando a seguir para descompactar o arquivo _helidon-levelup-2023-main.zip_.
    
        <copy>unzip ~/helidon-levelup-2023-main.zip</copy>
        

Agora você pode _acessar o próximo laboratório_.

## Confirmação

*   **Autor** - Joe DiPol
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, Fev de 2023