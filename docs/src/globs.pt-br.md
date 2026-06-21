---
título: Padrões Glob - Zed
descrição: Como funcionam os padrões glob no Zed para correspondência de arquivos, filtragem de pesquisa e configuração. Referência de sintaxe e exemplos.
---

# Globs

O Zed suporta o uso de padrões [glob](<https://en.wikipedia.org/wiki/Glob_(programming)>), que são o nome formal para os caracteres curinga de correspondência de caminhos no estilo do shell do Unix, como `*.md` ou `docs/src/**/*.md`, suportados pelo sh, bash, zsh, etc. Um glob é semelhante, mas distinto de uma [regex (expressão regular)](https://en.wikipedia.org/wiki/Regular_expression). No Zed, os globs são comumente usados na correspondência de nomes de arquivos.

## Glob Flavor

Zed usa duas caixas enferrujadas diferentes para combinar padrões de globos:

- [ignore crate](https://docs.rs/ignore/latest/ignore/) para padrões glob correspondentes armazenados nos arquivos `.gitignore`
- [crate glob](https://docs.rs/glob/latest/glob/) para comparar caminhos de arquivos no Zed

Embora expressões simples sejam portáveis entre ambientes (por exemplo, executar `ls *.py` ou `*.tmp` em um `.gitignore`), há divergências significativas no suporte e na sintaxe de recursos mais avançados (classes de caracteres, exclusões, `**`, etc.) entre as diferentes implementações. No restante deste documento, descreveremos os globs conforme suportados no Zed por meio da implementação do crate `glob`. Consulte [Referências](#references) abaixo para obter links de documentação sobre a sintaxe de padrões glob para `.gitignore`, shells e outras linguagens de programação.

O crate `glob` é implementado inteiramente em Rust e não depende das interfaces `glob` / `fnmatch` fornecidas pela libc da sua plataforma. Isso significa que os globs no Zed devem se comportar de maneira semelhante em todas as plataformas.

## Introdução

Um “padrão” glob é usado para corresponder a um nome de arquivo ou a um caminho completo de arquivo. Por exemplo, ao usar “Pesquisar todos os arquivos” {#kb project_search::ToggleFocus}, você pode clicar no botão “Alternar filtros” em forma de funil ou {#kb project_search::ToggleFilters}, e serão exibidos campos de pesquisa adicionais para “Incluir” e “Excluir”, que permitem especificar padrões glob para corresponder a caminhos e nomes de arquivos.

### Vários padrões

É possível especificar vários padrões globais nos filtros do Project Search, separando-os por vírgulas. Ao usar padrões separados por vírgulas, o Zed lida corretamente com chaves dentro de cada padrão:

- `*.ts, *.tsx` — Identifica arquivos TypeScript e TSX
- `src/{components,utils}/**/*.ts, tests/**/*.test.ts` — Identifica arquivos TypeScript em diretórios específicos, além de arquivos de teste

Cada padrão é avaliado de forma independente. As vírgulas dentro das chaves (como `{a,b}`) são tratadas como parte do padrão, e não como separadores.

**Importante:** Embora as chaves sejam preservadas nos padrões, o Zed não as expande em vários padrões. O padrão `src/{a,b}/*.ts` corresponde à estrutura literal do caminho, e não a `src/a/*.ts` OU `src/b/*.ts`. Isso difere do comportamento do shell.

Ao criar um padrão glob, você pode usar um ou vários caracteres especiais:

| Caractere especial | Significado                                                           |
| ----------------- | ----------------------------------------------------------------- |
| `?`               | Corresponde a qualquer caractere único                                      |
| `*`               | Corresponde a qualquer sequência (possivelmente vazia) de caracteres               |
| `**`              | Corresponde ao diretório atual e a subdiretórios arbitrários        |
| `[abc]`           | Corresponde a qualquer um dos caracteres entre colchetes                         |
| `[a-z]`           | Corresponde a qualquer um dos caracteres de um determinado intervalo (ordenados por Unicode)         |
| `[!...]`          | A negação de `[...]` (corresponde a um caractere que não esteja entre colchetes) |

Notas:

1. Os caracteres de chaves `{` e `}` são caracteres literais do padrão, e não operadores de expansão. O padrão `src/{a,b}/*.ts` corresponde a caminhos que contenham o texto literal `{a,b}`, e não a caminhos que correspondam a `src/a/*.ts` ou `src/b/*.ts`, como ocorre na expansão de padrões do shell.
2. Para corresponder a um caractere literal `-` entre colchetes, ele deve vir primeiro `[-abc]` ou por último `[abc-]`.
3. Para corresponder ao caractere literal `[`, use `[[]` ou coloque-o como o primeiro caractere do grupo `[[abc]`.
4. Para corresponder ao caractere literal `]`, use `[]]` ou coloque-o como o último caractere do grupo `[abc]]`.

## Exemplos

### Extensões de arquivo correspondentes

Se você quiser pesquisar apenas arquivos Markdown, adicione `*.md` ao campo de pesquisa “Incluir”.

### Correspondência sem distinção entre maiúsculas e minúsculas

No Zed, os padrões de busca (globs) diferenciam maiúsculas de minúsculas; portanto, `*.c` não corresponderá a `main.C` (mesmo em sistemas de arquivos que não diferenciam maiúsculas de minúsculas, como o HFS+/APFS no macOS). Em vez disso, use colchetes para corresponder a caracteres. Assim, em vez de `*.c`, use `*.[cC]`.

### Diretórios correspondentes

Se você quiser pesquisar no [repositório do Zed](https://github.com/zed-industries/zed) por exemplos de [Configuração de servidores de linguagem](https://zed.dev/docs/configuring-languages# configuring-language-servers) (em `"lsp"` no arquivo settings.json do Zed), você poderia pesquisar por `"lsp"` e, no filtro “Incluir”, especificar `docs/**/*.md`. Isso encontraria apenas arquivos cujo caminho estivesse no diretório `docs` ou em qualquer subdiretório aninhado `**/` dessa pasta, com um nome de arquivo que terminasse em `.md`.

Se, em vez disso, você quisesse se limitar apenas às páginas da [Documentação específica da linguagem Zed](https://zed.dev/docs/languages), poderia definir um padrão mais restrito: `docs/src/languages/*.md`. Isso corresponderia a [`docs/src/languages/rust.md`] (https://github.com/zed-industries/zed/blob/main/docs/src/languages/rust.md) e [`docs/src/languages/cpp.md`] (https://github.com/zed-industries/zed/blob/main/docs/src/languages/cpp.md), mas não [`docs/src/configuring-languages.md`](https://github.com/zed-industries/zed/blob/main/docs/src/configuring-languages.md).

### Caracteres curinga implícitos

Ao usar os filtros “Incluir” / “Excluir” em uma Pesquisa de Projeto, cada padrão de busca é envolvido por curingas implícitas. Por exemplo, para excluir da sua pesquisa quaisquer arquivos com “license” no caminho ou no nome do arquivo, basta digitar `license` na caixa de exclusão. Nos bastidores, o Zed transforma `license` em `**license**`. Isso significa que arquivos com nomes como `license.*`, `*.license` ou aqueles localizados dentro de um subdiretório chamado `license` serão todos filtrados. Isso permite que os usuários filtrem facilmente por `*.ts` sem precisar se lembrar de digitar `**/*.ts` todas as vezes.

Como alternativa, se nas configurações do Zed você quiser uma substituição de [`file_types`](./reference/all-settings.md#file-types) que se aplique apenas a um determinado diretório, é necessário incluir explicitamente os caracteres curinga. Por exemplo, se você tiver um diretório de arquivos de modelo com a extensão `html` que deseja que sejam reconhecidos como modelos Jinja2, pode usar o seguinte:

```json [settings]
{
  "file_types": {
    "C++": ["[cC]"],
    "Jinja2": ["**/templates/*.html"]
  }
}
```

## Referências

Embora os globs no Zed sejam implementados conforme descrito acima, ao escrever código usando globs em outras linguagens, consulte a documentação sobre globs da sua plataforma:

- [macOS fnmatch](https://developer.apple.com/library/archive/documentation/System/Conceptual/ManPages_iPhoneOS/man3/fnmatch.3.html) (Biblioteca Padrão C do BSD)
- [Linux fnmatch](https://www.gnu.org/software/libc/manual/html_node/Wildcard-Matching.html) (Biblioteca Padrão C do GNU)
- [POSIX fnmatch](https://pubs.opengroup.org/onlinepubs/9699919799/functions/fnmatch.html) (Especificação POSIX)
- [node-glob](https://github.com/isaacs/node-glob) (pacote `glob` do Node.js)
- [Python glob](https://docs.python.org/3/library/glob.html) (Biblioteca Padrão do Python)
- [Golang glob](https://pkg.go.dev/path/filepath#Match) (Biblioteca Padrão do Go)
- [Padrões do gitignore](https://git-scm.com/docs/gitignore) (Formato dos padrões do gitignore)
- [PowerShell: Sobre caracteres curinga](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_wildcards) (Caracteres curinga no PowerShell)
