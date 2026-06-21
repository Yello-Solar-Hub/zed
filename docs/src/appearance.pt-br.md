---
título: Aparência e personalização visual - Zed
descrição: Personalize os temas, as fontes, os ícones, a densidade da interface do usuário e outras configurações visuais do Zed de acordo com suas preferências.
---

# Aparência

Personalize a aparência do Zed de acordo com suas preferências. Este guia aborda temas, fontes, ícones e outras configurações visuais.

Para obter informações sobre como funciona o sistema de configurações, consulte [Todas as configurações](./reference/all-settings.md).

## Personalize o Zed em 5 minutos

Veja como fazer com que o Zed se sinta em casa:

1. **Escolha um tema**: Pressione {#kb theme_selector::Toggle} para abrir o Seletor de Temas. Use as setas para percorrer a lista e visualizar os temas em tempo real e pressione Enter para aplicar.

2. **Alternar rapidamente entre os modos claro e escuro**: Pressione {#kb theme::ToggleMode}. Se você estiver usando atualmente um valor estático `"theme": "..."`, a primeira alternância o converterá para configurações dinâmicas de modo com temas padrão.

3. **Escolha um tema de ícones**: Execute {#action icon_theme_selector::Toggle} na paleta de comandos para navegar pelos temas de ícones.

4. **Defina sua fonte**: Abra o Editor de Configurações com {#kb zed::OpenSettings} e procure por `buffer_font_family`. Defina-a como a fonte de programação de sua preferência.

5. **Ajustar o tamanho da fonte**: No mesmo Editor de Configurações, procure por `buffer_font_size` e `ui_font_size` para ajustar os tamanhos das fontes do editor e da interface.

É isso aí. Agora você tem uma configuração personalizada do Zed.

## Temas

Instale temas na página Extensões ({#action zed::Extensions}) e, em seguida, alterne entre eles usando o Seletor de Temas ({#kb theme_selector::Toggle}).

O Zed oferece temas distintos para os modos claro e escuro, com alternância automática de acordo com as preferências do seu sistema:

```json [settings]
{
  "theme": {
    "mode": "system",
    "light": "One Light",
    "dark": "One Dark"
  }
}
```

Você também pode substituir atributos específicos do tema para obter um controle mais preciso.

→ [Documentação sobre temas](./themes.md)

## Temas de ícones

Personalize os ícones de arquivos e pastas no Painel de Projetos e nas guias. Navegue pelos temas de ícones disponíveis com o Seletor de Temas de Ícones ({#action icon_theme_selector::Toggle} na paleta de comandos).

Assim como os temas de cores, os temas de ícones oferecem variantes separadas para os estilos claro e escuro:

```json [settings]
{
  "icon_theme": {
    "mode": "system",
    "light": "Zed (Default)",
    "dark": "Zed (Default)"
  }
}
```

→ [Documentação sobre temas de ícones](./icon-themes.md)

## Fontes

O Zed utiliza três configurações de fonte para diferentes contextos:

| Cenário                | Utilizado para                  |
| ---------------------- | ------------------------- |
| `buffer_font_family`   | Texto do editor               |
| `ui_font_family`       | Elementos de interface        |
| `terminal.font_family` | [Terminal](./terminal.md) |

Exemplo de configuração:

```json [settings]
{
  "buffer_font_family": "JetBrains Mono",
  "buffer_font_size": 14,
  "ui_font_family": "Inter",
  "ui_font_size": 16,
  "terminal": {
    "font_family": "JetBrains Mono",
    "font_size": 14
  }
}
```

### Ligaduras de fonte

Para desativar as ligaduras das fontes:

```json [settings]
{
  "buffer_font_features": {
    "calt": false
  }
}
```

### Altura da linha

Ajuste o espaçamento entre linhas com `buffer_line_height`:

- `"confortável"` — proporção de 1,618 (padrão)
- `"padrão"` — proporção de 1,3
- `{ "custom": 1.5 }` — Proporção personalizada

## Elementos da interface do usuário

O Zed oferece amplo controle sobre os elementos da interface do usuário, incluindo:

- **Barra de abas** — Mostrar/ocultar, botões de navegação, ícones de arquivos, status do Git
- **Barra de status** — Seletor de idioma, posição do cursor, finais de linha
- **Barra de rolagem** — Visibilidade, indicadores do git diff, resultados da pesquisa
- **Minimapa** — Exibição da visão geral do código
- **Margem inferior** — Números de linha, indicadores de dobra, pontos de quebra
- **Painéis** — Dimensionamento e acoplamento do Painel do Projeto, do Terminal e do Painel do Agente

→ [Documentação sobre personalização visual](./visual-customization.md) para todas as configurações dos elementos da interface do usuário

## O que vem a seguir

- [Todas as configurações](./reference/all-settings.md) — Referência completa de configurações
- [Atribuições de teclas](./key-bindings.md) — Personalize os atalhos de teclado
- [Modo Vim](./vim.md) — Ativar a edição modal
