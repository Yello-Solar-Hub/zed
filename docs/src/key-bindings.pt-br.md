---
título: Atribuições de teclas e atalhos - Zed
descrição: Personalize os atalhos de teclado do Zed. Reatribua ações, crie sequências de teclas e defina atalhos específicos para cada contexto.
---

# Atribuições de teclas

O sistema de atalhos do Zed é totalmente personalizável. Você pode reatribuir qualquer ação, criar sequências de teclas e definir atalhos específicos para cada contexto.

## Mapas de teclas predefinidos

Se você está acostumado com as configurações padrão de um editor específico, pode alterar seu `base_keymap` pela janela de configurações ({#kb zed::OpenSettings}) ou diretamente pelo arquivo `settings.json` ({#kb zed::OpenSettingsFile}).
Atualmente, oferecemos suporte a:

- VS Code (padrão)
- Átomo
- Emacs (Beta)
- JetBrains
- Sublime Text
- TextMate
- Cursor
- Nenhum (desativa _todas_ as combinações de teclas)

Essa configuração também pode ser alterada por meio da paleta de comandos, utilizando a ação {#action zed::ToggleBaseKeymapSelector}.

Você também pode ativar o `vim_mode` ou o `helix_mode`, que adicionam atalhos modais.
Para obter mais informações, consulte a documentação sobre o [modo Vim](./vim.md) e o [modo Helix](./helix.md).

## Editor de mapa de teclas

Você pode acessar o editor de mapa de teclas por meio da ação {#kb zed::OpenKeymap} ou executando a ação {#action zed::OpenKeymap} na paleta de comandos. É possível adicionar ou alterar facilmente uma combinação de teclas para uma ação usando os botões `Alterar combinação de teclas` ou `Adicionar combinação de teclas`, localizados no canto inferior esquerdo da paleta de comandos.

Nessa seção, você pode ver todas as ações existentes no Zed, bem como as combinações de teclas associadas a elas por padrão.

Você também pode personalizá-las diretamente de lá, seja clicando no ícone de lápis que aparece ao passar o mouse sobre uma ação específica, clicando duas vezes na linha da ação ou pressionando a tecla `enter`.

Tudo o que você acabar fazendo no editor de mapa de teclas também será refletido no arquivo `keymap.json`.

## Configurações de teclado do usuário

O arquivo de mapeamento de teclas está armazenado nos seguintes locais para cada plataforma:

- macOS/Linux: `~/.config/zed/keymap.json`
- Windows: `~\AppData\Roaming\Zed/keymap.json`

Você pode abrir o mapa de teclas com a ação {#action zed::OpenKeymapFile} da paleta de comandos.

Este arquivo contém uma matriz JSON de objetos com `"bindings"`.
Se nenhum `"context"` for definido, as ligações permanecerão sempre ativas.
Se estiver definido, a ligação só estará ativa quando o [contexto corresponder](#contexts).

Em cada seção de atalhos, uma [sequência de teclas](#keybinding-syntax) é associada a [uma ação](#actions).
Caso sejam detectados conflitos, eles são resolvidos conforme [descrito abaixo](#precedência).

Se você estiver usando um teclado que não seja QWERTY e que utilize caracteres latinos, talvez seja recomendável definir `use_key_equivalents` como `true`. Consulte [Teclados que não são QWERTY](#non-qwerty-keyboards) para obter mais informações.

Por exemplo:

```json [keymap]
[
  {
    "bindings": {
      "ctrl-right": "editor::SelectLargerSyntaxNode",
      "ctrl-left": "editor::SelectSmallerSyntaxNode"
    }
  },
  {
    "context": "ProjectPanel && not_editing",
    "bindings": {
      "o": "project_panel::Open"
    }
  }
]
```

Você pode ver todas as configurações padrão de teclas do Zed para cada plataforma nos arquivos de mapas de teclas padrão:

- [macOS](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-macos.json)
- [Windows](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-windows.json)
- [Linux](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-linux.json).

Se você quiser depurar problemas com mapeamentos de teclas personalizados, pode usar {#action dev::OpenKeyContextView} na paleta de comandos.
Por favor, abra [um ticket](https://github.com/zed-industries/zed) caso encontre algo que, na sua opinião, deveria funcionar, mas não está funcionando.

### Sintaxe de atalhos de teclado

O Zed tem a capacidade de reconhecer não apenas o pressionamento de uma única tecla, mas também uma sequência de teclas digitadas em ordem. Cada tecla no mapa `"bindings"` é uma sequência de pressionamentos de teclas separados por um espaço.

Cada pressionamento de tecla é uma sequência de modificadores seguida por uma tecla. Os modificadores são:

- `ctrl-` A tecla Control
- `cmd-`, `win-` ou `super-` para o modificador de plataforma (tecla Command no macOS, tecla Windows no Windows e tecla Super no Linux).
- `alt-` para alt (opção no macOS)
- `shift-` A tecla Shift
- `fn-` A tecla de função
- `secondary-` Equivalente a `cmd` quando o Zed está sendo executado no macOS e a `ctrl` quando está sendo executado no Windows e no Linux

As teclas podem ser qualquer ponto de código Unicode que seu teclado gere (por exemplo, `a`, `0`, `£` ou `ç`), ou qualquer tecla com nome (`tab`, `f1`, `shift` ou `cmd`). Se você estiver usando um layout não latino (por exemplo, cirílico), poderá associar a tecla ao caractere cirílico ou ao caractere latino que ela gera com a tecla `cmd` pressionada.

Alguns exemplos:

```json [keymap]
{
  "bindings": {
    "cmd-k cmd-s": "zed::OpenKeymap", // matches ⌘-k then ⌘-s
    "space e": "editor::ShowCompletions", // type space then e
    "ç": "editor::ShowCompletions", // matches ⌥-c
    "shift shift": "file_finder::Toggle" // matches pressing and releasing shift twice
  }
}
```

O modificador `shift-` só pode ser usado em combinação com uma letra para indicar a versão em maiúscula. Por exemplo, `shift-g` corresponde à digitação de `G`. Embora em muitos teclados a tecla Shift seja usada para digitar caracteres de pontuação como `(`, o pressionamento da tecla não é considerado modificado e, portanto, `shift-(` não corresponde.

O modificador `alt-` pode ser usado em vários layouts para gerar uma tecla diferente. Por exemplo, em um teclado do macOS dos EUA, a combinação `alt-c` digita `ç`. Você pode definir ambas as opções no seu arquivo de mapa de teclas; no entanto, por convenção, o Zed denomina essa combinação como `alt-c`.

É possível definir uma combinação de teclas com base apenas na pressão de uma tecla modificadora. Por exemplo, `shift shift` pode ser usado para implementar o atalho “Search Everywhere” da JetBrains. Nesse caso, a ação é executada ao soltar a tecla, e não ao pressioná-la.

### Contextos

Se um grupo de ligação tiver uma chave `"context"`, ele será comparado com os contextos atualmente ativos no Zed.

Os contextos do Zed formam uma árvore, cuja raiz é `Workspace`. Os espaços de trabalho contêm painéis (Panes) e painéis (Panels), e os painéis contêm editores (Editors), etc. A maneira mais fácil de verificar quais contextos estão ativos em um determinado momento é a visualização de contexto de teclas, que pode ser acessada com o comando {#action dev::OpenKeyContextView} na paleta de comandos.

Por exemplo:

```
# in an editor, it might look like this:
Workspace os=macos keyboard_layout=com.apple.keylayout.QWERTY
  Pane
    Editor mode=full extension=md vim_mode=insert

# in the project panel
Workspace os=macos
  Dock
    ProjectPanel not_editing
```

As expressões de contexto podem conter a seguinte sintaxe:

- `X && Y`, `X || Y` para e/ou duas condições
- `!X` para verificar se uma condição é falsa
- `(X)` para agrupamento
- `X > Y` será considerado válido se um ancestral na árvore corresponder a X e esta camada corresponder a Y.

Por exemplo:

- `"context": "Editor"` - corresponde a qualquer editor (incluindo campos de entrada embutidos)
- `"context": "Editor && mode == full"` - corresponde aos principais editores utilizados para editar código
- `"context": "!Editor && !Terminal"` — corresponde a qualquer lugar, exceto onde o foco estiver em um Editor ou Terminal
- `"context": "os == macos > Editor"` — corresponde a qualquer editor no macOS.

Vale ressaltar que os atributos só estão disponíveis no nó em que são definidos. Isso significa que, se você quiser (por exemplo) ativar uma combinação de teclas apenas quando o depurador estiver parado no modo normal do Vim, será necessário usar `debugger_stopped > vim_mode == normal`.

> Observação: Antes da versão Zed v0.197.x, o operador `!` considerava apenas um nó por vez, e `>` significava “pai”, e não “ancestral”. Isso significava que `!Editor` corresponderia ao contexto `Workspace > Pane > Editor`, pois (o que causava confusão) o `Pane` correspondia a `!Editor`, e que `os == macos > Editor` não correspondia ao contexto `Workspace > Pane > Editor` devido ao nó intermediário `Pane`.

Se você estiver usando o modo Vim, temos informações sobre como [os modos do Vim influenciam o contexto](./vim.md#contexts). O modo Helix é baseado no modo Vim e utiliza os mesmos contextos.

### Ações

Quase todas as funcionalidades do Zed são disponibilizadas como ações.
Embora não haja uma lista explicitamente documentada, é possível encontrar a maioria deles pesquisando na paleta de comandos, consultando os mapas de teclas padrão do [macOS](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-macos.json), [Windows](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-windows.json) ou [Linux](https://github.com/zed-industries/zed/blob/main/assets/keymaps/default-linux.json), ou usando o autocompletar do Zed no seu arquivo de mapa de teclas.

A maioria das ações não requer argumentos e, portanto, você pode defini-las como strings: `"ctrl-a": "language_selector::Toggle"`. Algumas exigem um único argumento e devem ser definidas como um array: `"cmd-1": ["workspace::ActivatePane", 0]`. Algumas ações exigem vários argumentos e são associadas como um array composto por uma string e um objeto: `"ctrl-a": ["pane::DeploySearch", { "replace_enabled": true }]`.

### Precedência

Quando várias combinações de teclas têm a mesma combinação e estão ativas ao mesmo tempo, a prioridade é determinada de duas maneiras:

- As ligações que correspondem aos nós inferiores na árvore de contexto têm prioridade. Isso significa que, se você tiver uma ligação com o contexto `Editor`, ela terá prioridade sobre uma ligação com o contexto `Workspace`. As ligações sem contexto correspondem ao nível mais baixo da árvore.
- Se houver várias combinações de teclas que correspondam no mesmo nível da árvore, a combinação definida posteriormente terá precedência. Como as combinações de teclas do usuário são carregadas após as combinações de teclas do sistema, isso permite que as combinações do usuário tenham precedência sobre as combinações de teclas integradas.

O outro tipo de conflito que surge é quando há duas combinações de teclas, sendo que uma delas é um prefixo da outra. Por exemplo, se você tiver `"ctrl-w":"editor::DeleteToNextWordEnd"` e `"ctrl-w left":"editor::DeleteToEndOfLine"`.

Quando isso acontece, e ambas as combinações de teclas estiverem ativas no contexto atual, o Zed aguardará 1 segundo após você digitar `ctrl-w` para verificar se você está prestes a digitar `left`. Se você não digitar nada, ou se digitar uma tecla diferente, a função `DeleteToNextWordEnd` será acionada. Caso contrário, a função `DeleteToEndOfLine` será acionada.

### Teclados que não seguem o padrão QWERTY

O suporte do Zed a teclados que não sejam QWERTY ainda está em desenvolvimento.

Se o seu teclado for capaz de digitar todo o conjunto de caracteres ASCII (DVORAK, COLEMAK, etc.), os atalhos devem funcionar como você espera.

Caso contrário, continue lendo...

#### macOS

Em teclados cirílicos, hebraicos, armênios e outros que são, em sua maioria, não-ASCII, o macOS mapeia automaticamente as teclas para o intervalo ASCII quando a tecla `cmd` é mantida pressionada. O Zed vai um passo além e pode sempre comparar os pressionamentos de teclas com o layout ASCII ou com o layout real, independentemente dos modificadores e da configuração `use_key_equivalents`. Por exemplo, em tailandês, pressionar `ctrl-ๆ` corresponderá aos atalhos associados a `ctrl-q` ou `ctrl-ๆ`.

Em teclados que suportam alfabetos latinos estendidos (AZERTY francês, QWERTZ alemão, etc.), muitas vezes não é possível digitar todo o conjunto de caracteres ASCII sem a tecla `option`. Isso gera uma ambiguidade: `option-2` produz `@`. Para garantir que todos os atalhos de teclado integrados ainda possam ser digitados nesses teclados, reorganizamos as combinações de teclas. Por exemplo, os atalhos associados à tecla `@` no QWERTY são movidos para a tecla `"` no layout espanhol. Esse mapeamento se baseia nas configurações padrão do sistema macOS e pode ser visualizado executando {#action dev::OpenKeyContextView} na paleta de comandos.

Se você estiver definindo atalhos no seu mapa de teclas pessoal, poderá ativar o mapeamento de teclas equivalentes definindo `use_key_equivalents` como `true` no seu mapa de teclas:

```json [keymap]
[
  {
    "use_key_equivalents": true,
    "bindings": {
      "ctrl->": "editor::Indent" // parsed as ctrl-: when a German QWERTZ keyboard is active
    }
  }
]
```

### Linux

Desde a versão v0.196.0, no Linux, se a tecla digitada não produzir um caractere ASCII, usamos a tecla equivalente no layout QWERTY para os atalhos de teclado. Isso significa que muitos atalhos podem ser digitados em diversos layouts.

Ainda não remapeamos atalhos, portanto, todos os atalhos integrados podem ser digitados em qualquer layout. Se o seu layout não permitir a digitação de alguns caracteres ASCII, talvez você precise de combinações de teclas personalizadas. Pretendemos melhorar isso.

## Dicas e truques

### Desativando uma ligação

Se você quiser que uma determinada ligação não execute nenhuma ação em um determinado contexto, pode usar
`null` como ação. Isso é útil caso você pressione a combinação de teclas por acidente e
quer desativá-lo ou se quiser digitar o caractere que seria digitado por
a sequência, ou se você quiser desativar as combinações de teclas múltiplas que começam com essa tecla.

```json [keymap]
[
  {
    "context": "Workspace",
    "bindings": {
      "cmd-r": null // cmd-r will do nothing when the Workspace context is active
    }
  }
]
```

Uma ligação `null` segue as mesmas regras de precedência que as ações normais; portanto, ela também desativa todas as ligações que corresponderiam em níveis superiores da árvore. Se você quiser que uma ligação que corresponda em um nível superior da árvore tenha precedência sobre uma ligação em nível inferior, será necessário reatribuir essa ligação à ação desejada, no contexto desejado.

Isso é útil para evitar que o Zed recorra a uma combinação de teclas padrão quando a ação especificada for condicional e se propagar. Por exemplo, `buffer_search::DeployReplace` só é acionada quando a barra de pesquisa não está visível. Se a barra de pesquisa estiver visível, a ação se propagaria e acionaria a ação padrão definida para essa combinação de teclas, como abrir o dock direito. Para evitar que isso aconteça:

```json [keymap]
[
  {
    "context": "Workspace",
    "bindings": {
      "cmd-r": null // cmd-r will do nothing when the search bar is in view
    }
  },
  {
    "context": "Workspace",
    "bindings": {
      "cmd-r": "buffer_search::DeployReplace" // cmd-r will deploy replace when the search bar is not in view
    }
  }
]
```

### Remapeamento de teclas

Um pedido comum é poder associar um único toque de tecla a uma sequência. É possível fazer isso com a ação `workspace::SendKeystrokes`.

```json [keymap]
[
  {
    "bindings": {
      // Move down four times
      "alt-down": ["workspace::SendKeystrokes", "down down down down"],
      // Expand the selection (editor::SelectLargerSyntaxNode);
      // copy to the clipboard; and then undo the selection expansion.
      "cmd-alt-c": [
        "workspace::SendKeystrokes",
        "ctrl-shift-right ctrl-shift-right ctrl-shift-right cmd-c ctrl-shift-left ctrl-shift-left ctrl-shift-left"
      ]
    }
  },
  {
    "context": "Editor && vim_mode == insert",
    "bindings": {
      "j k": ["workspace::SendKeystrokes", "escape"]
    }
  }
]
```

Existem algumas limitações a isso, a saber:

- Nenhuma operação assíncrona será executada até que todas as suas combinações de teclas tenham sido processadas. Por exemplo, isso significa que, embora você possa usar uma combinação de teclas para abrir um arquivo (como no exemplo `cmd-alt-r`), não é possível enviar novas teclas e esperar que elas sejam interpretadas pela nova visualização.
- Outros exemplos de operações assíncronas são: abrir a paleta de comandos, comunicar-se com um servidor de linguagem, alterar o idioma de um buffer e qualquer ação que envolva a rede.
- Há um limite de 100 chaves simuladas por vez.

O argumento passado para `SendKeystrokes` é uma lista de pressionamentos de teclas separados por espaços (usando a mesma sintaxe apresentada acima). Devido à forma como os pressionamentos de teclas são analisados, qualquer segmento que não seja reconhecido como um pressionamento de tecla será enviado literalmente para o campo de entrada que estiver em foco no momento.

Se o argumento passado para `SendKeystrokes` contiver a combinação de teclas usada para acioná-lo, será utilizada a definição com a segunda maior precedência dessa combinação. Isso permite que você amplie o comportamento padrão de uma combinação de teclas.

### Encaminhar teclas para o terminal

Se você estiver usando Linux ou Windows, talvez queira redirecionar combinações de teclas para o terminal integrado, em vez de deixá-las serem processadas pelo Zed.

Por exemplo, `ctrl-n` abre uma nova aba no Zed no Linux. Se você quiser enviar `ctrl-n` para o terminal integrado quando ele estiver em foco, adicione o seguinte ao seu mapa de teclas:

```json [keymap]
{
  "context": "Terminal",
  "bindings": {
    "ctrl-n": ["terminal::SendKeystroke", "ctrl-n"]
  }
}
```

### Atribuições de teclas para tarefas

Você também pode atribuir teclas para executar tarefas do Zed definidas no seu arquivo `tasks.json`.
Consulte a [documentação sobre tarefas](tasks.md#custom-keybindings-for-tasks) para saber mais.
