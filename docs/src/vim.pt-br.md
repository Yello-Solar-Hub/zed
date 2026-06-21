---
título: Modo Vim - Zed
descrição: Emulação completa do Vim no Zed, com movimentos, objetos de texto, modo visual, macros e extensões específicas do Zed.
---

# Modo Vim

O Zed inclui uma camada de emulação do Vim. Esta página aborda como ativar e desativar o modo Vim, as combinações de teclas, os recursos específicos do Zed e as opções de configuração.

## O projeto do modo vim do Zed

O modo Vim reproduz o comportamento dos movimentos e comandos sempre que faz sentido e utiliza funcionalidades específicas do Zed nos casos em que a abordagem do Zed é mais adequada. O objetivo é proporcionar uma experiência familiar que funcione imediatamente, sem a necessidade de configuração.

Isso inclui suporte à navegação semântica, cursores múltiplos ou outros recursos normalmente oferecidos por plug-ins, como o texto circundante.

Portanto, o modo Vim do Zed não é uma réplica exata do Vim, mas combina o design modal do Vim com os recursos modernos do Zed para oferecer uma experiência mais fluida. Ele também é configurável, então você pode adicionar seus próprios atalhos de teclado ou substituir os padrões.

### Principais diferenças

Existem quatro tipos de recursos no modo vim que utilizam a funcionalidade principal do Zed, o que resulta em algumas diferenças de comportamento:

1. **Movimentos**: o modo vim utiliza a análise semântica do Zed para ajustar o comportamento dos movimentos de acordo com a linguagem. Por exemplo, em Rust, saltar para o colchete correspondente com `%` funciona com o caractere de barra vertical `|`. Em JavaScript, `w` considera `$` como um caractere de palavra.
2. **Seleções visuais de blocos**: o modo vim utiliza o cursor múltiplo do Zed para emular seleções visuais de blocos, tornando essas seleções muito mais flexíveis. Por exemplo, tudo o que você inserir após uma seleção de bloco é atualizado em todas as linhas em tempo real, e você pode adicionar ou remover cursores a qualquer momento.
3. **Macros**: o modo vim utiliza o sistema de gravação do Zed para macros do vim. Assim, é possível capturar e reproduzir ações mais complexas, como o autocompletamento.
4. **Pesquisar e substituir**: o modo vim utiliza o sistema de pesquisa do Zed; portanto, a sintaxe das expressões regulares é um pouco diferente da do Vim. [Acesse a seção “Diferenças nas expressões regulares”](#regex-differences) para obter mais detalhes.

> **Observação:** Os fundamentos do modo vim do Zed já devem atender a muitos casos de uso, e estamos sempre buscando aprimorá-lo. Se você perceber que faltam recursos dos quais depende em seu fluxo de trabalho, por favor, [abra uma issue no GitHub](https://github.com/zed-industries/zed/issues).

## Ativando e desativando o modo Vim

Ao abrir o Zed pela primeira vez, você verá uma caixa de seleção na tela de boas-vindas que permite ativar o modo vim.

Caso você não tenha percebido, é possível ativar ou desativar o modo Vim a qualquer momento, abrindo a paleta de comandos e usando o comando {#action workspace::ToggleVimMode} do workspace.

> **Observação**: Este comando alterna a seguinte propriedade nas suas configurações de usuário:
>
> ```json [configurações]```
> {
>   "vim_mode": true
> }
> ```

## Recursos específicos do Zed

O Zed foi desenvolvido com uma base moderna que (entre outras coisas) utiliza o Tree-sitter e servidores de linguagem para compreender o conteúdo do arquivo que você está editando e oferece suporte a múltiplos cursores por padrão.

O modo Vim possui várias combinações de teclas “essenciais do Zed” que ajudarão você a aproveitar ao máximo o conjunto de recursos específicos do Zed.

### Servidor de idiomas

Os comandos a seguir utilizam o servidor de linguagem para ajudá-lo a navegar e refatorar seu código.

| Comando                                  | Atalho padrão |
| ---------------------------------------- | ---------------- |
| Ir para a definição                         | `g d`            |
| Ir para a declaração                        | `g D`            |
| Ir para a definição do tipo                    | `g y`            |
| Ir para a implementação                     | `g I`            |
| Renomear (alterar definição)               | `c d`            |
| Ir para todas as referências à palavra atual | `g A`            |
| Encontrar símbolo no arquivo atual              | `g s`            |
| Localizar símbolo em todo o projeto            | `g S`            |
| Passe para o próximo diagnóstico                    | `g ]` ou `] d`   |
| Ir para o diagnóstico anterior                | `g [` ou `[ d`   |
| Mostrar erro no texto (ao passar o mouse)                | `g h`            |
| Abra o menu de ações de código               | `g .`            |

### Git

| Comando                         | Atalho padrão |
| ------------------------------- | ---------------- |
| Ir para a próxima alteração no Git           | `] c`            |
| Ir para a alteração anterior no Git       | `[ c`            |
| Expandir bloco de diferenças                | `d o`            |
| Alternar entre versões                   | `d O`            |
| Etapa e próxima (na visualização de diferenças)   | `d u`            |
| Desmarcar e avançar (na visualização de diferenças) | `d U`            |
| Restaurar alteração                  | `d p`            |

### Ativista que fica em cima de árvores

O Tree-sitter é o analisador que o Zed utiliza para compreender a estrutura do seu código. O Zed oferece movimentos que alteram a posição atual do cursor e objetos de texto que podem ser usados como alvo de ações.

| Comando                         | Atalho padrão            |
| ------------------------------- | --------------------------- |
| Ir para o método seguinte/anterior      | `] m` / `[ m`               |
| Ir para o método seguinte/anterior fim  | `] M` / `[ M`               |
| Ir para a próxima/anterior seção     | `] ]` / `[ [`               |
| Ir para a próxima/anterior seção ou para o final | `] [` / `[ ]`               |
| Ir para o próximo/anterior comentário     | `] /`, `] *` / `[ /`, `[ *` |
| Selecione um nó de sintaxe maior     | `[ x`                       |
| Selecione um nó de sintaxe menor    | `] x`                       |

| Objetos de texto                                               | Atalho padrão |
| ---------------------------------------------------------- | ---------------- |
| Em torno de uma classe, definição, etc.                           | `a c`            |
| Dentro de uma classe, definição etc.                           | `i c`            |
| Em torno de uma função, método etc.                             | `a f`            |
| Dentro de uma função, método, etc.                            | `i f`            |
| Um comentário                                                  | `g c`            |
| Um argumento, um item de lista, etc.                            | `i a`            |
| Um argumento, ou item de lista, etc. (incluindo a vírgula final) | `a a`            |
| Em torno de uma tag semelhante à do HTML                                    | `a t`            |
| Dentro de uma tag semelhante à do HTML                                    | `i t`            |
| O nível de recuo atual, além de uma linha antes e uma linha depois    | `a I`            |
| O nível de recuo atual e a linha anterior              | `a i`            |
| O nível de recuo atual                                   | `i i`            |

Observe que as definições para os alvos da família de movimentos `[m]` são as mesmas que as
limites definidos por `af`. Os destinos do `[[` são os mesmos que os definidos por `ac`, embora
Se não houver classes, também se utilizam funções. Da mesma forma, usa-se `gc` para encontrar `[ /`. `g c`

A definição de funções, classes e comentários depende da linguagem, e é possível adicionar suporte
para extensões, adicionando um [`textobjects.scm`]. A definição de argumentos e tags ocorre em
o nível “Tree-sitter”, mas procura determinados padrões na árvore de análise e, no momento, não é configurável
por idioma.

### Cursor múltiplo

Esses comandos ajudam você a gerenciar vários cursores no Zed.

| Comando                                                                           | Atalho padrão |
| --------------------------------------------------------------------------------- | ---------------- |
| Adicionar um cursor que selecione a próxima ocorrência da palavra atual                          | `g l`            |
| Adicionar um cursor que selecione a instância anterior da palavra atual                      | `g L`            |
| Adicionar um cursor no final de cada linha da seleção visual atual             | `g A`            |
| Adicionar um cursor no primeiro caractere de cada linha da seleção visual atual | `g I`            |
| Adicionar uma seleção visual para cada ocorrência da palavra atual                         | `g a`            |
| Ignorar a seleção da última palavra e adicionar a próxima                                          | `g >`            |
| Ignorar a seleção da última palavra e adicionar a anterior                                      | `g <`            |

### Gerenciamento de painéis

Esses comandos abrem novos painéis ou levam a painéis específicos.

| Comando                                    | Atalho padrão   |
| ------------------------------------------ | ------------------ |
| Iniciar uma pesquisa em todo o projeto                 | `g /`              |
| Abrir o trecho da pesquisa atual            | `g <espaço>`        |
| Abrir o trecho da pesquisa atual em uma janela dividida | `<ctrl-w> <espaço>` |
| Ir para a definição em uma divisão                | `<ctrl-w> g d`     |
| Ir para a definição do tipo em uma divisão           | `<ctrl-w> g D`     |

### No modo de inserção

Os comandos a seguir ajudam você a abrir o menu de autocompletar do Zed, solicitar uma sugestão do GitHub Copilot ou abrir o assistente de IA integrado sem sair do modo de inserção.

| Comando                                                                      | Atalho padrão |
| ---------------------------------------------------------------------------- | ---------------- |
| Abra o menu de sugestões                                                     | `Ctrl+X Ctrl+O`  |
| Solicitar sugestão do GitHub Copilot (requer que o GitHub Copilot esteja configurado) | `Ctrl+X Ctrl+C`  |
| Abrir o assistente de IA integrado (requer um assistente configurado)               | `Ctrl+X Ctrl+A`  |
| Abra o menu de ações de código                                                   | `Ctrl+X Ctrl+L`  |
| Oculta todas as sugestões                                                        | `Ctrl+X Ctrl+Z`  |

### Plug-ins compatíveis

O modo Vim do Zed inclui recursos normalmente oferecidos por plug-ins no ecossistema do Vim:

- Você pode selecionar objetos de texto com `ys` (yank surround), alterar a seleção com `cs` e excluir a seleção com `ds`.
- Você pode comentar e descomentar trechos com `gc` no modo visual e com `gcc` no modo normal.
- O painel do projeto oferece vários atalhos inspirados no plugin `netrw` do Vim: navegação com `hjkl`, abrir arquivo com `o`, abrir arquivo em uma nova aba com `t`, etc.
- Você pode adicionar atalhos de teclado ao seu mapa de teclas para navegar por nomes em “camelCase”. [Acesse a seção Atalhos de teclado opcionais](#optional-key-bindings) para saber como fazer isso.
- Você pode usar `gR` para fazer [ReplaceWithRegister](https://github.com/vim-scripts/ReplaceWithRegister).
- Você pode usar `cx` para acessar as funcionalidades do [vim-exchange](https://github.com/tommcdo/vim-exchange). Observe que ele não possui uma combinação de teclas padrão no modo visual, mas você pode adicionar uma ao seu mapa de teclas (consulte a seção [combinações de teclas opcionais](#optional-key-bindings)).
- Você pode navegar para níveis de recuo relativos à posição do cursor usando o plugin [indent wise](https://github.com/jeetsukumaran/vim-indentwise) com os comandos `[-`, `]-`, `[+`, `]+`, `[=`, `]=`.
- Você pode selecionar texto entre aspas com os objetos de texto AnyQuotes e texto entre colchetes com os objetos de texto AnyBrackets. O Zed também oferece os objetos MiniQuotes e MiniBrackets, que proporcionam um comportamento alternativo de seleção baseado no plugin [mini.ai](https://github.com/echasnovski/mini.nvim/blob/main/readmes/mini-ai.md) para o Neovim. Consulte a seção [Objetos de texto “Quote” e “Bracket”](#quote-and-bracket-text-objects) abaixo para obter mais detalhes.
- É possível configurar os objetos de texto AnyQuotes, AnyBrackets, MiniQuotes e MiniBrackets para selecionar texto entre aspas e entre colchetes usando diferentes estratégias de seleção. Consulte a seção [Funcionalidade Any Bracket](#any-bracket-functionality) abaixo para obter mais detalhes.

### Qualquer funcionalidade de chaves

O Zed oferece duas estratégias diferentes para selecionar texto entre aspas ou entre colchetes. Esses objetos de texto **não estão habilitados por padrão** e devem ser configurados no seu mapa de teclas para que possam ser utilizados.

#### Personagens incluídos

Cada tipo de objeto de texto funciona com caracteres específicos:

| Objeto de texto              | Personagens                                                                             |
| ------------------------ | -------------------------------------------------------------------------------------- |
| AnyQuotes/MiniQuotes     | Aspas simples (`'`), aspas duplas (`"`), backtick (`` ` ``)                             |
| AnyBrackets/MiniBrackets | Parênteses (`()`), colchetes (`[]`), chaves (`{}`), colchetes angulares (`<>`) |

Tanto a variante “Any” quanto a “Mini” utilizam os mesmos conjuntos de caracteres, mas diferem na estratégia de seleção.

#### AnyQuotes e AnyBrackets (comportamento tradicional do Vim)

Esses objetos de texto implementam o comportamento tradicional do Vim:

- **Prioridade de seleção**: localiza primeiro as aspas ou parênteses mais internos (mais próximos)
- **Mecanismo de fallback**: Se nenhum for encontrado, recorre à linha atual
- **Correspondência baseada em caracteres**: concentra-se exclusivamente nos caracteres de abertura e fechamento, sem levar em conta a sintaxe
- **Semelhança com o Vim padrão**: O AnyBrackets reproduz o comportamento de comandos como `ci<`, `ci(`, etc., no Vim padrão, incluindo possíveis casos extremos (como considerar `>` em `=>` como um delimitador de fechamento)

#### Mini-aspas e mini-colchetes (comportamento do mini.ai)

Esses objetos de texto implementam o comportamento do plugin [mini.ai](https://github.com/echasnovski/mini.nvim/blob/main/readmes/mini-ai.md) para o Neovim:

- **Prioridade de seleção**: Pesquisa primeiro na linha atual antes de expandir para as linhas adjacentes
- **Integração com o Tree-sitter**: Utiliza consultas do Tree-sitter para seleções mais sensíveis ao contexto
- **Correspondência sensível à sintaxe**: É capaz de distinguir entre parênteses reais e caracteres semelhantes em outros contextos (como `>` em `=>`)

#### Escolhendo entre abordagens

- Use **AnyQuotes/AnyBrackets** se você:

  - Preferir o comportamento tradicional do Vim
  - Desejo uma seleção consistente baseada em caracteres, priorizando os delimitadores mais internos
  - Preciso de um comportamento que se assemelhe bastante aos objetos de texto do Vim padrão

- Use **MiniQuotes/MiniBrackets** se você:
  - Prefiro o comportamento do plugin mini.ai
  - Quer seleções mais sensíveis ao contexto usando o Tree-sitter?
  - Priorizar a linha atual na pesquisa

#### Exemplo de configuração

Para usar esses objetos de texto, é preciso adicionar ligações ao seu mapa de teclas. Aqui está um exemplo de configuração que os torna disponíveis ao usar os operadores de objetos de texto (`i` e `a`) ou o comando “change-surrounds” (`cs`):

```json [keymap]
{
  "context": "vim_operator == a || vim_operator == i || vim_operator == cs",
  "bindings": {
    // Traditional Vim behavior
    "q": "vim::AnyQuotes",
    "b": "vim::AnyBrackets",

    // mini.ai plugin behavior
    "Q": "vim::MiniQuotes",
    "B": "vim::MiniBrackets"
  }
}
```

Com essa configuração, você pode usar comandos como:

- `cib` - Alterar o conteúdo entre colchetes usando o comportamento do AnyBrackets
- `ciB` - Alterar o conteúdo entre colchetes usando o comportamento do MiniBrackets
- `ciq` - Alterar o conteúdo entre aspas usando o comportamento do AnyQuotes
- `ciQ` - Alterar o conteúdo entre aspas usando o comportamento do MiniQuotes

## Paleta de comandos

O modo Vim permite que você abra a paleta de comandos do Zed com `:`. Em seguida, basta digitar para acessar qualquer comando comum do Zed. Além disso, o modo Vim adiciona aliases para comandos populares do Vim, garantindo que sua memória motora se adapte ao Zed. Por exemplo, você pode digitar `:w` ou `:write` para salvar o arquivo.

Abaixo, você encontrará tabelas que listam os comandos que podem ser usados na paleta de comandos. Colocamos os caracteres opcionais entre colchetes para indicar que eles podem ser omitidos.

> **Observação**: Ainda não emulamos toda a funcionalidade da linha de comando do Vim. Mais especificamente, os comandos atualmente não aceitam argumentos. Por favor, [relate problemas no GitHub](https://github.com/zed-industries/zed) sempre que identificar algo que esteja faltando na paleta de comandos.

### Gerenciamento de arquivos e janelas

Esta tabela mostra os comandos para gerenciar janelas, abas e painéis. Como os comandos não aceitam argumentos no momento, não é possível especificar um nome de arquivo ao salvar ou criar um novo arquivo.

| Comando         | Descrição                                          |
| --------------- | ---------------------------------------------------- |
| `:w[escrever][!]`   | Salvar o arquivo atual                                |
| `:wq[!]`        | Salve o arquivo e feche o buffer                   |
| `:q[sair][!]`    | Fechar o buffer                                     |
| `:wa[ll][!]`    | Salvar todos os arquivos abertos                                  |
| `:wqa[ll][!]`   | Salve todos os arquivos abertos e feche todos os buffers            |
| `:qa[ll][!]`    | Fechar todos os buffers                                    |
| `:[e]x[it][!]`  | Fechar o buffer                                     |
| `:up[data]`     | Salvar o arquivo atual                                |
| `:cq`           | Encerrar completamente (fechar todas as instâncias do Zed em execução) |
| `:bd[elete][!]` | Fechar o arquivo ativo em todos os painéis                   |
| `:vs[plit]`     | Dividir o painel verticalmente                            |
| `:sp[lit]`      | Dividir o painel horizontalmente                          |
| `:novo`          | Criar um novo arquivo em uma divisão horizontal              |
| `:vne[w]`       | Criar um novo arquivo em uma divisão vertical                |
| `:tabedit`      | Criar um novo arquivo em uma nova aba                       |
| `:tabnew`       | Criar um novo arquivo em uma nova aba                       |
| `:tabn[ext]`    | Vá para a próxima aba                                   |
| `:tabp[rev]`    | Ir para a aba anterior                                   |
| `:tabc[perder]`   | Fechar a aba atual                                |
| `:ls`           | Mostrar todos os buffers                                     |

> **Observação:** O caractere `!` é usado para forçar a execução do comando sem salvar as alterações ou solicitar confirmação antes de sobrescrever um arquivo.

### Comandos do Ex

Esses comandos do ex abrem os diversos painéis e janelas do Zed.

| Comando                      | Atalho padrão |
| ---------------------------- | ---------------- |
| Abra o painel do projeto       | `:E[explorar]`     |
| Abra o painel de colaboração | `:C[ollab]`      |
| Abra o painel de bate-papo          | `:Ch[at]`        |
| Abra o painel de IA            | `:A[I]`          |
| Abra o painel do Git           | `:G[it]`         |
| Abra o painel de depuração         | `:D[debug]`       |
| Abra o painel de notificações | `:No[tif]`       |
| Abra a janela de comentários     | `:fe[edback]`    |
| Abra a janela de diagnóstico  | `:cl[ist]`       |
| Abra o terminal            | `:te[rm]`        |
| Abra a janela de extensões   | `:Ext[ensões]`  |

### Navegando pelos diagnósticos

Esses comandos permitem navegar pelos diagnósticos.

| Comando                  | Descrição                    |
| ------------------------ | ------------------------------ |
| `:cn[ext]` ou `:ln[ext]` | Passe para o próximo diagnóstico      |
| `:cp[rev]` ou `:lp[rev]` | Vá para os diagnósticos anteriores |
| `:cc` ou `:ll`           | Abrir a página de erros           |

### Git

Esses comandos interagem com o sistema de controle de versão git.

| Comando         | Descrição                                             |
| --------------- | ------------------------------------------------------- |
| `:dif[fupdate]` | Visualizar as diferenças sob o cursor (`d o` no modo normal)   |
| `:rev[ert]`     | Reverter a alteração sob o cursor (`d p` no modo normal) |

### Saltar

Esses comandos levam a posições específicas no arquivo.

| Comando             | Descrição                         |
| ------------------- | ----------------------------------- |
| `:<número>`         | Ir para um número de linha               |
| `:$`                | Ir para o final do arquivo         |
| `:/foo` e `:?foo` | Ir para a próxima/anterior linha que contenha “foo” |

### Substituição

Este comando substitui texto. Ele emula o comando `substitute` do Vim. O comando `substitute` do Vim utiliza expressões regulares, e o Zed usa uma sintaxe ligeiramente diferente da do Vim. Você pode saber mais sobre a sintaxe do Zed abaixo, [na seção sobre diferenças nas expressões regulares](#regex-differences). O Zed substituirá apenas a primeira ocorrência do padrão de busca na linha atual. Para substituir todas as ocorrências, acrescente o sinalizador `g`.

| Comando                 | Descrição                       |
| ----------------------- | --------------------------------- |
| `:[intervalo]s/foo/bar/[g]` | Substitua todas as ocorrências de “foo” por “bar” |

### Edição

Esses comandos ajudam você a editar texto.

| Comando           | Descrição                                             |
| ----------------- | ------------------------------------------------------- |
| `:j[oin]`         | Junte-se à fila atual                                   |
| `:d[elete][l][p]` | Excluir a linha atual                                 |
| `:s[ort] [i]`     | Classificar a seleção atual (com i, sem distinção entre maiúsculas e minúsculas) |
| `:y[ank]`         | Copiar a seleção ou linha atual               |

### Conjunto

Esses comandos modificam as opções do editor localmente para o buffer atual.

| Comando                         | Descrição                                                                                   |
| ------------------------------- | --------------------------------------------------------------------------------------------- |
| `:se[t] [no]wrap`               | As linhas mais longas do que a largura da janela serão quebradas, e a exibição continuará na linha seguinte |
| `:se[t] [no]nú[mero]`           | Imprima o número da linha antes de cada linha                                                   |
| `:se[t] [no]r[elativo]nu[mero]` | Altera o número exibido para que seja relativo ao cursor                                     |
| `:se[t] [no]i[gnore]c[ase]`     | Determina se a pesquisa no buffer e no projeto utiliza correspondência que diferencia maiúsculas de minúsculas                    |

### Mnemônicos de comando

O Zed não vem com nenhum mnemônico de comando por padrão, mas você pode definir aliases curtos para os comandos do Zed usando a configuração `command_aliases` no seu arquivo de configurações. Quando você digita um alias dessa lista na paleta de comandos, ele é convertido no comando mapeado.

#### Exemplo de configuração

Para configurar mnemônicos de comando, adicione a chave `command_aliases` ao seu arquivo de configurações. Aqui está um exemplo de configuração com mnemônicos úteis:

```json [settings]
{
  "command_aliases": {
    "zlog": "zed::OpenLog",
    "newf": "workspace::NewFile",
    "diffs": "editor::ToggleSelectedDiffHunks",
    "crp": "workspace::CopyRelativePath",
    "cpp": "workspace::CopyPath",
    "reveal": "editor::RevealInFileManager",
    "clank": "editor::CancelLanguageServerWork"
  }
}
```

Com essa configuração, você pode usar comandos como:

- `:zlog` - Abre o log do Zed
- `:newf` - Criar um novo arquivo
- `:diffs` - Alternar entre os trechos de comparação selecionados
- `:crp` - Copia o caminho relativo para o arquivo atual
- `:cpp` - Copia o caminho completo do arquivo atual
- `:reveal` - Exibe o arquivo atual no gerenciador de arquivos
- `:clank` - Cancelar o trabalho do servidor de linguagem

## Personalização de atalhos de teclado

### Selecionando o contexto correto

As combinações de teclas do Zed são avaliadas apenas quando a propriedade `"context"` corresponde à sua localização no editor. Por exemplo, se você adicionar combinações de teclas ao contexto `"Editor"`, elas só funcionarão quando você estiver editando um arquivo. Se você adicionar combinações de teclas ao contexto `"Workspace"`, elas funcionarão em qualquer lugar no Zed. Aqui está um exemplo de uma combinação de teclas que salva o arquivo quando você está editando-o:

```json [keymap]
{
  "context": "Editor",
  "bindings": {
    "ctrl-s": "workspace::Save"
  }
}
```

Os contextos são aninhados; portanto, quando você está editando um arquivo, o contexto é o `"Editor"`, que está dentro do contexto `"Pane"`, que por sua vez está dentro do contexto `"Workspace"`. É por isso que quaisquer atalhos de teclado que você adicionar ao contexto `"Workspace"` funcionarão quando você estiver editando um arquivo. Veja um exemplo:

```json [keymap]
// This key binding will work when you're editing a file. It comes built into Zed by default as the workspace: save command.
{
  "context": "Workspace",
  "bindings": {
    "ctrl-s": "workspace::Save"
  }
}
```

Os contextos são expressões. Eles suportam operadores booleanos como `&&` (e) e `||` (ou). Por exemplo, você pode usar o contexto `"Editor && vim_mode == normal"` para criar atalhos de teclado que só funcionam quando você estiver editando um arquivo _e_ estiver no modo normal do Vim.

O modo Vim adiciona vários contextos ao contexto `"Editor"`:

| Operador             | Descrição                                                                                                                                                                        |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| VimControl           | Indica que os atalhos de teclado do Vim devem funcionar. Atualmente, um alias para `vim_mode == normal \|\| vim_mode == visual \|\| vim_mode == `operador`, mas a definição pode mudar com o tempo |
| vim_mode == normal   | Modo normal                                                                                                                                                                        |
| vim_mode == visual   | Modo visual                                                                                                                                                                        |
| vim_mode == inserção   | Modo de inserção                                                                                                                                                                        |
| vim_mode == replace  | Modo de substituição                                                                                                                                                                       |
| vim_mode == aguardando  | Aguardando uma tecla qualquer (por exemplo, após digitar `f` ou `t`)                                                                                                                       |
| vim_mode == operador | Aguardando o acionamento de outro comando (por exemplo, após digitar `c` ou `d`)                                                                                                             |
| vim_operator         | É definido como `none`, a menos que `vim_mode == operator`; nesse caso, é definido como a combinação de teclas padrão do operador atual (por exemplo, após digitar `d`, `vim_operator == d`)                    |

> **Observação**: Os contextos são comparados apenas em um nível por vez. Portanto, é possível usar a expressão `"Editor && vim_mode == normal"`, mas `"Workspace && vim_mode == normal"` nunca será correspondida, pois definimos o contexto vim no nível `"Editor"`.

### Contextos úteis para atalhos de teclado no modo Vim

Aqui está um modelo com contextos úteis do modo Vim para ajudá-lo a personalizar suas combinações de teclas no modo Vim. Você pode copiá-lo e integrá-lo ao seu mapa de teclas de usuário.

```json [keymap]
[
  {
    "context": "VimControl && !menu",
    "bindings": {
      // Put key bindings here if you want them to work in normal & visual mode.
    }
  },
  {
    "context": "vim_mode == normal && !menu",
    "bindings": {
      // "shift-y": ["workspace::SendKeystrokes", "y $"] // Use neovim's yank behavior: yank to end of line.
    }
  },
  {
    "context": "vim_mode == insert",
    "bindings": {
      // "j k": "vim::NormalBefore" // In insert mode, make jk escape to normal mode.
    }
  },
  {
    "context": "EmptyPane || SharedScreen",
    "bindings": {
      // Put key bindings here (in addition to the context above) if you want them to
      // work when no editor exists.
      // "space f": "file_finder::Toggle"
    }
  }
]
```

> **Observação**: Se você quiser emular os comandos `map` do Vim (`nmap`, etc.), pode usar a ação `workspace::SendKeystrokes` no contexto correto.

### Atribuições de teclas opcionais

Por padrão, é possível navegar entre os diferentes arquivos abertos no editor usando atalhos como `ctrl+w`, seguidos por uma das teclas `hjkl` para se deslocar para a esquerda, para baixo, para cima ou para a direita, respectivamente.

Mas não é possível usar os mesmos atalhos para alternar entre todas as janelas do editor (o terminal, o painel de projetos, o painel de agentes, etc.). Se você quiser usar os mesmos atalhos para navegar entre as janelas, pode adicionar as seguintes combinações de teclas ao seu mapa de teclas de usuário.

```json [keymap]
{
  "context": "Dock",
  "bindings": {
    "ctrl-w h": "workspace::ActivatePaneLeft",
    "ctrl-w l": "workspace::ActivatePaneRight",
    "ctrl-w k": "workspace::ActivatePaneUp",
    "ctrl-w j": "workspace::ActivatePaneDown"
    // ... or other keybindings
  }
}
```

O deslocamento por subpalavras, que permite navegar e selecionar palavras individuais em `camelCase` ou `snake_case`, não está ativado por padrão. Para ativá-lo, adicione estas combinações de teclas ao seu mapa de teclas.

```json [keymap]
{
  "context": "VimControl && !menu && vim_mode != operator",
  "bindings": {
    "w": "vim::NextSubwordStart",
    "b": "vim::PreviousSubwordStart",
    "e": "vim::NextSubwordEnd",
    "g e": "vim::PreviousSubwordEnd"
  }
}
```

> Observação: Operações como `dw` não são afetadas. Se você quiser que as operações
> use também o movimento por subpalavra e remova `vim_mode != operator` do `context`.

O modo Vim oferece atalhos para colocar a seleção entre colchetes no modo normal (`ys`), mas não possui um atalho para fazer isso no modo visual. Por padrão, `shift-s` substitui a seleção (apaga o texto e entra no modo de inserção). Para usar `shift-s` para colocar a seleção entre colchetes no modo visual, você pode adicionar o seguinte objeto ao seu mapa de teclas.

```json [keymap]
{
  "context": "vim_mode == visual",
  "bindings": {
    "shift-s": "vim::PushAddSurrounds"
  }
}
```

Em editores de texto não modais, a navegação do cursor normalmente continua além do fim da linha. O Zed, no entanto, lida com esse comportamento exatamente como o Vim por padrão: o cursor para nos limites da linha. Se você preferir que o cursor continue entre as linhas, substitua estas combinações de teclas:

```json [keymap]
// In VimScript, this would look like this:
// set whichwrap+=<,>,[,],h,l
{
  "context": "VimControl && !menu",
  "bindings": {
    "left": "vim::WrappingLeft",
    "right": "vim::WrappingRight",
    "h": "vim::WrappingLeft",
    "l": "vim::WrappingRight"
  }
}
```

O recurso [Sneak motion](https://github.com/justinmk/vim-sneak) permite navegar rapidamente para qualquer sequência de dois caracteres no seu texto. Você pode ativá-lo adicionando as seguintes combinações de teclas ao seu mapa de teclas. Por padrão, a tecla `s` está mapeada para `vim::Substitute`. Adicionar essas combinações substituirá esse comportamento; portanto, certifique-se de que essa alteração esteja de acordo com suas preferências de fluxo de trabalho.

```json [keymap]
{
  "context": "vim_mode == normal || vim_mode == visual",
  "bindings": {
    "s": "vim::PushSneak",
    "shift-s": "vim::PushSneakBackward"
  }
}
```

A ação “saltar para a palavra” no estilo Helix exibe marcadores de salto no início das palavras visíveis. Ela não possui uma combinação de teclas padrão no modo Vim, mas você pode ativá-la adicionando uma combinação de teclas ao seu mapa de teclas. Este exemplo usa `g w`, que corresponde à combinação padrão do Helix, mas substitui a combinação padrão de reajuste de linha do modo Vim.

```json [keymap]
{
  "context": "vim_mode == normal || vim_mode == visual",
  "bindings": {
    "g w": "vim::HelixJumpToWord"
  }
}
```

O recurso [vim-exchange](https://github.com/tommcdo/vim-exchange) não possui uma combinação de teclas padrão para o modo visual, pois a combinação `shift-x` entra em conflito com a combinação padrão `shift-x` do modo visual (`vim::VisualDeleteLine`). Para atribuir a combinação de teclas padrão do vim-exchange, adicione a seguinte combinação de teclas ao seu mapa de teclas:

```json [keymap]
{
  "context": "vim_mode == visual",
  "bindings": {
    "shift-x": "vim::Exchange"
  }
}
```

### Restaurando as configurações padrão de edição de texto e os atalhos de teclado do Zed

Se você estiver usando o modo vim no Linux ou no Windows, talvez perceba que ele substitui atalhos de teclado dos quais você não consegue abrir mão: `ctrl+v` para colar, `ctrl+f` para pesquisar, etc. Você pode restaurá-los copiando estes dados para o seu mapa de teclas:

```json [keymap]
{
  "context": "Editor && !menu",
  "bindings": {
    "ctrl-f": "buffer_search::Deploy",      // vim default: page down
    "ctrl-c": "editor::Copy",               // vim default: return to normal mode
    "ctrl-x": "editor::Cut",                // vim default: decrement
    "ctrl-v": "editor::Paste",              // vim default: visual block mode
    "ctrl-a": "editor::SelectAll",          // vim default: increment
    "ctrl-y": "editor::Undo",               // vim default: line up
    "ctrl-t": "project_symbols::Toggle",    // vim default: go to older tag
    "ctrl-o": "workspace::Open",            // vim default: go back
    "ctrl-s": "workspace::Save",            // vim default: show signature
    "ctrl-b": "workspace::ToggleLeftDock"   // vim default: down
  }
},
```

## Alterando as configurações do modo vim

Você pode alterar as seguintes configurações para modificar o comportamento do modo vim:

| Imóvel                     | Descrição                                                                                                                                                                                   | Valor padrão |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| modo_padrão                 | O modo padrão de inicialização. Uma das seguintes opções: “normal”, “insert”, “replace”, “visual”, “visual_line”, “visual_block” ou “helix_normal”.                                                                  | "normal"      |
| use_system_clipboard         | Determina como a área de transferência do sistema é utilizada:<br><ul><li>"always": usar em todas as operações</li><li>"never": usar apenas quando explicitamente especificado</li><li>"on_yank": usar em operações de yank</li></ul> | "sempre"      |
| use_multiline_find           | obsoleto                                                                                                                                                                                    |
| use_smartcase_find           | Se for `true`, os movimentos `f` e `t` não diferenciam maiúsculas de minúsculas quando a letra de destino estiver em minúscula.                                                                                                      | false         |
| usar a pesquisa com expressões regulares             | Se for `true`, a pesquisa do Vim utilizará o modo de expressões regulares                                                                                                                                                | verdadeiro          |
| gdefault                     | Se for `true`, o comando `:substitute` substitui todas as ocorrências em uma linha por padrão (como se a opção `g` tivesse sido especificada). A opção `g`, por sua vez, alterna esse comportamento, substituindo apenas a primeira ocorrência.                    | false         |
| toggle_relative_line_numbers | Se for `true`, os números de linha são relativos no modo normal e absolutos no modo de inserção, oferecendo o melhor das duas opções.                                                                         | false         |
| diagramas_personalizados              | Um objeto que permite adicionar dígrafos personalizados. Veja um exemplo a seguir.                                                                                                                  | {}            |
| highlight_on_yank_duration   | A duração da animação de destaque (em ms). Defina como `0` para desativar                                                                                                                         | 200           |

Aqui está um exemplo de como adicionar um digrafo para o emoji de zumbi. Isso permite que você digite `ctrl-k f z` para inserir um emoji de zumbi. Você pode adicionar quantos digrafos quiser.

```json [settings]
{
  "vim": {
    "custom_digraphs": {
      "fz": "🧟‍♀️"
    }
  }
}
```

Aqui está um exemplo dessas configurações alteradas:

```json [settings]
{
  "vim": {
    "default_mode": "insert",
    "use_system_clipboard": "never",
    "use_smartcase_find": true,
    "use_regex_search": true,
    "gdefault": true,
    "toggle_relative_line_numbers": true,
    "highlight_on_yank_duration": 50,
    "custom_digraphs": {
      "fz": "🧟‍♀️"
    }
  }
}
```

## Configurações úteis do Zed para o modo vim

Aqui estão algumas configurações gerais do Zed que podem ajudar você a ajustar sua experiência com o Vim:

| Imóvel                | Descrição                                                                                                                                                   | Valor padrão        |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| cursor_blink            | Se for `true`, o cursor pisca.                                                                                                                                 | `true`               |
| números_de_linha_relativos   | Se estiver definido como `"enabled"`, os números de linha na margem esquerda são relativos ao cursor. Se estiver definido como `"wrapped"`, eles também são exibidos nas linhas que foram quebradas.                              | `"desativado"`         |
| barra de rolagem               | Objeto que controla a exibição da barra de rolagem. Defina como `{ "show": "never" }` para ocultar a barra de rolagem.                                                              | `{ "show": "auto" }` |
| scroll_beyond_last_line | Se definido como `"one_page"`, permite rolar a tela até uma página além da última linha. Defina como `"off"` para impedir esse comportamento.                                        | `"one_page"`         |
| margem_de_rolagem_vertical  | O número de linhas a serem mantidas acima ou abaixo do cursor durante a rolagem. Defina como `0` para permitir que o cursor chegue até as bordas da tela na direção vertical.          | `3`                  |
| números_de_linha_da_margem     | Controla a exibição dos números de linha na margem interna. Defina a propriedade `"line_numbers"` como `false` para ocultar os números de linha.                                        | `true`               |
| aliases_de_comandos         | Objeto que define aliases para comandos na paleta de comandos. Você pode usá-lo para definir nomes de atalhos para comandos que usa com frequência. Veja exemplos a seguir. | `{}`                 |

Aqui está um exemplo dessas configurações alteradas:

```json [settings]
{
  // Disable cursor blink
  "cursor_blink": false,
  // Use relative line numbers
  "relative_line_numbers": "enabled",
  // Hide the scroll bar
  "scrollbar": { "show": "never" },
  // Prevent the buffer from scrolling beyond the last line
  "scroll_beyond_last_line": "off",
  // Allow the cursor to reach the edges of the screen
  "vertical_scroll_margin": 0,
  "gutter": {
    // Disable line numbers completely
    "line_numbers": false
  },
  "command_aliases": {
    "W": "w",
    "Wq": "wq",
    "Q": "q"
  }
}
```

A propriedade `command_aliases` é um único objeto que mapeia chaves ou sequências de chaves para comandos do modo Vim. O exemplo acima define vários aliases: `W` para `w`, `Wq` para `wq` e `Q` para `q`.

## Diferenças entre expressões regulares

O Zed utiliza um mecanismo de expressões regulares diferente do Vim. Isso significa que, em alguns casos, você terá que usar uma sintaxe diferente. Aqui estão as diferenças mais comuns:

- **Grupos de captura**: O Vim usa `\(` e `\)` para representar grupos de captura; no Zed, esses caracteres são `(` e `)`. Por outro lado, no Vim, `(` e `)` representam parênteses literais, mas no Zed eles devem ser escapados como `\(` e `\)`.
- **Correspondências**: Ao substituir, o Vim usa o caractere barra invertida seguido de um número para representar um grupo de captura correspondente. Por exemplo, `\1`. O Zed usa o sinal de dólar em vez disso. Portanto, quando no Vim você usa `\0` para representar a correspondência inteira, no Zed a sintaxe é `$0`. O mesmo vale para grupos de captura numerados: `\1` no Vim é `$1` no Zed.
- **Opção global**: Por padrão, no Vim, as buscas com expressões regulares encontram apenas a primeira ocorrência em uma linha, e é preciso acrescentar `/g` ao final da consulta para encontrar todas as ocorrências. No Zed, as buscas com expressões regulares são globais por padrão.
- **Distinção entre maiúsculas e minúsculas**: O Vim usa `/i` para indicar uma busca que não distingue entre maiúsculas e minúsculas. No Zed, você pode escrever `(?i)` no início do padrão ou alternar a distinção entre maiúsculas e minúsculas com o atalho {#kb search::ToggleCaseSensitive}.

> **Observação**: Para facilitar a transição, a paleta de comandos corrigirá os parênteses e substituirá os grupos automaticamente quando você digitar um comando de substituição no estilo Vim, `:%s//`. Assim, o Zed converterá `%s:/\(a\)(b)/\1/` em uma busca por “(a)\(b\)” e uma substituição por “$1”.

Para conhecer toda a sintaxe suportada pelo mecanismo de expressões regulares do Zed, [consulte a documentação do crate regex](https://docs.rs/regex/latest/regex/#syntax).
