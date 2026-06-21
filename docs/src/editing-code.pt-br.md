---
título: Edição de código no Zed
descrição: Principais recursos de edição de código do Zed, incluindo multicursor, refatoração, ações de código e integração com servidor de linguagem.
---

# Edição de código

O Zed oferece ferramentas para ajudá-lo a escrever e modificar código com eficiência. Esta seção aborda os principais recursos de edição que funcionam em conjunto com o seu servidor de linguagem.

## O que há nesta seção

- **[Autocompletamento de código](./completions.md)** — Autocompletamento a partir de servidores de linguagem e previsões de edição baseadas em IA
- **[Snippets](./snippets.md)** — Insira modelos de código reutilizáveis com tabulações
- **[Formatação e verificação de código](./configuring-languages.md#formatting-and-linting)** — Configure a formatação automática do código e a integração com o linter
- **[Diagnósticos e correções rápidas](./diagnostics.md)** — Visualize erros e avisos e aplique correções a partir do seu servidor de linguagem
- **[Multibuffers](./multibuffers.md)** — Edite vários arquivos simultaneamente com vários cursores

## Como esses recursos funcionam em conjunto

Ao editar código, o Zed combina informações provenientes de várias fontes:

1. **Servidores de linguagem** oferecem sugestões de autocompletar, diagnósticos e correções rápidas com base nos tipos e na estrutura do seu projeto
2. **A função de edição de sugestões** sugere alterações que envolvem vários caracteres ou várias linhas à medida que você digita
3. **Multibuffers** permitem aplicar alterações em vários arquivos de uma só vez

Por exemplo, você poderia:

- Renomeie uma função usando a refatoração de renomeação do seu servidor de linguagem
- Veja os resultados em um multibuffer que exibe todos os arquivos afetados
- Use vários cursores para fazer edições adicionais em todos os locais
- Receba um feedback de diagnóstico imediato caso ocorra algum problema

## Artigos relacionados

- [Recursos de IA](./ai/overview.md) — Edição baseada em agentes, transformações de código inline e autocompletamento de código com IA
- [Configuração de idiomas](./configuring-languages.md) — Configure servidores de idiomas para o seu projeto
- [Atribuições de teclas](./key-bindings.md) — Personalize os atalhos de teclado para comandos de edição
