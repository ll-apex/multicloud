# Configurar o Ambiente de Laboratório

## Introdução

Neste laboratório, você criará um repositório no Oracle Cloud Container Image Registry. Além disso, você aceitará o contrato de licença para imagens do Servidor WebLogic no Oracle Container Registry.

Tempo Estimado: 15 minutos

### Objetivos

Neste laboratório, você vai:

*   Crie um repositório no Oracle Cloud Container Image Registry.
*   Aceite a licença para Imagens do Servidor WebLogic no Oracle Container Registry.

### Pré-requisitos

*   Você deve ter uma conta ativada do [Oracle Cloud Infrastructure](https://cloud.oracle.com/en_US/cloud-infrastructure).
*   Você deve ter uma Conta Oracle.
*   Você deve ter um editor de texto.

## Tarefa 1: Criação de um Repositório

Nesta tarefa, você cria um repositório público. No laboratório 5, enviaremos a Imagem Auxiliar neste repositório.

1.  No Remote Desktop, para abrir o Firefox, clique em **Atividades**, digite **Fire** na caixa de pesquisa e clique em **Firefox**, conforme mostrado. ![open firefox](images/open-firefox.png)
    
2.  Faça log-in na [Console do Oracle Cloud](https://cloud.oracle.com) usando suas credenciais.
    
3.  Na Console, selecione o _Menu do Hambúrguer_ -> _Serviços do Desenvolvedor_ -> _Registro de Contêiner_, conforme mostrado. ![Registro de contêiner](images/container-registry.png)
    
4.  Selecione seu compartimento, no qual você tem permissão para criar o repositório. Clique em _Criar repositório_. ![Criar Repositório](images/create-repository.png)
    
5.  Informe _`test-model-your_firstname`_ como nome do Repositório e Acesso como _Público_ e clique em _Criar_. ![Detalhes do Repositório](images/repository-details.png)
    
6.  Quando seu repositório estiver pronto. Anote o namespace da tenancy no seu arquivo de texto. ![Observação da Tenancy NameSpace](images/tenancy-namespace.png)
    

## Tarefa 2: Aceitando a licença para Imagens do Servidor WebLogic

Nesta tarefa, aceitamos o contrato de licença para imagens do Servidor WebLogic reside no Oracle Container Registry. Como no Laboratório 5, usaremos a imagem do Servidor WebLogic 12.2.1.3.0 como nossa Imagem Principal. Portanto, para obter acesso às Imagens do Servidor WebLogic, aceitamos o contrato de licença.

1.  Copie e cole o link do Oracle Container Registry [https://container-registry.oracle.com/](https://container-registry.oracle.com/) no browser e entre. Para isso, você precisa de uma Conta Oracle. ![Acesso ao Registro de Contêiner](images/container-registry-sign-in.png)
    
2.  Informe suas _Credenciais da Conta Oracle_ nos campos Nome de Usuário e Senha e clique em _Acessar_. ![Log-in no Container Registry](images/login-container-registry.png)
    
3.  Na Home page do Oracle Container Registry, Procure _weblogic_. ![Pesquisar WebLogic](images/search-weblogic.png)
    
4.  Clique em _weblogic_ conforme mostrado e selecione _Inglês_ como idioma. Em seguida, clique em _Continuar_. ![Clique em WebLogic](images/click-weblogic.png) ![Selecionar Idioma](images/select-language.png)
    
5.  Clique em _Aceitar_ para aceitar o contrato de licença. ![Aceitar Licença](images/accept-license.png)
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, outubro de 2023