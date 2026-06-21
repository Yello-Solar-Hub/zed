---
título: Configuração do servidor de idiomas e do Tree-sitter - Zed
descrição: Configure o suporte a linguagens no Zed usando o Tree-sitter para realce de sintaxe e o LSP para diagnósticos, autocompletar e formatação.
---

# Configurando os idiomas suportados

O suporte a linguagens do Zed baseia-se em duas tecnologias:

1. O **Tree-sitter** é responsável pelo realce de sintaxe e por recursos baseados na estrutura, como o painel de esboço.
2. O **Protocolo de Servidor de Linguagem (LSP)** oferece recursos semânticos: autocompletar código, diagnósticos, ir para a definição e refatoração.

Esta página aborda configurações específicas de linguagem, associações de arquivos, configuração do servidor de linguagem, formatação, verificação de código e destaque de sintaxe.

Para obter uma lista dos idiomas suportados, consulte [Idiomas suportados](./languages.md). Para adicionar suporte a novos idiomas, consulte [Extensões de idioma](./extensions/languages.md).

## Configurações específicas do idioma

O Zed permite que você substitua as configurações globais para idiomas específicos. Essas configurações personalizadas são definidas no arquivo `settings.json`, na chave `languages`.

Aqui está um exemplo de configurações específicas de idioma:

```json [settings]
"languages": {
  "Python": {
    "tab_size": 4,
    "formatter": "language_server",
    "format_on_save": "on"
  },
  "JavaScript": {
    "tab_size": 2,
    "formatter": {
      "external": {
        "command": "prettier",
        "arguments": ["--stdin-filepath", "{buffer_path}"]
      }
    }
  }
}
```

Você pode personalizar uma ampla variedade de configurações para cada idioma, incluindo:

- [`tab_size`](./reference/all-settings.md#tab-size): O número de espaços para cada nível de recuo
- [`formatter`](./reference/all-settings.md#formatter): A ferramenta utilizada para a formatação de código
- [`format_on_save`](./reference/all-settings.md#format-on-save): Se o código deve ser formatado automaticamente ao salvar
- [`enable_language_server`](./reference/all-settings.md#enable-language-server): Ativar ou desativar o suporte ao servidor de idiomas
- [`hard_tabs`](./reference/all-settings.md#hard-tabs): Use tabulações em vez de espaços para indentação
- [`preferred_line_length`](./reference/all-settings.md#preferred-line-length): O comprimento máximo recomendado para as linhas
- [`soft_wrap`](./reference/all-settings.md#soft-wrap): Como quebrar linhas longas de código
- [`show_completions_on_input`](./reference/all-settings.md#show-completions-on-input): Se deve ou não exibir sugestões de preenchimento automático à medida que você digita
- [`show_completion_documentation`](./reference/all-settings.md#show-completion-documentation): Determina se a documentação dos itens do menu de autocompletar deve ser exibida embutida ou ao lado deles
- [`colorize_brackets`](./reference/all-settings.md#colorize-brackets): Determina se as consultas de colchetes do Tree-Sitter devem ser usadas para detectar e colorir os colchetes no editor (também conhecidos como “colchetes arco-íris”)

Essas configurações permitem que você mantenha estilos específicos de codificação em diferentes idiomas e projetos.

## Associações de arquivos

O Zed detecta automaticamente os tipos de arquivo com base em suas extensões, mas você pode personalizar essas associações para se adequarem ao seu fluxo de trabalho.

Para configurar associações personalizadas de arquivos, use a configuração [`file_types`](./reference/all-settings.md#file-types) no seu arquivo `settings.json`:

```json [settings]
"file_types": {
  "C++": ["c"],
  "TOML": ["MyLockFile"],
  "Dockerfile": ["Dockerfile*"]
}
```

Essa configuração instrui o Zed a:

- Tratar arquivos `.c` como C++ em vez de C
- Reconhecer arquivos com o nome “MyLockFile” como TOML
- Aplicar a sintaxe do Dockerfile a qualquer arquivo cujo nome comece com “Dockerfile”

Você pode usar padrões glob para uma correspondência mais flexível, o que permite lidar com convenções de nomenclatura complexas em seus projetos.

## Trabalhando com servidores de idiomas

Os servidores de linguagem são uma parte essencial dos recursos de codificação inteligente do Zed, oferecendo funcionalidades como autocompletar, ir para a definição e verificação de erros em tempo real.

### O que são servidores de idiomas?

Os servidores de linguagem implementam o Protocolo de Servidor de Linguagem (LSP), que padroniza a comunicação entre o editor e as ferramentas específicas de cada linguagem. Isso permite que o Zed ofereça suporte a recursos avançados para várias linguagens de programação sem precisar implementar cada recurso separadamente.

Algumas das principais funcionalidades oferecidas pelos servidores de idiomas incluem:

- Autocompletar código
- Verificação de erros e diagnóstico
- Navegação no código (ir para a definição, localizar referências)
- Ações no código (Renomear, extrair método)
- Informações ao passar o mouse
- Pesquisa por símbolo no espaço de trabalho

### Gerenciamento de servidores de idiomas

O Zed simplifica o gerenciamento do servidor de idiomas para os usuários:

1. Download automático: Ao abrir um arquivo com um tipo de arquivo compatível, o Zed baixa automaticamente o servidor de idiomas apropriado. O Zed pode solicitar que você instale uma extensão para tipos de arquivo conhecidos.

2. Local de armazenamento:

   - macOS: `~/Library/Application Support/Zed/languages`
   - Linux: `$XDG_DATA_HOME/zed/languages`, `$FLATPAK_XDG_DATA_HOME/zed/languages` ou `$HOME/.local/share/zed/languages`

3. Atualizações automáticas: O Zed mantém seus servidores de idiomas atualizados, garantindo que você sempre tenha os recursos e melhorias mais recentes.

### Escolha de servidores de idiomas

Algumas linguagens no Zed oferecem várias opções de servidores de linguagem. Você pode ter várias extensões instaladas que incluem servidores de linguagem voltados para a mesma linguagem, o que pode resultar em sobreposição de funcionalidades. Para garantir que você obtenha a funcionalidade de sua preferência, o Zed permite que você defina a prioridade dos servidores de linguagem a serem usados e a ordem em que serão utilizados.

Você pode definir sua preferência usando a configuração `language_servers`:

```json [settings]
  "languages": {
    "PHP": {
      "language_servers": ["intelephense", "!phpactor", "!phptools", "..."]
    }
  }
```

Neste exemplo:

- O `intelephense` está definido como o servidor de idiomas principal.
- O `phpactor` e o `phptools` estão desativados (observe o prefixo `!`).
- `"..."` expande-se para o restante dos servidores de linguagem registrados para PHP que ainda não constam na lista.

A entrada `"..."` funciona como um curinga que inclui qualquer servidor de idioma registrado que você não tenha mencionado explicitamente. Os servidores listados pelo nome mantêm sua posição, e `"..."` preenche os restantes nesse ponto da lista. Servidores com o prefixo `!` são totalmente excluídos. Isso significa que, se uma nova extensão de servidor de idioma for instalada ou um novo servidor for registrado para um idioma, `"..."` o incluirá automaticamente. Se você quiser controle total sobre quais servidores estão habilitados, omita `"..."` — apenas os servidores que você listar pelo nome serão usados.

#### Exemplos

Suponha que você esteja trabalhando com Ruby. A configuração padrão é:

```json [settings]
{
  "language_servers": [
    "solargraph",
    "!ruby-lsp",
    "!rubocop",
    "!sorbet",
    "!steep",
    "!kanayago",
    "..."
  ]
}
```

Quando você sobrescreve `language_servers` em suas configurações, sua lista **substitui** totalmente a lista padrão. Isso significa que servidores desativados por padrão, como `kanayago`, serão reativados por `"..."`, a menos que você os desative explicitamente novamente.

| Configuração                                     | Resultado                                                             |
| ------------------------------------------------- | ------------------------------------------------------------------ |
| `["..."]`                                         | `solargraph`, `ruby-lsp`, `rubocop`, `sorbet`, `steep`, `kanayago` |
| `["ruby-lsp", "..."]`                             | `ruby-lsp`, `solargraph`, `rubocop`, `sorbet`, `steep`, `kanayago` |
| `["ruby-lsp", "!solargraph", "!kanayago", "..."]` | `ruby-lsp`, `rubocop`, `sorbet`, `steep`                           |
| `["ruby-lsp", "solargraph"]`                      | `ruby-lsp`, `solargraph`                                           |

> Observação: No primeiro exemplo, `"..."` inclui `kanayago`, embora ele esteja desativado por padrão. A substituição substituiu a lista padrão, de modo que a entrada `"!kanayago"` não está mais presente. Para mantê-lo desativado, você deve incluir `"!kanayago"` na sua configuração.

### Cadeias de ferramentas

Alguns servidores de linguagem precisam ser configurados com uma “toolchain” atualizada, que consiste na instalação de uma versão específica de um compilador e/ou interpretador de uma linguagem de programação, podendo incluir um conjunto completo de dependências de um projeto.
Um exemplo do que Zed considera uma cadeia de ferramentas é um ambiente virtual em Python.
Nem todas as linguagens no Zed oferecem suporte à descoberta e seleção de cadeias de ferramentas, mas, para aquelas que oferecem, é possível especificar a cadeia de ferramentas por meio de um seletor de cadeias de ferramentas (usando {#action toolchain::Select}). Para saber mais sobre cadeias de ferramentas no Zed, consulte [`toolchains`](./toolchains.md).

### Configuração de servidores de idiomas

Ao configurar servidores de linguagem no seu `settings.json`, as sugestões de autocompletar incluem todos os adaptadores LSP disponíveis reconhecidos pelo Zed, e não apenas aqueles que estão ativos no momento para as linguagens carregadas. Isso ajuda você a descobrir e configurar servidores de linguagem antes de abrir arquivos que os utilizam.

Muitos servidores de idiomas aceitam opções de configuração personalizadas. Você pode defini-las na seção `lsp` do seu arquivo `settings.json`:

```json [settings]
  "lsp": {
    "rust-analyzer": {
      "initialization_options": {
        "check": {
          "command": "clippy"
        }
      }
    }
  }
```

Este exemplo configura o Rust Analyzer para usar o Clippy para uma verificação adicional de conformidade ao salvar arquivos.

#### Objetos aninhados

Ao configurar as opções do servidor de linguagem no Zed, é importante usar objetos aninhados em vez de strings delimitadas por pontos. Isso é particularmente relevante ao trabalhar com configurações mais complexas. Vejamos um exemplo prático usando o servidor de linguagem TypeScript:

Suponha que você queira definir as seguintes configurações para o TypeScript:

- Ativar verificações rigorosas de valores nulos
- Defina a versão alvo do ECMAScript como ES2020

Veja a seguir como você deve estruturar essas configurações no arquivo `settings.json` do Zed:

```json [settings]
"lsp": {
  "typescript-language-server": {
    "initialization_options": {
      // These are not supported (VSCode dotted style):
      // "preferences.strictNullChecks": true,
      // "preferences.target": "ES2020"
      //
      // These is correct (nested notation):
      "preferences": {
        "strictNullChecks": true,
        "target": "ES2020"
      },
    }
  }
}
```

#### Possíveis opções de configuração

Dependendo de como um determinado servidor de idioma é implementado, ele pode depender de diferentes opções de configuração, ambas especificadas no LSP.

- [opções de inicialização](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#version_3_17_0)

Enviado uma vez durante a inicialização do servidor de idiomas; requer a reinicialização do servidor para que as alterações sejam reaplicadas.

Por exemplo, o rust-analyzer e o clangd dependem exclusivamente dessa forma de configuração.

```json [settings]
  "lsp": {
    "rust-analyzer": {
      "initialization_options": {
        "checkOnSave": false
      }
    }
  }
```

- [Solicitação de configuração](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspace_configuration)

Pode ser consultado pelo servidor várias vezes.
A maioria dos servidores utilizaria exclusivamente essa forma de configuração.

```json [settings]
"lsp": {
  "tailwindcss-language-server": {
    "settings": {
      "tailwindCSS": {
        "emmetCompletions": true,
      },
    }
  }
}
```

Além das opções de configuração do servidor relacionadas ao LSP, alguns servidores no Zed permitem configurar a forma como o binário é executado pelo Zed.

Os servidores de idiomas são baixados ou iniciados automaticamente caso sejam encontrados no seu caminho; se você quiser especificar um binário alternativo explicitamente, pode fazer isso nas configurações:

```json [settings]
  "lsp": {
    "rust-analyzer": {
      "binary": {
        // Whether to fetch the binary from the internet, or attempt to find locally.
        "ignore_system_version": false,
        "path": "/path/to/langserver/bin",
        "arguments": ["--option", "value"],
        "env": {
          "FOO": "BAR"
        }
      }
    }
  }
```

### Ativando ou desativando servidores de idiomas

Você pode ativar ou desativar o suporte ao servidor de idiomas globalmente ou por idioma:

```json [settings]
  "languages": {
    "Markdown": {
      "enable_language_server": false
    }
  }
```

Isso desativa o servidor de idiomas para arquivos Markdown, o que pode ser útil para melhorar o desempenho em grandes projetos de documentação. Você pode configurar isso globalmente no arquivo `~/.config/zed/settings.json` ou no arquivo `.zed/settings.json` localizado no diretório do seu projeto.

## Formatação e verificação de conformidade

O Zed oferece suporte à formatação e à verificação de código para manter um estilo de código consistente e detectar possíveis problemas antecipadamente.

### Configurando formatadores

O Zed oferece suporte tanto a formatadores integrados quanto externos. Consulte a documentação sobre [`formatter`](./reference/all-settings.md#formatter) para obter mais informações. Você pode configurar os formatadores globalmente ou por linguagem no seu arquivo `settings.json`:

```json [settings]
"languages": {
  "JavaScript": {
    "formatter": {
      "external": {
        "command": "prettier",
        "arguments": ["--stdin-filepath", "{buffer_path}"]
      }
    },
    "format_on_save": "on"
  },
  "Rust": {
    "formatter": "language_server",
    "format_on_save": "on"
  }
}
```

Este exemplo utiliza o Prettier para JavaScript e o formatador do servidor de linguagem para Rust, ambos configurados para formatar ao salvar.

Para desativar a formatação para um idioma específico:

```json [settings]
"languages": {
  "Markdown": {
    "format_on_save": "off"
  }
}
```

### Configurando linters

A verificação de conformidade no Zed é normalmente realizada por servidores de linguagem. Muitos servidores de linguagem permitem que você configure regras de verificação de conformidade:

```json [settings]
"lsp": {
  "eslint": {
    "settings": {
      "codeActionOnSave": {
        "rules": ["import/order"]
      }
    }
  }
}
```

Essa configuração configura o ESLint para organizar as importações ao salvar arquivos JavaScript.

Para executar as correções do linter automaticamente ao salvar:

```json [settings]
"languages": {
  "JavaScript": {
    "formatter": {
      "code_action": "source.fixAll.eslint"
    }
  }
}
```

### Formatação de seleções

O Zed permite formatar apenas o texto selecionado por meio de {#action editor::FormatSelections} ({#kb editor::FormatSelections}). Como
O funcionamento disso depende do formatador configurado:

- A ação só é exibida quando o formatador ativo pode realmente formatar intervalos para pelo menos um
  buffer selecionado.
- **Servidor de idiomas**: Envia uma solicitação de formatação de intervalo LSP para cada seleção. Isso fornece o
  formatação mais precisa, exclusiva para seleção, e só está disponível quando o servidor de idiomas configurado
  anuncia suporte à formatação de intervalos.
- **Prettier**: Utiliza a formatação de intervalo integrada do Prettier para formatar o intervalo que abrange todas as seleções. Qualquer
  As edições resultantes que estiverem fora dos intervalos selecionados são descartadas; assim, apenas o código selecionado é modificado.
- **Comandos externos**: Os formatadores de comandos externos não suportam formatação de intervalos e são ignorados durante a formatação
  seleções.
- **Formatação de ações de código**: As ações de código atuam sobre todo o buffer, portanto, não ativam
  `opções de formato` por si só.

### Integração de formatação e verificação de código

O Zed permite que você execute tanto a formatação quanto a verificação de conformidade ao salvar. Aqui está um exemplo que usa o Prettier para formatação e o ESLint para verificação de conformidade de arquivos JavaScript:

```json [settings]
"languages": {
  "JavaScript": {
    "formatter": [
      {
        "code_action": "source.fixAll.eslint"
      },
      {
        "external": {
          "command": "prettier",
          "arguments": ["--stdin-filepath", "{buffer_path}"]
        }
      }
    ],
    "format_on_save": "on"
  }
}
```

### Solução de problemas

Caso você encontre problemas com formatação ou verificação de código:

1. Verifique se há mensagens de erro no arquivo de log do Zed (use a paleta de comandos: {#action zed::OpenLog})
2. Certifique-se de que as ferramentas externas (formatadores, linters) estejam corretamente instaladas e incluídas no PATH
3. Verifique as configurações tanto nas configurações do Zed quanto nos arquivos de configuração específicos do idioma (por exemplo, `.eslintrc`, `.prettierrc`)

## Destaque de sintaxe e temas

O Zed oferece opções de personalização para realce de sintaxe e temas, permitindo que você adapte a aparência visual do seu código.

### Personalização do realce de sintaxe

O Zed utiliza gramáticas Tree-sitter para o realce de sintaxe. É possível substituir o realce padrão usando a configuração `theme_overrides`.

Este exemplo coloca os comentários em itálico e altera a cor das sequências de caracteres:

```json [settings]
"theme_overrides": {
  "One Dark": {
    "syntax": {
      "comment": {
        "font_style": "italic"
      },
      "string": {
        "color": "#00AA00"
      }
    }
  }
}
```

### Seleção e personalização de temas

Altere seu tema:

1. Use o seletor de temas ({#kb theme_selector::Toggle})
2. Ou defina isso no seu `settings.json`:

```json [settings]
"theme": {
  "mode": "dark",
  "dark": "One Dark",
  "light": "GitHub Light"
}
```

Crie temas personalizados criando um arquivo JSON na pasta `~/.config/zed/themes/`. O Zed detectará automaticamente e disponibilizará todos os temas presentes nesse diretório.

### Como usar extensões de tema

O Zed oferece suporte a extensões de tema. Navegue e instale extensões de tema no painel Extensões ({#kb zed::Extensions}).

Para criar sua própria extensão de tema, consulte o guia [Desenvolvendo extensões de tema](./extensions/themes.md).

## Como usar os recursos do Language Server

### Tokens semânticos

Os tokens semânticos oferecem um realce de sintaxe mais detalhado, utilizando informações de tipo e escopo provenientes de servidores de linguagem. Ative-os com a configuração `semantic_tokens`:

```json [settings]
"semantic_tokens": "combined"
```

- `"off"` — Apenas destaque para os “tree-sitters” (padrão)
- `"combinado"` — tokens semânticos do LSP sobrepostos ao Tree-Sitter
- `"full"` — Os tokens semânticos LSP substituem totalmente os tree-sitters

Você pode personalizar as cores e os estilos dos tokens por meio da configuração `global_lsp_settings.semantic_token_rules`.

→ [Documentação sobre tokens semânticos](./semantic-tokens.md)

### Dicas sobre incrustações

As dicas de inlay fornecem informações adicionais diretamente no código, como nomes de parâmetros ou tipos inferidos. Configure as dicas de inlay no seu `settings.json`:

```json [settings]
"inlay_hints": {
  "enabled": true,
  "show_type_hints": true,
  "show_parameter_hints": true,
  "show_other_hints": true
}
```

Para configurações de dicas de inlay específicas para cada idioma, consulte a documentação de cada idioma.

### Ações de código

As ações de código oferecem correções rápidas e opções de refatoração. Acesse as ações de código usando o comando {#action editor::ToggleCodeActions} ou clicando no ícone de lâmpada que aparece ao lado do cursor quando as ações estão disponíveis.

### Ir para a definição e referências

Use estes comandos para navegar pela sua base de código:

- {#action editor::GoToDefinition} (<kbd>f12|f12</kbd>)
- {#action editor::GoToTypeDefinition} (<kbd>cmd-f12|ctrl-f12</kbd>)
- {#action editor::FindAllReferences} (<kbd>Shift+F12|Shift+F12</kbd>)

### Renomear símbolo

Para renomear um símbolo em todo o projeto:

1. Coloque o cursor sobre o símbolo
2. Use o comando {#action editor::Rename} (<kbd>f2|f2</kbd>)
3. Digite o novo nome e pressione Enter

Esses recursos dependem das funcionalidades do servidor de idiomas para cada idioma.

Ao renomear um símbolo que aparece em vários arquivos, o Zed abrirá uma visualização em um multibuffer. Isso permite que você revise todas as alterações em todo o projeto antes de aplicá-las. Para confirmar a renomeação, basta salvar o multibuffer. Se decidir não prosseguir com a renomeação, você pode desfazer as alterações ou fechar o multibuffer sem salvar.

### Informações ao passar o mouse

Use o comando {#action editor::Hover} para exibir informações sobre o símbolo que está sob o cursor. Essas informações geralmente incluem detalhes sobre o tipo, documentação e links para recursos relevantes.

### Pesquisa de símbolos no espaço de trabalho

O comando {#action project_symbols::Toggle} permite pesquisar símbolos (funções, classes, variáveis) em todo o seu projeto. Isso é útil para navegar rapidamente por bases de código extensas.

### Autocompletar código

O Zed oferece sugestões inteligentes de autocompletamento de código à medida que você digita. Você pode acionar manualmente o autocompletamento com o comando {#action editor::ShowCompletions}. Use <kbd>tab|tab</kbd> ou <kbd>enter|enter</kbd> para aceitar as sugestões.

### Diagnósticos

Os servidores de linguagem fornecem diagnósticos em tempo real (erros, avisos, dicas) enquanto você programa. Visualize todos os diagnósticos do seu projeto usando o comando {#action diagnostics::Deploy}.
