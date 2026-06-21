# Modelines

As modelines são comentários especiais no início ou no final de um arquivo que configuram as opções do editor para aquele arquivo específico. O Zed é compatível com os formatos de modeline do Vim e do Emacs, permitindo que você especifique configurações como tamanho da tabulação, estilo de recuo e tipo de arquivo diretamente dentro dos seus arquivos.

## Configuração

Use a configuração [`modeline_lines`](./reference/all-settings.md#modeline-lines) para controlar quantas linhas o Zed procura por modelines:

```json [settings]
{
  "modeline_lines": 5
}
```

Defina como `0` para desativar completamente a análise da modeline.

## Emacs

O Zed oferece algum suporte à compatibilidade com [variáveis de arquivo do Emacs](https://www.gnu.org/software/emacs/manual/html_node/emacs/Specifying-File-Variables.html).

Exemplo:

```python
# -*- mode: python; tab-width: 4; indent-tabs-mode: nil; -*-
```

### Variáveis do Emacs compatíveis

| Variável                   | Descrição                    | Configuração do Zed                                                                                |
| -------------------------- | ------------------------------ | ------------------------------------------------------------------------------------------ |
| `modo`                     | Modalidade principal/língua            | Detecção de idioma                                                                         |
| `largura da tabulação`                | Largura de exibição da guia              | [`tab_size`](./reference/all-settings.md#tab-size)                                         |
| `preencher coluna`              | Coluna de quebra automática de linha               | [`preferred_line_length`](./reference/all-settings.md#preferred-line-length)               |
| `indent-tabs-mode`         | `nil` para espaços, `t` para tabulações | [`hard_tabs`](./reference/all-settings.md#hard-tabs)                                       |
| `electric-indent-mode`     | Indentação automática               | [`auto_indent`](./reference/all-settings.md#auto-indent)                                   |
| `require-final-newline`    | Garantir que haja uma nova linha no final           | [`ensure_final_newline_on_save`](./reference/all-settings.md#ensure-final-newline-on-save) |
| `show-trailing-whitespace` | Mostrar espaços em branco à direita       | [`show_whitespaces`](./reference/all-settings.md#show-whitespaces)                         |

## Vim

O Zed oferece algum suporte à compatibilidade com a [modeline do Vim](https://vimhelp.org/options.txt.html#modeline).

Exemplo:

```python
# vim: set ft=python ts=4 sw=4 et:
```

### Opções do Vim compatíveis

| Opção         | Aliases | Descrição                       | Configuração do Zed                                                                                |
| -------------- | ------- | --------------------------------- | ------------------------------------------------------------------------------------------ |
| `tipo de arquivo`     | `ft`    | Tipo de arquivo/idioma                | Detecção de idioma                                                                         |
| `tabstop`      | `ts`    | Número de espaços que uma tabulação ocupa | [`tab_size`](./reference/all-settings.md#tab-size)                                         |
| `textwidth`    | `tw`    | Largura máxima da linha                | [`preferred_line_length`](./reference/all-settings.md#preferred-line-length)               |
| `expandtab`    | `et`    | Use espaços em vez de tabulações        | [`hard_tabs`](./reference/all-settings.md#hard-tabs)                                       |
| `noexpandtab`  | `noet`  | Use tabulações em vez de espaços        | [`hard_tabs`](./reference/all-settings.md#hard-tabs)                                       |
| `autoindent`   | `ai`    | Ativar indentação automática           | [`auto_indent`](./reference/all-settings.md#auto-indent)                                   |
| `noautoindent` | `noai`  | Desativar a indentação automática          | [`auto_indent`](./reference/all-settings.md#auto-indent)                                   |
| `fim de linha`    | `eol`   | Garantir que haja uma nova linha no final              | [`ensure_final_newline_on_save`](./reference/all-settings.md#ensure-final-newline-on-save) |
| `noendofline`  | `noeol` | Desativar a quebra de linha final             | [`ensure_final_newline_on_save`](./reference/all-settings.md#ensure-final-newline-on-save) |

## Notas

- O primeiro kilobyte de um arquivo é verificado em busca de modelines.
- As linhas de modelo do Emacs têm precedência sobre as do Vim quando ambas estão presentes.
- As linhas de modelo nas primeiras linhas têm prioridade sobre as que estão no final do arquivo.
