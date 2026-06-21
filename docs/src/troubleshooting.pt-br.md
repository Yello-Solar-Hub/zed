---
título: Resolução de problemas
descrição: “Problemas comuns e soluções para o Zed em todas as plataformas.”
---

# Solução de problemas

Este guia aborda técnicas comuns de solução de problemas para o Zed.
Às vezes, você poderá identificar e resolver os problemas por conta própria usando essas informações.
Em outras ocasiões, a resolução de problemas significa coletar as informações certas (registros, perfis ou etapas de reprodução) para nos ajudar a diagnosticar e corrigir o problema.

> **Observação**: Para abrir a paleta de comandos, use `cmd-shift-p` no macOS ou `ctrl-shift-p` no Windows/Linux.

## Obter informações sobre o Zed e o sistema

Ao relatar problemas ou solicitar ajuda, é útil saber qual é a versão do Zed e as especificações do seu sistema. Você pode obter essas informações realizando as seguintes ações na paleta de comandos:

- {#action zed::About}: Descubra o número da versão do Zed
- {#action zed::CopySystemSpecsIntoClipboard}: Copie para a área de transferência o número da versão do Zed, a versão do sistema operacional e as especificações de hardware
- {#action zed::CopyInstalledExtensionsIntoClipboard}: Copie para a área de transferência uma lista das extensões instaladas e suas versões

## Diário do Zed

Muitas vezes, um bom ponto de partida para solucionar qualquer problema no Zed é o log do Zed, que pode conter pistas sobre o que está dando errado.
Você pode consultar as 1.000 linhas mais recentes do log executando a ação {#action zed::OpenLog} na paleta de comandos.
Se você quiser visualizar o arquivo completo, pode abri-lo no gerenciador de arquivos nativo do seu sistema operacional por meio do comando {#action zed::RevealLogInFileManager} na paleta de comandos.

Você encontrará o log do Zed no local correspondente em cada sistema operacional:

- macOS: `~/Library/Logs/Zed/Zed.log`
- Windows: `C:\Users\YOU\AppData\Local\Zed\logs\Zed.log`
- Linux: `~/.local/share/zed/logs/Zed.log` ou `$XDG_DATA_HOME`

> **Observação:** Em alguns casos, pode ser útil monitorar o log em tempo real, como, por exemplo, ao [desenvolver uma extensão do Zed](https://zed.dev/docs/extensions/developing-extensions).
> Exemplo: `tail -f ~/Library/Logs/Zed/Zed.log`

O log pode conter contexto suficiente para ajudá-lo a depurar o problema por conta própria, ou você pode encontrar erros específicos que serão úteis ao abrir um [issue no GitHub](https://github.com/zed-industries/zed/issues/new/choose) ou ao entrar em contato com a equipe do Zed em nosso [servidor do Discord](https://zed.dev/community-links#forums-and-discussions).

## Problemas de desempenho (análise de desempenho)

Se você estiver enfrentando problemas de desempenho no Zed (travamentos, congelamentos ou falta de resposta em geral), anexar um perfil de desempenho à sua solicitação nos ajudará a identificar exatamente o que está causando o problema.

### macOS

O Xcode Instruments (que vem incluído no download do [Xcode](https://apps.apple.com/us/app/xcode/id497799835)) é a ferramenta padrão para análise de desempenho no macOS.

1. Com o Zed em execução, abra o Instruments
1. Selecione `Time Profiler` como modelo de análise de desempenho
   ![Seletor de modelos de instrumentos com o Time Profiler selecionado](https://images.zed.dev/docs/troubleshooting/instruments-template-picker.webp)
1. Na configuração do `Time Profiler`, defina o alvo como o processo Zed em execução
1. Iniciar gravação
   ![Configuração do Time Profiler mostrando o menu suspenso de destino e o botão de gravação](https://images.zed.dev/docs/troubleshooting/instruments-target-and-record.webp)
1. Execute no Zed a ação que causa problemas de desempenho
1. Parar a gravação
   ![Um registro concluído do Time Profiler no Instruments](https://images.zed.dev/docs/troubleshooting/instruments-recording.webp)
1. Salvar o arquivo de rastreamento
1. Compactar o arquivo de rastreamento em um arquivo zip
1. Crie uma [issue no GitHub](https://github.com/zed-industries/zed/issues/new/choose) anexando o arquivo zip com o rastreamento

<!--### Windows-->

<!--### Linux-->

## Problemas com a inicialização e o espaço de trabalho

O Zed cria bancos de dados SQLite locais para armazenar dados relacionados ao seu espaço de trabalho e aos seus projetos. Esses bancos de dados armazenam, por exemplo, as abas e os painéis que você tem abertos em um projeto, a posição de rolagem de cada arquivo aberto, a lista de todos os projetos que você abriu (para o seletor modal de projetos recentes), etc. Você pode localizar e explorar esses bancos de dados nos seguintes locais:

- macOS: `~/Library/Application Support/Zed/db`
- Linux e FreeBSD: `~/.local/share/zed/db` (ou dentro de `XDG_DATA_HOME` ou `FLATPAK_XDG_DATA_HOME`)
- Windows: `%LOCALAPPDATA%\Zed\db`

A convenção de nomenclatura desses bancos de dados segue o formato `0-<zed_channel>`:

- Estável: `0-stable`
- Pré-visualização: `0-preview`
- Noturno: `0-nightly`
- Dev: `0-dev`

Embora seja raro, já observamos alguns casos em que os bancos de dados do espaço de trabalho ficaram corrompidos, o que impediu o Zed de iniciar.
Se você estiver enfrentando problemas na inicialização, pode verificar se o problema está relacionado ao espaço de trabalho movendo temporariamente o banco de dados de seu local e, em seguida, tentando iniciar o Zed novamente.

> **Observação**: A transferência do banco de dados do espaço de trabalho fará com que o Zed crie um novo.
> Seus projetos recentes, abas abertas etc. serão redefinidos para as configurações “de fábrica”.

Se o problema persistir após a regeneração do banco de dados, por favor, [relate o problema](https://github.com/zed-industries/zed/issues/new/choose).

## Problemas com o servidor de idiomas

Se você estiver enfrentando problemas relacionados ao servidor de idiomas, como diagnósticos desatualizados ou dificuldades para acessar definições, reiniciar o servidor de idiomas por meio do comando {#action editor::RestartLanguageServer} na paleta de comandos geralmente resolve o problema.

## Mensagens de erro do agente

### "Número máximo de tokens atingido"

Esse erro aparece quando a resposta do agente excede o limite máximo de tokens do modelo. Isso ocorre quando:

- O agente gera uma resposta extremamente longa
- O contexto da conversa, somado à resposta, excede a capacidade do modelo
- Os resultados da ferramenta são volumosos e consomem o orçamento de tokens disponível

**Para resolver isso:**

1. Crie um novo tópico para reduzir o tamanho do contexto
2. Use um modelo com um limite de tokens maior nas configurações de IA
3. Divida sua solicitação em tarefas menores e mais específicas
4. Limpe as saídas da ferramenta ou as mensagens anteriores usando os controles da thread

O limite de tokens varia de acordo com o modelo — consulte a documentação do fornecedor do seu modelo para saber os limites específicos.
