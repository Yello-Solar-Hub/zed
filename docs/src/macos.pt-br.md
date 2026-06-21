---
título: Zed no macOS
descrição: “O Zed é desenvolvido principalmente no macOS, o que o torna uma plataforma de primeira linha com suporte completo a todos os recursos.”
---

# Zed no macOS

O Zed é desenvolvido principalmente no macOS, o que o torna uma plataforma de primeira linha com suporte completo a todos os recursos.

## Instalando o Zed

Baixe o Zed na [página de download](https://zed.dev/download). O arquivo baixado é um `.dmg` — abra-o e arraste o Zed para a pasta “Aplicativos”.

Para acessar a versão de pré-visualização, que recebe atualizações cerca de uma semana antes da versão estável, acesse a [página de versões de pré-visualização](https://zed.dev/releases/preview).

Após a instalação, o Zed verifica se há atualizações automaticamente e avisa quando uma nova versão estiver disponível.

### Cerveja artesanal

Você também pode instalar o Zed usando o Homebrew:

```sh
brew install --cask zed
```

Para a versão de pré-visualização:

```sh
brew install --cask zed@preview
```

### Compilação a partir do código-fonte

Para compilar o Zed a partir do código-fonte, consulte a [documentação de desenvolvimento para macOS](./development/macos.md).

## Requisitos do sistema

- macOS 10.15.7 (Catalina) ou versão posterior
- Processador Apple Silicon (M1/M2/M3/M4) ou Intel

O Zed utiliza o Metal para renderização acelerada por GPU, que está disponível em todas as versões compatíveis do macOS.

## Instalando a CLI

O Zed inclui uma ferramenta de linha de comando para abrir arquivos e projetos a partir do Terminal. Para instalá-la:

1. Abrir o Zed
2. Abra a paleta de comandos com `Cmd+Shift+P`
3. Execute {#action cli::InstallCliBinary}

Isso cria um comando `zed` em `/usr/local/bin`. Em seguida, você pode abrir arquivos e pastas:

```sh
zed .                    # Open current folder
zed file.txt             # Open a file
zed project/ file.txt    # Open a folder and a file
```

Consulte a [Referência da CLI](./reference/cli.md) para ver todas as opções disponíveis.

## Desinstalar

1. Encerre o Zed, caso ele esteja em execução
2. Arraste o Zed da pasta “Aplicativos” para a Lixeira
3. Se desejar, remova suas configurações e extensões:

```sh
rm -rf ~/.config/zed
rm -rf ~/Library/Application\ Support/Zed
rm -rf ~/Library/Caches/Zed
rm -rf ~/Library/Logs/Zed
rm -rf ~/Library/Saved\ Application\ State/dev.zed.Zed.savedState
```

Se você instalou a CLI, remova-a com o seguinte comando:

```sh
rm /usr/local/bin/zed
```

## Solução de problemas

### O Zed não abre ou exibe um aviso de que está “danificado”

Se o macOS informar que o Zed está corrompido ou não pode ser aberto, é provável que seja um problema relacionado ao Gatekeeper. Tente o seguinte:

1. Clique com o botão direito do mouse (ou clique com a tecla Control pressionada) em “Zed”, na pasta “Aplicativos”
2. Selecione “Abrir” no menu de contexto
3. Clique em “Abrir” na caixa de diálogo que aparecer

Isso faz com que o macOS considere o aplicativo confiável.

Se isso não funcionar, remova o atributo de quarentena:

```sh
xattr -cr /Applications/Zed.app
```

### Comando da CLI não encontrado

Se o comando `zed` não estiver disponível após a instalação:

1. Verifique se `/usr/local/bin` está no seu PATH
2. Tente reinstalar a CLI por meio de {#action cli::InstallCliBinary} na paleta de comandos
3. Abra uma nova janela do terminal para atualizar o PATH

### Problemas com a GPU ou com a renderização

O Zed usa o Metal para renderização. Se você notar falhas gráficas:

1. Verifique se o macOS está atualizado
2. Reinicie o seu Mac para redefinir o estado da GPU
3. Verifique no Monitor de Atividade a carga na GPU causada por outros aplicativos

### Alto consumo de memória ou da CPU

Se o Zed consumir mais recursos do que o esperado:

1. Verifique se há servidores de idiomas em execução descontrolada na saída do terminal ({#action zed::OpenLog})
2. Tente desativar as extensões, uma por uma, para identificar possíveis conflitos
3. Para projetos grandes, considere usar as [configurações do projeto](./reference/all-settings.md#file-scan-exclusions) para excluir pastas desnecessárias da indexação

Para obter mais ajuda, consulte o [Guia de solução de problemas](./troubleshooting.md) ou acesse o [Discord do Zed](https://discord.gg/zed-community).
