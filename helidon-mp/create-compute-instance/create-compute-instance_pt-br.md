# Executar imagem do docker nativa do Aplicativo Helidon na Instância de Computação

## Introdução

Este laboratório orienta você no processo de extrair uma imagem do docker do Oracle Container Image Registry e executá-la em uma Máquina Virtual dentro do Oracle Cloud Infrastructure.

Tempo Estimado: 15 minutos

Assista ao vídeo abaixo para ver uma rápida introdução ao laboratório. [Execute a imagem do docker nativa do Aplicativo Helidon na Instância do Serviço Compute](videohub:1_dsfd22u5)

### Objetivos

Neste laboratório, você vai:

*   Criar uma instância de computação
*   Instalar o Docker na instância do serviço Compute
*   Execute o contêiner docker de imagem nativa do aplicativo na instância de computação.

### Pré-requisitos

Para executar este laboratório, você deve ter:

*   Conta do Oracle Cloud
*   Ter um recurso para criar a instância de computação
*   Aplicativo Helidon _myproject-your\_first\_name_ empacotado em contêiner disponível em um registro de contêiner.

## Tarefa 1: Criar a instância do Compute

1.  Na console da Nuvem, clique em _Compute_ -> _Instâncias_. ![instâncias de computação](images/compute-instance.png)
    
2.  Selecione o compartimento correto e clique em _Criar instância_. ![criar instância](images/create-instance.png)
    
3.  Select the following values and click _Create_.  
    **Name:** Leave default.  
    **Create in compatment** Select your own compartment. **Image:** Select _Oracle Linux 8_ image.  
    **Shape:** Select the shape **VM.Standard.E4.Flex** and then select 1 OCPU with 16GB memory.  
    **Primary network:** select _Create new virtual cloud network_ and leave default values.  
    **Subnet:** select _Create new public subnet_ and leave default values.  
    **Public IP address:** select _Assign a public IPv4 address_.  
    **Add SSH keys:** select _Generate a key pair for me_ and click _Save private key_ and _Save public key_ to save the key pair in your local machine. you will need to copy the private key to Code Editor later.
    
4.  Depois que a instância for criada, clique em _Copiar_ para copiar o IP público da instância. ![copiar ip](images/copy-ip.png)
    

## Tarefa 2: Instalar o docker na instância de computação

1.  No terminal dentro do Code Editor, execute o comando a seguir para criar uma chave privada.
    
        <copy>vi ~/opc.key</copy>
        
2.  Pressione _i_ para entrar no modo de inserção e colar o conteúdo da chave privada, que você baixou na tarefa 1 deste laboratório. Pressione a tecla _escape_ e insira _:wq_ para salvar o conteúdo do arquivo.
    
3.  Altere a permissão do arquivo executando o comando a seguir no terminal.
    
        <copy>chmod 600 ~/opc.key</copy>
        
4.  Execute o comando a seguir com seu próprio _IP PUBLIC_ para estabelecer conexão com a Instância de Computação que acabou de ser criada.
    
        <copy>ssh -i ~/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        
    
    ![opc ssh](images/ssh-opc.png)
    
    > **Na conta Free tier, não temos a pasta _.ssh_ por padrão, então você terá uma saída diferente da captura de tela.**
    
5.  Execute o comando a seguir para instalar o docker como usuário raiz na instância de computação e fornecer o recurso de usuário _opc_ para executar o docker.
    
        <copy>sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
        sudo dnf remove -y runc
        sudo dnf install -y docker-ce --nobest
        sudo systemctl enable docker.service
        sudo systemctl start docker.service
        sudo usermod -aG docker opc
        sudo reboot</copy>
        
6.  Aguarde de 1 a 2 minutos até que a máquina seja reinicializada e estabeleça conexão com a instância de computação novamente executando o seguinte comando:
    
        <copy>ssh -i ~/.ssh/opc.key opc@REPLACE_THIS_WITH_YOUR_PUBLIC_IP</copy>
        

## Tarefa 3: Extrair e executar o aplicativo Greeting na instância do serviço Compute

1.  Execute o comando a seguir para extrair a imagem do docker do Oracle Container Image Registry.
    
        <copy>docker pull ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
    
    você terá uma saída semelhante, conforme mostrado abaixo. ![extrair imagem](images/docker-pull.png)
    
2.  Execute o seguinte comando para executar o aplicativo:
    
        <copy>docker run --rm -p 8080:8080 ENDPOINT_OF_YOUR_REGION/NAMESPACE_OF_YOUR_TENANCY/myproject-your_first_name:1.0</copy>
        
3.  Você pode abrir um novo terminal e ssh para calcular uma instância e executar os seguintes comandos para exercer o aplicativo.
    
        <copy>
        curl -X GET http://localhost:8080/greet
        </copy>
        {"message":"Hello World!"}
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Joe
        </copy>
        {"message":"Hello Joe!"}
        
    
        <copy>
        curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting
        </copy>
        
    
        <copy>
        curl -X GET http://localhost:8080/greet/Jose
        </copy>
        {"message":"Hola Jose!"}
        
    
    Parabéns, você concluiu a implantação do aplicativo Helidon na instância do serviço Compute no Oracle Cloud Infrastructure.
    

## Confirmação

*   **Autor** - Dmitry Aleksandrov
*   **Contribuidores** - Ankit Pandey, Maciej Gruszka
*   **Última Atualização por/Data** - Ankit Pandey, abril de 2023