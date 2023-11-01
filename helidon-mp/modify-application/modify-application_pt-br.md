# Modificar o aplicativo MP do Helidon no Code Editor

## Introdução

Neste laboratório, você adiciona um ponto final personalizado à Classe JAVA no Code Editor.

Tempo Estimado: 10 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Modifique o aplicativo Helidon MP no Code Editor](videohub:1_sv1iug41)

### Objetivos

Neste laboratório, você vai:

*   Adicionar um ponto final personalizado na classe Java
*   Criar e executar o aplicativo modificado

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).

## Tarefa 1: Adicionar um Ponto Final personalizado

1.  No Code Editor, clique no arquivo _GreetResource.java_ para abri-lo. ![abrir arquivo](images/open-file.png)
    
2.  Como você pode ver no código, ele é totalmente baseado em MicroProfile. Isso significa que toda a funcionalidade é obtida com POJOs e anotações. Essas anotações são Padrão, portanto, portáteis entre diferentes fornecedores. Isso significa que você pode executar facilmente o código em execução em outras plataformas que suportam a mesma versão do MicroProfile. Para obter mais informações, você encontrará [aqui](https://microprofile.io/).
    
3.  Crie um novo ponto final que forneça títulos aleatoriamente de um grupo de cinco títulos. Para criar este ponto final, copie e cole o código abaixo na linha número 80, conforme mostrado abaixo.
    
        <copy>@Path("/title")
        @GET
        @Produces(MediaType.APPLICATION_JSON)
        public String getRandomTitle() {
            return new String[] {"Mr. ", "Mrs.", "Miss", "Dr.", "Dr. sc."} [(int)(Math.random()*5)];
        }</copy>
        
    
    ![adicionar código](images/add-code.png)
    
4.  Para salvar o conteúdo do arquivo, no Code Editor, clique em _File_ -> _Save_.
    
    > AutoSave é ativado por padrão no Code Editor.
    

## Tarefa 2: Executar o aplicativo

1.  Copie e cole o comando a seguir no terminal para executar o aplicativo.
    
        <copy>mvn clean package
        java -jar target/myproject.jar</copy>
        
2.  Abra um novo terminal/console e execute os seguintes comandos para verificar o aplicativo:
    
        <copy>curl -X GET http://localhost:8080/greet/title</copy>
        Mr.
        
    
    > Ele fornece o título de 5 opções aleatoriamente, você pode executar esse comando várias vezes.
    
3.  _Interrompa o aplicativo **myproject** digitando `Ctrl + C` no terminal em que o comando "java -jar target/myproject.jar" está sendo executado_. É MUITO IMPORTANTE, OUTRAS QUAISQUER QUARIAS NA LABEÇA.
    

## Confirmação

*   **Autor** - Dmitry Aleksandrov
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, abril de 2023