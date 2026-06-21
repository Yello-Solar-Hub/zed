---
título: Seletor de abas
descrição: “O Seletor de Abas oferece uma maneira rápida de navegar entre as abas abertas no Zed. Ele exibe uma lista das suas abas abertas, classificadas por uso recente, facilitando a..."
---

# Seletor de abas

O Seletor de Abas oferece uma maneira rápida de navegar entre as abas abertas no Zed. Ele
exibe uma lista das suas abas abertas, ordenadas por uso recente, facilitando o acesso
volte ao que você estava fazendo agora mesmo.

![Seletor de abas com vários painéis](https://zed.dev/img/features/tab-switcher.png)

## Comutação rápida

Quando o Seletor de Abas é aberto usando {#kb tab_switcher::Toggle}, em vez de
Ao executar a ação {#action tab_switcher::Toggle} a partir da paleta de comandos, ela permanecerá
permanece ativo enquanto a tecla <kbd class="keybinding">ctrl</kbd> estiver pressionada.

Enquanto mantiver pressionada a tecla <kbd class="keybinding">Ctrl</kbd>, cada tecla <kbd
class="keybinding">tab</kbd> pressiona para avançar para o próximo item (<kbd
class="keybinding">Shift</kbd> para alternar para trás) e, quando <kbd
class="keybinding">Ao soltar a tecla Ctrl</kbd>, o item selecionado é confirmado e
O comutador está fechado.

## Abrindo o seletor de abas

O Seletor de Abas também pode ser aberto com {#action tab_switcher::Toggle} ({#kb tab_switcher::Toggle})
ou {#action tab_switcher::ToggleAll}.

Enquanto o Seletor de abas estiver aberto, você pode:

- Pressione {#kb menu::SelectNext} para passar para a próxima guia da lista
- Pressione {#kb menu::SelectPrevious} para ir para a aba anterior
- Pressione <kbd class="keybinding">Enter</kbd> para confirmar a guia selecionada e fechar o seletor
- Pressione <kbd class="keybinding">Esc</kbd> para fechar o seletor e retornar à aba original da qual
  o comutador foi aberto
- Pressione {#kb tab_switcher::CloseSelectedItem} para fechar a aba selecionada no momento

À medida que você percorre a lista, o Zed atualizará o item ativo do painel para
corresponder à guia selecionada.

## Referência de ação

| Ação                                    | Descrição                                       |
| ----------------------------------------- | ------------------------------------------------- |
| {#action tab_switcher::Toggle}            | Abrir o seletor de abas do painel atual        |
| {#action tab_switcher::ToggleAll}         | Abrir o seletor de abas, exibindo as abas de todos os painéis |
| {#action tab_switcher::FecharItemSelecionado} | Fechar a aba selecionada no seletor de abas        |
