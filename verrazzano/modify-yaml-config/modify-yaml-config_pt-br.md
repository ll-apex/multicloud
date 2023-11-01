# Modificar o Arquivo YAML de Configuração dos Livros de Bobby

## Introdução

No Laboratório 6, enviamos a imagem atualizada do Docker para _bobby-helidon-stock-application_. Agora, queremos que a Verrazzano reimplante o aplicativo e os componentes atualizados sem afetar os serviços. Para isso, precisamos configurar o arquivo YAML para que o Verrazzano pegue a nova imagem e inicie um pod para ele. Depois que o pod estiver no estado _Em Execução_, ele encerrará o pod associado ao aplicativo e aos componentes anteriores.

Tempo estimado: 10 minutos

### Objetivos

Neste laboratório, você vai:

*   Modifique o arquivo `bobs-books-comp.yaml`.
*   Aplique as alterações usando `kubectl`.

### Pré-requisitos

Você deve ter um editor de texto, no qual pode colar os comandos e URLs e modificá-los, conforme seu ambiente. Em seguida, você pode copiar e colar os comandos modificados para executá-los no _Cloud Shell_.

## Tarefa 1: Modificar o arquivo bobs-books-comp.yaml

1.  Temos um arquivo de configuração de aplicativo, _bobs-books-comp.yaml_. No Laboratório 2, baixamos os arquivos yaml do aplicativo. Para alterar o diretório Home que contém o arquivo yaml, copie o comando a seguir e cole-o no _Cloud Shell_.
    
        <copy>cd ~</copy>
        
2.  Nesse local, temos o arquivo de configuração para o aplicativo bobs-books. Como parte do Laboratório 5, modificamos o aplicativo bobbys-helidon-stock e criamos uma nova imagem do Docker para esse componente. No Laboratório 6, enviamos essa imagem do Docker para o repositório do Oracle Cloud Container Registry. Agora, neste laboratório, modificaremos o arquivo _bobs-books-comp.yaml_ para que ele extraia a nova imagem atualizada do Docker do repositório do Oracle Cloud Container Registry. Para modificar o arquivo _bobs-books-comp.yaml_, copie o comando a seguir e cole-o no _Cloud Shell_.
    
        <copy>vi bobs-books-comp.yaml</copy>
        
    
    ![Abrir arquivo](images/openfile.png " ")
    
3.  Como parte do Laboratório 5, você salvou seu nome completo de imagem do Docker. Você precisa copiar a linha a seguir e colá-la no seu editor de texto. Em seguida, substitua `docker image full name` pelo nome da imagem do Docker. Em seguida, copie a linha modificada e pressione _i_ para inserir o texto no arquivo `*bobs-books-comp.yaml*`. Cole a saída na linha número 148 (certifique-se de manter o recuo) e comente a linha de saída número 147 com _#_, conforme mostrado na imagem a seguir, pressione _Esc_ e, em seguida, digite _:wq_ para salvar o arquivo.
    
        <copy>image:  `docker image full name`</copy>
        
    
    ![Inserir linha](images/insert-line.png " ")
    

## Tarefa 2: Aplicar as Alterações usando `kubectl`

1.  Para aplicar as alterações, copie e cole o comando a seguir no _Cloud Shell_. Quando você aplicar a alteração, um novo pod será inicializado para atender às solicitações do novo componente, enquanto o pod associado ao componente antigo continuará atendendo às solicitações. Posteriormente, depois que o novo pod atingir o estado _Em Execução_, o antigo pod começará a ser _Encerrado_. Eventualmente, somente o novo pod estará no estado _Executando_.
    
        <copy>kubectl apply -f bobs-books-comp.yaml -n bobs-books</copy>
        
    
    A saída deve ser semelhante à seguinte:
    
        $ kubectl apply -f ~/bobs-books-comp.yaml -n bobs-books
        component.core.oam.dev/robert-coh unchanged
        component.core.oam.dev/robert-helidon unchanged
        component.core.oam.dev/bobby-coh unchanged
        component.core.oam.dev/bobby-helidon configured
        component.core.oam.dev/bobby-wls unchanged
        component.core.oam.dev/bobs-mysql-configmap unchanged
        component.core.oam.dev/bobs-mysql-service unchanged
        component.core.oam.dev/bobs-mysql-deployment unchanged
        component.core.oam.dev/bobs-orders-configmap unchanged
        component.core.oam.dev/bobs-orders-wls unchanged
        $
        
    
    Você pode observar na saída; somente _component.core.oam.dev/bobby-helidon_ está configurado e outros componentes permanecem inalterados.
    
2.  Para ver como o novo pod está sendo inicializado e o antigo pod obtém o estado _Encerrando_, copie e cole o comando a seguir no _Cloud Shell_.
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    Você verá uma saída semelhante à seguinte:
    
        $ kubectl get pods -n bobs-books
        NAME                                         READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                           2/2    Running      0         130m
        bobbys-front-end-adminserver                 4/4    Running      0         127m
        bobbys-front-end-managed-server1             4/4    Running      0         126m
        bobbys-helidon-stock-application-64fb55-zzp  0/2    PodInitializing  0     10s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Running      0         130m
        bobs-bookstore-adminserver                   4/4    Running      0         127m
        bobs-bookstore-managed-server1               4/4    Running      0         126m
        mysql-65d864bf8c-xf64p                       2/2    Running      0         130m
        robert-helidon-bfdfb58b8-58qfs               2/2    Running      0         130m
        robert-helidon-bfdfb58b8-lkw8m               2/2    Running      0         130m
        roberts-coherence-0                          2/2    Running      0         130m
        roberts-coherence-1                          2/2    Running      0         130m
        bobbys-helidon-stock-application-64fb55-zzp  1/2    Running      0         28s
        bobbys-helidon-stock-application-64fb55-zzp  2/2    Running      0         34s
        bobbys-helidon-stock-application-77867f-8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867f-8h5  0/2    Terminating  0         130m
        $
        
    
    Depois de ver que todos os pods estão no Status _Executando_, pressione _CTRL + C_ para eliminar esse comando.
    
    Deixe o _Cloud Shell_ aberto conforme também precisamos dele em nosso último laboratório.
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023