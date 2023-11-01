# Modificar o Aplicativo Helidon para Adicionar Suporte ao Armazenamento de Objetos

## Introdução

O objetivo deste laboratório é demonstrar **como adicionar acesso ao Object Storage** por meio do aplicativo Helidon. Isso é feito substituindo uma variável que é usada para armazenar a palavra de saudação por um objeto que agora se tornará o novo contêiner de palavras de saudação e será armazenado e **recuperado de um bucket do Object Storage**. Como o objeto é persistido, o último valor da palavra de saudação sobreviverá às reinicializações do aplicativo. Sem essa alteração e com a palavra de saudação na memória por meio da variável, a palavra de saudação será redefinida para um valor padrão quando o aplicativo for reiniciado.

Tempo estimado: 15 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Suporte ao Object Storage](videohub:1_p5v2wehm)

### Objetivos

Neste laboratório, você vai:

*   Modifique o aplicativo Helidon para mostrar sua integração com serviços do OCI, como Armazenamento de objetos
*   Verificar a integração bem-sucedida do serviço Object Storage

### Pré-requisito

*   Uma Conta do Oracle Free Tier(Trial), Paga ou LiveLabs Cloud

## Tarefa 1: Modificar o aplicativo Helidon para integração do Object Storage

1.  Verifique se **oci.bucket.name** está configurado corretamente em **~/oci-mp/server/src/main/resources/META-INF/microprofile-config.properties**, que já deve ter sido definido na etapa 5 do Laboratório 2/Tarefa 3.
    
2.  No **Editor de Códigos**, clique no nome do arquivo **`pom.xml`** em _~/oci-mp/server/_ para abri-lo e adicionar a dependência **SDK do OCI do Object Storage** dentro da cláusula **dependências**, conforme mostrado abaixo.
    
        <copy><dependency>
                    <groupId>com.oracle.oci.sdk</groupId>
                    <artifactId>oci-java-sdk-objectstorage</artifactId>
        </dependency></copy>
        
    
    ![adicionar dependência](images/add-dependency.png)
    
    > Certifique-se de manter o recuo adequado.
    
3.  Modificamos o arquivo _~/oci-mp/server/src/main/java/ocw/hol/mp/oci/server/GreetingProvider.java_ para adicionar o acesso ao serviço **Object Storage**. No interesse do tempo, temos o código-fonte dessa alteração. Copie e cole o código a seguir para atualizar esse arquivo com as alterações necessárias. Você pode ler a _Observação_ abaixo da qual descreve as alterações feitas neste arquivo.
    
        <copy>cp ~/devops_helidon_to_instance_ocw_hol/source/GreetingProvider.java server/src/main/java/ocw/hol/mp/oci/server/</copy>
        
    
    ![armazenamento de objeto adicionado](images/os-added.png)
    
    > **Por favor, leia:-**
    
    *   Na seção de argumento do construtor, adicionou o parâmetro _ObjectStorage objectStorageClient_. Como isso faz parte da anotação _@Injected_, o parâmetro será automaticamente processado e definido pelo Helidon para conter o cliente que pode ser usado para se comunicar com o serviço Object Storage sem ter que adicionar várias linhas do código **OCI SDK** para esse fim.
    *   Na mesma seção de argumento do construtor, adicionou **ConfigProperty** que extrairá o valor de uma propriedade **oci.bucket.name** na configuração. Isso foi preenchido anteriormente em **microprofile-config.properties** durante a configuração inicial do aplicativo quando um script de utilitário chamado **`update_config_values.sh`** foi executado a partir do diretório do repositório **`devops_helidon_to_instance_ocw_hol`**.
    *   Usando o método **getNamespace() Object Storage SDK**, recupere o namespace do Object Storage, pois ele será usado posteriormente para recuperar ou armazenar um objeto:
    
        <copy>public GreetingProvider(@ConfigProperty(name = "app.greeting") String message,
                            ObjectStorage objectStorageClient,
                            @ConfigProperty(name = "oci.bucket.name") String bucketName) {
        try {
            this.bucketName = bucketName;
            GetNamespaceResponse namespaceResponse =
                    objectStorageClient.getNamespace(GetNamespaceRequest.builder().build());
            this.objectStorageClient = objectStorageClient;
            this.namespaceName = namespaceResponse.getValue();
            LOGGER.info("Object storage namespace: " + namespaceName);
            setMessage(message);
        } catch (Exception e) {
            LOGGER.warning("Error invoking getNamespace from Object Storage: " + e);
        }
        }     </copy>
        
    
    *   Logo abaixo da classe GreetingProvider, removeu a declaração da variável:
    
        <copy>private final AtomicReference<String> message = new AtomicReference<>();</copy>
        
    
    *   Substituído por variáveis globais locais, como abaixo. _LOGGER_ será usado para log-in enquanto _objectStorageClient_, _namespaceName_, _bucketName_ e _objectName_ serão usados em chamadas do _Object Storage SDK_.
    
        <copy>private static final Logger LOGGER = Logger.getLogger(GreetingProvider.class.getName());
        private ObjectStorage objectStorageClient;
        private String namespaceName;
        private String bucketName;
        private final String objectName = "hello.txt";</copy>
        
    
    *   Substituiu o conteúdo de _getMessage()_ pelo código abaixo. Isso usará o método **getObject() SDK** para recuperar a palavra de saudação do objeto **hello.txt** extraído do bucket.
    
        <copy>try {
        GetObjectResponse getResponse =
                objectStorageClient.getObject(
                        GetObjectRequest.builder()
                                .namespaceName(namespaceName)
                                .bucketName(bucketName)
                                .objectName(objectName)
                                .build());
        return new String(getResponse.getInputStream().readAllBytes());
        } catch (Exception e) {
            LOGGER.warning("Error invoking getObject from Object Storage: " + e);
            return "Hello-Error";
        }</copy>
        
    
    *   Substituiu o conteúdo do método **setMessage(String message)** nulo pelo código abaixo. Isso usará o método **putObject() SDK** para armazenar o objeto **hello.txt** que contém a palavra de saudação no bucket.
    
        <copy>try {
        byte[] contents = message.getBytes();
        PutObjectRequest putObjectRequest =
                PutObjectRequest.builder()
                        .namespaceName(namespaceName)
                        .bucketName(bucketName)
                        .objectName(objectName)
                        .putObjectBody(new ByteArrayInputStream(message.getBytes()))
                        .contentLength(Long.valueOf(contents.length))
                        .build();
        objectStorageClient.putObject(putObjectRequest);
        } catch (Exception e) {
            LOGGER.warning("Error invoking putObject from Object Storage: " + e);
        }</copy>
        
    
    *   Removida a importação (**import java.util.concurrent.atomic.AtomicReference;**) e a substituiu pelas novas importações para suportar todo o novo código que foi adicionado.
    
        <copy>import java.io.ByteArrayInputStream;
        import java.util.logging.Logger;
        
        import com.oracle.bmc.objectstorage.ObjectStorage;
        import com.oracle.bmc.objectstorage.requests.GetNamespaceRequest;
        import com.oracle.bmc.objectstorage.requests.GetObjectRequest;
        import com.oracle.bmc.objectstorage.requests.PutObjectRequest;
        import com.oracle.bmc.objectstorage.responses.GetNamespaceResponse;
        import com.oracle.bmc.objectstorage.responses.GetObjectResponse;</copy>
        

## Tarefa 2: Pressione a alteração do código do aplicativo Helidon e acione o pipeline DevOps

1.  Copie e cole o comando a seguir no terminal **para confirmar e enviar a alteração**.
    
        <copy>git add .
        git status
        git commit -m "Add support Object Storage integration"
        git push</copy>
        
    
    > O pipeline será acionado por esse git push.
    
2.  Aguarde até que o **ciclo de vida DevOps** seja concluído monitorando os logs do pipeline de criação e implantação. ![execução de implantação](images/deployment-run.png)
    

## Tarefa 3: Verificar a integração bem-sucedida do Object Storage

Teste usando curl e verifique se um novo objeto **hello.txt** foi adicionado ao **bucket**. Valide se o tamanho do objeto é igual ao tamanho da palavra de saudação. Por exemplo, se a palavra de saudação for _Olá_, o tamanho deverá ser **5**. Se a palavra de saudação for _Hola_, o tamanho deverá ser **4**.

1.  Configure o nó de implantação **`PUBLIC_IP`** como uma variável de ambiente.
    
        <copy>export PUBLIC_IP=$(~/devops_helidon_to_instance_ocw_hol/main/get.sh public_ip)</copy>
        
2.  Copie e cole o comando a seguir para fazer uma solicitação **GET**. Você terá uma saída semelhante, conforme mostrado abaixo.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hello World!","date":[2023,5,25]}
        
3.  Na **Console da Nuvem**, clique em _menu do hambúrguer_ -> _Armazenamento_ -> _Buckets_, conforme mostrado. ![menu do bucket](images/bucket-menu.png)
    
4.  Selecione o compartimento correto e clique em **app-bucket-helidonocw-hol-string** para abrir o **bucket**. ![selecionar compartimento](images/select-compartment.png)
    
5.  Verifique se o bucket agora contém um objeto **hello.txt** e tem um tamanho de **5 bytes** porque a palavra de saudação é **Olá**. Você também pode fazer download do objeto e verificar se o conteúdo é, na verdade, _Olá_. ![verificar tamanho](images/verify-size.png)
    
    > Deixe esta página aberta, pois atualizaremos esta página quando alterarmos a palavra de saudação na próxima etapa.
    
6.  Copie e cole o comando a seguir para substituir a palavra de saudação **Olá** por **Hola**.
    
        <copy>curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://$PUBLIC_IP:8080/greet/greeting 
        curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
7.  Verifique se o objeto **hello.txt** do bucket agora tem um tamanho de **4 bytes** porque a palavra de saudação foi substituída por **Hola**. Você também pode fazer download do objeto e verificar se o conteúdo foi alterado para **Hola**. ![tamanho do hola](images/hola-size.png)
    
8.  Reinicie o aplicativo usando a ferramenta **restart.sh** para demonstrar que o valor da palavra de saudação sobreviverá à medida que persistir no serviço **Object Storage**.
    
        <copy>~/devops_helidon_to_instance_ocw_hol/utils/restart.sh</copy>
        Created private.key and can be used to ssh to the deployment instance by running this command: "ssh -i private.key opc@xx.xx.xx.xx"
        FIPS mode initialized
        The authenticity of host 'xx.xx.xx.xx (xx.xx.xx.xx)' can't be established.
        ECDSA key fingerprint is SHA256:hJl8axCNhFcILDo+AwxMkodxhY+UxRD40d1ans83GTg.
        ECDSA key fingerprint is SHA1:IBUhyn05DaIs60GAQsruVXajhym.
        Are you sure you want to continue connecting (yes/no)? yes
        Warning: Permanently added 'xx.xx.xx.xx' (ECDSA) to the list of known hosts.
        Starting oci-mp-server.jar
        Helidon app is now running with pid 264792!
        Cleaning up ssh private.key
        
    
    ![reiniciar aplicativo](images/restart-application.png)
    
    > Digite _yes_, quando solicitado a informar **Are você tem certeza de que deseja continuar a conexão (yes/no)?**.
    
9.  Chame a solicitação Hello World padrão e **observe que a palavra de saudação ainda é Hola**.
    
        <copy>curl http://$PUBLIC_IP:8080/greet</copy>
        {"message":"Hola World!","date":[2023,5,25]}
        
10.  Atualize a página Bucket na **Console do Cloud** e você observará que o tamanho ainda é **4**, o que confirma que a palavra de saudação ainda é **Hola**. ![verificar persistência](images/verify-persistence.png)
    

**Parabéns!** Você concluiu o Workshop. Se quiser continuar com o **Lab 6**, que **exclui todos os recursos** criados durante este workshop.

## Saiba Mais

*   [Integração do Helidon OCI](https://helidon.io/docs/v3/#/mp/integrations/oci)

## Confirmação

*   **Autor** - Keith Lustria
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023