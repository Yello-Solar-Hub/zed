---
título: Contêineres de desenvolvimento - Zed
descrição: Abra projetos em contêineres de desenvolvimento com o Zed. Ambientes de desenvolvimento reproduzíveis por meio da configuração do arquivo devcontainer.json.
---

# Contêineres de desenvolvimento

Os Dev Containers oferecem um ambiente de desenvolvimento consistente e reproduzível, definindo as dependências, ferramentas e configurações do seu projeto em uma configuração de contêiner.

Se o seu repositório incluir um arquivo `.devcontainer/devcontainer.json`, o Zed poderá abrir um projeto dentro de um contêiner de desenvolvimento.

## Requisitos

- O Docker ou o Podman devem estar instalados e disponíveis no seu `PATH`. Se você usar o `podman`, é necessário definir a configuração `use_podman` no arquivo settings.json do Zed como true.
- Seu projeto deve conter um diretório/arquivo `.devcontainer/devcontainer.json`.

## Utilização do Dev Containers no Zed

### Sugestão automática

Ao abrir um projeto que contenha o diretório/arquivo `.devcontainer/devcontainer.json`, o Zed exibirá uma mensagem perguntando se você deseja abrir o projeto dentro do contêiner de desenvolvimento. Ao selecionar “Abrir no contêiner”, o programa irá:

1. Crie a imagem do contêiner de desenvolvimento (se necessário).
2. Inicie o contêiner.
3. Reabra o projeto vinculado ao ambiente de contêiner.

### Abertura manual

Se você fechar a janela de prompt ou quiser reabrir o projeto dentro de um contêiner mais tarde, pode usar a paleta de comandos do Zed para executar o comando “Project: Open Remote” e selecionar a opção de abrir o projeto em um contêiner de desenvolvimento.
Como alternativa, você pode acessar o modal “Projetos Remotos” (por meio do atalho {#kb projects::OpenRemote}) e selecionar a opção “Conectar Contêiner de Desenvolvimento”.

## Editando a configuração do contêiner de desenvolvimento

Se você modificar o arquivo `.devcontainer/devcontainer.json`, o Zed, no momento, não recompila nem recarrega o contêiner automaticamente. Após alterar a configuração:

- Interrompa ou encerre manualmente o contêiner existente (por exemplo, usando o comando `docker kill <contêiner>`).
- Reabra o projeto no contêiner.

## Trabalhando em um contêiner de desenvolvimento

Uma vez conectado, o Zed opera dentro do ambiente de contêineres para tarefas, terminais e servidores de idiomas.
Os arquivos são vinculados do seu espaço de trabalho ao contêiner, de acordo com a especificação do contêiner de desenvolvimento.

## Extensões

Você pode especificar extensões no arquivo `.devcontainer/devcontainer.json`, no campo “customizations”, da seguinte maneira:

```json
{
  ...
  "customizations": {
    "zed": {
      "extensions": ["vue", "ruby"],
    },
    "vscode": {
      ...
    },
    "codespaces": {
      ...
    },
  }
}
```

Observe que as extensões são carregadas para a sessão do Zed; portanto, elas também estarão disponíveis nas suas instâncias locais do Zed.

## Limitações conhecidas

> **Observação:** Esse recurso ainda está em desenvolvimento.

- **Alterações na configuração:** As atualizações no arquivo `devcontainer.json` não acionam recompilações ou recargas automáticas; os contêineres devem ser reiniciados manualmente.

## Veja também

- [Desenvolvimento remoto](./remote-development.md) para se conectar a servidores remotos via SSH.
- [Tarefas](./tasks.md) para executar comandos no terminal integrado.
