# ALTERNATE VERSION com solução simplificada

## Introdução

Neste laboratório, usaremos a imagem de aplicação bobbys-helidon-stock. Esta imagem é armazenada em nosso repositório público dentro do Oracle Cloud Container Registry. Também usamos o arquivo bobs-books-comp-mod.yaml modificado, que aponta para a imagem modificada bobbys-helidon-stock-application.

### Objetivos

Neste laboratório, você vai:

*   Faça download do arquivo bobs-books-comp-mod.yaml modificado.
*   Aplicar as Alterações usando o kubectl

### Pré-requisitos

*   Execute o Laboratório 1, que cria um cluster do OKE no Oracle Cloud Infrastructure.
*   Execute o Laboratório 2, que instala o Verrazzano no cluster do Kubernetes.
*   Execute o Laboratório 3, que implementa o aplicativo Bobs-Books.
*   Você deve ter um editor de texto, no qual pode colar os comandos e URLs e modificá-los, conforme seu ambiente. Em seguida, você pode copiar e colar os comandos modificados para executá-los no _Cloud Shell_.

## Tarefa 1:Faça download do arquivo modificado bobs-books-comp-mod.yaml

1.  Execute o comando a seguir para fazer download do arquivo bobs-books-comp-mod.yaml modificado.
    
        <copy>curl -LSs https://oracle-livelabs.github.io/multicloud/verrazzano/alternate-version/bobs-books-comp-mod.yaml >~/bobs-books-comp-mod.yaml</copy>
        
    
    ![Fazer download do arquivo](images/downloadfile.png " ")
    

## Tarefa 2: Aplicar as Alterações usando o kubectl

1.  Para aplicar as alterações, copie e cole o comando a seguir no _Cloud Shell_. Quando você aplicar a alteração, um novo pod será inicializado para atender às solicitações do novo componente, enquanto o pod associado ao componente antigo continuará atendendo às solicitações. Posteriormente, depois que o novo pod atingir o estado _Em Execução_, o antigo pod começará a ser _Encerrado_. Eventualmente, somente o novo pod estará no estado _Executando_.
    
        <copy>kubectl apply -f ~/bobs-books-comp-mod.yaml -n bobs-books</copy>
        
    
    ![Aplicar alterações](images/applychanges.png " ")
    
    Você pode observar na saída; somente _component.core.oam.dev/bobby-helidon_ está configurado e outros componentes permanecem inalterados.
    
2.  Para ver como o novo pod está sendo inicializado e o antigo pod obtém o estado _Encerrando_, copie e cole o comando a seguir no _Cloud Shell_.
    
        <copy>kubectl get pods -n bobs-books -w</copy>
        
    
    Você verá uma saída semelhante à seguinte:
    
        vera_zano@cloudshell:~ (us-ashburn-1)$ kubectl get pods -n bobs-books
        NAME                                               READY  STATUS   RESTARTS  AGE
        bobbys-coherence-0                                 2/2    Running  0         130m
        bobbys-front-end-adminserver                       4/4    Running  0         127m
        bobbys-front-end-managed-server1                   4/4    Running  0         126m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  0/2    PodInitializing  0         10s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Running  0         130m
        bobs-bookstore-adminserver                         4/4    Running  0         127m
        bobs-bookstore-managed-server1                     4/4    Running  0         126m
        mysql-65d864bf8c-xf64p                             2/2    Running  0         130m
        robert-helidon-bfdfb58b8-58qfs                     2/2    Running  0         130m
        robert-helidon-bfdfb58b8-lkw8m                     2/2    Running  0         130m
        roberts-coherence-0                                2/2    Running  0         130m
        roberts-coherence-1                                2/2    Running  0         130m
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  1/2    Running  0         28s
        bobbys-helidon-stock-application-64fb55cd5b-f8zzp  2/2    Running  0         34s
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  2/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        bobbys-helidon-stock-application-77867fc8dd-wl8h5  0/2    Terminating  0         130m
        vera_zano@cloudshell:~ (us-ashburn-1)$
        
    
    Depois de ver que todos os pods estão no Status _Executando_, pressione _CTRL + C_ para eliminar esse comando.
    

Deixe o _Cloud Shell_ aberto conforme também precisamos dele em nosso último laboratório.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, fevereiro de 2023