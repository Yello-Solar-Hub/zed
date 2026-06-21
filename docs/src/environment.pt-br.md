---
título: Variáveis de ambiente - Zed
descrição: Como o Zed detecta e utiliza variáveis de ambiente. Integração com o shell, suporte ao dotenv e solução de problemas.
---

# Variáveis de ambiente

_**Observação**: O que se segue se aplica apenas ao Zed 0.152.0 e versões posteriores._

Vários recursos do Zed são afetados por variáveis de ambiente:

- [Tarefas](./tasks.md)
- [Terminal integrado](./terminal.md)
- Pesquisa de servidores de idiomas
- Servidores de idiomas

Para aproveitar ao máximo esses recursos, é importante entender de onde o Zed obtém as variáveis de ambiente e como as utiliza.

## De onde o Zed obtém suas variáveis de ambiente?

A forma como o Zed é iniciado determina quais variáveis de ambiente ele pode usar. Isso inclui a execução a partir do Dock do macOS, de um gerenciador de janelas do Linux ou da CLI do `zed`.

### Executado a partir da CLI

Se o Zed for iniciado pela CLI (`zed`), ele herdará as variáveis de ambiente da sessão do shell em que está sendo executado.

Isso significa que, se você fizer

```
$ export MY_ENV_VAR=hello
$ zed .
```

A variável de ambiente `MY_ENV_VAR` agora está disponível no Zed. Por exemplo, no terminal integrado.

A partir do Zed 0.152.0, a CLI `zed` passará _sempre_ seu ambiente para o Zed, independentemente de uma instância do Zed estar ou não em execução anteriormente. Antes do Zed 0.152.0, isso não acontecia, e apenas a primeira instância do Zed herdava as variáveis de ambiente.

### Iniciado por meio do gerenciador de janelas, do Dock ou do iniciador

Quando o Zed é iniciado pelo Dock do macOS, por um ícone do GNOME ou do KDE no Linux, ou por um iniciador de aplicativos como o Alfred ou o Raycast, ele não possui um ambiente de shell associado do qual possa herdar suas variáveis de ambiente.

Para garantir um ambiente útil, o Zed inicia um shell de login no diretório home do usuário e lê seu ambiente. Esse ambiente é então definido no _processo_ do Zed, de modo que todas as janelas e projetos do Zed o herdam.

Como isso pode causar problemas para usuários que precisam de variáveis de ambiente diferentes para cada projeto (por exemplo, com `direnv`, `asdf` ou `mise`), o Zed inicia outro shell de login ao abrir um projeto. Esse segundo shell é executado no diretório do projeto. O ambiente desse shell _não_ é definido no processo, pois, caso contrário, abrir um novo projeto alteraria o ambiente de todas as janelas do Zed. Em vez disso, esse ambiente é armazenado e repassado ao executar tarefas, abrir terminais ou iniciar servidores de linguagem.

## Onde e como as variáveis de ambiente são utilizadas?

Existem dois conjuntos de variáveis de ambiente:

1. Variáveis de ambiente do processo Zed
2. Variáveis de ambiente armazenadas por projeto

As variáveis da equação (1) são sempre utilizadas, uma vez que estão armazenadas no próprio processo e todos os processos gerados (tarefas, terminais, servidores de linguagem, etc.) as herdam por padrão.

As variáveis da equação (2) são utilizadas explicitamente, dependendo da característica.

### Tarefas

As tarefas são criadas com um ambiente combinado. Por ordem de precedência (da mais baixa à mais alta, sendo que a última substitui a primeira):

- o ambiente de processo Zed
- se o projeto foi aberto a partir da CLI: o ambiente da CLI
- se o projeto não tiver sido aberto pela CLI: as variáveis de ambiente do projeto obtidas ao executar um shell de login na pasta raiz do projeto
- ambiente opcional, configurado explicitamente nas configurações

### Terminal integrado

Os terminais integrados, assim como as tarefas, são iniciados com um ambiente combinado. Por ordem de precedência (da mais baixa à mais alta):

- o ambiente de processo Zed
- se o projeto foi aberto a partir da CLI: o ambiente da CLI
- se o projeto não tiver sido aberto pela CLI: as variáveis de ambiente do projeto obtidas ao executar um shell de login na pasta raiz do projeto
- ambiente opcional, configurado explicitamente nas configurações

### Pesquisa de servidores de idiomas

Para algumas linguagens, os adaptadores do servidor de linguagem procuram o arquivo binário no `$PATH` do usuário. Exemplos:

- Vá
- Zig
- Rust (se [estiver configurado para isso](./languages/rust.md#binary))
- C
- TypeScript

Para essa consulta, o Zed utiliza o seguinte ambiente:

- se o projeto foi aberto a partir da CLI: o ambiente da CLI
- se o projeto não tiver sido aberto pela CLI: as variáveis de ambiente do projeto obtidas ao executar um shell de login na pasta raiz do projeto

### Servidores de idiomas

Depois de procurar um servidor de linguagem, Zed o inicia.

Esses processos do servidor de linguagem sempre herdam o ambiente de processo do Zed. No entanto, dependendo da consulta ao servidor de linguagem, variáveis de ambiente adicionais podem ser definidas ou substituir o ambiente do processo.

- Se o servidor de linguagem foi encontrado no `$PATH` do ambiente do projeto, esse ambiente do projeto é passado para o processo do servidor de linguagem. A origem do ambiente do projeto depende de como o projeto foi aberto (por meio da CLI ou não). Consulte a seção anterior sobre a localização do servidor de linguagem.
- Se o servidor de linguagem não for encontrado no ambiente do projeto, o Zed tenta instalá-lo e iniciá-lo globalmente. Nesse caso, o processo herda o ambiente do processo do Zed e, se o projeto tiver sido aberto via CLI, o ambiente da CLI.
