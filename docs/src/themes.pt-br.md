---
título: Temas - Zed
descrição: Navegue, instale e crie temas para o Zed. Inclui temas integrados e extensões de temas da comunidade.
---

# Temas

O Zed vem com vários temas integrados, e há mais temas disponíveis como extensões.

## Escolhendo um tema

Veja quais temas estão instalados e visualize-os através do Seletor de Temas, que você pode abrir a partir da paleta de comandos com a ação {#action theme_selector::Toggle} (associada à tecla {#kb theme_selector::Toggle}).

Navegar pela lista de temas usando as setas para cima e para baixo alterará o tema em tempo real, e pressionar Enter salvará o tema selecionado no seu arquivo de configurações.

## Como instalar novos temas

Você pode encontrar centenas de opções diferentes de temas na loja de extensões do Zed, à qual pode acessar pela paleta de comandos com {#action zed::Extensions} ou pelo [site do Zed](https://zed.dev/extensions?filter=themes).

Muitos temas populares foram adaptados para o Zed e, se você estiver com dificuldade para escolher um, acesse [zed-themes.com](https://zed-themes.com), uma galeria de terceiros que oferece visualizações de muitos deles.

## Crie seu tema

Você pode usar o [Zed's Theme Builder](https://zed.dev/theme-builder) para criar seu próprio tema personalizado com base em um tema já existente.

Essa ferramenta permite ajustar com precisão e visualizar como cada superfície ficará no aplicativo Zed.
Em seguida, você pode exportar o JSON para [uso local](./themes.md#local-themes) ou para [publicação na loja de extensões do Zed](./extensions/themes.md).

## Configurando um tema

O tema selecionado é armazenado no seu arquivo de configurações.
Você pode abrir seu arquivo de configurações a partir da paleta de comandos com {#action zed::OpenSettingsFile} (associado a {#kb zed::OpenSettingsFile}).

Por padrão, o Zed mantém dois temas: um para o modo claro e outro para o modo escuro.
Você pode definir o modo como `"dark"` ou `"light"` para ignorar o modo atual do sistema.

```json [settings]
{
  "theme": {
    "mode": "system",
    "light": "One Light",
    "dark": "One Dark"
  }
}
```

### Alternar o modo de tema pelo teclado

Use {#kb theme::ToggleMode} para alternar entre os modos claro e escuro do tema atual.

Se suas configurações atualmente utilizam um valor estático para o tema, como:

```json [settings]
{
  "theme": "Any Theme"
}
```

O primeiro botão de alternância ativa a seleção dinâmica de temas com os temas padrão:

```json [settings]
{
  "theme": {
    "mode": "system",
    "light": "One Light",
    "dark": "One Dark"
  }
}
```

É necessário definir manualmente os temas `light` e `dark` após a primeira alteração.

Depois disso, a alteração afeta apenas `theme.mode`.
Se `light` e `dark` forem o mesmo tema, a primeira alteração pode não produzir uma mudança visível na interface do usuário até que você defina valores diferentes para `light` e `dark`.

## Substituições de tema

Para substituir atributos específicos de um tema, use a configuração `theme_overrides`.
Essa configuração pode ser usada para definir substituições específicas do tema.

Por exemplo, adicione o seguinte ao seu arquivo `settings.json` se desejar alterar a cor de fundo do editor e exibir comentários e comentários de documentação em itálico:

```json [settings]
{
  "theme_overrides": {
    "One Dark": {
      "editor.background": "#333",
      "syntax": {
        "comment": {
          "font_style": "italic"
        },
        "comment.doc": {
          "font_style": "italic"
        }
      },
      "accents": [
        "#ff0000",
        "#ff7f00",
        "#ffff00",
        "#00ff00",
        "#0000ff",
        "#8b00ff"
      ]
    }
  }
}
```

Para ver uma lista completa de capturas (como `comment` e `comment.doc`), consulte [Extensões de linguagem: Destaque de sintaxe](./extensions/languages.md#syntax-highlighting).

Para ver uma lista dos atributos disponíveis do tema, consulte o arquivo JSON do seu tema.
Por exemplo, [assets/themes/one/one.json](https://github.com/zed-industries/zed/blob/main/assets/themes/one/one.json) para os temas padrão One Dark e One Light.

## Temas locais {#local-themes}

Salve novos temas localmente, colocando-os no diretório `~/.config/zed/themes` (macOS e Linux) ou `%USERPROFILE%\AppData\Roaming\Zed\themes\` (Windows).

Por exemplo, para criar um novo tema chamado `my-cool-theme`, crie um arquivo chamado `my-cool-theme.json` nesse diretório.
Ele estará disponível no seletor de temas na próxima vez que o Zed for carregado.
