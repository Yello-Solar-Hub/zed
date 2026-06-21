# Convenções de documentação do Zed

Este documento aborda as convenções estruturais para a documentação do Zed: o que documentar, como organizá-la e quando criar novas páginas.

Para obter informações sobre a voz, o tom e o estilo de redação, consulte o diretório [brand-voice/](./brand-voice/), que contém:

- `SKILL.md` — Princípios básicos de voz e fluxo de trabalho
- `rubric.md` — critérios de avaliação de qualidade em 8 pontos
- `taboo-phrases.md` — Padrões e expressões a evitar
- `voice-examples.md` — Exemplos de antes e depois da transformação

---

## O que precisa ser documentado

### Documento

- **Novos recursos voltados para o usuário** — Tudo com o que os usuários interagem diretamente
- **Novas configurações ou opções de configuração** — Inclua a chave da configuração, o tipo, o valor padrão e um exemplo
- **Novas combinações de teclas ou comandos** — Use a sintaxe `{#action ...}` e `{#kb ...}`
- **Todas as ações** — A exaustividade é importante; documente todas as ações, não apenas as que não são óbvias
- **Novos recursos de IA** — Ferramentas para agentes, provedores, fluxos de trabalho
- **Novos provedores ou integrações** — provedores de LLM, servidores MCP, agentes externos
- **Novas ferramentas** — Ferramentas para agentes, ferramentas MCP, ferramentas integradas
- **Novos painéis ou visualizações da interface do usuário** — Qualquer novo painel, barra lateral ou visualização com a qual os usuários interajam
- **APIs públicas de extensões** — Para desenvolvedores de extensões
- **Alterações que exigem adaptação** — Mesmo que a correção seja simples, documente o que mudou
- **Mudanças de comportamento específicas da versão** — Inclua referências à versão (por exemplo, “No Zed v0.224.0 e versões posteriores...”)

### Pular

- **Reestruturações internas** — Nenhuma alteração visível para o usuário, sem documentação
- **Correções de bugs** — A menos que a correção revele que a documentação existente estava incorreta
- **Melhorias de desempenho** — A menos que sejam perceptíveis ao usuário (por exemplo, tempo de inicialização)
- **Alterações nos testes** — Nunca documente os testes
- **Alterações na CI/ferramentas** — Infraestrutura interna

---

## Decisões sobre páginas versus seções

### Crie uma nova página quando:

- Apresentando um **recurso importante** com vários subrecursos (por exemplo, integração com o Git, modo Vim)
- O tema requer **exemplos detalhados de configuração**
- Os usuários pesquisariam **pelo nome** (por exemplo, “terminal Zed”, “trechos Zed”)
- É uma **nova categoria** (por exemplo, um novo tipo de provedor de IA)

### Adicione a uma página existente quando:

- Adicionar uma **configuração** a um recurso que já possui uma página
- Adicionar um **atalho de teclado** a um recurso existente
- A alteração é uma **pequena melhoria** na funcionalidade existente
- É uma **opção de configuração** para um recurso já existente

### Exemplos

| Alterar                               | Ação                                 |
| ------------------------------------ | -------------------------------------- |
| Novo recurso “Stash” para o Git          | Adicionar seção ao arquivo `git.md`                |
| Novo recurso “Desenvolvimento Remoto”  | Crie o arquivo `remote-development.md`         |
| Nova configuração `git.inline_blame.delay` | Adicionar à seção de configuração do Git existente     |
| Novo provedor de IA (por exemplo, “Ollama”)     | Adicionar seção ao arquivo `llm-providers.md`      |
| Nova categoria de ferramentas para agentes              | Possivelmente uma nova página, dependendo do escopo |

---

## Estrutura do documento

### Páginas preliminares

Cada página de documentação precisa de um frontmatter em YAML:

```yaml
---
title: Feature Name - Zed
description: One sentence describing what this page covers. Used in search results.
---
```

- `title`: Nome do recurso, opcionalmente com o sufixo “- Zed” para SEO
- `description`: Resumo conciso para mecanismos de busca e visualizações de links
- Mantenha os valores do frontmatter como entradas simples de uma única linha no formato `chave: valor` (sem
  valores com várias linhas, sem aspas) para compatibilidade com o pós-processador de documentação

#### Diretrizes de SEO para o frontmatter

- Escolha uma palavra-chave principal ou frase de intenção para cada página
- Escreva valores exclusivos para o `title` que indiquem claramente o tema da página e o público-alvo
  intenção; procure manter entre 50 e 60 caracteres
- Escreva valores para o campo `description` que resumam o que o leitor pode fazer na página;
  procure manter entre 140 e 160 caracteres
- Use a palavra-chave principal de forma natural no `title` e no corpo da página pelo menos uma vez
  (geralmente no parágrafo inicial); evite o excesso de palavras-chave

### Ordenação das seções

1. **Título** (`# Nome do recurso`) — Claro e de fácil leitura
2. **Parágrafo inicial** — O que é isso e por que você usaria (1-2 frases)
3. **Introdução / Como usar** — Como acessar ou ativar
4. **Funcionalidades essenciais** — Principais recursos e fluxos de trabalho
5. **Configuração** — Opções, com exemplos em JSON
6. **Atribuições de teclas / Ações** — Tabelas de referência
7. **Veja também** — Links para documentos relacionados

### Profundidade da seção

- Use `##` para as seções principais
- Use `###` para subseções
- Evite usar `####`, a menos que seja absolutamente necessário — se precisar, considere reestruturar o texto

### IDs de âncora

Adicione IDs de âncora explícitos às seções para as quais os usuários possam criar links diretamente:

```markdown
## Getting Started {#getting-started}

### Configuring Models {#configuring-models}
```

Use IDs de âncora quando:

- A seção é um ponto de referência comum
- Você precisa de um link estável que não seja desativado caso o texto do título seja alterado
- O título contém caracteres especiais que gerariam âncoras geradas automaticamente com aparência desagradável

---

## Convenções de formatação

### Formatação de código

Use a tag `code` embutida para:

- Nomes das configurações: `vim_mode`, `buffer_font_size`
- Atribuições de teclas: `cmd-shift-p`, `ctrl-w h`
- Comandos: `:w`, `:q`
- Caminhos dos arquivos: `~/.config/zed/settings.json`
- Nomes das ações: `git::Commit`
- Valores: `true`, `false`, `"eager"`

### Referências sobre ações e atalhos de teclado

Use a sintaxe especial do Zed para renderização dinâmica:

- {#action git::Commit} — Exibe o nome da ação
- {#kb git::Commit} — Exibe a combinação de teclas para essa ação

Isso garante que as combinações de teclas permaneçam corretas caso as configurações padrão sejam alteradas.

### Exemplos de JSON

Sempre use a anotação `[settings]` ou `[keymap]`:

```json [settings]
{
  "vim_mode": true
}
```

```json [keymap]
{
  "context": "Editor",
  "bindings": {
    "ctrl-s": "workspace::Save"
  }
}
```

### Tabelas

Use tabelas para:

- Listas de referência de ações/atalhos de teclado
- Opções de configuração com descrições
- Comparação de recursos

Mantenha as tabelas fáceis de ler — evite textos muito longos nas células das tabelas.

### Parágrafos

- Mantenha os parágrafos curtos (no máximo 2 a 3 frases)
- Uma ideia por parágrafo
- Use listas com marcadores para vários itens relacionados

### Pronomes

Minimize o uso de pronomes vagos como “isso”, “este” e “aquele”. Repita o substantivo para que os leitores saibam exatamente a que você está se referindo.

**Ruim:**

> A API gera um token após a autenticação. Ele deve ser armazenado com segurança.

**Bom:**

> A API gera um token após a autenticação. O token deve ser armazenado com segurança.

Isso aumenta a clareza tanto para os leitores humanos quanto para os sistemas de IA que analisam a documentação.

### Notas explicativas

Use citações em bloco para dicas, observações e avisos:

```markdown
> **Note:** This feature requires signing in.

> **Tip:** Hold `cmd` when submitting to automatically follow the agent.

> **Warning:** This action cannot be undone.
```

### Notas específicas da versão

Quando o comportamento variar de acordo com a versão, seja explícito:

```markdown
> **Note:** In Zed v0.224.0 and above, tool approval is controlled by `agent.tool_permissions.default`.
```

Inclua o número da versão e o que mudou. Isso ajuda os usuários de versões mais antigas a entender por que o comportamento é diferente.

---

## Reticulamento

### Links internos

Crie links para outros documentos usando caminhos relativos:

- `[Modo Vim](./vim.md)`
- `[Introdução rápida à IA](./ai/quick-start.md)`

### Links externos

- Inclua links para as páginas do `zed.dev` quando for o caso
- Inclua links para a documentação de origem (por exemplo, Tree-sitter, servidores de linguagem) ao explicar as integrações

### Seções “Veja também”

Inclua links relacionados no final das páginas, quando for útil:

```markdown
## See also

- [Agent Panel](./agent-panel.md): Agentic editing with file read/write
- [Inline Assistant](./inline-assistant.md): Prompt-driven code transformations
```

### Diretrizes de links para SEO

- Certifique-se de que cada página seja acessível a partir de pelo menos uma outra página da documentação (sem páginas órfãs)
  páginas)
- Para páginas que não sejam de referência, inclua pelo menos três links internos para documentos relacionados
  sempre que possível
- As páginas de referência (por exemplo, `docs/src/reference/*`) podem conter menos links quando
  links adicionais aumentariam o ruído
- Adicione links para documentos intimamente relacionados, sempre que eles ajudem os usuários a concluir a próxima tarefa
- Use um texto descritivo para o link que informe aos usuários o que eles encontrarão na página vinculada
  página
- Para páginas de destaque que tenham uma página de marketing correspondente, inclua um
  Link de marketing `zed.dev`, além dos links para a documentação

---

## Documentação específica para cada idioma

Os documentos de idiomas na pasta `src/languages/` seguem uma estrutura consistente:

1. Nome da língua e breve descrição
2. Instalação/configuração (se necessário)
3. Configuração do servidor de idiomas
4. Configuração de formatação
5. Configurações específicas do idioma
6. Limitações conhecidas (se houver)

Mantenha a documentação da linguagem focada na configuração específica do Zed, e não em tutoriais gerais sobre a linguagem.

---

## Documentação de configurações

Ao documentar as configurações:

1. **Mostrar primeiro a abordagem do Editor de Configurações (interface do usuário)** — A maioria das configurações é compatível com a interface do usuário
2. **Em seguida, exiba o JSON** como “ou adicione ao seu arquivo de configurações:”
3. **Indique a chave de configuração** na formatação do código
4. **Descreva o que ele faz** em uma frase
5. **Mostrar o tipo e o valor padrão**, caso não seja óbvio
6. **Forneça um exemplo completo em JSON**

Exemplo:

> Configure o recurso “inline blame” em Configurações ({#kb zed::OpenSettings}) pesquisando por “inline blame” ou adicione ao seu arquivo de configurações:
>
> ```json [configurações]```
> {
>   "git": {
>     "inline_blame": {
>       "ativado": false
>     }
>   }
> }
> ```

Para configurações exclusivamente em JSON (tipos complexos sem suporte na interface do usuário), mencione isso e inclua um link para as instruções:

> Adicione o seguinte ao seu arquivo de configurações ([como editar](./configuring-zed.md#settings-files)):

### Locais dos arquivos de configuração

- **macOS/Linux:** `~/.config/zed/settings.json`
- **Windows:** `%AppData%\Zed\settings.json`

### Locais dos arquivos de mapeamento de teclas

- **macOS/Linux:** `~/.config/zed/keymap.json`
- **Windows:** `%AppData%\Zed\keymap.json`

---

## Terminologia

Utilize uma terminologia consistente ao longo de todo o texto:

| Uso             | Em vez de                             |
| --------------- | -------------------------------------- |
| pasta          | diretório                              |
| projeto         | espaço de trabalho                              |
| Editor de configurações | interface de usuário de configurações                            |
| paleta de comandos | barra de comandos                            |
| painel           | barra lateral (seja específico: “Painel do Projeto”) |

---

## Requisitos de formatação

Toda a documentação deve ser formatada com o **Prettier** (largura de linha de 80 caracteres):

```sh
cd docs && npx prettier --check src/
```

Antes que qualquer alteração na documentação seja considerada concluída:

1. Execute o Prettier para formatar: `cd docs && npx prettier --write src/`
2. Verifique se o comando é executado corretamente: `cd docs && npx prettier --check src/`

---

## Lista de verificação de qualidade

Antes de finalizar a documentação:

- [ ] A seção introdutória inclui `title` e `description`
- [ ] A página possui uma palavra-chave principal ou frase de intenção bem definida
- [ ] A palavra-chave principal aparece naturalmente no corpo da página (sem excesso de palavras-chave)
- [ ] O parágrafo inicial explica o que é e por quê
- [ ] As configurações mostram primeiro a interface do usuário e, em seguida, os exemplos em JSON
- [ ] As ações utilizam a sintaxe `{#action ...}` e `{#kb ...}`
- [ ] Todas as ações são documentadas (a integridade é importante)
- [ ] IDs de âncora nas seções que provavelmente serão vinculadas
- [ ] Notas sobre versões em que o comportamento varia de acordo com a versão
- [ ] Não há páginas órfãs (com links vindos de outros lugares)
- [ ] As páginas que não são de referência incluem pelo menos três links úteis para documentos internos
- [ ] As principais páginas de destaque incluem um link de marketing relevante do `zed.dev`
- [ ] Passa na verificação de formatação do Prettier
- [ ] Atende aos critérios da rubrica de voz da marca (consulte `brand-voice/rubric.md`)

---

## Exemplos de referência

Consulte `../.doc-examples/` para ver exemplos selecionados de recursos bem documentados. Use-os como modelos ao redigir nova documentação.

---

## Referência

Para regras específicas de automação (restrições de segurança, classificação de alterações, formatos de saída), consulte `docs/AGENTS.md`.
