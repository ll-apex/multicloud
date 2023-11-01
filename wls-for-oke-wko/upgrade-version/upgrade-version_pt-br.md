# Faça um upgrade na Versão do Servidor WebLogic (Opcional)

## Introdução

Neste laboratório, modificamos a imagem principal, usamos a Imagem do Servidor WebLogic com a tag _12.2.1.4-slim-ol8_. Em seguida, reimplantamos o domínio usando a interface do usuário do Kit de Ferramentas do Kubernetes WebLogic. Por último, verificamos se os pods do servidor recém-gerenciado estão usando as imagens atualizadas do Servidor WebLogic por meio da Console Remota WebLogic.

Tempo Estimado: 10 minutos

### Objetivos

Neste laboratório, você vai:

*   Use a Imagem do Servidor WebLogic (12.2.1.4) como Imagem Primária.
*   Reimplante o Domínio WebLogic.

### Pré-requisitos

*   Acesso ao noVNC Remote Desktop criado no laboratório 1.

## Tarefa 1: Informar detalhes da nova Imagem do Servidor WebLogic como Imagem Principal

Nesta tarefa, atualizamos a imagem principal para usar a imagem do Servidor WebLogic 12.2.1.4.0 atualizada.

1.  Volte para a interface do usuário do Kit de Ferramentas do Kubernetes WebLogic, clique em _WebLogic Domain_. A Tag do Servidor WebLogic foi alterada para _12.2.1.4-slim-ol8_. ![Atualizar Tag da Imagem Principal](images/update-primary-image-tag.png)

## Tarefa 2: Atualizar um aplicativo implantado por uma reinicialização incremental dos pods do servidor

Nesta tarefa, reimplantamos o Domínio WebLogic. Posteriormente, usamos a Console Remota WebLogic para verificar se os pods do servidor estão usando a Imagem atualizada do Servidor WebLogic 12.2.1.4.0.

1.  Clique em _Implantar Domínio_. Isso implantará novamente o domínio. ![Reimplantar Domínio](images/redeploy-domain.png)
    
    > **Somente para sua informação:**  
    > à medida que alteramos nossa imagem principal, observaremos a reinicialização incremental dos servidores um por um. Quando você clica em _Implantar Domínio_, ele inicia um _job do Introspector_, que encerra os pods do servidor admin em execução e cria um novo pod para o servidor admin que usa a imagem do Servidor WebLogic 12.2.1.4.0. O Introspector faz o mesmo processo com os dois servidores gerenciados.
    
2.  Depois que você vir a janela _WebLogic Implantação de Domínio no Kubernetes Concluída_, clique em _Ok_. ![Implantação Concluída](images/deployment-complete.png)
    
3.  Volte para o _Terminal_ e copie o comando abaixo e cole no terminal. Você observará a reinicialização incremental dos servidores um por um. Primeiro, os pods do Servidor Admin são encerrados e aparecem no estado _Executando_.
    
        <copy>kubectl get pods -n test-domain-ns -w</copy>
        
    
    ![Exibir Pods](images/view-pods.png)
    
4.  Para verificar se os pods do Servidor Admin e do Servidor Gerenciado estão usando a imagem do Servidor WebLogic atualizada, clique no ícone Árvore de Monitoramento e selecione _Ambiente_ -> _Servidores_ -> _admin-server_. Você pode ver, ele está usando 12.2.1.4.0. ![Versão do WLS](images/wls-version.png)
    

Congratulation !!!

Este é o fim do workshop.

Esperamos que você tenha encontrado esse workshop útil.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, outubro de 2023