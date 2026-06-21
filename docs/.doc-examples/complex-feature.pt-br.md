<!--
  EXEMPLO DE REFERÊNCIA: Documentação de recursos complexos

  Este exemplo mostra a documentação de um recurso importante com vários
  subfuncionalidades, opções de configuração e uma tabela de referência de ações.

  Principais padrões a serem observados:
  - IDs de âncora em todas as seções principais para links diretos estáveis
  - O parágrafo inicial explica o que é e por quê
  - Seção de configuração com exemplos em JSON
  - Formatação correta das notas explicativas (> **Nota:** e > **Dica:**)
  - Tabela de referência abrangente das medidas, no final
  - Utiliza a sintaxe {#action ...} e {#kb ...} em todo o texto
-->

---

descrição: O Zed é um editor de texto que oferece suporte a diversos recursos do Git
título: Documentação sobre a integração do Zed Editor com o Git

---

# Git

O Zed possui suporte integrado ao Git, o que permite gerenciar o controle de versões sem sair do editor. O Painel do Git mostra o estado da sua árvore de trabalho, a área de preparação e as informações do branch. As alterações feitas na linha de comando são refletidas imediatamente no Zed.

Para operações que o Zed não suporta nativamente, você pode usar o terminal integrado.

## Painel do Git {#git-panel}

O Painel do Git mostra o estado da sua árvore de trabalho e da área de preparação do Git.

Você pode abrir o Painel do Git usando {#action git_panel::ToggleFocus} ou clicando no ícone do Git na barra de status.

No painel, você pode ver rapidamente o status do seu projeto: qual repositório e qual branch estão ativos, quais arquivos foram alterados e o status atual de preparação de cada arquivo.

O Zed monitora seu repositório para que as alterações feitas na linha de comando sejam refletidas instantaneamente.

### Configuração {#configuration}

Abra o Editor de Configurações (`Cmd+,` no macOS, `Ctrl+,` no Linux/Windows) para personalizar o comportamento do Git. As configurações estão distribuídas em duas páginas:

- **Painéis > Painel do Git**: Posição do painel, visualização em árvore ou plana, estilo de exibição do status
- **Controle de versão**: Indicadores na margem, atribuição de culpa em linha, estilos de trechos

#### Movendo o painel do Git

Por padrão, o Painel do Git fica ancorado à esquerda. Acesse **Painéis > Painel do Git** e altere a opção **Ancoragem do Painel do Git** para movê-lo para a direita ou para a parte inferior.

#### Alternar para a visualização em árvore

Por padrão, o Painel Git exibe uma lista simples dos arquivos alterados. Para ver os arquivos organizados por hierarquia de pastas, selecione a opção **Visualização em árvore** no menu de contexto do painel ou ative-a em **Painéis > Painel Git**.

#### Atribuição de culpa em linha

O Zed exibe as informações de blame do Git na linha atual. Para desativar esse recurso ou adicionar um atraso antes que ele apareça, acesse **Controle de versão > Blame do Git em linha**.

#### Ocultando os indicadores da calha

As barras coloridas na margem lateral que indicam linhas adicionadas, modificadas e excluídas podem ser ocultadas. Acesse **Controle de versão > Git Gutter** e defina **Visibilidade** como “Ocultar”.

#### Comprimento da linha da mensagem de commit

O Zed limita as mensagens de commit a 72 caracteres (uma convenção do Git). Para alterar isso, procure por “Git Commit” em Configurações e ajuste o **Comprimento preferencial da linha**.

## Diferenças entre projetos {#project-diff}

Você pode ver todas as alterações registradas pelo Git no Zed abrindo o Project Diff ({#kb git::Diff}), acessível por meio da ação {#action git::Diff} na Paleta de Comandos ou no Painel do Git.

Todas as alterações exibidas no Project Diff funcionam exatamente da mesma forma que qualquer outro multibuffer: são trechos editáveis de arquivos.

Você pode colocar ou retirar cada bloco, bem como um arquivo inteiro, clicando nos botões da barra de abas ou usando os atalhos de teclado correspondentes.

### Destaque de diferenças entre palavras {#word-diff}

Por padrão, o Zed destaca as palavras alteradas nas linhas modificadas para facilitar a identificação exata das alterações. Para desativar essa função globalmente, abra o Editor de Configurações e acesse **Idiomas e Ferramentas > Diversos**; em seguida, desative a opção **Comparação de Palavras Ativada**.

Para desativar a comparação de palavras apenas para determinados idiomas, adicione o seguinte ao seu arquivo settings.json:

```json
{
  "languages": {
    "Markdown": {
      "word_diff_enabled": false
    }
  }
}
```

## Histórico do arquivo {#file-history}

O Histórico do arquivo mostra o histórico de commits de um arquivo específico. Cada entrada exibe o autor do commit, a data e hora e a mensagem. Ao selecionar um commit, é aberta uma visualização de diferenças filtrada para mostrar apenas as alterações feitas nesse arquivo nesse commit.

Para visualizar o Histórico de Arquivos:

- Clique com o botão direito do mouse em um arquivo no Painel do Projeto e selecione “Exibir histórico do arquivo”
- Clique com o botão direito do mouse em um arquivo no Painel do Git e selecione “Exibir histórico do arquivo”
- Clique com o botão direito do mouse em uma guia do editor e selecione “Exibir histórico do arquivo”
- Use a Paleta de Comandos e procure por “histórico de arquivos”

## Fetch, Push e Pull {#fetch-push-pull}

Faça o fetch, o push ou o pull do seu repositório Git no Zed usando os botões disponíveis no Painel do Git ou na Paleta de Comandos, consultando as respectivas ações: {#action git::Fetch}, {#action git::Push} e {#action git::Pull}.

### Configuração de envio {#push-configuration}

O Zed respeita a configuração de push do Git. Ao fazer um push, o Zed verifica os seguintes itens, nesta ordem:

1. `pushRemote` configurado para o branch atual
2. `remote.pushDefault` na sua configuração do Git
3. O controle remoto de rastreamento da filial

Isso corresponde ao comportamento padrão do Git; portanto, se você tiver configurado `pushRemote` ou `pushDefault` no seu `.gitconfig` ou por meio do `git config`, o Zed utilizará essas configurações.

## Controles remotos {#remotes}

Quando seu repositório tiver vários remotos, o Zed exibe um seletor de remotos no Painel do Git. Clique no botão “remoto” ao lado de “push/pull” para escolher qual remoto usar para essa operação.

## Fluxo de trabalho de preparação {#staging-workflow}

O Zed possui dois fluxos de trabalho principais para preparação, utilizando o Project Diff ou o painel diretamente.

### Usando o Project Diff {#staging-project-diff}

Na visualização “Project Diff”, você pode se concentrar em cada bloco e prepará-los individualmente clicando nos botões da barra de abas ou usando o atalho de teclado {#action git::StageAndNext} ({#kb git::StageAndNext}).

Da mesma forma, marque todos os blocos de código ao mesmo tempo com o atalho de teclado {#action git::StageAll} ({#kb git::StageAll}) e, em seguida, faça o commit imediatamente com {#action git::Commit} ({#kb git::Commit}).

### Como usar o Git Panel {#staging-git-panel}

No painel, basta digitar uma mensagem de commit e clicar no botão “Commit” ou executar {#action git::Commit}. Isso colocará automaticamente todos os arquivos rastreados na área de preparação (indicados por um `[·]` na caixa de seleção da entrada) e fará o commit deles.

As entradas podem ser preparadas para envio usando a caixa de seleção de cada entrada individualmente. Todas as alterações podem ser preparadas para envio usando o botão na parte superior do painel ou {#action git::StageAll}.

## Confirmando {#confirmando}

O Zed oferece duas áreas de texto para commit:

1. A primeira opção está disponível bem na parte inferior do Painel do Git. Ao pressionar {#kb git::Commit}, todas as suas alterações preparadas são confirmadas imediatamente.
2. A segunda opção está disponível por meio da ação {#action git::ExpandCommitEditor} ou pressionando a tecla {#kb git::ExpandCommitEditor} quando o foco estiver na área de texto de commit do Painel do Git.

### Desfazer um commit {#undo-commit}

Assim que você fizer um commit no Zed, no Painel do Git, você verá uma barra logo abaixo da área de texto do commit, que exibirá o commit enviado recentemente.
Lá, você pode usar o botão “Uncommit”, que executa o comando `git reset HEADˆ--soft`.

### Configurando o comprimento da linha de commit

Por padrão, o Zed define o comprimento da linha de commit como `72`, mas isso pode ser configurado no seu arquivo `settings.json` local.

Veja mais informações sobre como definir o `preferred-line-length` na seção [Configuração](#configuration).

## Gestão de Filiais {#branch-management}

### Criação e alternância entre ramificações {#create-switch-branches}

Crie um novo branch usando {#action git::Branch} ou mude para um branch existente usando {#action git::Switch} ou {#action git::CheckoutBranch}.

### Excluindo ramificações {#delete-branches}

Para excluir um branch, abra o seletor de branches com {#action git::Switch}, localize o branch que deseja excluir e use a opção de exclusão. O Zed solicitará uma confirmação antes da exclusão para evitar a perda acidental de dados.

> **Observação:** Não é possível excluir o branch que você está acessando no momento. Primeiro, mude para um branch diferente.

## Conflitos de mesclagem {#merge-conflicts}

Quando você se depara com conflitos de mesclagem após uma mesclagem, um rebase ou um pull, o Zed destaca as regiões conflitantes em seus arquivos e exibe botões de resolução acima de cada conflito.

### Visualizando conflitos {#viewing-conflicts}

Os arquivos em conflito aparecem no Painel do Git com um ícone de aviso. Você também pode ver os conflitos na visualização “Project Diff”, onde cada região em conflito é destacada:

- As alterações em relação ao seu branch atual estão destacadas em verde
- As alterações provenientes do branch de origem estão destacadas em azul

### Resolução de conflitos {#resolving-conflicts}

Cada conflito exibe três botões:

- **Use [nome-do-ramo]**: Mantenha as alterações de um ramo (mostra o nome real do ramo, como “main”)
- **Usar [outro-ramo]**: Manter as alterações do outro ramo (como “feature-branch”)
- **Use os dois**: mantenha os dois conjuntos de alterações, colocando as alterações do seu branch em primeiro lugar

Clique em um botão para resolver esse conflito. Os marcadores de conflito são removidos e substituídos pelo conteúdo escolhido por você. Depois de resolver todos os conflitos em um arquivo, marque-o para envio e faça o commit para concluir a fusão.

> **Dica:** Para conflitos complexos que exigem edição manual, você pode editar o arquivo diretamente. Remova os marcadores de conflito (`<<<<<<<`, `=======`, `>>>>>>>`) e mantenha o conteúdo desejado.

## Armazenando {#stashing}

O `Git stash` permite salvar temporariamente suas alterações não confirmadas e reverter o diretório de trabalho para um estado limpo. Isso é particularmente útil quando você precisa alternar rapidamente entre branches ou baixar atualizações sem confirmar um trabalho incompleto.

### Criando pastas {#creating-stashes}

Para armazenar todas as suas alterações atuais, use a ação {#action git::StashAll}. Isso salvará tanto as alterações marcadas quanto as não marcadas em uma nova entrada do stash e limpará seu diretório de trabalho.

### Gerenciamento de Stashes {#managing-stashes}

O Zed oferece um seletor de stashes acessível por meio de {#action git::ViewStash} ou pelo menu de opções adicionais do Painel do Git. No seletor de stashes, você pode:

- **Ver lista de stashes**: Navegue por todos os seus stashes salvos, com suas descrições e horários de criação
- **Diffs abertos**: Veja exatamente quais alterações estão armazenadas em cada stash
- **Aplicar stashes**: Aplica as alterações do stash ao seu diretório de trabalho, mantendo a entrada do stash
- **Extrair itens da pilha**: Aplicar as alterações na pilha e remover o item da pilha da lista
- **Excluir itens do stash**: Exclua itens indesejados do stash sem aplicá-los

### Operações do Quick Stash {#quick-stash}

Para agilizar os fluxos de trabalho, o Zed oferece ações diretas para trabalhar com o stash mais recente:

- **Aplicar o stash mais recente**: Use {#action git::StashApply} para aplicar o stash mais recente sem removê-lo
- **Recuperar o stash mais recente**: Use {#action git::StashPop} para aplicar e remover o stash mais recente

### Visualização de diferenças do Stash {#stash-diff-view}

Para visualizar o conteúdo de um stash, selecione-o no seletor de stash e pressione {#kb stash_picker::ShowStashItem}. Na visualização de diferenças, você pode usar estas combinações de teclas:

| Ação                               | Atribuição de teclas                   |
| ------------------------------------ | ---------------------------- |
| Aplicar o produto                          | {#kb git::ApplyCurrentStash} |
| Kit de maquiagem (aplicar e remover)         | {#kb git::PopCurrentStash}   |
| Descartar o produto (remover sem aplicar) | {#kb git::DropCurrentStash}  |

## Suporte à IA no Git {#ai-support}

Atualmente, o Zed oferece suporte à geração de mensagens de commit com base em LLM.
Você pode pedir à IA para gerar uma mensagem de commit selecionando o editor de mensagens no Git Panel e clicando no ícone de lápis no canto inferior esquerdo ou usando o atalho de teclado {#action git::GenerateCommitMessage} ({#kb git::GenerateCommitMessage}).

> **Observação:** É necessário ter um provedor de LLM configurado, seja por meio de suas próprias chaves de API, seja por meio dos modelos de IA hospedados pela Zed.
> Acesse [Guia rápido de IA](./ai/quick-start.md) para saber como fazer isso.

Você pode especificar o modelo de sua preferência para uso definindo a configuração do agente `commit_message_model`.
Consulte [Modelos específicos para cada recurso](./ai/agent-settings.md#feature-specific-models) para obter mais informações.

```json [settings]
{
  "agent": {
    "commit_message_model": {
      "provider": "anthropic",
      "model": "claude-3-5-haiku"
    }
  }
}
```

Para adicionar instruções personalizadas que se apliquem apenas à geração de mensagens de commit, use o campo `commit_message_instructions` nas configurações do seu agente:

```json [settings]
{
  "agent": {
    "commit_message_instructions": "Use the Conventional Commits format: <type>(<scope>): <description>."
  }
}
```

Essas instruções são enviadas ao modelo além de quaisquer arquivos de instruções, como `.rules` ou `AGENTS.md`. Para adicionar instruções que se apliquem tanto às mensagens de commit quanto ao agente de maneira mais ampla, use o arquivo global `AGENTS.md`, localizado em `~/.config/zed/AGENTS.md` no macOS e no Linux, e em `%APPDATA%\Zed\AGENTS.md` no Windows.

## Integrações com o Git {#git-integrations}

O Zed se integra a serviços populares de hospedagem do Git para garantir que os hashes dos commits do Git e as referências a Issues, Pull Requests e Merge Requests se tornem links clicáveis.

Atualmente, o Zed oferece suporte a links para as versões hospedadas de
[GitHub](https://github.com),
[GitLab](https://gitlab.com),
[Bitbucket](https://bitbucket.org),
[SourceHut](https://sr.ht) e
[Codeberg](https://codeberg.org).

### Instâncias auto-hospedadas {#self-hosted}

O Zed identifica automaticamente os provedores de hospedagem do Git verificando se há palavras-chave na sua URL remota do Git. Por exemplo, se a sua URL auto-hospedada contiver `gitlab`, `gitea` ou outros nomes de provedores reconhecidos, o Zed registrará automaticamente esse provedor de hospedagem sem a necessidade de qualquer configuração.

No entanto, se a URL da sua instância do Git auto-hospedada não contiver palavras-chave identificadoras, você pode configurar manualmente o Zed para criar links clicáveis para a sua instância, adicionando uma configuração `git_hosting_providers` para que os hashes de commit e os links permanentes apontem para o seu domínio:

```json [settings]
{
  "git_hosting_providers": [
    {
      "provider": "gitlab",
      "name": "Corp GitLab",
      "base_url": "https://git.example.corp"
    }
  ]
}
```

O campo `provider` especifica qual tipo de serviço de hospedagem você está usando. Os valores aceitos para `provider` são `github`, `gitlab`, `bitbucket`, `gitea`, `forgejo` e `sourcehut`. O campo `name` é opcional e serve como nome de exibição para sua instância, enquanto `base_url` é a URL raiz do seu servidor auto-hospedado.

Você pode configurar vários provedores personalizados caso trabalhe com várias instâncias auto-hospedadas.

### Links permanentes {#permalinks}

O Zed também possui um recurso chamado “Copiar Permalink” para criar um link permanente para um trecho de código no seu serviço de hospedagem Git.
Esses links são úteis para compartilhar uma linha específica ou um intervalo de linhas em um arquivo em um commit específico.
Execute esta ação por meio da [Paleta de Comandos](./getting-started.md#command-palette) (procure por `permalink`),
criando [atalhos de teclado personalizados](key-bindings.md#custom-key-bindings) para o
Ações `editor::CopyPermalinkToLine` ou `editor::OpenPermalinkToLine`
ou simplesmente clicando com o botão direito do mouse e selecionando `Copiar permalink` com a(s) linha(s) selecionada(s) no seu editor.

## Atalhos de teclado do Diff Hunk {#diff-hunks}

Ao visualizar arquivos com alterações, o Zed exibe trechos de comparação que podem ser expandidos ou recolhidos para uma análise detalhada:

- **Expandir todos os trechos de diferença**: {#action editor::ExpandAllDiffHunks} ({#kb editor::ExpandAllDiffHunks})
- **Ocultar todos os trechos de diferenças**: Pressione `Escape` (associado a {#action editor::Cancel})
- **Alternar entre os trechos de diferenças selecionados**: {#action editor::ToggleSelectedDiffHunks} ({#kb editor::ToggleSelectedDiffHunks})
- **Navegar entre trechos**: {#action editor::GoToHunk} e {#action editor::GoToPreviousHunk}

> **Dica:** A tecla `Escape` é a maneira mais rápida de recolher todos os trechos de comparação expandidos e voltar à visão geral das suas alterações.

## Referência de ação {#action-reference}

| Ação                                    | Atribuição de teclas                            |
| ----------------------------------------- | ------------------------------------- |
| {#action git::Add}                        | {#kb git::Adicionar}                        |
| {#action git::StageAll}                   | {#kb git::StageAll}                   |
| {#action git::UnstageAll}                 | {#kb git::UnstageAll}                 |
| {#action git::ToggleStaged}               | {#kb git::ToggleStaged}               |
| {#action git::StageAndNext}               | {#kb git::StageAndNext}               |
| {#action git::UnstageAndNext}             | {#kb git::UnstageAndNext}             |
| {#action git::Commit}                     | {#kb git::Commit}                     |
| {#action git::ExpandCommitEditor}         | {#kb git::ExpandCommitEditor}         |
| {#action git::Push}                       | {#kb git::Push}                       |
| {#action git::ForcePush}                  | {#kb git::ForcePush}                  |
| {#action git::Pull}                       | {#kb git::Pull}                       |
| {#action git::PullRebase}                 | {#kb git::PullRebase}                 |
| {#action git::Fetch}                      | {#kb git::Fetch}                      |
| {#action git::Diff}                       | {#kb git::Diff}                       |
| {#action git::Restore}                    | {#kb git::Restore}                    |
| {#action git::RestoreFile}                | {#kb git::RestoreFile}                |
| {#action git::Branch}                     | {#kb git::Branch}                     |
| {#action git::Switch}                     | {#kb git::Switch}                     |
| {#action git::CheckoutBranch}             | {#kb git::CheckoutBranch}             |
| {#action git::Blame}                      | {#kb git::Blame}                      |
| {#action git::StashAll}                   | {#kb git::StashAll}                   |
| {#action git::StashPop}                   | {#kb git::StashPop}                   |
| {#action git::StashApply}                 | {#kb git::StashApply}                 |
| {#action git::ViewStash}                  | {#kb git::ViewStash}                  |
| {#action editor::ToggleGitBlameInline}    | {#kb editor::ToggleGitBlameInline}    |
| {#action editor::ExpandAllDiffHunks}      | {#kb editor::ExpandAllDiffHunks}      |
| {#action editor::Alternar entre trechos selecionados} | {#kb editor::Alternar entre trechos selecionados} |

> **Observação:** Nem todas as ações possuem atalhos de teclado padrão, mas podem ser configuradas [personalizando seu mapa de teclas](./key-bindings.md#user-keymaps).

## Configuração da CLI do Git {#cli-configuration}

Se você também quiser usar o Zed como seu [editor de mensagens de commit do Git](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration#_core_editor) ao fazer commits pela linha de comando, pode usar `zed --wait`:

```sh
git config --global core.editor "zed --wait"
```

Ou adicione o seguinte ao seu ambiente de shell (em `~/.zshrc`, `~/.bashrc`, etc.):

```sh
export GIT_EDITOR="zed --wait"
```
