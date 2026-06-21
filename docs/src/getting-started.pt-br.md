---
título: Introdução ao Zed
descrição: Comece a usar o Zed, o editor de código-fonte aberto e rápido. Comandos essenciais, configuração do ambiente e noções básicas de navegação.
---

# Introdução

O Zed é um editor de código-fonte aberto com ferramentas integradas de colaboração e inteligência artificial.

Este guia aborda os comandos essenciais, a configuração do ambiente e os conceitos básicos de navegação.

## Introdução rápida

### Página de Boas-Vindas

Ao abrir o Zed sem uma pasta, você vê a página de boas-vindas na área principal do editor. A página de boas-vindas oferece ações rápidas para abrir uma pasta, clonar um repositório ou visualizar a documentação. Assim que você abre uma pasta ou um arquivo, a página de boas-vindas desaparece. Se você dividir o editor em vários painéis, a página de boas-vindas aparecerá apenas no painel central quando ele estiver vazio — os demais painéis exibirão o estado padrão de vazio.

Para reabrir a página de boas-vindas, feche todos os itens no painel central ou use a paleta de comandos para procurar por “Welcome”.

### 1. Abrir um projeto

Abrir uma pasta a partir da linha de comando:

```sh
zed ~/projects/my-app
```

Ou use `Cmd+O` (macOS) / `Ctrl+O` (Linux/Windows) para abrir uma pasta diretamente no Zed.

Por padrão, os novos projetos são abertos na barra lateral de threads da janela atual. Para abri-los em uma nova janela, use `zed -n ~/projects/my-app` ou pressione `Cmd+Enter` ao selecionar a opção “Abrir recentes”. Consulte [Janelas e Projetos](./windows-and-projects.md) para obter mais detalhes.

### 2. Aprenda os comandos essenciais

| Ação          | macOS         | Linux/Windows  |
| --------------- | ------------- | -------------- |
| Paleta de comandos | `Cmd+Shift+P` | `Ctrl+Shift+P` |
| Ir para o arquivo      | `Cmd+P`       | `Ctrl+P`       |
| Ir para o símbolo    | `Cmd+Shift+O` | `Ctrl+Shift+O` |
| Localizar no projeto | `Cmd+Shift+F` | `Ctrl+Shift+F` |
| Alternar terminal | `` Ctrl+` ``  | `` Ctrl+` ``   |
| Abrir configurações   | `Cmd+,`       | `Ctrl+,`       |

A paleta de comandos (`Cmd+Shift+P`) é a sua porta de acesso a todas as ações do Zed. Se você esquecer um atalho, procure-o lá.

### Layout do painel

Use **Layout do painel > Agentic** no menu do usuário na barra de título (ou a ação {#action workspace::UseAgenticLayout}) quando quiser que o Painel do Agente e a Barra Lateral de Tópicos fiquem lado a lado à esquerda. Use **Layout do painel > Clássico** (ou {#action workspace::UseClassicLayout}) para restaurar o layout voltado para o editor.

### 3. Configure seu editor

Abra o Editor de Configurações com `Cmd+,` (macOS) ou `Ctrl+,` (Linux/Windows). Procure qualquer configuração e altere-a diretamente.

Primeiras alterações comuns:

- **Tema**: Pressione `Cmd+K Cmd+T` (macOS) ou `Ctrl+K Ctrl+T` (Linux/Windows) para abrir o seletor de temas
- **Fonte**: Procure por `buffer_font_family` em Configurações
- **Formatar ao salvar**: Procure por `format_on_save` e defina como `on`

### 4. Defina seu idioma

O Zed inclui suporte integrado para vários idiomas. Para os demais, instale a extensão:

1. Abra as extensões com `Cmd+Shift+X` (macOS) ou `Ctrl+Shift+X` (Linux/Windows)
2. Pesquise seu idioma
3. Clique em “Instalar”

Consulte [Idiomas](./languages.md) para obter instruções de configuração específicas para cada idioma.

### 5. Experimente os recursos de IA

O Zed inclui assistência por IA integrada. Abra o Painel do Agente com `Cmd+Shift+A` (macOS) ou `Ctrl+Shift+A` (Linux/Windows) para iniciar uma conversa, ou use `Cmd+Enter` (macOS) / `Ctrl+Enter` (Linux/Windows) para obter assistência diretamente no texto.

Consulte [Visão geral da IA](./ai/overview.md) para configurar provedores e saber quais são as possibilidades.

## Está vindo de outro editor?

Temos guias específicos para quem está mudando de outro editor:

- [VS Code](./migrate/vs-code.md) — Importar configurações, mapear atalhos de teclado, encontrar recursos equivalentes
- [IntelliJ IDEA](./migrate/intellij.md) — Adapte-se à abordagem do Zed para navegação e refatoração
- [PyCharm](./migrate/pycharm.md) — Configurar o desenvolvimento em Python no Zed
- [WebStorm](./migrate/webstorm.md) — Configurar fluxos de trabalho de JavaScript/TypeScript
- [RustRover](./migrate/rustrover.md) — Desenvolvimento em Rust no Zed

Você também pode ativar combinações de teclas conhecidas:

- **Vim**: Ative o `vim_mode` nas configurações. Consulte [Modo Vim](./vim.md).
- **Helix**: Ative o `helix_mode` nas configurações. Consulte [Modo Helix](./helix.md).

## Participe da comunidade

O Zed é um projeto de código aberto. Junte-se a nós no GitHub ou no Discord para contribuir com código, relatar bugs ou sugerir novos recursos.

- [Discord](https://discord.com/invite/zedindustries)
- [Discussões do GitHub](https://github.com/zed-industries/zed/discussions)
- [Zed Reddit](https://www.reddit.com/r/ZedEditor)
