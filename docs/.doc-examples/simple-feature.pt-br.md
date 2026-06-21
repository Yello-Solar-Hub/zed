<!--
  EXEMPLO DE REFERÊNCIA: Documentação de recursos simples / Visão geral

  Este exemplo mostra uma documentação concisa com uma visão geral do recurso
  ou guia de navegação.

  Principais padrões a serem observados:
  - IDs de âncora em todas as seções
  - Um breve parágrafo introdutório explica o que este texto aborda
  - Cada seção é concisa (1 a 2 parágrafos, no máximo)
  - Links para documentação detalhada sobre cada recurso
  - Tabela de referência rápida no final
  - Utiliza a sintaxe {#kb ...} para todas as combinações de teclas
-->

---

título: Como localizar e navegar pelo código - Zed
descrição: Navegue pela sua base de código no Zed usando o localizador de arquivos, a pesquisa por projeto, a função “Ir para a definição”, a pesquisa por símbolos e a paleta de comandos.

---

# Como encontrar e navegar

O Zed oferece várias maneiras de navegar rapidamente pelo seu código. Aqui está uma visão geral das principais ferramentas de navegação.

## Paleta de comandos {#command-palette}

A Paleta de Comandos ({#kb command_palette::Toggle}) é a sua porta de acesso a praticamente tudo no Zed. Digite alguns caracteres para filtrar os comandos e, em seguida, pressione Enter para executá-los.

[Saiba mais sobre a Paleta de Comandos →](./command-palette.md)

## Localizador de Arquivos {#file-finder}

Abra qualquer arquivo do seu projeto com {#kb file_finder::Toggle}. Digite parte do nome do arquivo ou do caminho para filtrar os resultados.

## Pesquisa de projetos {#project-search}

Faça uma busca em todos os arquivos com {#kb pane::DeploySearch}. Os resultados aparecem em um [multibuffer](./multibuffers.md), permitindo que você edite as correspondências diretamente no local.

## Ir para a definição {#go-to-definition}

Vá até o local onde um símbolo está definido com {#kb editor::GoToDefinition} (ou `Cmd+Clique` / `Ctrl+Clique`). Se houver várias definições, elas serão abertas em um multibuffer.

## Ir para o símbolo {#go-to-symbol}

- **Arquivo atual:** {#kb outline::Toggle} abre um esboço dos símbolos no arquivo ativo
- **Projeto inteiro:** {#kb project_symbols::Toggle} pesquisa símbolos em todos os arquivos

## Painel de Esboço {#outline-panel}

O Painel de Estrutura ({#kb outline_panel::ToggleFocus}) exibe uma visualização em árvore persistente dos símbolos no arquivo atual. Ele é especialmente útil com [multibuffers](./multibuffers.md) para navegar pelos resultados de pesquisa ou diagnósticos.

[Saiba mais sobre o Painel de Esboço →](./outline-panel.md)

## Seletor de abas {#tab-switcher}

Alterne rapidamente entre as abas abertas com {#kb tab_switcher::Toggle}. As abas são classificadas por ordem de uso recente — mantenha pressionada a tecla Ctrl e pressione Tab para percorrê-las.

[Saiba mais sobre o Seletor de Abas →](./tab-switcher.md)

## Referência rápida {#quick-reference}

| Tarefa              | Atribuição de teclas                       |
| ----------------- | -------------------------------- |
| Paleta de comandos   | {#kb command_palette::Alternar}    |
| Abrir arquivo         | {#kb file_finder::Alternar}        |
| Pesquisa de projetos    | {#kb pane::DeploySearch}         |
| Ir para a definição  | {#kb editor::Ir para a definição}     |
| Encontre referências   | {#kb editor::EncontrarTodasAsReferências}  |
| Símbolo no arquivo    | {#kb outline::Alternar}            |
| Símbolo no projeto | {#kb project_symbols::Toggle}    |
| Painel de Esboço     | {#kb outline_panel::ToggleFocus} |
| Seletor de abas      | {#kb tab_switcher::Toggle}       |

## Veja também {#see-also}

- [Paleta de comandos](./command-palette.md) — Documentação completa da paleta de comandos
- [Multibuffers](./multibuffers.md) — Edite vários arquivos ao mesmo tempo
- [Painel de Esboço](./outline-panel.md) — Visualização da árvore de símbolos
- [Seletor de abas](./tab-switcher.md) — Alternar entre os arquivos abertos
