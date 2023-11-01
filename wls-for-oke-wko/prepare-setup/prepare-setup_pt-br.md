# Preparar Configuração

## Introdução

Este laboratório mostrará como fazer download do arquivo zip da pilha do Oracle Resource Manager (ORM) necessário para configurar o recurso necessário para executar este workshop. Em seguida, você cria uma instância de computação e uma VCN (Virtual Cloud Network) que fornece acesso a uma área de trabalho remota.

Tempo Estimado: 10 minutos

### Objetivos

*   Fazer download da pilha do ORM
*   Criar Computação + Rede usando a Pilha do Gerenciador de Recursos

### Pré-requisitos

Este laboratório pressupõe que você tenha:

*   Uma conta no Oracle Free Tier ou Paid Cloud

## Tarefa 1: Fazer download do arquivo zip da pilha do Oracle Resource Manager (ORM)

1.  Clique no link abaixo para fazer download do arquivo zip do Resource Manager necessário para criar seu ambiente:
    
    _Observação 1:_ Se estiver fornecendo um único download de Pilha para o workshop, use esta expressão simples.
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  Salve em sua pasta de downloads.
    

## Tarefa 2: Criar Pilha: Computação + Rede

1.  Identifique o arquivo zip da pilha do ORM baixado na **Tarefa 1: Fazer Download do arquivo zip da pilha do Oracle Resource Manager (ORM)**.
    
2.  Abra o menu de hambúrguer no canto superior esquerdo. Clique em **Serviços ao Desenvolvedor** e escolha **Gerenciador de Recursos** > **Pilhas**. Escolha o compartimento no qual você deseja instalar a pilha. Clique em **Criar Pilha**. ![pilha de menus](images/menu-stack.png) ![selecionar compartimento](images/select-compartment.png)
    
3.  Selecione **Minha Configuração**, escolha **. Botão de arquivo zip**, clique no link **Procurar** e selecione o arquivo zip cujo download você fez ou arraste-o para o explorador de arquivos. Clique em **Próximo**. ![procurar zip](images/browse-zip.png)
    
4.  Informe ou selecione o seguinte e clique em **Próximo**.
    
    **Contagem de Instâncias:** Aceite o padrão, 1.
    
    **Selecionar Domínio de Disponibilidade:** Selecione um domínio de disponibilidade na lista drop-down.
    
    **Precisa de Acesso Remoto via SSH?** Manter desmarcado para acesso somente na área de trabalho remota - O padrão.
    
    **Usar Forma de Instância Flexível com Contagem de OCPUs Ajustável?:** Mantenha o padrão como marcado (a menos que você planeje usar uma forma fixa).
    
    **Forma da Instância:** Mantenha o padrão ou selecione na lista de formas Flex no menu drop-down (por exemplo, VM.Standard.E4. Flex).
    
    **Selecionar Contagem de OCPUs por Instância:** Aceite o padrão mostrado. Por exemplo, (1) provisionará 1 OCPUs e 16 GB de memória.
    
    **Usar VCN Existente?:** Aceite o padrão deixando-o desmarcado. Isso criará uma nova VCN. ![configuração principal](images/main-config.png) ![forma da instância](images/instance-shape.png)
    
5.  Selecione **Executar Aplicação** e clique em **Criar**. ![executar aplicação](images/run-apply.png)
    

Agora você pode ir para o próximo laboratório.

## Confirmação

*   **Autor** - Ankit Pandey
*   **Contribuidores** - Maciej Gruszka, Sid Joshi
*   **Última Atualização por/Data** - Ankit Pandey, outubro de 2023