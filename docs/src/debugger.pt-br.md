---
título: Depurador - Zed
descrição: Depure código no Zed usando o Protocolo de Adaptador de Depuração (DAP). Pontos de interrupção, execução passo a passo e inspeção de variáveis em várias linguagens.
---

# Depurador

O Zed utiliza o [Protocolo de Adaptador de Depuração (DAP)](https://microsoft.github.io/debug-adapter-protocol/) para oferecer funcionalidades de depuração em várias linguagens de programação.
O DAP é um protocolo padronizado que define como depuradores, editores e IDEs se comunicam entre si.
Isso permite que o Zed ofereça suporte a vários depuradores sem a necessidade de implementar uma lógica de depuração específica para cada linguagem.
O Zed implementa o lado do cliente do protocolo, e vários _adaptadores de depuração_ implementam o lado do servidor.

Este protocolo permite recursos como definir pontos de interrupção, executar o código passo a passo, inspecionar variáveis,
e muito mais, de maneira consistente entre diferentes linguagens de programação e ambientes de execução.

## Idiomas suportados

Para depurar código escrito em uma linguagem específica, o Zed precisa encontrar um adaptador de depuração para essa linguagem. Alguns adaptadores de depuração são fornecidos pelo Zed sem configuração adicional, enquanto outros são fornecidos pelas [extensões de linguagem](./extensions/debugger-extensions.md). Atualmente, as seguintes linguagens têm adaptadores de depuração disponíveis:

<!-- mantenha isso ordenado -->

- [C](./languages/c.md#debugging) (embutido)
- [C++](./languages/cpp.md#debugging) (embutido)
- [Go](./languages/go.md#debugging) (integrado)
- [Java](./languages/java.md#debugging) (fornecido pela extensão)
- [JavaScript](./languages/javascript.md#debugging) (embutido)
- [PHP](./languages/php.md#debugging) (integrado)
- [Python](./languages/python.md#debugging) (embutido)
- [Ruby](./languages/ruby.md#debugging) (fornecido pela extensão)
- [Rust](./languages/rust.md#debugging) (integrado)
- [Swift](./languages/swift.md#debugging) (fornecido por extensão)
- [TypeScript](./languages/typescript.md#debugging) (integrado)

> Se a sua linguagem não estiver na lista, você pode contribuir adicionando um adaptador de depuração para ela. Consulte nossa documentação sobre [extensões do depurador](./extensions/debugger-extensions.md) para obter mais informações.

Acesse esses links para obter informações e exemplos específicos sobre cada linguagem e adaptador, ou continue lendo para saber mais sobre os recursos gerais de depuração do Zed, que se aplicam a todos os adaptadores.

## Introdução

Para a maioria das linguagens, a maneira mais rápida de começar é executar {#action debugger::Start} ({#kb debugger::Start}). Isso abre o _modal de novo processo_, que exibe uma lista contextual de tarefas de depuração pré-configuradas para o projeto atual. As tarefas de depuração são criadas a partir de testes, pontos de entrada (como uma função `main`) e de outras fontes — consulte a documentação da sua linguagem para obter informações completas sobre o que é suportado.

Você pode abrir o mesmo modal clicando no botão “mais” no canto superior direito do painel de depuração.

Para linguagens que não oferecem tarefas de depuração pré-configuradas (isso inclui C, C++ e algumas linguagens suportadas por extensões), é possível definir configurações de depuração no arquivo `.zed/debug.json`, localizado na raiz do seu projeto. Esse arquivo deve ser uma matriz de objetos de configuração:

```json [debug]
[
  {
    "adapter": "CodeLLDB",
    "label": "First configuration"
    // ...
  },
  {
    "adapter": "Debugpy",
    "label": "Second configuration"
    // ...
  }
]
```

Consulte a documentação do seu idioma para ver exemplos de configurações que abrangem casos de uso típicos. Depois de adicionar as configurações ao arquivo `.zed/debug.json`, elas aparecerão na lista do modal de novo processo.

O Zed também carregará as configurações de depuração do arquivo `.vscode/launch.json` e as exibirá na janela modal de novo processo caso não sejam encontradas configurações no arquivo `.zed/debug.json`.

#### Configurações globais de depuração

Se você executar os mesmos perfis de inicialização em vários projetos, poderá armazená-los uma única vez na sua configuração de usuário. Chame {#action zed::OpenDebugTasks} na paleta de comandos para abrir o arquivo global `debug.json`; o Zed o cria ao lado do seu arquivo `settings.json` de usuário e o mantém sincronizado com a interface do depurador. O arquivo fica em:

- **macOS:** `~/Library/Application Support/Zed/debug.json`
- **Linux/BSD:** `$XDG_CONFIG_HOME/zed/debug.json` (se não for encontrado, usa `~/.config/zed/debug.json`)
- **Windows:** `%APPDATA%\Zed\debug.json`

Preencha este arquivo com o mesmo conjunto de objetos que você colocaria em `.zed/debug.json`. Todos os cenários definidos nesse arquivo são incorporados a cada espaço de trabalho, de modo que suas predefinições de inicialização favoritas aparecem automaticamente na caixa de diálogo “Nova sessão de depuração”.

### Lançamento e fixação

O depurador Zed oferece duas maneiras de depurar seu programa; você pode _iniciar_ uma nova instância do seu programa ou _conectar-se_ a um processo já em execução.
A escolha depende do que você está tentando alcançar.

Ao iniciar uma nova instância, o Zed (e o adaptador de depuração subjacente) costuma ser mais eficaz na captura das informações de depuração do que ao se conectar a um processo já existente, uma vez que controla o ciclo de vida de todo o programa.
Executar testes unitários ou uma compilação de depuração do seu aplicativo é um bom caso de uso para a inicialização.

Em comparação com a inicialização, anexar-se a um processo existente pode parecer uma opção inferior, mas isso está longe de ser verdade; há casos em que não é possível reiniciar o programa, porque, por exemplo, o bug não é reproduzível fora de um ambiente de produção ou devido a outras circunstâncias.

## Configuração

O Zed exige os campos `adapter` e `label` para todas as tarefas de depuração. Além disso, o Zed utilizará o campo `build` para executar quaisquer etapas de configuração necessárias antes do início do depurador [(veja abaixo)](#build-tasks) e pode aceitar um campo `tcp_connection` para se conectar a um processo existente.

Todos os demais campos são fornecidos pelo adaptador de depuração e podem conter [variáveis de tarefa](./tasks.md#variables). A maioria dos adaptadores suporta `request`, `program` e `cwd`:

```json [debug]
[
  {
    // The label for the debug configuration and used to identify the debug session inside the debug panel & new process modal
    "label": "Example Start debugger config",
    // The debug adapter that Zed should use to debug the program
    "adapter": "Example adapter name",
    // Request:
    //  - launch: Zed will launch the program if specified, or show a debug terminal with the right configuration
    //  - attach: Zed will attach to a running program to debug it, or when the process_id is not specified, will show a process picker (only supported for node currently)
    "request": "launch",
    // The program to debug. This field supports path resolution with ~ or . symbols.
    "program": "path_to_program",
    // cwd: defaults to the current working directory of your project ($ZED_WORKTREE_ROOT)
    "cwd": "$ZED_WORKTREE_ROOT"
  }
]
```

Consulte a documentação do seu adaptador de depuração para obter mais informações sobre os campos que ele suporta.

### Tarefas de compilação

O Zed permite incorporar uma tarefa Zed no campo `build`, que é executada antes do depurador ser iniciado. Isso é útil para configurar o ambiente ou executar quaisquer etapas de configuração necessárias antes do depurador ser iniciado.

```json [debug]
[
  {
    "label": "Build Binary",
    "adapter": "CodeLLDB",
    "program": "path_to_program",
    "request": "launch",
    "build": {
      "command": "make",
      "args": ["build", "-j8"]
    }
  }
]
```

As tarefas de compilação também podem fazer referência às tarefas existentes por meio de um rótulo sem substituição:

```json [debug]
[
  {
    "label": "Build Binary",
    "adapter": "CodeLLDB",
    "program": "path_to_program",
    "request": "launch",
    "build": "my build task" // Or "my build task for $ZED_FILE"
  }
]
```

### Criação automática de cenários

Quando recebe uma tarefa no Zed, o Zed pode criar automaticamente um cenário para você. A criação automática de cenários também é a base da nossa funcionalidade de criação de cenários a partir da margem interna.
Atualmente, a criação automática de cenários é compatível com Rust, Go, Python, JavaScript e TypeScript.

## Pontos de interrupção

Para definir um ponto de interrupção, basta clicar ao lado do número da linha na margem lateral do editor.
Os pontos de interrupção podem ser ajustados de acordo com suas necessidades; para acessar opções adicionais de um determinado ponto de interrupção, clique com o botão direito do mouse no ícone do ponto de interrupção na margem e selecione a opção desejada.
No momento, você pode:

- Adicione um registro de log a um ponto de interrupção, o que fará com que uma mensagem de log seja exibida sempre que esse ponto de interrupção for atingido.
- Defina o ponto de interrupção como condicional, de modo que a execução só seja interrompida nesse ponto quando a condição for satisfeita. A sintaxe das condições varia de acordo com o adaptador.
- Adicione um contador de ocorrências a um ponto de interrupção, de modo que ele só pare nesse ponto após ter sido atingido um determinado número de vezes.
- Desative um ponto de interrupção, o que impedirá que ele seja acionado, mantendo-o visível na margem lateral.

Alguns adaptadores de depuração (por exemplo, CodeLLDB e JavaScript) também _verificam_ se seus pontos de interrupção podem ser acionados; os pontos de interrupção que não podem ser acionados são destacados de forma mais visível na interface do usuário.

Todos os pontos de interrupção ativados para um determinado projeto também são listados no item “Pontos de interrupção” na interface de usuário da sua sessão de depuração. A partir do item “Pontos de interrupção” na interface de usuário, você também pode gerenciar pontos de interrupção de exceção.
O adaptador de depuração será interrompido sempre que ocorrer uma exceção de um determinado tipo. Os tipos de exceção suportados dependem do adaptador de depuração.

## Trabalhando com painéis divididos

Ao depurar com vários painéis divididos abertos, o Zed exibe a linha de depuração ativa em um painel e mantém o layout dos demais. Se você tiver o mesmo arquivo aberto em vários painéis, o depurador escolhe o painel em que o arquivo já estiver na guia ativa — ele não alterna entre as guias nos painéis em que o arquivo estiver inativo.

Assim que o depurador seleciona um painel, ele continua usando esse painel para os pontos de interrupção subsequentes durante a sessão. Se você arrastar a aba com a linha de depuração ativa para uma divisão diferente, o depurador acompanha a mudança e passa a usar o novo painel.

Isso garante que o depurador não atrapalhe seu fluxo de trabalho ao percorrer o código em arquivos diferentes.

## Configurações

As configurações do depurador estão agrupadas na chave `debugger` no arquivo `settings.json`:

- `dock`: Determina a posição do painel de depuração na interface do usuário.
- `stepping_granularity`: Determina a granularidade do passo.
- `save_breakpoints`: Se os pontos de interrupção devem ser reutilizados entre as sessões do Zed.
- `button`: Se o botão de depuração deve ser exibido na barra de status.
- `timeout`: Tempo, em milissegundos, até ocorrer um erro de tempo limite ao se conectar a um adaptador de depuração TCP.
- `log_dap_communications`: Se as mensagens entre os adaptadores de depuração ativos e o Zed devem ser registradas no log.
- `format_dap_log_messages`: Determina se as mensagens DAP devem ser formatadas ao serem adicionadas ao registrador do adaptador de depuração.

### Doca

- Descrição: A posição do painel de depuração na interface do usuário.
- Padrão: `bottom`
- Configuração: debugger.dock

**Opções**

1. `left` - O painel de depuração ficará ancorado no lado esquerdo da interface do usuário.
2. `direita` - O painel de depuração ficará ancorado no lado direito da interface do usuário.
3. `bottom` - O painel de depuração ficará ancorado na parte inferior da interface do usuário.

```json [settings]
"debugger": {
  "dock": "bottom"
},
```

### Nível de granularidade dos passos

- Descrição: A granularidade de passo que o depurador utilizará
- Padrão: `line`
- Configuração: `debugger.stepping_granularity`

**Opções**

1. Instrução — Essa etapa deve permitir que o programa continue sendo executado até que a instrução atual tenha concluído sua execução.
   O significado de uma instrução é determinado pelo adaptador e pode ser considerado equivalente a uma linha.
   Por exemplo, `for(int i = 0; i < 10; i++)` poderia ser considerado como tendo três instruções: `int i = 0`, `i < 10` e `i++`.

```json [settings]
{
  "debugger": {
    "stepping_granularity": "statement"
  }
}
```

2. Linha — O passo deve permitir que o programa seja executado até que a linha atual do código-fonte tenha sido executada.

```json [settings]
{
  "debugger": {
    "stepping_granularity": "line"
  }
}
```

3. Instrução — O passo deve permitir a execução de uma instrução (por exemplo, uma instrução x86).

```json [settings]
{
  "debugger": {
    "stepping_granularity": "instruction"
  }
}
```

### Salvar pontos de interrupção

- Descrição: Se os pontos de interrupção devem ser salvos entre as sessões do Zed.
- Padrão: `true`
- Configuração: `debugger.save_breakpoints`

**Opções**

valores `booleanos`

```json [settings]
{
  "debugger": {
    "save_breakpoints": true
  }
}
```

### Botão

- Descrição: Se o botão deve ser exibido na barra de ferramentas do depurador.
- Padrão: `true`
- Configuração: `debugger.button`

**Opções**

valores `booleanos`

```json [settings]
{
  "debugger": {
    "button": true
  }
}
```

### Tempo limite

- Descrição: Tempo, em milissegundos, até ocorrer um erro de tempo limite ao se conectar a um adaptador de depuração TCP.
- Padrão: `2000`
- Configuração: `debugger.timeout`

**Opções**

valores `inteiros`

```json [settings]
{
  "debugger": {
    "timeout": 3000
  }
}
```

### Valores em linha

- Descrição: Determina se as dicas embutidas no editor, que mostram os valores das variáveis no seu código durante as sessões de depuração, devem ser ativadas.
- Padrão: `true`
- Configuração: `inlay_hints.show_value_hints`

**Opções**

```json [settings]
{
  "inlay_hints": {
    "show_value_hints": false
  }
}
```

As sugestões de valores embutidas também podem ser ativadas ou desativadas no menu “Controles do Editor”, na barra de ferramentas do editor.

### Log Dap Communications

- Descrição: Determina se as mensagens entre os adaptadores de depuração ativos e o Zed devem ser registradas. (Utilizado para o desenvolvimento de DAP)
- Padrão: false
- Configuração: debugger.log_dap_communications

**Opções**

valores `booleanos`

```json [settings]
{
  "debugger": {
    "log_dap_communications": true
  }
}
```

### Formatar mensagens de log do Dap

- Descrição: Determina se as mensagens DAP devem ser formatadas ao serem adicionadas ao registrador do adaptador de depuração. (Utilizado no desenvolvimento de DAP)
- Padrão: false
- Configuração: debugger.format_dap_log_messages

**Opções**

valores `booleanos`

```json [settings]
{
  "debugger": {
    "format_dap_log_messages": true
  }
}
```

### Personalização de adaptadores de depuração

- Descrição: Caminho e argumentos personalizados do programa para substituir a forma como o Zed inicia um adaptador de depuração específico.
- Padrão: específico do adaptador
- Configuração: `dap.$ADAPTER.binary` e `dap.$ADAPTER.args`

Você pode passar `binary`, `args` ou ambos. `binary` deve ser um caminho para um _adaptador de depuração_ (como `lldb-dap`), e não para um _depurador_ (como o próprio `lldb`). A configuração `args` substitui quaisquer argumentos que o Zed, de outra forma, passaria para o adaptador.

```json [settings]
{
  "dap": {
    "CodeLLDB": {
      "binary": "/Users/name/bin/lldb-dap",
      "args": ["--wait-for-debugger"]
    }
  }
}
```

## Tema

O Debugger oferece as seguintes opções de tema:

- `debugger.accent`: Cor usada para destacar pontos de interrupção e símbolos relacionados a pontos de interrupção
- `editor.debugger_active_line.background`: Cor de fundo da linha de depuração ativa

## Solução de problemas

Se você estiver enfrentando problemas com o depurador, por favor, [abra uma issue no GitHub](https://github.com/zed-industries/zed/issues/new?template=04_bug_debugger.yml), fornecendo o máximo de contexto possível. Há também alguns recursos que você pode usar para coletar mais informações sobre o problema:

- Quando você tiver uma sessão em execução no painel de depuração, poderá executar a ação {#action dev::CopyDebugAdapterArguments} para copiar para a área de transferência um blob JSON que descreve como o Zed inicializou a sessão. Isso é especialmente útil quando a sessão não consegue iniciar e é um excelente contexto a ser incluído caso você abra uma issue no GitHub.
- Você também pode usar a ação {#action dev::OpenDebugAdapterLogs} para visualizar um rastreamento de todas as comunicações do Zed com os adaptadores de depuração durante as sessões de depuração mais recentes.
