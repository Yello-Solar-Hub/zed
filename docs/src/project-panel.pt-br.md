---
título: Painel do Projeto - Zed
descrição: Navegue pelos arquivos e diretórios da área de trabalho com o painel de projetos do Zed. Crie, renomeie, envie para a lixeira e exclua arquivos e diretórios.
---

# Painel do Projeto

O painel do projeto exibe uma visualização em árvore dos arquivos e diretórios do seu espaço de trabalho.
Alterne a exibição com {#action project_panel::ToggleFocus} ({#kb
project_panel::ToggleFocus}), ou clique no botão **Painel do Projeto** no
barra de status.

![Painel do projeto](https://images.zed.dev/docs/project-panel/panel.png)

## Navegando

Use as setas do teclado para navegar pelas opções. {#kb
project_panel::ExpandSelectedEntry} expande um diretório e {#kb
project_panel::CollapseSelectedEntry} a oculta. {#kb
project_panel::CollapseAllEntries} oculta todos os diretórios de uma só vez. Pressione {#kb
project_panel::Open} ou clique para visualizar um arquivo selecionado, sem precisar nomeá-lo
aba permanente. Ao editar o arquivo ou clicar duas vezes nele, ele passa a ser uma aba permanente.

### Revelação automática

Por padrão, ao alternar entre arquivos no editor, o arquivo em questão será automaticamente destacado no
painel do projeto e role a tela até que ele fique visível. Isso pode ser desativado com o
Configuração `project_panel.auto_reveal_entries`.

### Rolagem fixa

Quando `project_panel.sticky_scroll` está ativado (por padrão), os diretórios superiores ficam fixados na parte superior
do painel à medida que você rola a tela, para que você sempre saiba em qual diretório está.

![Painel do projeto: Rolagem fixa ativada](https://images.zed.dev/docs/project-panel/sticky-scroll-true.png)

![Painel do projeto: Rolagem fixa desativada](https://images.zed.dev/docs/project-panel/sticky-scroll-false.png)

### Dobragem de diretórios

Quando `project_panel.auto_fold_dirs` está ativado (por padrão), cadeias de diretórios, cada uma contendo um
os diretórios filhos únicos são agrupados em uma única linha (por exemplo,
`src/utils/helpers` em vez de três níveis separados). Clique com o botão direito do mouse em uma pasta recolhida
diretório e selecione **Desdobrar diretório** para expandir a cadeia, ou **Dobrar
Diretório** para recolhê-lo novamente.

![Painel do projeto: Opção “Dobrar diretórios automaticamente” ativada](https://images.zed.dev/docs/project-panel/auto-fold-dirs-true.png)

![Painel do projeto: Desativação da dobragem automática de diretórios](https://images.zed.dev/docs/project-panel/auto-fold-dirs-false.png)

## Seleção de vários itens

Mantenha a tecla `shift` pressionada enquanto pressiona as setas para cima/para baixo para marcar entradas adicionais.
A maioria das operações com arquivos, como cortar, copiar, enviar para a lixeira, excluir e arrastar, se aplicam ao arquivo inteiro
conjunto de entradas marcadas.

Quando exatamente dois arquivos estiverem marcados, {#action project_panel::CompareMarkedFiles}
({#kb project_panel::CompareMarkedFiles}) abre uma visualização de diferenças comparando esses arquivos.

![Painel do projeto: Comparar arquivos marcados](https://images.zed.dev/docs/project-panel/compare-marked-files.png)

## Operações com arquivos

Clique com o botão direito do mouse em uma entrada para ver a lista completa de operações disponíveis ou use o
atalhos de teclado a seguir.

### Criação de arquivos e diretórios

- {#action project_panel::NewFile} ({#kb project_panel::NewFile}) cria um novo
  arquivo dentro do diretório selecionado.
- {#action project_panel::NewDirectory} ({#kb project_panel::NewDirectory})
  cria um novo diretório.

Um editor embutido é exibido para que você possa digitar o nome. Pressione `enter` para
Confirme ou pressione a tecla `Esc` para cancelar.

### Renomeação

Pressione {#kb project_panel::Rename} para renomear a entrada selecionada. O nome do arquivo
O nome do tronco já está pré-selecionado para que você possa digitar um novo nome sem alterar acidentalmente
a extensão. Pressione `enter` para confirmar ou `escape` para
cancelar.

### Recortar, copiar e colar

- {#action project_panel::Cut} ({#kb project_panel::Cut}) marca as entradas para
  mudança.
- {#action project_panel::Copy} ({#kb project_panel::Copy}) marca as entradas para
  copiando.
- {#action project_panel::Paste} ({#kb project_panel::Paste}) os coloca no
  diretório selecionado.

Quando a colagem causaria um conflito de nomes, o Zed acrescenta o sufixo “cópia” (por exemplo,
`arquivo copy.txt`, `arquivo copy 2.txt`). Se um único arquivo for colado com um
sufixo, o editor de renomeação é aberto automaticamente para que você possa ajustar o nome.

### Duplicado

{#action project_panel::Duplicate} ({#kb project_panel::Duplicate}) copia e
cola as entradas selecionadas de uma só vez.

### Lixeira e Excluir

- {#action project_panel::Lixeira} ({#kb project_panel::Lixeira}) move as entradas para
  a lixeira do sistema.
- {#action project_panel::Delete} ({#kb project_panel::Delete}) permanentemente
  exclui entradas.

Ambas as ações exibem uma janela de confirmação com a lista dos arquivos afetados. Se algum dos
Se os arquivos tiverem alterações não salvas, o prompt exibirá um aviso.

### Arrastar e soltar

Arraste os itens dentro do painel para movê-los. Mantenha a tecla `alt` pressionada ao soltá-los para copiá-los
em vez de mover. Você também pode arrastar arquivos da pasta do seu sistema operacional
gerenciador no painel do projeto para copiá-los para o projeto. É possível arrastar e soltar
pode ser desativado com a configuração `project_panel.drag_and_drop`.

## Integração com o Git

Quando `project_panel.git_status` está ativado (por padrão), os nomes dos arquivos e diretórios são destacados
para refletir seu status no Git — modificado, adicionado, excluído, não rastreado ou em conflito.

Definir `project_panel.git_status_indicator` como `true` (desativado por padrão) adiciona um ícone com uma letra ao lado
ao lado de cada nome: **M** (modificado), **A** (adicionado), **D** (excluído), **U**
(não rastreado) ou **!** (conflito).

![Painel do projeto: Integração com o Git](https://images.zed.dev/docs/project-panel/git-status.png)

Use {#action project_panel::SelectNextGitEntry} e {#action
project_panel::SelectPrevGitEntry} para alternar entre os arquivos rastreados com
alterações não confirmadas. O menu do botão direito do mouse também oferece a opção **Restaurar arquivo** para
desfazer as alterações e clicar em **Exibir histórico do arquivo** para consultar o registro de commits de um arquivo.

## Diagnósticos

A configuração `project_panel.show_diagnostics` controla se os erros e avisos
Os indicadores aparecem nos ícones de arquivos e pastas. Defina como `"all"` para ver ambos os erros
e avisos, `"errors"` apenas para erros ou `"off"` para ocultá-los. Diagnósticos
propaga-se para cima — se um arquivo localizado em um nível mais profundo de um diretório apresentar um erro, seu ancestral
As pastas também exibem um indicador.

Ative `project_panel.diagnostic_badges` (desativado por padrão) para exibir erros e avisos numéricos
números ao lado de cada entrada. Use {#action project_panel::SelectNextDiagnostic} e
{#action project_panel::SelectPrevDiagnostic} para navegar entre os arquivos que
possuem recursos de diagnóstico.

Consulte também [Diagnósticos e soluções rápidas](./diagnostics.md) para conhecer as configurações de diagnóstico do editor e das abas.

## Filtragem e classificação

### Ocultando arquivos

- `project_panel.hide_gitignore` oculta os arquivos correspondentes ao arquivo `.gitignore`. Alternar
  isso com {#action project_panel::ToggleHideGitIgnore}.
- `project_panel.hide_hidden` oculta arquivos dotfiles e outras entradas ocultas. Alternar
  com {#action project_panel::ToggleHideHidden}.

### Classificação

A configuração `project_panel.sort_mode` controla o agrupamento:

- `"directories_first"` (padrão) — os diretórios aparecem antes dos arquivos em cada
  nível.
- `"files_first"` — os arquivos aparecem antes dos diretórios.
- `"misto"` — diretórios e arquivos são classificados juntos.

A configuração `project_panel.sort_order` controla a comparação de nomes:

- `"default"` — ordenação natural, sem distinção entre maiúsculas e minúsculas (`file2` antes de `file10`).
- `"upper"` — os nomes em maiúsculas são agrupados primeiro, seguidos dos que estão em minúsculas.
- `"lower"` — os nomes em letras minúsculas são agrupados primeiro, seguidos dos nomes em letras maiúsculas.
- `"unicode"` — ordem dos pontos de código Unicode brutos, sem conversão de maiúsculas e minúsculas.

## Outras ações

- {#action project_panel::RevealInFileManager} ({#kb
  project_panel::RevealInFileManager}) exibe a entrada selecionada no Finder /
  Explorador de Arquivos.
- {#action project_panel::NewSearchInDirectory} ({#kb
  project_panel::NewSearchInDirectory}) abre uma pesquisa no projeto restrita ao
  diretório selecionado.
- {#action project_panel::RemoveFromProject} remove a pasta raiz do espaço de trabalho
  do projeto.
