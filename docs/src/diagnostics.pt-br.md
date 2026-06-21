---
título: Diagnóstico — Erros e avisos no Zed
descrição: Visualize e navegue por erros, avisos e diagnósticos de código provenientes de servidores de linguagem no Zed.
---

# Diagnósticos

O Zed obtém seus diagnósticos dos servidores de linguagem e oferece suporte tanto à variante “push” quanto à variante “pull” do LSP, o que o torna compatível com todos os servidores de linguagem existentes.

# Diagnósticos regulares

Por padrão, o Zed exibe todos os diagnósticos como texto sublinhado no editor e na barra de rolagem.

Os diagnósticos do editor poderiam ser filtrados com o

```json [settings]
"diagnostics_max_severity": null
```

configuração do editor (valores possíveis: `"off"`, `"error"`, `"warning"`, `"info"`, `"hint"`, `null` (padrão, todos os diagnósticos)).

As barras de rolagem são configuradas com o

```json [settings]
"scrollbar": {
  "diagnostics": "all",
}
```

configuração (valores possíveis: `"none"`, `"error"`, `"warning"`, `"information"`, `"all"` (padrão))

É possível passar o cursor sobre os diagnósticos para exibir uma dica de ferramenta com a mensagem de diagnóstico completa e renderizada.
Ou então, os comandos `editor::GoToDiagnostic` e `editor::GoToPreviousDiagnostic` poderiam ser usados para navegar entre os diagnósticos no editor, exibindo uma janela pop-up para o diagnóstico ativo no momento.

# Diagnóstico em tempo real (Lente de erros)

O Zed permite exibir informações de diagnóstico como uma janela à direita do código.
Essa opção está desativada por padrão, mas pode ser ativada (ou desativada) temporariamente pelo menu do editor ou permanentemente, usando o

```json [settings]
"diagnostics": {
  "inline": {
    "enabled": true,
    "max_severity": null, // same values as the `diagnostics_max_severity` from the editor settings
  }
}
```

# Outros locais da interface do usuário

## Painel do Projeto

As entradas do painel do projeto podem ser coloridas de acordo com a gravidade dos diagnósticos contidos no arquivo.

Para configurar, use

```json [settings]
"project_panel": {
  "show_diagnostics": "all",
}
```

configuração (valores possíveis: `"off"`, `"errors"`, `"all"` (padrão))

## Guias do editor

Assim como no painel do projeto, as guias do editor podem ser coloridas com o

```json [settings]
"tabs": {
  "show_diagnostics": "off",
}
```

configuração (valores possíveis: `"off"` (padrão), `"errors"`, `"all"`)
