<!--
  EXEMPLO DE REFERÊNCIA: Documentação de referência

  Este exemplo ilustra a documentação de conteúdo de API/referência, como ferramentas,
  ações ou outros itens enumeráveis.

  Principais padrões a serem observados:
  - IDs de âncora em categorias e itens individuais para links diretos
  - O parágrafo inicial explica o que são e onde são utilizados
  - Organizados em categorias lógicas
  - Cada item possui uma descrição clara e prática
  - Links para documentos de configuração relacionados
  - Seção “Veja também” para tópicos relacionados
-->

---

título: Ferramentas para agentes de IA - Zed
descrição: Ferramentas integradas para o agente de IA do Zed, incluindo edição de arquivos, pesquisa de código, comandos de terminal, pesquisa na web e diagnósticos.

---

# Ferramentas

O agente integrado do Zed tem acesso a essas ferramentas para ler, pesquisar e editar sua base de código. Essas ferramentas são utilizadas no [Painel do Agente](./agent-panel.md) durante as conversas com agentes de IA.

Você pode configurar permissões para ações das ferramentas, incluindo situações em que elas são aprovadas automaticamente, negadas automaticamente ou exigem sua confirmação caso a caso. Consulte [Permissões das ferramentas](./tool-permissions.md) para ver a lista de ferramentas sujeitas a permissões e mais detalhes.

Para adicionar ferramentas personalizadas além das integradas, consulte [servidores MCP](./mcp.md).

## Ferramentas de leitura e pesquisa {#read-search-tools}

### `diagnósticos` {#diagnósticos}

Obtém erros e avisos relativos a um arquivo específico ou a todo o projeto, o que é útil após fazer edições para determinar se são necessárias alterações adicionais.
Quando um caminho é fornecido, exibe todos os diagnósticos relativos a esse arquivo específico.
Quando nenhum caminho é fornecido, exibe um resumo do número de erros e avisos para todos os arquivos do projeto.

### `fetch` {#fetch}

Recupera uma URL e retorna o conteúdo no formato Markdown. Útil para fornecer documentos como contexto.

### `find_path` {#find-path}

Localiza rapidamente arquivos por meio da correspondência com padrões glob (como `**/*.js`), retornando os caminhos dos arquivos correspondentes em ordem alfabética.

### `grep` {#grep}

Pesquisa o conteúdo dos arquivos em todo o projeto usando expressões regulares, sendo a opção preferida para localizar símbolos no código sem saber os caminhos exatos dos arquivos.

### `list_directory` {#list-directory}

Lista os arquivos e diretórios em um determinado caminho, fornecendo uma visão geral do conteúdo do sistema de arquivos.

### `read_file` {#read-file}

Lê o conteúdo de um arquivo especificado no projeto, permitindo o acesso ao conteúdo do arquivo.

### `search_web` {#search-web}

Pesquisa informações na web, apresentando resultados com trechos e links de páginas relevantes, o que é útil para acessar informações em tempo real.

## Ferramentas de edição {#edit-tools}

### `copy_path` {#copy-path}

Copia um arquivo ou diretório de forma recursiva no projeto, o que é mais eficiente do que ler e gravar arquivos manualmente ao duplicar conteúdo.

### `create_directory` {#create-directory}

Cria um novo diretório no caminho especificado dentro do projeto, criando todos os diretórios superiores necessários (semelhante ao comando `mkdir -p`).

### `delete_path` {#delete-path}

Exclui um arquivo ou diretório (incluindo o conteúdo de forma recursiva) no caminho especificado e confirma a exclusão.

### `edit_file` {#edit-file}

Edita arquivos substituindo texto específico por um novo conteúdo.

### `move_path` {#move-path}

Mova ou renomeie um arquivo ou diretório no projeto, realizando uma renomeação caso apenas o nome do arquivo seja diferente.

### `write_file` {#write-file}

Cria um novo arquivo ou sobrescreve um arquivo existente com um conteúdo totalmente novo.

### `terminal` {#terminal}

Executa comandos do shell e retorna a saída combinada, criando um novo processo do shell para cada invocação.

## Outras ferramentas {#other-tools}

### `spawn_agent` {#spawn-agent}

Cria um subagente com sua própria janela de contexto para executar uma tarefa delegada. Útil para realizar investigações paralelas, concluir tarefas independentes ou realizar pesquisas nas quais apenas o resultado importa. Cada subagente tem acesso às mesmas ferramentas que o agente pai.

## Veja também {#see-also}

- [Painel do Agente](./agent-panel.md) — Onde você interage com os agentes de IA
- [Permissões de ferramentas](./tool-permissions.md) — Configure quais ferramentas exigem aprovação
- [Servidores MCP](./mcp.md) — Adicione ferramentas personalizadas por meio do Protocolo de Contexto de Modelo
