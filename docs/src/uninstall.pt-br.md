---
título: Desinstalar
descrição: “Este guia explica como desinstalar o Zed em diferentes sistemas operacionais.”
---

# Desinstalar

Este guia explica como desinstalar o Zed em diferentes sistemas operacionais.

## macOS

### Instalação padrão

Se você instalou o Zed baixando-o do site:

1. Encerre o Zed, caso ele esteja em execução
2. Abra o Finder e acesse a pasta “Aplicativos”
3. Arraste o Zed para a Lixeira (ou clique com o botão direito e selecione “Mover para a Lixeira”)
4. Esvaziar a Lixeira

### Instalação do Homebrew

Se você instalou o Zed usando o Homebrew, use o seguinte comando:

```sh
brew uninstall --cask zed
```

Ou, para a versão de pré-visualização:

```sh
brew uninstall --cask zed@preview
```

### Exclusão de dados do usuário (opcional)

Para remover completamente todos os arquivos de configuração e dados do Zed:

1. Abrir o Finder
2. Pressione `Cmd + Shift + G` para abrir “Ir para a pasta”
3. Exclua os seguintes diretórios, caso existam:
   - `~/Biblioteca/Suporte a Aplicativos/Zed`
   - `~/Biblioteca/Estado salvo do aplicativo/dev.zed.Zed.savedState`
   - `~/Biblioteca/Logs/Zed`
   - `~/Biblioteca/Caches/dev.zed.Zed`
   - `~/Biblioteca/Caches/Zed`
   - `~/.config/zed`
   - `~/.local/state/Zed`

## Linux

### Desinstalação padrão

Se o Zed tiver sido instalado usando o script de instalação padrão, execute:

```sh
zed --uninstall
```

Será exibida uma mensagem perguntando se você deseja manter ou excluir suas preferências. Após fazer sua escolha, você deverá ver uma mensagem informando que o Zed foi desinstalado com sucesso.

Se o comando `zed` não for encontrado no seu PATH, tente:

```sh
$HOME/.local/bin/zed --uninstall
```

ou:

```sh
$HOME/.local/zed.app/bin/zed --uninstall
```

### Gerenciador de pacotes

Se você instalou o Zed usando um gerenciador de pacotes (como o Flatpak, o Snap ou um gerenciador de pacotes específico da distribuição), consulte a documentação desse gerenciador de pacotes para obter instruções sobre como desinstalá-lo.

### Remoção manual

Se o comando de desinstalação falhar ou se o Zed tiver sido instalado em um local personalizado, você pode removê-lo manualmente:

- Diretório de instalação: `~/.local/zed.app` (ou seu caminho de instalação personalizado)
- Link simbólico binário: `~/.local/bin/zed`
- Configuração e dados: `~/.config/zed`

## Windows

### Instalação padrão

1. Encerre o Zed, caso ele esteja em execução
2. Abra as Configurações (tecla Windows + I)
3. Vá para “Aplicativos” > “Aplicativos instalados” (ou “Aplicativos e recursos” no Windows 10)
4. Pesquisar por “Zed”
5. Clique no menu com os três pontos ao lado do Zed e selecione “Desinstalar”
6. Siga as instruções para concluir a desinstalação

Como alternativa, você pode:

1. Abra o menu Iniciar
2. Clique com o botão direito do mouse em Zed
3. Selecione “Desinstalar”

### Exclusão de dados do usuário (opcional)

Para remover completamente todos os arquivos de configuração e dados do Zed:

1. Pressione a `tecla Windows + R` para abrir a caixa de diálogo “Executar”
2. Digite `%APPDATA%` e pressione Enter
3. Exclua a pasta `Zed`, caso ela exista
4. Pressione `tecla Windows + R` novamente, digite `%LOCALAPPDATA%` e pressione Enter
5. Exclua a pasta `Zed`, caso ela exista

## Solução de problemas

Caso você encontre problemas durante a desinstalação:

- **macOS/Windows**: Certifique-se de que o Zed esteja completamente encerrado antes de tentar desinstalá-lo. Verifique o Gerenciador de Atividades (macOS) ou o Gerenciador de Tarefas (Windows) para ver se há algum processo do Zed em execução.
- **Linux**: Se o script de desinstalação falhar, verifique a mensagem de erro e considere a remoção manual dos diretórios listados acima.
- **Todas as plataformas**: Se você quiser começar do zero, mas manter o Zed instalado, pode excluir os diretórios de configuração em vez de desinstalar o aplicativo por completo.

Para obter mais ajuda, consulte nossa [documentação específica para Linux](./linux.md) ou acesse a [comunidade do Zed](https://zed.dev/community-links).
