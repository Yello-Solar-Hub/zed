# Diretrizes para o Agente de Automação de Documentação

Este arquivo rege as atualizações automatizadas da documentação acionadas por alterações no código. Todas as fases de automação devem estar em conformidade com essas regras.

## Sistema de Documentação

Esta documentação utiliza o **mdBook** (https://rust-lang.github.io/mdBook/).

### Arquivos importantes

- **`docs/src/SUMMARY.md`**: Índice no formato mdBook (https://rust-lang.github.io/mdBook/format/summary.html)
- **`docs/book.toml`**: configuração do mdBook
- **`docs/.prettierrc`**: Configuração do Prettier (largura de linha de 80 caracteres)

### Formato SUMMARY.md

O arquivo `SUMMARY.md` define a estrutura do livro. Regras de formatação:

- Os títulos dos capítulos são links: `[Título](./caminho/para/arquivo.md)`
- Aninhamento por meio de recuo (2 espaços por nível)
- Separadores: `---` para linhas horizontais entre seções
- Capítulos em rascunho: `[Título]()` (parênteses vazios, ainda não escrito)

Exemplo:

```markdown
# Section Title

- [Chapter](./chapter.md)
  - [Nested Chapter](./nested.md)

---

# Another Section
```

### Pré-processador personalizado

A documentação utiliza um pré-processador personalizado (`docs_preprocessor`) que expande comandos especiais:

| Sintaxe                      | Objetivo                               | Exemplo                       |
| --------------------------- | ------------------------------------- | ----------------------------- |
| {#kb action::NomeDaAção}    | Tecla de atalho para a ação                 | {#kb agent::Alternar foco}      |
| {#action agent::ActionName} | Referência de ação (exibida como comando) | {#action agent::AbrirConfigurações} |

**Regras:**

- Sempre use a sintaxe do pré-processador para as combinações de teclas, em vez de codificá-las diretamente
- Os nomes das ações utilizam `snake_case` no namespace e `PascalCase` para a ação
- Espaços de nomes comuns: `agent::`, `editor::`, `assistant::`, `vim::`

### Requisitos de formatação

Toda a documentação deve ser formatada com o **Prettier**:

```sh
cd docs && npx prettier --check src/
```

Antes que qualquer alteração na documentação seja considerada concluída:

1. Execute o Prettier para formatar: `cd docs && npx prettier --write src/`
2. Verifique se o comando é executado corretamente: `cd docs && npx prettier --check src/`

Configuração do Prettier: largura de linha de 80 caracteres (`docs/.prettierrc`)

### Ancoragens de seção

Use a sintaxe `{#anchor-id}` para títulos de seção que possam ser transformados em links:

```markdown
## Getting Started {#getting-started}

### Custom Models {#anthropic-custom-models}
```

Os IDs de âncora devem ser:

- Letras minúsculas com hífens
- Único na página
- Descritivo (pode incluir o contexto pai, como `anthropic-custom-models`)

### Anotações em blocos de código

Use anotações após o identificador de linguagem para indicar o contexto do arquivo:

```markdown
\`\`\`json [settings]
{
"agent": { ... }
}
\`\`\`

\`\`\`json [keymap]
[
{ "bindings": { ... } }
]
\`\`\`
```

Anotações válidas: `[settings]` (para settings.json), `[keymap]` (para keymap.json)

### Formatação de citações em bloco

Use rótulos em negrito para as legendas:

```markdown
> **Note:** Important information the user should know.

> **Tip:** Helpful advice that saves time or improves workflow.

> **Warn:** Caution about potential issues or gotchas.
```

### Referências das imagens

As imagens estão hospedadas externamente. Formato de referência:

```markdown
![Alt text description](https://zed.dev/img/path/to/image.webp)
```

### Reticulamento

- Links relativos para o mesmo diretório: `[Painel do Agente](./agent-panel.md)`
- Com âncoras: `[Modelos personalizados](./llm-providers.md#anthropic-custom-models)`
- Diretório pai: `[Telemetria](../telemetry.md)`

## Voz e tom

### Princípios Fundamentais

- **Prático em vez de promocional**: Concentre-se no que os usuários podem fazer, e não em vender o Zed. Evite termos de marketing como “poderoso”, “revolucionário” ou “o melhor da categoria”.
- **Ser sincero sobre as limitações**: Quando o Zed não tiver um recurso ou não atingir o nível de detalhamento de outra ferramenta, diga isso diretamente. Apresente as limitações juntamente com soluções alternativas ou fluxos de trabalho alternativos.
- **Direto e conciso**: Use frases curtas. Vá direto ao ponto. Os desenvolvedores estão dando uma olhada rápida, não lendo romances.
- **Segunda pessoa**: Dirija-se ao leitor usando “você”. Evite “o usuário” ou “alguém”.
- **Presente**: “Zed abre o arquivo”, e não “Zed vai abrir o arquivo”.

### O que evitar

- Superlativos sem fundamento (“incrivelmente rápido”, “perfeitamente integrado”)
- Expressões evasivas (“simplesmente”, “apenas”, “facilmente”) — se algo for simples, as instruções vão deixar isso claro
- Tom de desculpa pela falta de recursos — mencione a limitação e siga em frente
- Comparações que menosprezam outras ferramentas — seja objetivo, não competitivo
- Uso frequente de travessões “em” ou “en”.

## Exemplos de textos bem escritos

### Bom: Direto e prático

```bash
To format on save, open the Settings Editor (`Cmd+,`) and search for `format_on_save`. Set it to `on`.

Or add this to your settings.json:
{
  "format_on_save": "on"
}
```

### Ruim: Prolixo e promocional

```bash
Zed provides a powerful and seamless formatting experience. Simply navigate to the settings and you'll find the format_on_save option which enables Zed's incredible auto-formatting capabilities.
```

### Ponto positivo: Sinceridade em relação às limitações

```bash
Zed doesn't index your project like IntelliJ does. You open a folder and start working immediately—no waiting. The trade-off: cross-project analysis relies on language servers, which may not go as deep.

**How to adapt:**
- Use `Cmd+Shift+F` for project-wide text search
- Use `Cmd+O` for symbol search (powered by your language server)
```

### Ruim: Defensivo ou desdenhoso

```bash
While some users might miss indexing, Zed's approach is actually better because it's faster.
```

## Âmbito

### Documentação abrangida pelo escopo

- Todos os arquivos Markdown na pasta `docs/src/`
- `docs/src/SUMMARY.md` (índice do mdBook)
- Documentação específica para cada idioma em `docs/src/languages/`
- Documentação sobre recursos (IA, extensões, configuração, etc.)

### Fora do escopo (não modificar)

- `CHANGELOG.md`, `CONTRIBUTING.md`, `README.md` na raiz do repositório
- Comentários de código embutidos e rustdoc
- `CLAUDE.md`, `GEMINI.md` ou outros arquivos de instruções de IA
- Configuração de compilação (`book.toml`, arquivos de tema, `docs_preprocessor`)
- Qualquer arquivo fora da pasta `docs/src/`

## Padrões de estrutura de página

### Layout padrão da página

A maioria das páginas de documentação segue esta estrutura:

1. **Título** (H1) - Uma única frase ou expressão
2. **Visão geral/Introdução** - 1 a 3 parágrafos explicando do que se trata
3. **Introdução** `{#getting-started}` - Pré-requisitos e primeiros passos
4. **Conteúdo principal** - Detalhes dos recursos, organizados por tópico
5. **Avançado/Configuração** - Opções para usuários avançados
6. **Veja também** (opcional) - Links para documentação relacionada

### Padrão de documentação de configurações

Ao documentar as configurações:

1. Mostre primeiro a abordagem do Editor de Configurações (IU)
2. Em seguida, exiba o JSON da seguinte forma: “Ou adicione isto ao seu settings.json:”
3. Sempre apresente um JSON completo e válido, com a estrutura que o envolve:

```json [settings]
{
  "agent": {
    "default_model": {
      "provider": "anthropic",
      "model": "claude-sonnet-4"
    }
  }
}
```

### Padrão de documentação de provedores/recursos

Para cada provedor ou recurso distinto:

1. Título H3 com âncora: `### Nome do provedor {#provider-name}`
2. Breve descrição (1 a 2 frases)
3. Etapas de configuração (lista numerada)
4. Exemplo de configuração (bloco de código JSON)
5. Seção de modelos personalizados, se aplicável: `#### Modelos personalizados {#provider-custom-models}`

## Regras de estilo

Herdar todas as convenções de `docs/.rules`. Pontos principais:

### Voz

- Segunda pessoa (“você”), presente do indicativo
- Direto e conciso — sem expressões evasivas (“simplesmente”, “apenas”, “facilmente”)
- Sinceridade quanto às limitações; sem linguagem promocional

### Formatação

- Atribuições de teclas: crases com `+` para combinações simultâneas de teclas (`Cmd+Shift+P`)
- Mostrar as variantes do macOS e do Linux/Windows quando houver diferenças
- Use blocos de código `sh` para comandos de terminal
- Configurações: exibir primeiro a interface do Editor de Configurações e, em segundo lugar, o JSON

### Terminologia

| Uso             | Em vez de                                                            |
| --------------- | --------------------------------------------------------------------- |
| pasta          | diretório                                                             |
| projeto         | espaço de trabalho                                                             |
| Editor de configurações | interface de usuário de configurações                                                           |
| paleta de comandos | barra de comandos                                                           |
| painel           | janela de ferramentas, barra lateral (seja específico: “Painel do Projeto”, “Painel do Terminal”) |
| servidor de idiomas | LSP (escreva por extenso na primeira ocorrência; depois, basta usar LSP)                           |

## Convenções específicas do Zed

### Arquivos de regras reconhecidos

Ao documentar regras/instruções para a IA, observe que o Zed reconhece esses arquivos (em ordem de prioridade):

- `.regras`
- `.cursorrules`
- `.windsurfrules`
- `.clinerules`
- `.github/copilot-instructions.md`
- `AGENT.md`
- `AGENTS.md`
- `CLAUDE.md`
- `GEMINI.md`

### Locais dos arquivos de configuração

- macOS: `~/.config/zed/settings.json`
- Linux: `~/.config/zed/settings.json`
- Windows: `%AppData%\Zed\settings.json`

### Locais dos arquivos de mapeamento de teclas

- macOS: `~/.config/zed/keymap.json`
- Linux: `~/.config/zed/keymap.json`
- Windows: `%AppData%\Zed\keymap.json`

## Restrições de segurança

### Não deve

- Excluir os arquivos de documentação existentes
- Remover as seções que documentam as funcionalidades existentes
- Alterar URLs ou links de âncora sem verificar as referências
- Modificar a estrutura do arquivo `SUMMARY.md` sem alterar o conteúdo correspondente
- Adicionar documentação hipotética para recursos ainda não lançados
- Incluir detalhes internos de implementação que não sejam relevantes para os usuários

### É obrigatório

- Manter a estrutura existente ao atualizar o conteúdo
- Manter a compatibilidade com versões anteriores das configurações/comandos documentados
- Indique explicitamente a incerteza, em vez de dar palpites
- Incluir um link para a documentação relacionada ao adicionar novas seções

## Alterar classificação

### Requer atualização da documentação

- Novos recursos ou comandos voltados para o usuário
- Alterações nas combinações de teclas ou nos comportamentos padrão
- Esquema de configurações ou opções modificados
- Funcionalidades obsoletas ou removidas
- Alterações na API que afetam as extensões

### Não requer atualização da documentação

- Refatoração interna sem alterações comportamentais
- Otimizações de desempenho (a menos que sejam visíveis ao usuário)
- Correções de bugs que restauram o comportamento documentado
- Alterações no teste
- Alterações no CI/CD

## Formato de saída

### Plano de Documentação da Fase 4

Ao elaborar um plano de documentação, utilize esta estrutura:

```markdown
## Documentation Impact Assessment

### Summary

Brief description of code changes analyzed.

### Documentation Updates Required: [Yes/No]

### Planned Changes

#### 1. [File Path]

- **Section**: [Section name or "New section"]
- **Change Type**: [Update/Add/Deprecate]
- **Reason**: Why this change is needed
- **Description**: What will be added/modified

#### 2. [File Path]

...

### Uncertainty Flags

- [ ] [Description of any assumptions or areas needing confirmation]

### No Changes Needed

- [List files reviewed but not requiring updates, with brief reason]
```

### Formato de resumo da Fase 6

```markdown
## Documentation Update Summary

### Changes Made

| File           | Change            | Related Code      |
| -------------- | ----------------- | ----------------- |
| path/to/doc.md | Brief description | link to PR/commit |

### Rationale

Brief explanation of why these updates were made.

### Review Notes

Any items reviewers should pay special attention to.
```

## Diretrizes de Conduta

### Conservador por padrão

- Quando não tiver certeza se deve documentar algo, marque-o para revisão humana
- É melhor fazer atualizações menores e específicas do que reescritas abrangentes
- Não “melhore” a documentação que não esteja relacionada à alteração no código que a motivou

### Rastreabilidade

- Toda alteração na documentação deve estar vinculada a uma alteração específica no código
- Inclua referências a commits, PRs ou issues relevantes nos resumos

### Atualizações incrementais

- Atualize as seções existentes em vez de criar documentação paralela
- Manter a coerência com o conteúdo ao redor
- Siga os padrões estabelecidos em cada área da documentação
