# Verificando as alterações por meio do acesso ao aplicativo e da pilha Atualizada

## Introdução

No Laboratório 7, aplicamos alterações em bobbys-helidon-stock-application e o pod para ele está no estado _Em Execução_. Neste laboratório, verificaremos as alterações no aplicativo, na Console do Verrazzano e na Console do Grafana.

Tempo estimado: 05 minutos

### Objetivos

Neste laboratório, você vai:

*   Verifique as alterações no aplicativo Livros de Bobby.
*   Verifique as alterações na Console do Grafana.

### Pré-requisitos

*   Você deve ter um editor de texto, no qual pode colar os comandos e URLs e modificá-los, conforme seu ambiente. Em seguida, você pode copiar e colar os comandos modificados para executá-los no _Cloud Shell_.

## Tarefa 1: Verificar as Alterações na Aplicação dos Livros de Bobby

1.  Abra a guia Livros de Bobby e selecione Atualizar. Se você tiver fechado essa guia, copie e cole o comando a seguir no editor de texto e substitua XX.XX.XX.XX pelo EXTERNAL\_IP para o aplicativo. Você observará que o Nome do Livro está em letras maiúsculas.
    
        <copy>https://bobs-books.bobs-books.XX.XX.XX.XX.nip.io/bobbys-front-end/</copy>
        
    
    ![Livro de Bobby](images/bobbysbooks.png " ")
    

## Tarefa 2: Verificar as Alterações na Console do Grafana

1.  Volte para a Home Page do Verrazzano.
    
    ![Home do Verrazzano](images/verrazzao-home.png " ")
    
2.  Selecione o link do Grafana para abrir a _Console do Grafana_.
    
    ![Link do Grafana](images/grafana-link.png " ")
    
3.  Selecione _Home_, digite _Helidon_ e, em seguida, selecione _Painel de Controle de Monitoramento do Helidon_.
    
    ![Pesquisar Helidon](images/search-helidon.png " ")
    
4.  Em ServiceID, selecione _bobs-books\_default\_bobs-books\_bobby-helidon_ e, na instância, selecione a instância recém-criada. Neste caso, você obterá informações para o _bobby-helidon-stock-application_ modificado.
    
    ![Novo Componente](images/new-component.png " ")
    
    Parabéns! Você concluiu os laboratórios com sucesso.
    

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, agosto de 2023