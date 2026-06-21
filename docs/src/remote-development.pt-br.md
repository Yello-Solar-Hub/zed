---
título: Desenvolvimento remoto em Zed — Fluxos de trabalho com SSH
descrição: Use o desenvolvimento remoto no Zed para editar código via SSH com o desempenho de uma interface de usuário local, terminais remotos, servidores de linguagem e tarefas.
---

# Desenvolvimento remoto

O Desenvolvimento Remoto permite que você edite código em um servidor remoto enquanto executa o Zed localmente. A interface do usuário permanece ágil, pois é executada no seu computador, enquanto os servidores de linguagem, as tarefas e os terminais são executados no servidor.

Para os fluxos de trabalho do dia a dia, combine o desenvolvimento remoto com [Tarefas](./tasks.md),
[Terminal](./terminal.md) e [Depurador](./debugger.md).

## Visão geral

O desenvolvimento remoto requer dois computadores: sua máquina local, que executa a interface do usuário do Zed, e o servidor remoto, que executa um servidor Zed sem interface gráfica. Os dois se comunicam via SSH; portanto, você precisará conseguir acessar o servidor remoto por SSH a partir da sua máquina local para usar esse recurso.

![Visão geral da arquitetura do Zed Remote Development](https://zed.dev/img/remote-development/diagram.png)

No seu computador local, o Zed executa sua interface de usuário, se comunica com modelos de linguagem, usa o Tree-sitter para analisar e destacar a sintaxe do código e armazena alterações não salvas e projetos recentes. O código-fonte, os servidores de linguagem, as tarefas e o terminal são executados no servidor remoto. [Os recursos de IA](./ai/overview.md) funcionam em sessões remotas, incluindo o Painel do Agente e o Assistente Inline.

> **Observação:** Na versão original do desenvolvimento remoto, o tráfego era encaminhado pelos servidores do Zed. A partir da versão v0.157 do Zed, esse modo não está mais disponível.

## Configuração

1. Baixe e instale a versão mais recente do [Zed](https://zed.dev/releases). É necessário ter, no mínimo, o Zed v0.159.
1. Use {#kb projects::OpenRemote} para abrir a caixa de diálogo “Projetos remotos”.
1. Clique em “Conectar novo servidor” e insira o comando que você usa para acessar o servidor via SSH. Consulte [Opções SSH compatíveis](#supported-ssh-options) para saber quais opções podem ser passadas.
1. Seu computador local tentará se conectar ao servidor remoto usando o binário `ssh` que está em seu caminho. Supondo que a conexão seja bem-sucedida, o Zed fará o download do servidor no host remoto e o iniciará.
1. Assim que o servidor Zed estiver em execução, você será solicitado a escolher um caminho a ser aberto no servidor remoto.
   > **Observação:** Atualmente, o Zed não lida muito bem com a abertura de diretórios muito grandes (por exemplo, `/` ou `~`, que podem conter mais de 100.000 arquivos). Estamos trabalhando para melhorar isso, mas, enquanto isso, sugerimos abrir apenas projetos específicos ou subpastas de repositórios únicos muito grandes.

Em casos simples, nos quais não é necessário especificar nenhum argumento SSH, você pode executar `zed ssh://[<usuário>@]<host>[:<porta>]/<caminho>` para abrir diretamente uma pasta ou um arquivo remoto. A CLI também aceita o formato semelhante ao scp: `zed ssh://[<usuário>@]<host>:~/projeto` ou `zed ssh://[<usuário>@]<host>:/caminho/absoluto`. Se você quiser criar um link direto para um projeto SSH, use um link no formato: `zed://ssh/[<usuário>@]<host>[:<porta>]/<caminho>`.

## Plataformas compatíveis

A máquina remota deve ser capaz de executar o servidor do Zed. As plataformas a seguir devem funcionar, mas observe que não testamos exaustivamente todas as distribuições do Linux:

- macOS Catalina ou versão posterior (Intel ou Apple Silicon)
- Linux (x86_64 ou arm64; ainda não oferecemos suporte a plataformas de 32 bits)
- O Windows ainda não é compatível como servidor remoto, mas pode ser usado como máquina local para se conectar a servidores remotos.

## Configuração

A lista de servidores remotos está armazenada no seu arquivo de configurações {#kb zed::OpenSettings}. Você pode editar essa lista usando a caixa de diálogo Projetos Remotos {#kb projects::OpenRemote}, o que oferece certa robustez — por exemplo, ela verifica se a conexão pode ser estabelecida antes de gravá-la no arquivo de configurações.

```json [settings]
{
  "ssh_connections": [
    {
      "host": "192.168.1.10",
      "projects": [{ "paths": ["~/code/zed/zed"] }]
    }
  ]
}
```

O Zed chama o comando `ssh` que está no seu caminho e, portanto, herdará qualquer configuração que você tenha no arquivo `~/.ssh/config` para o host em questão. Dito isso, se você precisar substituir alguma configuração, pode definir as seguintes opções adicionais em cada conexão:

```json [settings]
{
  "ssh_connections": [
    {
      "host": "192.168.1.10",
      "projects": [{ "paths": ["~/code/zed/zed"] }],
      // any argument to pass to the ssh master process
      "args": ["-i", "~/.ssh/work_id_file"],
      "port": 22, // defaults to 22
      // defaults to your username on your local machine
      "username": "me"
    }
  ]
}
```

Existem duas opções adicionais específicas do Zed por conexão: `upload_binary_over_ssh` e `nickname`:

```json [settings]
{
  "ssh_connections": [
    {
      "host": "192.168.1.10",
      "projects": [{ "paths": ["~/code/zed/zed"] }],
      // by default Zed will download the server binary from the internet on the remote.
      // When this is true, it'll be downloaded to your laptop and uploaded over SSH.
      // This is useful when your remote server has restricted internet access.
      "upload_binary_over_ssh": true,
      // Shown in the Zed UI to help distinguish multiple hosts.
      "nickname": "lil-linux"
    }
  ]
}
```

Se você usar a linha de comando para abrir uma conexão com um host digitando `zed ssh://192.168.1.10/~/.vimrc`, as opções adicionais serão lidas do seu arquivo de configurações, identificando a primeira conexão que corresponder ao host, nome de usuário e porta da URL na linha de comando.

Além disso, vale a pena observar que, embora seja possível passar uma senha na linha de comando `zed ssh://user:password@host/~`, não oferecemos suporte à gravação de senhas no arquivo de configurações. Se você estiver se conectando repetidamente ao mesmo host, deve configurar a autenticação por chave.

## Desenvolvimento remoto no Windows (SSH)

O Zed no Windows oferece suporte ao acesso remoto via SSH e solicitará as credenciais quando necessário.

Caso encontre problemas de autenticação, verifique se o seu agente de chaves SSH está em execução (por exemplo, o ssh-agent ou o agente do seu cliente Git) e se o ssh.exe está no PATH.

### Solução de problemas do SSH no Windows

Quando for solicitado que você insira suas credenciais, use a caixa de diálogo gráfica do askpass. Caso ela não apareça, verifique se há conflitos com o gerenciador de credenciais e se as solicitações da interface gráfica não estão sendo bloqueadas pelo seu terminal.

## Suporte à WSL

O Zed oferece suporte à abertura nativa de pastas dentro do WSL no Windows.

### Abrindo uma pasta local no WSL

Para abrir uma pasta local dentro de um contêiner do WSL, use a ação `projects: open in wsl` e selecione a pasta que deseja abrir. Será exibida uma lista das distribuições do WSL disponíveis para abrir a pasta.

### Abrindo uma pasta que já está no WSL

Para abrir uma pasta que já esteja localizada dentro de um contêiner do WSL, use a ação `projects: open wsl` e selecione a distribuição do WSL. A distribuição será adicionada à janela `Projetos Remotos`, onde você poderá abrir a pasta.

## Redirecionamento de porta

Se você quiser se conectar às portas do seu servidor remoto a partir do seu computador local, é possível configurar o encaminhamento de portas no seu arquivo de configurações. Isso é particularmente útil para o desenvolvimento de sites, pois permite que você carregue o site no navegador enquanto trabalha.

```json [settings]
{
  "ssh_connections": [
    {
      "host": "192.168.1.10",
      "port_forwards": [{ "local_port": 8080, "remote_port": 80 }]
    }
  ]
}
```

Isso fará com que as solicitações da sua máquina local para `localhost:8080` sejam encaminhadas para a porta 80 da máquina remota. Nos bastidores, isso utiliza o argumento `-L` do ssh.

Por padrão, essas portas estão vinculadas ao localhost; portanto, outros computadores na mesma rede que sua máquina de desenvolvimento não podem acessá-las. Você pode configurar o local_host para se vincular a uma interface diferente; por exemplo, 0.0.0.0 se vinculará a todas as interfaces locais.

```json [settings]
{
  "ssh_connections": [
    {
      "host": "192.168.1.10",
      "port_forwards": [
        {
          "local_port": 8080,
          "remote_port": 80,
          "local_host": "0.0.0.0"
        }
      ]
    }
  ]
}
```

Essas portas também são configuradas por padrão para a interface `localhost` no host remoto. Se for necessário alterar isso, você também pode definir o host remoto:

```json [settings]
{
  "ssh_connections": [
    {
      "host": "192.168.1.10",
      "port_forwards": [
        {
          "local_port": 8080,
          "remote_port": 80,
          "remote_host": "docker-host"
        }
      ]
    }
  ]
}
```

## Configurações do Zed

Ao abrir um projeto remoto, há três locais de configuração relevantes:

- As configurações locais do Zed (em `~/.zed/settings.json` no macOS ou `~/.config/zed/settings.json` no Linux) na sua máquina local.
- As configurações do servidor Zed (no mesmo local) no servidor remoto.
- As configurações do projeto (no arquivo `.zed/settings.json` ou `.editorconfig` do seu projeto)

Tanto o Zed local quanto o Zed do servidor leem as configurações do projeto, mas não têm acesso ao arquivo de configurações principal um do outro.

A escolha do arquivo de configurações a ser usado depende do tipo de configuração que você deseja fazer:

- As configurações do projeto devem ser usadas para aspectos que afetam o projeto: configurações de recuo, qual formatador ou servidor de linguagem usar, etc.
- As configurações do servidor devem ser usadas para itens que afetam o servidor: caminhos para servidores de idiomas, configurações de proxy, etc.
- As configurações locais devem ser usadas para aspectos que afetam a interface do usuário: tamanho da fonte, etc.

Além disso, quaisquer extensões que você tenha instalado localmente serão propagadas para o servidor remoto. Isso significa que os servidores de idiomas, etc., funcionarão corretamente.

## Configuração do proxy

O servidor remoto não utilizará a configuração de proxy do seu computador local, pois eles podem estar sujeitos a políticas de rede diferentes. Se o servidor remoto exigir um proxy para acessar a internet, você deverá configurá-lo diretamente no próprio servidor remoto.

Na maioria dos casos, seu servidor remoto já terá variáveis de ambiente de proxy configuradas. O Zed as utilizará automaticamente ao baixar servidores de idiomas, ao se comunicar com modelos LLM, etc.

Se necessário, você pode definir essas variáveis de ambiente na configuração do shell do servidor (por exemplo, `~/.bashrc`):

```bash
export http_proxy="http://proxy.example.com:8080"
export https_proxy="http://proxy.example.com:8080"
export no_proxy="localhost,127.0.0.1"
```

Como alternativa, você pode configurar o proxy no arquivo `~/.config/zed/settings.json` (Linux) ou `~/.zed/settings.json` (macOS) da máquina remota:

```json
{
  "proxy": "http://proxy.example.com:8080"
}
```

Consulte a [documentação sobre proxy](./reference/all-settings.md#network-proxy) para conhecer os tipos de proxy compatíveis e as opções de configuração adicionais.

## Inicializando o servidor remoto

Depois que você definir as opções do SSH, o Zed executa o comando `ssh` na sua máquina local para estabelecer uma conexão com o ControlMaster usando as opções fornecidas.

Quaisquer solicitações que o SSH precisar serão exibidas na interface do usuário, para que você possa verificar as chaves do host, digitar senhas de chave etc.

Assim que a conexão principal for estabelecida, o Zed verificará se o arquivo binário do servidor remoto está presente em `~/.zed_server` no servidor remoto e se a versão dele corresponde à versão atual do Zed que você está usando.

Se ele não estiver lá ou se a versão não corresponder, o Zed tentará baixar a versão mais recente. Por padrão, ele fará o download diretamente de `https://zed.dev`, mas se você definir: `{"upload_binary_over_ssh":true}` nas configurações desse servidor, ele baixará o arquivo binário para sua máquina local e, em seguida, o enviará para o servidor remoto.

Se você quiser manter o binário do servidor por conta própria, é possível. Você pode baixar nossas versões pré-compiladas no [GitHub](https://github.com/zed-industries/zed/releases) ou [compilar a sua própria](https://zed.dev/docs/development) com o comando `cargo build -p remote_server --release`. Se fizer isso, você deverá enviá-lo para `~/.zed_server/zed-remote-server-{RELEASE_CHANNEL}- {VERSION}` no servidor, por exemplo, `~/.zed_server/zed-remote-server-stable-0.217.3+stable.105.80433cb239e868271457ac376673a5f75bc4adb1`. A versão deve corresponder exatamente à versão do próprio Zed que você está usando.

## Manutenção da conexão SSH

Assim que o servidor for inicializado, o Zed criará novas conexões SSH (reutilizando o ControlMaster existente) para executar o servidor de desenvolvimento remoto.

Cada conexão tenta executar o servidor de desenvolvimento no modo proxy. Esse modo inicia o daemon caso ele não esteja em execução e se reconecta a ele caso já esteja. Dessa forma, quando sua conexão cair e for reiniciada, você poderá continuar trabalhando sem interrupção.

Caso a reconexão falhe, o daemon não será reutilizado. Dito isso, as alterações não salvas são, por padrão, armazenadas localmente, para que você não perca o trabalho. Você sempre poderá se reconectar ao projeto posteriormente, e o Zed restaurará as alterações não salvas.

Se você estiver enfrentando problemas de conexão, poderá ver mais informações no log do Zed com o comando `cmd-shift-p Abrir Log`. Caso encontre algo inesperado, por favor, abra um [issue no GitHub](https://github.com/zed-industries/zed/issues/new) ou entre em contato nos fóruns #support no [Discord](https://zed.dev/community-links).

## Opções SSH compatíveis

Nos bastidores, o Zed recorre ao binário `ssh` para se conectar ao servidor remoto. Criamos um mestre de controle SSH por projeto e, em seguida, o utilizamos para multiplexar as conexões SSH para o próprio protocolo Zed, quaisquer terminais que você abrir e tarefas que você executar. Lemos as configurações do seu arquivo de configuração do SSH, mas, se você quiser especificar opções adicionais para o mestre de controle SSH, pode configurar o Zed para defini-las.

Ao digitar na caixa de diálogo “Conectar novo servidor”, você pode usar aspas no estilo bash para passar opções que contenham um espaço. Depois de criar um servidor, ele será adicionado à matriz `"ssh_connections": []` no seu arquivo de configurações. Você pode editar o arquivo de configurações diretamente para fazer alterações nas conexões SSH.

Opções disponíveis:

- `-p` / `-l` — são equivalentes a passar a porta e o nome de usuário na string do host.
- `-L` / `-R` for port forwarding
- `-i` - to use a specific key file
- `-o` - to set custom options
- `-J` / `-w` - to proxy the SSH connection
- `-F` for specifying an `ssh_config`
- And also... `-4`, `-6`, `-A`, `-B`, `-C`, `-D`, `-I`, `-K`, `-P`, `-X`, `-Y`, `-a`, `-b`, `-c`, `-i`, `-k`, `-l`, `-m`, `-o`, `-p`, `-w`, `-x`, `-y`

Note that we deliberately disallow some options (for example `-t` or `-T`) that Zed will set for you.

## Known Limitations

- You can't open files from the remote Terminal by typing the `zed` command.

## See also

- [Running & Testing](./running-testing.md): Run tasks, terminal commands, and
  debugger sessions while you work remotely.
- [Git Worktrees](./git.md#git-worktrees): Create and switch between linked
  Git worktrees. Zed supports the worktree picker in remote projects when the
  remote connection is active.
- [Configuring Zed](./configuring-zed.md): Manage shared and project settings,
  including `.zed/settings.json`.
- [Agent Panel](./ai/agent-panel.md): Use AI workflows in remote projects.
- [Remote Development on zed.dev](https://zed.dev/remote-development): Product
  overview and release updates.
