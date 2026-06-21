---
título: Temas de ícones
descrição: “O Zed vem com um tema de ícones integrado, e há mais temas de ícones disponíveis como extensões.”
---

# Temas de ícones

O Zed vem com um tema de ícones integrado, e há mais temas de ícones disponíveis como extensões.

## Como selecionar um tema de ícones

Veja quais temas de ícones estão instalados e visualize-os usando o Seletor de Temas de Ícones, que você pode abrir a partir da paleta de comandos com {#action icon_theme_selector::Toggle}.

Navegar pela lista de temas de ícones usando as setas para cima e para baixo alterará o tema de ícones em tempo real, e pressionar Enter salvará a escolha no seu arquivo de configurações.

## Instalando mais temas de ícones

Mais temas de ícones estão disponíveis na página “Extensões”, à qual você pode acessar pela paleta de comandos com {#action zed::Extensions} ou pelo [site do Zed](https://zed.dev/extensions?filter=icon-themes).

## Configurando temas de ícones

O tema de ícones que você selecionou está armazenado no seu arquivo de configurações.
Você pode abrir seu arquivo de configurações a partir da paleta de comandos com {#action zed::OpenSettingsFile} (associado a {#kb zed::OpenSettingsFile}).

Assim como acontece com os temas, o Zed permite configurar diferentes temas de ícones para os modos claro e escuro.
Você pode definir o modo como `"light"` ou `"dark"` para ignorar o modo atual do sistema.

```json [settings]
{
  "icon_theme": {
    "mode": "system",
    "light": "Light Icon Theme",
    "dark": "Dark Icon Theme"
  }
}
```

## Desenvolvimento de temas de ícones

Veja: [Desenvolvendo temas de ícones para o Zed](./extensions/icon-themes.md)
