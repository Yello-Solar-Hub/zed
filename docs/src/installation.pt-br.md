---
título: Instalar o Zed — macOS, Linux, Windows
descrição: Baixe e instale o Zed no macOS, Linux ou Windows. Inclui opções como o Homebrew, download direto e gerenciador de pacotes.
---

# Instalando o Zed

## Baixar o Zed

### macOS

Baixe as versões estáveis mais recentes na [página de downloads](https://zed.dev/download). Se quiser baixar nossa versão de pré-visualização, você a encontrará na [página de lançamentos](https://zed.dev/releases/preview). Após a primeira instalação manual, o Zed verificará periodicamente se há atualizações disponíveis.

Você também pode instalar o Zed (versão estável) pelo Homebrew:

```sh
brew install --cask zed
```

Além disso, uma prévia do Zed:

```sh
brew install --cask zed@preview
```

### Windows

Baixe as versões estáveis mais recentes na [página de downloads](https://zed.dev/download). Se quiser baixar nossa versão de pré-lançamento, você a encontrará na [página de lançamentos](https://zed.dev/releases/preview). Após a primeira instalação manual, o Zed verificará periodicamente se há atualizações disponíveis.

Além disso, você pode instalar o Zed usando o winget:

```sh
winget install -e --id ZedIndustries.Zed
```

### Linux

Para a maioria dos usuários do Linux, a maneira mais fácil de instalar o Zed é por meio do nosso script de instalação:

```sh
curl -f https://zed.dev/install.sh | sh
```

Agora você pode, opcionalmente, especificar uma **versão** do Zed a ser instalada usando a variável de ambiente `ZED_VERSION`:

```sh
# Install the latest stable version (default)
curl -f https://zed.dev/install.sh | sh

# Install a specific version
curl -f https://zed.dev/install.sh | ZED_VERSION=0.216.0 sh
```

Para instalar a versão de pré-visualização, que recebe atualizações cerca de uma semana antes da versão estável:

```sh
curl -f https://zed.dev/install.sh | ZED_CHANNEL=preview sh
```

Este script é compatível com `x86_64` e `AArch64`, bem como com as principais distribuições do Linux: Ubuntu, Arch, Debian, RedHat, CentOS, Fedora e outras.

Se o Zed for instalado usando este script de instalação, ele poderá ser desinstalado a qualquer momento executando o comando de shell `zed --uninstall`. O shell perguntará se você deseja manter suas preferências ou excluí-las. Após fazer sua escolha, você deverá ver uma mensagem informando que o Zed foi desinstalado com sucesso.

Caso este script não seja suficiente para o seu caso de uso, você encontre problemas ao executar o Zed ou haja erros ao desinstalar o Zed, consulte nossa [documentação específica para Linux](./linux.md).

## Requisitos do sistema

### macOS

O Zed é compatível com as seguintes versões do macOS:

| Versão       | Nome de código | Status da Apple   | Status do Zed          |
| ------------- | -------- | -------------- | ------------------- |
| macOS 26.x    | Tahoe    | Compatível      | Compatível           |
| macOS 15.x    | Sequoia  | Compatível      | Compatível           |
| macOS 14.x    | Sonoma   | Compatível      | Compatível           |
| macOS 13.x    | Ventura  | Compatível      | Compatível           |
| macOS 12.x    | Monterey | Fim da vida útil (EOL) 16/09/2024 | Compatível           |
| macOS 11.x    | Big Sur  | Fim da vida útil (EOL) 26/09/2023 | Parcialmente compatível |
| macOS 10.15.x | Catalina | Fim da vida útil (EOL) 12/09/2022 | Parcialmente compatível |

As versões do macOS classificadas como “Compatíveis parcialmente” (Big Sur e Catalina) não oferecem suporte ao compartilhamento de tela por meio do Zed Collaboration. Esses recursos utilizam o [LiveKit SDK](https://livekit.io), que depende do [ScreenCaptureKit.framework](https://developer.apple.com/documentation/screencapturekit/), disponível apenas no macOS 12 (Monterey) e versões mais recentes.

#### Hardware do Mac

O Zed é compatível com máquinas equipadas com processadores Intel (x86_64) ou Apple (aarch64) que atendam aos requisitos do macOS mencionados acima:

- MacBook Pro (início de 2015 e modelos mais recentes)
- MacBook Air (início de 2015 e modelos mais recentes)
- MacBook (início de 2016 e modelos mais recentes)
- Mac Mini (final de 2014 e modelos mais recentes)
- Mac Pro (final de 2013 ou mais recente)
- iMac (final de 2015 e modelos mais recentes)
- iMac Pro (todos os modelos)
- Mac Studio (todos os modelos)

### Linux

O Zed é compatível com processadores Intel/AMD de 64 bits (x86_64) e Arm de 64 bits (aarch64).

O Zed requer um driver Vulkan 1.3 e os seguintes portais para desktop:

- `org.freedesktop.portal.FileChooser`
- `org.freedesktop.portal.OpenURI`
- `org.freedesktop.portal.Secret` ou `org.freedesktop.Secrets`

### Windows

O Zed é compatível com as seguintes versões do Windows:
| Versão | Status do Zed |
| ------------------------- | ------------------- |
| Windows 11, versão 22H2 e posteriores | Compatível |
| Windows 10, versão 1903 e posteriores | Compatível |

É necessário um sistema operacional de 64 bits para executar o Zed.

#### Hardware do Windows

O Zed é compatível com máquinas equipadas com processadores x64 (Intel, AMD) ou Arm64 (Qualcomm) que atendam aos seguintes requisitos:

- Placa de vídeo: Uma GPU compatível com DirectX 11 (a maioria dos PCs a partir de 2012).
- Driver: Driver atual da NVIDIA/AMD/Intel/Qualcomm (não o “Microsoft Basic Display Adapter”).

### FreeBSD

Ainda não está disponível para download oficial. Pode ser compilado [a partir do código-fonte](./development/freebsd.md).

### Web

Não é compatível no momento. Consulte nossa [página sobre compatibilidade de plataformas](https://github.com/zed-industries/zed/issues/5391).
