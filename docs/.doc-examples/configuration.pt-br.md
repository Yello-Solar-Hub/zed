<!--
  EXEMPLO DE REFERÊNCIA: Documentação de configuração

  Este exemplo mostra a documentação sobre ajustes e configuração.

  Principais padrões a serem observados:
  - IDs de âncora em todas as seções principais
  - O parágrafo inicial explica o que este guia aborda
  - Vários exemplos de JSON com a anotação [settings]
  - Caminhos de arquivos específicos da plataforma
  - Formatação correta das legendas
  - Seção “Veja também” no final (não “O que vem a seguir”)
-->

---

título: Configurando o Zed - Configurações e preferências
descrição: Configure o Zed usando o Editor de Configurações, arquivos JSON e substituições específicas do projeto. Abrange todas as opções de configuração.

---

# Configurando o Zed

Este guia explica como funciona o sistema de configurações do Zed, incluindo o Editor de Configurações, os arquivos de configuração JSON e as configurações específicas do projeto.

Para personalização visual (temas, fontes, ícones), consulte [Aparência](./appearance.md).

## Editor de configurações {#settings-editor}

O **Editor de configurações** ({#kb zed::OpenSettings}) é a principal forma de configurar o Zed. Ele oferece uma interface com função de pesquisa, na qual você pode navegar pelas configurações disponíveis, ver seus valores atuais e fazer alterações.

Para abri-lo:

- Pressione {#kb zed::OpenSettings}
- Ou execute {#action zed::OpenSettings} na paleta de comandos

À medida que você digita na caixa de pesquisa, as configurações correspondentes são exibidas, acompanhadas de descrições e controles para modificá-las. As alterações são salvas automaticamente no seu arquivo de configurações.

> **Observação:** Nem todas as configurações estão disponíveis no Editor de Configurações ainda. Algumas opções avançadas, como formatadores de idioma, exigem a edição direta do arquivo JSON.

## Arquivos de configuração {#settings-files}

### Configurações do usuário {#user-settings}

Suas configurações de usuário se aplicam globalmente a todos os projetos. Abra o arquivo com {#kb zed::OpenSettingsFile} ou execute {#action zed::OpenSettingsFile} na paleta de comandos.

O arquivo está localizado em:

- macOS: `~/.config/zed/settings.json`
- Linux: `~/.config/zed/settings.json` (ou `$XDG_CONFIG_HOME/zed/settings.json`)
- Windows: `%APPDATA%\Zed\settings.json`

A sintaxe é JSON, com suporte a comentários `//`.

### Configurações padrão {#default-settings}

Para ver todas as configurações disponíveis com seus valores padrão, execute {#action zed::OpenDefaultSettings} na paleta de comandos. Isso abre uma referência somente leitura que você pode usar ao editar suas próprias configurações.

### Configurações do projeto {#project-settings}

Para substituir as configurações do usuário em um projeto específico, crie um arquivo `.zed/settings.json` na raiz do seu projeto. Execute {#action zed::OpenProjectSettings} para criar esse arquivo.

As configurações do projeto têm precedência sobre as configurações do usuário apenas para esse projeto.

```json [settings]
// .zed/settings.json
{
  "tab_size": 2,
  "formatter": "prettier",
  "format_on_save": "on"
}
```

Você também pode adicionar arquivos de configuração em subdiretórios para obter um controle mais detalhado.

> **Observação:** Nem todas as configurações podem ser definidas no nível do projeto. As configurações que afetam o editor de forma global (como `theme` ou `vim_mode`) só funcionam nas configurações do usuário. As configurações do projeto se limitam ao comportamento do editor e às opções de ferramentas de linguagem, como `tab_size`, `formatter` e `format_on_save`.

## Como as configurações são mescladas {#how-settings-merge}

As configurações são aplicadas em camadas:

1. **Configurações padrão** — As configurações padrão integradas do Zed
2. **Configurações do usuário** — Suas preferências gerais
3. **Configurações do projeto** — Substituições específicas do projeto

As camadas posteriores substituem as anteriores. No caso das configurações de objetos (como `terminal`), as propriedades são mescladas, em vez de substituídas por completo.

## Substituições de canal por versão {#release-channel-overrides}

Use configurações diferentes para as versões Stable, Preview ou Nightly adicionando chaves de canal de nível superior:

```json [settings]
{
  "theme": "One Dark",
  "vim_mode": false,
  "nightly": {
    "theme": "Rosé Pine",
    "vim_mode": true
  },
  "preview": {
    "theme": "Catppuccin Mocha"
  }
}
```

Com esta configuração:

- A versão **Stable** usa o One Dark com o modo vim desativado
- A **Pré-visualização** usa o Catppuccin Mocha com o modo vim desativado
- O **Nightly** usa o Rosé Pine com o modo vim ativado

As alterações feitas no Editor de configurações se aplicam a todos os canais.

## Configurações de links diretos {#deep-links}

O Zed oferece suporte a links diretos que abrem configurações específicas diretamente:

```
zed://settings/theme
zed://settings/vim_mode
zed://settings/buffer_font_size
```

Eles são úteis para compartilhar dicas de configuração ou incluir links na documentação.

## Exemplo de configuração {#example-configuration}

```json [settings]
{
  "theme": {
    "mode": "system",
    "light": "One Light",
    "dark": "One Dark"
  },
  "buffer_font_family": "JetBrains Mono",
  "buffer_font_size": 14,
  "tab_size": 2,
  "format_on_save": "on",
  "autosave": "on_focus_change",
  "vim_mode": false,
  "terminal": {
    "font_family": "JetBrains Mono",
    "font_size": 14
  },
  "languages": {
    "Python": {
      "tab_size": 4
    }
  }
}
```

## Veja também {#see-also}

- [Aparência](./appearance.md) — Temas, fontes e personalização visual
- [Atribuições de teclas](./key-bindings.md) — Personalize os atalhos de teclado
- [Introdução rápida à IA](./ai/quick-start.md) — Configurar provedores de IA, modelos e ajustes do agente
- [Todas as configurações](./reference/all-settings.md) — Referência completa sobre configurações
