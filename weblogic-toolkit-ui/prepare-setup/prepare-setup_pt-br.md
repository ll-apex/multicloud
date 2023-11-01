# Preparar Configuração

## Introdução

Este laboratório mostrará como fazer download do arquivo zip da pilha do Oracle Resource Manager (ORM) necessário para configurar o recurso necessário para executar este workshop. Este workshop requer uma instância de computação e uma VCN (Virtual Cloud Network).

Tempo Estimado: 10 minutos

### Objetivos

*   Fazer download da pilha do ORM
*   Configurar uma VCN (Virtual Cloud Network) existente

### Pré-requisitos

Este laboratório pressupõe que você tenha:

*   Uma conta no Oracle Free Tier ou Paid Cloud

## Tarefa 1: Fazer download do arquivo zip da pilha do Oracle Resource Manager (ORM)

1.  Clique no link abaixo para fazer download do arquivo zip do Resource Manager necessário para criar seu ambiente:
    
    _Observação 1:_ Se estiver fornecendo um único download de Pilha para o workshop, use esta expressão simples.
    
    *   [wls-oke-toolkit-mkplc-freetier.zip](https://objectstorage.us-ashburn-1.oraclecloud.com/p/bh1LaVd0DpYAVbAcrL4k-Y1WLC-KAEo117Msw7P2kN-xvNOWGaVcGtjxnkBVumb8/n/natdsecurity/b/stack/o/wls-oke-toolkit-mkplc-freetier.zip)
2.  Salve em sua pasta de downloads.
    

É altamente recomendável usar essa pilha para criar uma VCN independente/dedicada com sua(s) instância(s). Vá para a _Tarefa 3_ para seguir nossas recomendações. Se preferir usar uma VCN existente, vá para a próxima etapa, conforme indicado abaixo, para atualizar sua VCN existente com as regras de Saída necessárias.

## Tarefa 2: Adicionando Regras de Segurança a uma VCN Existente

Este workshop requer que um determinado número de portas esteja disponível, um requisito que pode ser atendido usando a execução de pilha padrão do ORM que cria uma VCN dedicada. Para usar uma VCN existente, as portas a seguir devem ser adicionadas às regras de Entrada.

| Porto | Descrição |
| :-- | :-- |
| 22 | SSH |
| 80 | noVNC Área de Trabalho Remota (Proxy NGINX) |
| 6080 | noVNC Área de Trabalho Remota |

1.  Vá para _Networking >> Virtual Cloud Networks_.
2.  Escolha a rede.
3.  Em Recursos, selecione Listas de Segurança.
4.  Clique em Listas de Segurança Padrão no botão Criar Lista de Segurança
5.  Clique no botão Add Ingress Rule
6.  Informe o seguinte:
    *   CIDR de Origem: 0.0.0.0/0
    *   Faixa de Portas de Destino: _Consulte a tabela acima_
7.  Clique no botão Add Ingress Rules.

## Tarefa 3: Configurar Computação

Usando os detalhes das duas Tarefas acima, vá para o laboratório _Configuração do Ambiente_ para configurar seu ambiente de workshop usando o Oracle Resource Manager (ORM) e uma das seguintes opções:

*   Criar Pilha: _Computação + Rede_
*   Criar Pilha: _Somente computação_ com uma VCN existente na qual as listas de segurança foram atualizadas de acordo com a _Tarefa 2_ acima

Agora você pode ir para o próximo laboratório.

## Confirmação

*   **Autor** - Rene Fontcha, LiveLabs Líder de Plataforma, Tecnologia NA
*   **Contribuidores** - Meghana Banka
*   **Última Atualização em Por/Data** - Rene Fontcha, LiveLabs Líder de Plataforma, Tecnologia NA, fevereiro de 2022