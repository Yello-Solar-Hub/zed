---
título: Como localizar e navegar pelo código - Zed
descrição: Navegue pela sua base de código no Zed com o localizador de arquivos, a pesquisa por projeto, a função “Ir para a definição”, a pesquisa por símbolos e a paleta de comandos.
---

# Como encontrar e navegar

O Zed oferece várias maneiras de navegar rapidamente pelo seu código. Aqui está uma visão geral das principais ferramentas de navegação.

## Paleta de comandos

A Paleta de Comandos ({#kb command_palette::Toggle}) é a sua porta de acesso a praticamente tudo no Zed. Digite alguns caracteres para filtrar os comandos e, em seguida, pressione Enter para executá-los.

[Saiba mais sobre a Paleta de Comandos →](./command-palette.md)

## Painel do Projeto

O Painel de Projetos ({#kb project_panel::ToggleFocus}) exibe uma visualização em árvore dos arquivos e diretórios do seu espaço de trabalho. Navegue, crie, renomeie, mova e exclua arquivos sem sair do editor. Ele também exibe o status do Git e diagnósticos de forma rápida e prática.

[Saiba mais sobre o Painel de Projetos →](./project-panel.md)

## Localizador de arquivos

Abra qualquer arquivo do seu projeto com {#kb file_finder::Toggle}. Digite parte do nome do arquivo ou do caminho para filtrar os resultados.

## Pesquisa de Projetos

Faça uma busca em todos os arquivos com {#kb pane::DeploySearch}. Digite a consulta no campo de busca e, em seguida, pressione Enter para executar a busca.

Os resultados são exibidos em um [multibuffer](./multibuffers.md), permitindo que você edite as correspondências diretamente no local.

## Ir para a definição

Vá até o local onde um símbolo está definido com {#kb editor::GoToDefinition} (ou `Cmd+Clique` / `Ctrl+Clique`). Se houver várias definições, elas serão abertas em um multibuffer.

## Ir para o símbolo

- **Arquivo atual:** {#kb outline::Toggle} abre um esboço dos símbolos no arquivo ativo
- **Projeto inteiro:** {#kb project_symbols::Toggle} pesquisa símbolos em todos os arquivos

## Painel de Esboço

O Painel de Estrutura ({#kb outline_panel::ToggleFocus}) exibe uma visualização em árvore persistente dos símbolos no arquivo atual. Ele é especialmente útil com [multibuffers](./multibuffers.md) para navegar pelos resultados de pesquisa ou diagnósticos.

[Saiba mais sobre o Painel de Esboço →](./outline-panel.md)

## Seletor de abas

Alterne rapidamente entre as abas abertas com {#kb tab_switcher::Toggle}. As abas são classificadas por ordem de uso recente — mantenha pressionada a tecla Ctrl e pressione Tab para percorrê-las.

[Saiba mais sobre o Seletor de Abas →](./tab-switcher.md)

## Referência rápida

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
| Painel do Projeto     | {#kb project_panel::ToggleFocus} |
