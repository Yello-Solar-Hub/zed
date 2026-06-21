---
título: Tarefas - Executar comandos no Zed
descrição: Execute e reexecute comandos de shell a partir do Zed usando definições de tarefas. Suporta variáveis, modelos e tarefas específicas de linguagem.
---

# Tarefas

O Zed oferece maneiras de executar (e reexecutar) comandos usando seu [terminal](./terminal.md) integrado para exibir os resultados. Esses comandos podem ler um subconjunto limitado do estado do Zed (como o caminho do arquivo que está sendo editado no momento ou o texto selecionado).

```json [tasks]
[
  {
    "label": "Example task",
    "command": "for i in {1..5}; do echo \"Hello $i/5\"; sleep 1; done",
    //"args": [],
    // Env overrides for the command, will be appended to the terminal's environment from the settings.
    "env": { "foo": "bar" },
    // Current working directory to spawn the command into, defaults to current project root.
    //"cwd": "/path/to/working/directory",
    // Whether to use a new terminal tab or reuse the existing one to spawn the process, defaults to `false`.
    "use_new_terminal": false,
    // Whether to allow multiple instances of the same task to be run, or rather wait for the existing ones to finish, defaults to `false`.
    "allow_concurrent_runs": false,
    // What to do with the terminal pane and tab, after the command was started:
    // * `always` — always show the task's pane, and focus the corresponding tab in it (default)
    // * `no_focus` — always show the task's pane, add the task's tab in it, but don't focus it
    // * `never` — do not alter focus, but still add/reuse the task's tab in its pane
    "reveal": "always",
    // What to do with the terminal pane and tab, after the command has finished:
    // * `never` — Do nothing when the command finishes (default)
    // * `always` — always hide the terminal tab, hide the pane also if it was the last tab in it
    // * `on_success` — hide the terminal tab on task success only, otherwise behaves similar to `always`
    "hide": "never",
    // Which shell to use when running a task inside the terminal.
    // May take 3 values:
    // 1. (default) Use the system's default terminal configuration in /etc/passwd
    //      "shell": "system"
    // 2. A program:
    //      "shell": {
    //        "program": "sh"
    //      }
    // 3. A program with arguments:
    //     "shell": {
    //         "with_arguments": {
    //           "program": "/bin/bash",
    //           "args": ["--login"]
    //         }
    //     }
    "shell": "system",
    // Whether to show the task line in the output of the spawned task, defaults to `true`.
    "show_summary": true,
    // Whether to show the command line in the output of the spawned task, defaults to `true`.
    "show_command": true,
    // Which edited buffers to save before running the task:
    // * `all` — save all edited buffers
    // * `current` — save currently active buffer only
    // * `none` — don't save any buffers
    "save": "none"
    // Represents the tags for inline runnable indicators, or spawning multiple tasks at once.
    // "tags": []
  }
]
```

Existem duas ações que orientam o fluxo de trabalho no uso de tarefas: {#action task::Spawn} e {#action task::Rerun}.
{#action task::Spawn} abre um modal com todas as tarefas disponíveis no arquivo atual.
{#action task::Rerun} reexecuta a tarefa criada mais recentemente. Você também pode reexecutar tarefas a partir da janela modal de tarefas.

Por padrão, a reexecução de tarefas reutiliza o mesmo terminal (devido à configuração padrão `"use_new_terminal": false`), mas aguarda a conclusão da tarefa anterior antes de iniciar (devido à configuração padrão `"allow_concurrent_runs": false`).

Mantenha `"use_new_terminal": false` e defina `"allow_concurrent_runs": true` para permitir o cancelamento de tarefas anteriores ao executar novamente.

## Modelos de tarefas

É possível definir tarefas:

- no arquivo global `tasks.json`; essas tarefas estão disponíveis em todos os projetos Zed nos quais você trabalha. Esse arquivo geralmente fica em `~/.config/zed/tasks.json`. Você pode editá-las usando a ação {#action zed::OpenTasks}.
- no arquivo `.zed/tasks.json` específico da árvore de trabalho (local); essas tarefas estão disponíveis apenas ao trabalhar em um projeto que inclua essa árvore de trabalho. Você pode editar tarefas específicas da árvore de trabalho usando a ação {#action zed::OpenProjectTasks}.
- na hora, com [tarefas únicas](#oneshot-tasks). Essas tarefas são específicas do projeto e não são mantidas entre as sessões.
- por extensão de idioma.

## Variáveis

As tarefas do Zed funcionam exatamente como o seu shell; isso também significa que você pode acessar variáveis de ambiente usando a sintaxe `$VAR_NAME`, semelhante à do sh. Algumas variáveis de ambiente adicionais foram definidas para sua conveniência.
Essas variáveis permitem que você extraia informações do editor atual e as utilize em suas tarefas. Estão disponíveis as seguintes variáveis:

- `ZED_COLUMN`: coluna da linha atual
- `ZED_ROW`: linha atual
- `ZED_FILE`: caminho absoluto do arquivo aberto no momento (por exemplo, `/Users/my-user/path/to/project/src/main.rs`)
- `ZED_FILENAME`: nome do arquivo atualmente aberto (por exemplo, `main.rs`)
- `ZED_DIRNAME`: caminho absoluto do arquivo aberto no momento, sem o nome do arquivo (por exemplo, `/Users/my-user/path/to/project/src`)
- `ZED_RELATIVE_FILE`: caminho do arquivo aberto no momento, relativo a `ZED_WORKTREE_ROOT` (por exemplo, `src/main.rs`)
- `ZED_RELATIVE_DIR`: caminho do diretório do arquivo atualmente aberto, relativo a `ZED_WORKTREE_ROOT` (por exemplo, `src`)
- `ZED_STEM`: parte do nome (nome do arquivo sem extensão) do arquivo aberto no momento (por exemplo, `main`)
- `ZED_SYMBOL`: símbolo selecionado no momento; deve corresponder ao último símbolo exibido na trilha de navegação de símbolos (por exemplo, `mod tests > fn test_task_contexts`)
- `ZED_SELECTED_TEXT`: texto selecionado no momento
- `ZED_LANGUAGE`: idioma do buffer aberto no momento (por exemplo, `Rust`, `Python`, `Shell Script`)
- `ZED_WORKTREE_ROOT`: caminho absoluto para a raiz da árvore de trabalho atual. (por exemplo, `/Users/my-user/path/to/project`)
- `ZED_MAIN_GIT_WORKTREE`: caminho absoluto para o diretório de trabalho da árvore de trabalho principal do Git. Para checkouts normais, isso equivale a `ZED_WORKTREE_ROOT`; para árvores de trabalho do Git vinculadas, trata-se do diretório de trabalho do repositório original.
- `ZED_CUSTOM_RUST_PACKAGE`: (específico do Rust) nome do pacote pai do arquivo-fonte $ZED_FILE.

Para usar uma variável em uma tarefa, coloque um sinal de dólar (`$`) na frente dela:

```json [tasks]
{
  "label": "echo current file's path",
  "command": "echo $ZED_FILE"
}
```

Você também pode usar a sintaxe detalhada, que permite especificar um valor padrão caso uma determinada variável não esteja disponível: `${ZED_FILE:default_value}`

Essas variáveis ambientais também podem ser utilizadas nos campos `cwd`, `args` e `label` das tarefas.

### Cotação de variáveis

Ao trabalhar com caminhos que contenham espaços ou outros caracteres especiais, certifique-se de que as variáveis estejam devidamente escapadas.

Por exemplo, em vez disso (o que dará erro se o caminho contiver um espaço):

```json [tasks]
{
  "label": "stat current file",
  "command": "stat $ZED_FILE"
}
```

Forneça o seguinte:

```json [tasks]
{
  "label": "stat current file",
  "command": "stat",
  "args": ["$ZED_FILE"]
}
```

Ou inclua explicitamente aspas escapadas desta forma:

```json [tasks]
{
  "label": "stat current file",
  "command": "stat \"$ZED_FILE\""
}
```

### Filtragem de tarefas com base em variáveis

As definições de tarefas que contêm variáveis ausentes no momento em que a lista de tarefas é determinada são filtradas.
Por exemplo, a tarefa a seguir aparecerá na janela modal de exibição somente se houver uma seleção de texto:

```json [tasks]
{
  "label": "selected text",
  "command": "echo \"$ZED_SELECTED_TEXT\""
}
```

Defina valores padrão para essas variáveis para que essas tarefas sejam sempre exibidas:

```json [tasks]
{
  "label": "selected text with default",
  "command": "echo \"${ZED_SELECTED_TEXT:no text selected}\""
}
```

## Tarefas pontuais

A mesma janela modal de tarefa aberta por meio de {#action task::Spawn} permite a execução de comandos arbitrários semelhantes aos do Bash: digite um comando no campo de texto da janela modal e use `opt-enter` para executá-lo.

A janela modal de tarefas mantém esses comandos ad hoc ativos durante toda a sessão; o comando {#action task::Rerun} também reexecutará essas tarefas caso tenham sido as últimas a serem iniciadas.

Você também pode ajustar a tarefa selecionada no momento em uma janela modal (a tecla `tab` é a combinação de teclas padrão). Ao fazer isso, o comando da tarefa será inserido em um prompt, que poderá ser editado e executado como uma tarefa única.

### Tarefas temporárias

Você pode usar o modificador `cmd` ao criar uma tarefa por meio de um modal; as tarefas criadas dessa forma não terão sua contagem de uso aumentada (portanto, não serão recriadas com {#action task::Rerun} e não terão uma classificação alta no modal de tarefas).
O objetivo das tarefas efêmeras é manter o fluxo de trabalho com o uso contínuo da função {#action task::Rerun}.

### Mais controle sobre a reexecução de tarefas

Por padrão, as tarefas capturam suas variáveis em um contexto uma única vez, e essa “tarefa resolvida” é sempre executada novamente.

Isso pode ser controlado com o argumento `"reevaluate_context"` da tarefa: definir esse argumento como `true` forçará a reavaliação da tarefa antes de cada execução.

```json [keymap]
{
  "context": "Workspace",
  "bindings": {
    "alt-t": ["task::Rerun", { "reevaluate_context": true }]
  }
}
```

## Atribuições de teclas personalizadas para tarefas

Você pode definir seus próprios atalhos de teclado para suas tarefas por meio de um argumento adicional à função `task::Spawn`. Se quiser associar a tarefa mencionada anteriormente, “exibir o caminho do arquivo atual”, ao atalho `alt-g`, basta adicionar o trecho a seguir ao seu arquivo [`keymap.json`](./key-bindings.md):

```json [keymap]
{
  "context": "Workspace",
  "bindings": {
    "alt-g": ["task::Spawn", { "task_name": "echo current file's path" }]
  }
}
```

Observe que essas tarefas também podem ter um “destino” especificado para controlar onde a tarefa gerada deve aparecer.
Isso pode ser útil para abrir um aplicativo de terminal que você deseja usar na área central:

```json [tasks]
// In tasks.json
{
  "label": "start lazygit",
  "command": "lazygit -p $ZED_WORKTREE_ROOT"
}
```

```json [keymap]
// In keymap.json
{
  "context": "Workspace",
  "bindings": {
    "alt-g": [
      "task::Spawn",
      { "task_name": "start lazygit", "reveal_target": "center" }
    ]
  }
}
```

## Ganchos

Além de serem iniciadas manualmente, as tarefas podem ser configuradas para serem executadas automaticamente em resposta a determinados eventos do Zed, adicionando-se um hook ao campo `hooks` de um modelo de tarefa. Uma tarefa com um hook correspondente será resolvida e iniciada quando esse evento ocorrer.

Atualmente, os seguintes ganchos são suportados:

- `create_worktree` — é executado depois que o Zed cria uma nova árvore de trabalho Git vinculada, seja diretamente pela CLI ou a partir do [seletor de árvores de trabalho](./git.md#git-worktrees). A tarefa é iniciada com `ZED_WORKTREE_ROOT` apontando para a árvore de trabalho recém-criada e `ZED_MAIN_GIT_WORKTREE` apontando para o diretório de trabalho do repositório original, o que torna esses hooks ideais para copiar arquivos não rastreados (como arquivos `.env`) ou executar comandos de configuração específicos para cada árvore de trabalho.

As tarefas de hook são processadas a partir dos mesmos arquivos `tasks.json` — tanto globais quanto locais na árvore de trabalho — que as tarefas iniciadas manualmente, e várias tarefas podem ser registradas para o mesmo hook; todas elas são executadas quando o hook é acionado. Uma tarefa de hook ainda se beneficia dos campos habituais de configuração de tarefas — `cwd`, `env`, `reveal`, `hide` e assim por diante —, de modo que você pode controlar o quanto da interface do terminal é exibido enquanto ela é executada.

```json [tasks]
[
  {
    "label": "copy .env into new worktree",
    "command": "cp",
    "args": ["$ZED_MAIN_GIT_WORKTREE/.env", "$ZED_WORKTREE_ROOT/.env"],
    "hooks": ["create_worktree"],
    "reveal": "no_focus",
    "hide": "on_success"
  }
]
```

As tarefas que definem `hooks` continuam disponíveis na janela modal de tarefas, assim como qualquer outra tarefa, de modo que o mesmo modelo pode ser reutilizado para execuções manuais.

## Comandos personalizados do Git

O Git Graph permite executar tarefas com comandos personalizados do Git a partir do menu de contexto do commit.
Para adicionar um comando, defina uma tarefa no seu arquivo global `tasks.json` com a tag `git-command` (tarefas locais da árvore de trabalho ainda não são suportadas).
Quando exibida a partir do menu de contexto de um commit, a tarefa é resolvida com base no commit e no repositório selecionados e, por padrão, é executada a partir da raiz do repositório selecionado.
Ao clicar com o botão direito do mouse em uma referência (um branch, uma referência remota ou uma tag), abre-se um menu de contexto específico para essa referência, no qual a tarefa é adicionalmente associada à referência clicada por meio de `ZED_GIT_REF`.

As tarefas do comando Git Graph suportam as variáveis de tarefa específicas do Git listadas abaixo.
Essas variáveis são fornecidas apenas durante a resolução de tarefas do comando Git Graph.
Outras variáveis de tarefa, como `ZED_FILE`, `ZED_SELECTED_TEXT`, `ZED_WORKTREE_ROOT` e `ZED_MAIN_GIT_WORKTREE`, não são fornecidas às tarefas do comando Git Graph, a menos que utilizem valores padrão.

- `ZED_GIT_SHA`: SHA completo do commit selecionado.
- `ZED_GIT_SHA_SHORT`: SHA curto do commit selecionado.
- `ZED_GIT_REPOSITORY_NAME`: nome do repositório Git selecionado.
- `ZED_GIT_REPOSITORY_PATH`: caminho absoluto para o diretório de trabalho do repositório Git selecionado.
- `ZED_GIT_REF`: nome da referência clicada (um branch, uma referência remota ou uma tag). Só é fornecido quando o menu é aberto a partir do rótulo de uma referência.

Por exemplo:

```json [tasks]
[
  {
    "label": "Branches containing commit: $ZED_GIT_SHA_SHORT",
    "command": "git",
    "args": ["branch", "-a", "--contains", "$ZED_GIT_SHA"],
    "tags": ["git-command"]
  },
  {
    "label": "Check out $ZED_GIT_REF",
    "command": "git",
    "args": ["checkout", "$ZED_GIT_REF"],
    "tags": ["git-command"]
  }
]
```

## Formato de tarefas do VS Code

Ao importar tarefas do VS Code a partir do arquivo `.vscode/tasks.json`, é possível omitir o campo `label`. O Zed gera automaticamente os rótulos com base no tipo de tarefa:

- **Tarefas do npm**: `npm: <script>` (por exemplo, `npm: start`)
- **tarefas do gulp**: `gulp: <tarefa>` (por exemplo, `gulp: build`)
- **tarefas do shell**: Utiliza a string `comando` diretamente (por exemplo, `echo hello`) ou `shell` se o comando estiver vazio
- **Tarefas sem tipo**: `Tarefa sem título`

Arquivo de tarefas de exemplo com rótulos gerados automaticamente:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "type": "npm",
      "script": "start"
    },
    {
      "type": "shell",
      "command": "cargo build --release"
    }
  ]
}
```

Essas tarefas aparecem no seletor de tarefas como “npm: start” e “cargo build --release”. Você pode substituir o rótulo gerado especificando explicitamente o campo `label`.

## Vinculação de tags executáveis a modelos de tarefa

O Zed permite substituir a ação padrão para indicadores executáveis embutidos por meio do arquivo `tasks.json` local do espaço de trabalho e global, seguindo a seguinte hierarquia de precedência:

1. Arquivo `tasks.json` do espaço de trabalho
2. `tasks.json` global
3. Associações de tags fornecidas pela linguagem (padrão).

Para marcar uma tarefa, adicione o nome da tag executável ao campo `tags` no modelo da tarefa:

```json [tasks]
{
  "label": "echo current file's path",
  "command": "echo $ZED_FILE",
  "tags": ["rust-test"]
}
```

Ao fazer isso, você pode alterar qual tarefa é exibida no indicador de tarefas executáveis.

## Atribuições de teclas para executar tarefas vinculadas a objetos executáveis

Quando você tem uma definição de tarefa vinculada ao executável, é possível executá-la rapidamente usando as [Ações de Código](https://zed.dev/docs/configuring-languages?#code-actions), que podem ser acionadas tanto pelo comando {#action editor::ToggleCodeActions} quanto pelo atalho `cmd-.`/`ctrl-.`. Sua tarefa será a primeira na lista suspensa. A tarefa será executada imediatamente se não houver outras Ações de Código para essa linha.

## Executando scripts do Bash

É possível executar scripts do Bash diretamente no Zed. Ao abrir um arquivo `.sh` ou `.bash`, o Zed detecta automaticamente que o script pode ser executado e o disponibiliza no seletor de tarefas.

Para executar um script do Bash:

1. Abra a paleta de comandos com {#kb command_palette::Toggle}
2. Pesquise por “task” e selecione **task: spawn**
3. Selecione o script da lista

Os scripts Bash são marcados com a tag `bash-script`, o que permite filtrá-los ou referenciá-los nas configurações de tarefas.

Se você precisar passar argumentos ou personalizar o ambiente de execução, adicione uma configuração de tarefa no seu arquivo `.zed/tasks.json`:

```json
[
  {
    "label": "run my-script.sh with args",
    "command": "./my-script.sh",
    "args": ["--verbose", "--output=results.txt"],
    "tags": ["bash-script"]
  }
]
```

## Inicialização do shell

Quando o Zed executa uma tarefa, ele inicia o comando em um shell de login. Isso garante que os arquivos de inicialização do seu shell (`.bash_profile`, `.zshrc`, etc.) sejam carregados antes da execução da tarefa.

Esse comportamento permite que as tarefas tenham acesso às mesmas variáveis de ambiente, aliases e modificações no PATH que você configurou no seu perfil do shell. Se uma tarefa não conseguir encontrar um comando que funcione no seu terminal, verifique se os arquivos de configuração do seu shell estão configurados corretamente.

Para substituir o shell usado nas tarefas, configure a opção `terminal.shell`:

```json
{
  "terminal": {
    "shell": {
      "program": "/bin/zsh"
    }
  }
}
```

Consulte [Configuração do terminal](./terminal.md) para ver todas as opções do shell.
