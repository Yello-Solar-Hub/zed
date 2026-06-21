---
título: Modo Helix - Zed
descrição: Configurações de teclas no estilo Helix e edição modal no Zed. Edição com prioridade à seleção, baseada no modo Vim.
---

# Modo Helix

_Trabalho em andamento. Nem todas as combinações de teclas do Helix foram implementadas ainda._

O modo Helix do Zed é uma camada de emulação que traz as combinações de teclas no estilo Helix e a edição modal para o Zed. Ele se baseia no [modo Vim](./vim.md) do Zed, portanto grande parte da funcionalidade principal é compartilhada. Ativar o `helix_mode` também ativará o `vim_mode`.

Para obter um guia sobre os recursos relacionados ao Vim que também estão disponíveis no modo Helix, consulte nossa [documentação do modo Vim](./vim.md).

Para verificar o status atual do modo Helix ou para solicitar um recurso do Helix que ainda não está disponível, consulte a ["discussão 'Já estamos no modo Helix?'"](https://github.com/zed-industries/zed/discussions/33580).

Para obter uma lista detalhada das combinações de teclas padrão do Helix, acesse a [documentação oficial do Helix](https://docs.helix-editor.com/keymap.html).

## Principais diferenças

Qualquer objeto de texto que funcione com `m i` ou `m a` também funciona com `]` e `[`; assim, por exemplo, `] (` seleciona o próximo par de parênteses após o cursor.
