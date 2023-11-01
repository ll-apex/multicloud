# Modificar a Aplicação do Livro de Bobby e Criar uma Nova Imagem do Componente da Aplicação

## Introdução

Nesse lab, modificaremos o aplicativo bobbys-helidon-stock-application por meio do _Cloud Shell_. Posteriormente, criaremos uma nova imagem do Docker para aplicativo bobbys-helidon-stock. Essa imagem do aplicativo bobbys-helidon-stock é um componente do aplicativo Livros de Bobby.

Tempo estimado: 10 minutos

### Objetivos

Neste laboratório, você vai:

*   Modificar bobbys-helidon-stock-application.
*   Crie uma nova imagem do Docker para bobbys-helidon-stock-application.

### Pré-requisitos

Você deve ter um editor de texto, no qual pode colar os comandos e URLs e modificá-los, conforme seu ambiente. Em seguida, você pode copiar e colar os comandos modificados para executá-los no _Cloud Shell_.

## Tarefa 1: Modificar bobbys-helidon-stock-application

1.  Selecione a guia Livro de Bobby, clique em _Livros_ e, em seguida, clique na imagem do livro _O Hobbit_, conforme mostrado:
    
    ![Clicar em Livros](images/clickbooks.png " ")
    
    Mostra o nome do livro no formato _O Hobbit_, conforme mostrado na imagem.
    
    ![O Hobbit](images/thehobbit.png " ")
    
2.  Queremos converter o nome do livro em letras maiúsculas (THE HOBBIT). Precisamos baixar o código-fonte do aplicativo Bobby's Books. Certifique-se de que você está na pasta home. Copie os comandos a seguir e cole-os no _Cloud Shell_.
    
        <copy>cd ~
        git clone -b v0.16.0 https://github.com/verrazzano/examples.git</copy>
        
    
    ![Clonar Repositório](images/clonerepository.png " ")
    
3.  Para exibir os arquivos dentro do aplicativo Livro de Bobby, copie o comando a seguir e cole-o no _Cloud Shell_.
    
        <copy>ls -la ~/examples/bobs-books/</copy>
        
    
    A saída deve ser semelhante à seguinte: `bash $ ls -la ~/examples/bobs-books/ total 16 drwxr-xr-x. 5 ankit_x_pa oci 100 Mar 21 12:14 . drwxr-xr-x. 7 ankit_x_pa oci 4096 Mar 21 12:14 .. drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobbys-books drwxr-xr-x. 5 ankit_x_pa oci 4096 Mar 21 12:14 bobs-bookstore-order-manager -rw-r--r--. 1 ankit_x_pa oci 942 Mar 21 12:14 README.md drwxr-xr-x. 4 ankit_x_pa oci 72 Mar 21 12:14 roberts-books $`
    
4.  Agora, vamos fazer alterações no JAVA\_FILE relevante. Para abrir o arquivo, copie o comando a seguir e cole-o no _Cloud Shell_.
    
        <copy>vi ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/src/main/java/org/books/bobby/BookResource.java</copy>
        
    
    ![Abrir Arquivo](images/openfile.png " ")
    
5.  Pressione _i_ para modificar o código. Para adicionar uma nova linha no número de linha 84, copie a linha a seguir e cole-a no número de linha 84, conforme mostrado:
    
        <copy>optional.get().setTitle(optional.get().getTitle().toUpperCase());</copy>
        
    
    ![Inserir Linha](images/insertline.png " ")
    
6.  Pressione _Esc_ e _:wq_ para salvar as alterações.
    
    ![Salvar alterações](images/savechanges.png " ")
    

## Tarefa 2: Criar uma nova imagem do Docker para o aplicativo bobbys-helidon-stock

1.  Como vamos criar uma nova imagem do Docker para _bobbys-helidon-stock-application_, que tem dependências em _bobbys-coherence-application_, executamos um comando do _Maven_ para limpar o arquivo de aplicação bobbys-coherence existente e compilar, criar, empacotar e instalar um novo arquivo de aplicação bobby-coherence em um repositório Maven local. Para alterar para a pasta do diretório _bobbys-coherence_ e compilar, criar e instalar o arquivo compactado do aplicativo _bobbys-coherence_ em um repositório Maven local, copie o comando a seguir e cole-o no _Cloud Shell_.
    
        <copy>
        cd ~/examples/bobs-books/bobbys-books/bobbys-coherence/
        mvn clean install
        </copy>
        
    
    A saída resultante é semelhante à seguinte (abreviada para mostrar somente o início e o fim da saída): `bash $ mvn clean install [INFO] Scanning for projects... [INFO] [INFO] ------------< io.verrazzano.example.books:bobbys-coherence >------------ [INFO] Building bobbys-coherence 1.0-SNAPSHOT [INFO] --------------------------------[ jar ]--------------------------------- [INFO] [INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ bobbys-coherence --- [[INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/target/bobbys-coherence.jar to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.jar [INFO] Installing /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-coherence/pom.xml to /home/ankit_x_pa/.m2/repository/io/verrazzano/example/books/bobbys-coherence/1.0-SNAPSHOT/bobbys-coherence-1.0-SNAPSHOT.pom [INFO] ------------------------------------------------------------------------ [INFO] BUILD SUCCESS [INFO] ------------------------------------------------------------------------ [INFO] Total time: 8.887 s [INFO] Finished at: 2023-03-22T04:09:11Z [INFO] ------------------------------------------------------------------------ $`
    
2.  Como modificamos o _bobbys-helidon-stock-application_, precisamos compilar, construir e empacotar esse aplicativo. Para alterar o diretório _bobbys-helidon-stock-application_ e empacotar _bobbys-helidon-stock-application_ em um arquivo JAR, copie o comando a seguir e cole-o no _Cloud Shell_. Na segunda imagem, você pode ver a criação do arquivo `bobbys-helidon-stock-application.jar` em `~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target`.
    
        <copy>cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/
        mvn clean package
        </copy>
        
    
    The resulting output is similar to the following (abbreviated to show only the start and end of output): \`\`\`bash $ cd ~/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/ $ mvn clean package \[INFO\] Scanning for projects... \[INFO\] ------------------------------------------------------------------------ \[INFO\] Detecting the operating system and CPU architecture \[INFO\] ------------------------------------------------------------------------
    
         [INFO] --- maven-jar-plugin:2.5:jar (default-jar) @ bobbys-helidon-stock-application ---
         [INFO] Building jar: /home/ankit_x_pa/examples/bobs-books/bobbys-books/bobbys-helidon-stock-application/target/bobbys-helidon-stock-application.jar
         [INFO] ------------------------------------------------------------------------
         [INFO] BUILD SUCCESS
         [INFO] ------------------------------------------------------------------------
         [INFO] Total time:  10.106 s
         [INFO] Finished at: 2023-03-22T04:11:38Z
         [INFO] ------------------------------------------------------------------------
         $
         ```
        
3.  Vamos criar uma imagem do Docker para a aplicação bobby-stock-helidon, mas esse aplicativo usa uma versão específica do JDK e não queremos alterar os arquivos do Docker que criam a nova imagem. Então, fazemos download do JDK necessário. Para fazer download da versão do JDK necessária, copie o comando abaixo e cole-o no _Cloud Shell_.
    
        <copy>wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2_linux-x64_bin.tar.gz</copy>
        
    
    A saída deve ser semelhante à seguinte: \`\`bash $ wget https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz --2023-03-22 04:20:16-- https://download.java.net/java/GA/jdk14.0.2/205943a0976c4ed48cb16f1043c5c647/12/GPL/openjdk-14.0.2\_linux-x64\_bin.tar.gz Resolvendo download.java.net (download.java.net)... 2.23.220.73 Conectando-se a download.java.net (download.java.net)|2.23.220.73|:443... conectado. Pedido HTTP enviado, aguardando resposta... 200 OK Comprimento: 198606200 (189M) \[application/x-gzip\] Salvando em: 'openjdk-14.0.2\_linux-x64\_bin.tar.gz'
    
         100%[==============================================================================================================================>] 198,606,200  194MB/s   in 1.0s   
        
         2023-03-22 04:20:17 (194 MB/s) - ‘openjdk-14.0.2_linux-x64_bin.tar.gz’ saved [198606200/198606200]
         $
         ```
        
4.  Estamos criando uma imagem do Docker, que enviaremos por upload para o Oracle Cloud Container Registry no Laboratório 6. Para criar o nome do arquivo, precisamos das seguintes informações:
    
    *   Namespace da Tenancy
    *   Ponto final da Região
5.  Para localizar o Namespace da tenancy, clique no Ícone _Usuário_ -> _Tenancy_, conforme mostrado. Nas **Definições de armazenamento de objetos**, você encontrará o Namespace. Copie e salve-o em algum lugar no seu arquivo de texto, pois também o usaremos no Laboratório 6.
    
    ![Copiar Tenancynamespace](images/copy-tenancynamespace.png " ")
    
6.  Localize o _Ponto Final da sua Região_. Consulte a tabela documentada neste URL [https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab](https://docs.oracle.com/en-us/iaas/Content/Registry/Concepts/registryprerequisites.htm#Availab). No exemplo mostrado, o ponto final da região é _Sul do Reino Unido (Londres)_ (como o nome da região) e seu ponto final é _lhr.ocir.io_. Localize o ponto final do seu próprio _Nome da Região_ e salve-o no arquivo de texto. Você também precisará dele para o próximo laboratório.
    
    ![Pontos finais](images/end-point.png " ")
    
    > Agora você tem o namespace e o ponto final da tenancy para sua região.
    
7.  Agora você tem o Namespace da Tenancy e o Ponto Final para sua região. Copie o comando a seguir e cole-o no seu editor de texto. Em seguida, substitua **`END_POINT_OF_YOUR_REGION`** pelo ponto final do nome da sua região, **`NAMESPACE_OF_YOUR_TENANCY`** pelo namespace da sua tenancy e **`your_first_name`** pelo seu nome em letras minúsculas.
    
        <copy>docker build --force-rm=true -f Dockerfile -t END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0 .</copy>
        
    
    ![Build do Docker](images/docker-build.png " ")
    
    ![Imagem criada](images/image-created.png " ")
    
    > Por exemplo, no meu caso, o comando é `docker build --force-rm=true -f Dockerfile -t lhr.ocir.io/tenancynamespace/helidon-stock-application-ankit`.
    
    Isso cria a imagem do Docker, que enviaremos ao repositório do Oracle Cloud Container Registry no Laboratório 6. Você precisa copiar o nome completo da imagem substituída **`END_POINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/helidon-stock-application-your_first_name:1.0`** no seu texto editor.In Lab 6, quando precisar criar o repositório, dê o nome **`helidon-stock-application-your_first_name`**.
    
    Deixe o _Cloud Shell_ aberto; precisamos dele para o próximo laboratório.
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023