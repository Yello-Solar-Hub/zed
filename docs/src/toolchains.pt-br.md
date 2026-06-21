---
título: Conjuntos de ferramentas
descrição: “Os projetos Zed oferecem uma interface de usuário dedicada à seleção da cadeia de ferramentas, que permite escolher um conjunto de ferramentas para trabalhar com uma determinada linguagem no projeto atual.”
---

# Cadeias de ferramentas

Os projetos Zed incluem um seletor de cadeia de ferramentas que permite escolher as ferramentas utilizadas para uma linguagem no projeto atual.

Por exemplo, em projetos em Python, os ambientes virtuais definem as dependências e os caminhos do interpretador. Os servidores de linguagem precisam desse ambiente para analisar seu código corretamente.
Com o seletor de cadeia de ferramentas, você pode escolher o ambiente virtual adequado em um menu suspenso, em vez de configurar manualmente os caminhos do servidor de linguagem.

Você pode até mesmo selecionar diferentes cadeias de ferramentas para diferentes subprojetos dentro do seu projeto Zed. A definição de um subprojeto é específica para cada linguagem.
Em cenários colaborativos, apenas o responsável pelo projeto pode visualizar e modificar uma cadeia de ferramentas ativa.

Em [projetos remotos](./remote-development.md), você pode usar o seletor de cadeia de ferramentas para controlar a cadeia de ferramentas ativa no host SSH. Ao [compartilhar seu projeto](./collaboration/overview.md), o seletor de cadeia de ferramentas não fica disponível para os usuários convidados.

## Por que precisamos de cadeias de ferramentas?

A cadeia de ferramentas ativa é utilizada ao iniciar os servidores de linguagem. Sem a cadeia de ferramentas correta, os servidores de linguagem podem não conseguir resolver as dependências, e recursos como “Ir para a definição” ou “Autocompletar código” podem não funcionar.

A cadeia de ferramentas ativa também é relevante ao iniciar um shell no painel do terminal: algumas cadeias de ferramentas oferecem “scripts de ativação” para shells, que disponibilizam essas cadeias de ferramentas no ambiente do shell para sua conveniência. O Zed executará esses scripts de ativação automaticamente quando você criar um novo terminal.

Isso também se aplica às [tarefas](./tasks.md). O Zed executa as tarefas como se você tivesse aberto uma nova aba no terminal e executado o comando da tarefa por conta própria; portanto, a execução das tarefas também é afetada pela cadeia de ferramentas ativa e pelo seu script de ativação.

## Seleção de cadeias de ferramentas

A cadeia de ferramentas ativa (se houver) é exibida na barra de status à direita. Clique nela para abrir o seletor de cadeias de ferramentas ou execute a ação da paleta de comandos ({#action toolchain::Select}).

O Zed irá inferir automaticamente um conjunto de cadeias de ferramentas para você escolher, com base no projeto em que você está trabalhando. Além disso, uma opção padrão será selecionada para você, da melhor maneira possível, quando você abrir um projeto pela primeira vez.

A seleção da cadeia de ferramentas se aplica ao subprojeto atual, que pode ser todo o seu projeto ou apenas uma parte dele. Em um monorepo, por exemplo, você pode escolher uma cadeia de ferramentas diferente para cada subprojeto.

## Adicionando cadeias de ferramentas manualmente

Se a detecção automática não for suficiente para você, é possível adicionar cadeias de ferramentas manualmente. Para isso, clique no botão “Adicionar cadeia de ferramentas” no seletor de cadeias de ferramentas. A partir daí, você pode informar o caminho para uma cadeia de ferramentas e definir o nome que desejar para ela.
