---
título: Terminal integrado - Zed
descrição: O terminal integrado do Zed, com múltiplas instâncias, shells personalizados e integração avançada com editores.
---

# Terminal

O Zed inclui um emulador de terminal integrado que oferece suporte a várias instâncias de terminal, shells personalizados e integração profunda com o editor.

## Terminais de abertura

| Ação                  | macOS           | Linux/Windows   |
| ----------------------- | --------------- | --------------- |
| Alternar painel de terminais   | `` Ctrl+` ``    | `` Ctrl+` ``    |
| Abrir um novo terminal       | `Ctrl+~`        | `Ctrl+~`        |
| Abrir o terminal no centro | Paleta de comandos | Paleta de comandos |

Você também pode abrir um terminal a partir da paleta de comandos com {#action terminal_panel::Toggle} ou {#action workspace::NewTerminal}.

### Painel de terminais x Terminal central

Os terminais podem ser abertos em dois locais:

- **Painel do Terminal** — Ancorado na parte inferior (padrão), à esquerda ou à direita da área de trabalho. Alterne com `` Ctrl+` ``.
- **Painel central** — Abre como uma aba normal ao lado dos seus arquivos. Use {#action workspace::NewCenterTerminal} na paleta de comandos.

## Trabalhando com vários terminais

Crie terminais adicionais com `Cmd+N` (macOS) ou `Ctrl+N` (Linux/Windows) enquanto estiver com o foco no painel de terminais. Cada terminal aparece como uma aba no painel.

Divida os terminais horizontalmente com `Cmd+D` (macOS) ou `Ctrl+Shift+5` (Linux/Windows).

## Configurando o Shell

Por padrão, o Zed usa o shell padrão do seu sistema (conforme definido em `/etc/passwd` em sistemas Unix). Para usar um shell diferente:

```json [settings]
{
  "terminal": {
    "shell": {
      "program": "/bin/zsh"
    }
  }
}
```

Para passar argumentos para o seu shell:

```json [settings]
{
  "terminal": {
    "shell": {
      "with_arguments": {
        "program": "/bin/bash",
        "args": ["--login"]
      }
    }
  }
}
```

## Diretório de trabalho

Controle onde os novos terminais são iniciados:

| Valor                                         | Comportamento                                                                                                          |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `"current_file_directory"`                    | Utiliza o diretório do arquivo atual; caso não seja encontrado, recorre ao diretório do projeto e, em seguida, ao primeiro projeto na área de trabalho |
| `"diretório_do_projeto_atual"`                 | Utiliza o diretório do projeto do arquivo atual (padrão)                                                               |
| `"primeiro_diretório_do_projeto"`                   | Utiliza o primeiro projeto do seu espaço de trabalho                                                                          |
| `"always_home"`                               | Sempre começa no seu diretório pessoal                                                                              |
| `{ "always": { "directory": "~/projects" } }` | Sempre começa em um diretório específico                                                                             |

```json [settings]
{
  "terminal": {
    "working_directory": "first_project_directory"
  }
}
```

## Variáveis de ambiente

Adicionar variáveis de ambiente a todas as sessões do terminal:

```json [settings]
{
  "terminal": {
    "env": {
      "EDITOR": "zed --wait",
      "MY_VAR": "value"
    }
  }
}
```

> **Dica:** Use `:` para separar vários valores em uma única variável: `"PATH": "/custom/path:$PATH"`

### Detecção de ambientes virtuais do Python

O Zed pode ativar automaticamente ambientes virtuais do Python ao abrir um terminal. Por padrão, ele procura pelos diretórios `.env`, `env`, `.venv` e `venv`:

```json [settings]
{
  "terminal": {
    "detect_venv": {
      "on": {
        "directories": [".venv", "venv"],
        "activate_script": "default"
      }
    }
  }
}
```

A opção `activate_script` aceita os valores `"default"`, `"csh"`, `"fish"` e `"nushell"`.

Para desativar a detecção de ambiente virtual:

```json [settings]
{
  "terminal": {
    "detect_venv": "off"
  }
}
```

## Fontes e aparência

O terminal pode usar fontes diferentes das do editor:

```json [settings]
{
  "terminal": {
    "font_family": "JetBrains Mono",
    "font_size": 14,
    "font_features": {
      "calt": false
    },
    "line_height": "comfortable"
  }
}
```

Opções de altura de linha:

- `"confortável"` — proporção de 1,618, ideal para leitura (padrão)
- `"padrão"` — proporção de 1,3, mais adequada para aplicações TUI com caracteres que desenham caixas
- `{ "custom": 1.5 }` — Proporção personalizada

### Cursor

Configurar a aparência do cursor:

```json [settings]
{
  "terminal": {
    "cursor_shape": "bar",
    "blinking": "on"
  }
}
```

Formatos do cursor: `"bloco"`, `"barra"`, `"sublinhado"`, `"oco"`

Opções de piscar: `"off"`, `"terminal_controlled"` (padrão), `"on"`

### Contraste mínimo

O Zed ajusta as cores do terminal para manter a legibilidade. O valor padrão `45` garante que o texto permaneça visível. Defina como `0` para desativar o ajuste de contraste e usar as cores exatas do tema:

```json [settings]
{
  "terminal": {
    "minimum_contrast": 0
  }
}
```

## Rolagem

Navegue pelo histórico do terminal com estas combinações de teclas:

| Ação           | macOS                          | Linux/Windows    |
| ---------------- | ------------------------------ | ---------------- |
| Rolar a página para cima   | `Shift+PageUp` ou `Cmd+Up`     | `Shift+PageUp`   |
| Role a página para baixo | `Shift+PageDown` ou `Cmd+Down` | `Shift+PageDown` |
| Rolar a página para cima   | `Shift+Seta para cima`                     | `Shift+Seta para cima`       |
| Rolar a linha para baixo | `Shift+Seta para baixo`                   | `Shift+Seta para baixo`     |
| Voltar ao topo    | `Shift+Home` ou `Cmd+Home`     | `Shift+Home`     |
| Role até o final da página | `Shift+End` ou `Cmd+End`       | `Shift+End`      |

Ajuste a velocidade de rolagem com:

```json [settings]
{
  "terminal": {
    "scroll_multiplier": 3.0
  }
}
```

## Copiar e colar

| Ação | macOS   | Linux/Windows  |
| ------ | ------- | -------------- |
| Copiar   | `Cmd+C` | `Ctrl+Shift+C` |
| Colar  | `Cmd+V` | `Ctrl+Shift+V` |

### Copiar ao selecionar

Copiar automaticamente o texto selecionado para a área de transferência:

```json [settings]
{
  "terminal": {
    "copy_on_select": true
  }
}
```

### Manter a seleção após a cópia

Por padrão, o texto permanece selecionado após ser copiado. Para desmarcar a seleção:

```json [settings]
{
  "terminal": {
    "keep_selection_on_copy": false
  }
}
```

## Pesquisar

Pesquise o conteúdo do terminal com `Cmd+F` (macOS) ou `Ctrl+Shift+F` (Linux/Windows). Isso abre a mesma barra de pesquisa usada no editor.

## Modo Vi

Ative ou desative a navegação no estilo vi no terminal com `Ctrl+Shift+Espaço`. Isso permite que você navegue e selecione texto usando os atalhos de teclado do vi.

## Limpar terminal

Limpar a tela do terminal:

- macOS: `Cmd+K`
- Linux/Windows: `Ctrl+Shift+L`

## Option como Meta (macOS)

Para usuários do Emacs ou aplicativos que utilizam combinações de teclas Meta, habilite a opção “Option como Meta”:

```json [settings]
{
  "terminal": {
    "option_as_meta": true
  }
}
```

Isso reinterpreta a tecla Option como Meta, permitindo que sequências como `Alt+X` funcionem corretamente.

## Modo de rolagem alternativo

Quando ativada, os eventos de rolagem do mouse são convertidos em pressionamentos das setas do teclado em aplicativos como `vim` ou `less`:

```json [settings]
{
  "terminal": {
    "alternate_scroll": "on"
  }
}
```

## Hiperlinks de caminho

O Zed detecta caminhos de arquivos na saída do terminal e os torna clicáveis. `Cmd+Clique` (macOS) ou `Ctrl+Clique` (Linux/Windows) abre o arquivo no Zed, indo diretamente para o número da linha, caso seja detectado.

Formatos comuns reconhecidos:

- `src/main.rs:42` — Abre na linha 42
- `src/main.rs:42:10` — Abre na linha 42, coluna 10
- `Arquivo “script.py”, linha 10` — Tracebacks do Python

## Configuração do painel

### Posição na doca

```json [settings]
{
  "terminal": {
    "dock": "bottom"
  }
}
```

Opções: `"bottom"` (padrão), `"left"`, `"right"`

### Tamanho padrão

```json [settings]
{
  "terminal": {
    "default_width": 640,
    "default_height": 320
  }
}
```

### Botão do terminal

Ocultar o botão do terminal na barra de status:

```json [settings]
{
  "terminal": {
    "button": false
  }
}
```

### Barra de ferramentas

Mostrar o título do terminal em uma barra de navegação:

```json [settings]
{
  "terminal": {
    "toolbar": {
      "breadcrumbs": true
    }
  }
}
```

O título pode ser definido pelo seu shell usando a sequência de escape `\e]2;Title\007`.

## Integração com tarefas

O terminal se integra ao [sistema de tarefas](./tasks.md) do Zed. Quando você executa uma tarefa, ela é executada no terminal. Para reexecutar a última tarefa a partir do terminal, use o seguinte comando:

- macOS: `Cmd+Alt+R`
- Linux/Windows: `Ctrl+Shift+R` ou `Alt+T`

## Assistência por IA

Obtenha ajuda com os comandos do terminal usando o [Assistente Inline](./ai/inline-assistant.md):

- macOS: `Ctrl+Enter`
- Linux/Windows: `Ctrl+Enter` ou `Ctrl+I`

Isso abre o Assistente Inline para ajudar a explicar erros, sugerir comandos ou solucionar problemas. Os agentes de IA no [Painel do Agente](./ai/agent-panel.md) também podem executar comandos de terminal como parte de seu fluxo de trabalho.

## Envio de texto e pressionamentos de teclas

Para uma personalização avançada das combinações de teclas, você pode enviar texto bruto ou sequências de teclas para o terminal:

```json [keymap]
{
  "context": "Terminal",
  "bindings": {
    "alt-left": ["terminal::SendText", "\u001bb"],
    "ctrl-c": ["terminal::SendKeystroke", "ctrl-c"]
  }
}
```

## Todas as configurações do terminal

Para obter a lista completa das configurações do terminal, consulte a [seção “Terminal” em “Todas as configurações”](./reference/all-settings.md#terminal).

## O que vem a seguir

- [Tarefas](./tasks.md) — Executar comandos e scripts a partir do Zed
- [REPL](./repl.md) — Execução interativa de código
- [Referência da CLI](./reference/cli.md) — Interface de linha de comando para abrir arquivos no Zed
