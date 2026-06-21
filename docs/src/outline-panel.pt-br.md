---
título: Painel de Esboço - Zed
descrição: Navegue pela estrutura do código com o painel de esboço do Zed. Visualize símbolos, salte para definições e explore os esboços dos arquivos.
---

# Painel de Esboço

Além do esboço modal (`cmd-shift-o`), o Zed oferece um painel de esboço. O painel de esboço pode ser ativado por meio do comando `cmd-shift-b` ({#action outline_panel::ToggleFocus} na paleta de comandos) ou clicando no botão `Painel de Esboço` na barra de status.

Ao visualizar um buffer “singleton” (ou seja, um único arquivo em uma aba), o painel de esboço funciona de maneira semelhante ao do modal de esboço — ele exibe o esboço dos símbolos do buffer atual. Cada entrada de símbolo mostra seu prefixo de tipo (como “struct”, “fn”, “mod”, “impl”) junto com o nome do símbolo, ajudando você a identificar rapidamente que tipo de símbolo está sendo exibido. Clicar em uma entrada permite que você salte para a seção associada no arquivo. A visualização do esboço também rolará automaticamente até a seção associada à posição atual do cursor dentro do arquivo.

![Como usar o painel de esboço em um buffer único](https://zed.dev/img/outline-panel/singleton.png)

## Uso com múltiplos buffers

O painel de contornos realmente se destaca quando usado com múltiplos buffers. Aqui estão alguns exemplos de sua versatilidade:

### Resultados da pesquisa do projeto

Tenha uma visão geral dos resultados da pesquisa em todo o seu projeto.

![Como usar o painel de esboço em uma pesquisa de projeto com vários buffers](https://zed.dev/img/outline-panel/project-search.png)

### Diagnóstico de Projetos

Veja um resumo de todos os erros e avisos relatados pelo servidor de linguagem.

![Como usar o painel de esboço ao visualizar o diagnóstico do projeto em modo multibuffer](https://zed.dev/img/outline-panel/project-diagnostics.png)

### Encontrar todas as referências

Navegue rapidamente por todas as referências ao usar a ação {#action editor::FindAllReferences}.

![Como usar o painel de esboço ao visualizar o multibuffer `encontrar todas as referências`](https://zed.dev/img/outline-panel/find-all-references.png)

A visualização em esboço oferece uma excelente maneira de navegar rapidamente até partes específicas do seu código e ajuda a manter o contexto ao trabalhar com grandes conjuntos de resultados em múltiplos buffers.
