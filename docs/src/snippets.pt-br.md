---
título: Trechos - Zed
descrição: Crie e utilize trechos de código no Zed com tabulações, espaços reservados, variáveis e acionadores no âmbito da linguagem.
---

# Trechos

Use a ação {#action snippets::ConfigureSnippets} para criar um novo arquivo de snippets ou editar um arquivo de snippets existente para um [escopo](#scopes) especificado.

Os trechos de código estão localizados no diretório `~/.config/zed/snippets`, para o qual você pode navegar usando a ação {#action snippets::OpenFolder}.

## Exemplo de configuração

```json
{
  // Each snippet must have a name and body, but the prefix and description are optional.
  // The prefix is used to trigger the snippet, but when omitted then the name is used.
  // Use placeholders like $1, $2 or ${1:defaultValue} to define tab stops.
  // The $0 determines the final cursor position.
  // Placeholders with the same value are linked.
  // If the snippet contains the $ symbol outside of a placeholder, it must be escaped with two slashes (e.g. \\$var).
  "Log to console": {
    "prefix": "log",
    "body": ["console.info(\"Hello, ${1:World}!\")", "$0"],
    "description": "Logs to console"
  }
}
```

## Âmbitos

O escopo é determinado pelo nome da linguagem em letras minúsculas, por exemplo, `python.json` para Python, `shell script.json` para Shell Script, mas há algumas exceções a essa regra:

| Âmbito      | Nome do arquivo        |
| ---------- | --------------- |
| Global     | snippets.json   |
| JSX        | javascript.json |
| Texto simples | plaintext.json  |

Para criar trechos de JSX, é preciso usar o arquivo de trechos `javascript.json`, em vez de `jsx.json`, mas isso não se aplica a TSX e TypeScript, que seguem a regra acima.

## Limitações conhecidas

- Quando é passada uma lista de prefixos, apenas o primeiro prefixo é utilizado.
- Atualmente, apenas o formato de arquivo de trechos `json` é compatível.
