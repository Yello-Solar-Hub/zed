---
título: Tokens semânticos e destaque de sintaxe - Zed
descrição: Ative e configure o realce semântico de tokens no Zed para obter uma coloração de sintaxe mais rica e compatível com servidores de linguagem.
---

# Tokens semânticos

Os tokens semânticos oferecem um realce de sintaxe mais detalhado, utilizando informações de servidores de linguagem. Ao contrário do realce do Tree-Sitter, que se baseia exclusivamente na sintaxe, os tokens semânticos compreendem o significado do seu código — distinguindo entre variáveis locais e parâmetros, ou entre uma definição de classe e uma referência a uma classe.

## Ativação de tokens semânticos

Os tokens semânticos são controlados pela configuração `semantic_tokens`. Por padrão, os tokens semânticos estão desativados.

```json [settings]
{
  "semantic_tokens": "combined"
}
```

Essa configuração aceita três valores:

| Valor        | Descrição                                                                                                                                                 |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `"desligado"`      | Não solicitar tokens semânticos de servidores de idiomas. Utiliza apenas o realce do Tree-Sitter. (Padrão)                                                         |
| `"combinado"` | Use os tokens semânticos do LSP em conjunto com o realce do Tree-Sitter. O Tree-Sitter fornece o realce básico, e os tokens semânticos sobrepõem informações adicionais. |
| `"completo"`     | Utilize exclusivamente tokens semânticos LSP. O destaque do Tree-sitter está totalmente desativado para buffers com suporte a tokens semânticos.                                 |

Você pode configurar isso globalmente ou por idioma:

```json [settings]
{
  "semantic_tokens": "off",
  "languages": {
    "Rust": {
      "semantic_tokens": "combined"
    },
    "TypeScript": {
      "semantic_tokens": "full"
    }
  }
}
```

> **Observação:** A alteração do modo `semantic_tokens` pode exigir que o servidor de linguagem seja reiniciado para que a mudança entre em vigor. Use o comando {#action editor::RestartLanguageServer} da paleta de comandos caso o realce não seja atualizado imediatamente.

## Personalização das cores dos tokens

Os tokens semânticos são formatados por meio de regras que mapeiam os tipos de tokens e modificadores do LSP para estilos de tema ou cores personalizadas. O Zed oferece configurações padrão adequadas, mas você pode personalizá-las no seu settings.json: adicione regras sob a chave `global_lsp_settings.semantic_token_rules`.

As regras são comparadas em ordem, e a primeira regra que corresponder é a que prevalece.
As regras definidas pelo usuário têm a maior prioridade, seguidas pelas regras de idioma fornecidas pela extensão e, em seguida, pelos padrões do Zed.

### Estrutura das regras

Cada regra pode especificar:

| Imóvel           | Descrição                                                                                                        |
| ------------------ | ------------------------------------------------------------------------------------------------------------------ |
| `token_type`       | O tipo de token semântico LSP a ser correspondido (por exemplo, `"variável"`, `"função"`, `"classe"`). Se omitido, corresponde a todos os tipos. |
| `token_modifiers`  | Uma lista de modificadores que devem estar todos presentes (por exemplo, `["declaration"]`, `["readonly", "static"]`).                  |
| `estilo`            | Uma lista de nomes de estilos de tema para testar. É utilizado o primeiro encontrado no tema atual.                              |
| `foreground_color` | Substituir a cor de primeiro plano no formato hexadecimal (por exemplo, `"#ff0000"`).                                                       |
| `cor_de_fundo` | Substituir a cor de fundo no formato hexadecimal.                                                                           |
| `sublinhado`        | Valor booleano ou código hexadecimal de cor. Se for `true`, sublinha com a cor do texto.                                                   |
| `riscado`    | Cor booleana ou hexadecimal. Se for `true`, o texto será riscado com a cor do texto.                                              |
| `font_weight`      | `"normal"` ou `"negrito"`.                                                                                            |
| `font_style`       | `"normal"` ou `"itálico"`.                                                                                          |

### Exemplo: Destacando referências não resolvidas

Para destacar as referências não resolvidas:

```json [settings]
{
  "global_lsp_settings": {
    "semantic_token_rules": [
      {
        "token_type": "unresolvedReference",
        "foreground_color": "#c93f3f",
        "font_weight": "bold"
      }
    ]
  }
}
```

### Exemplo: Destacando código inseguro

Para destacar operações inseguras no Rust:

```json [settings]
{
  "global_lsp_settings": {
    "semantic_token_rules": [
      {
        "token_type": "punctuation",
        "token_modifiers": ["unsafe"],
        "foreground_color": "#AA1111",
        "font_weight": "bold"
      }
    ]
  }
}
```

### Exemplo: Como usar os estilos do tema

Em vez de definir as cores diretamente no código, use os estilos do seu tema como referência:

```json [settings]
{
  "global_lsp_settings": {
    "semantic_token_rules": [
      {
        "token_type": "variable",
        "token_modifiers": ["mutable"],
        "style": ["variable.mutable", "variable"]
      }
    ]
  }
}
```

É utilizado o primeiro estilo encontrado no tema atual, oferecendo opções alternativas.

### Exemplo: Desativando um tipo de token

Para desativar o destaque de um tipo específico de token, adicione uma regra vazia que corresponda a ele:

```json [settings]
{
  "global_lsp_settings": {
    "semantic_token_rules": [
      {
        "token_type": "comment"
      }
    ]
  }
}
```

Como as regras do usuário têm prioridade máxima e a primeira correspondência prevalece, essa regra vazia impede que qualquer estilo seja aplicado aos tokens de comentário.

## Regras padrão

As regras padrão de tokenização semântica do Zed mapeiam os tipos de tokens padrão do LSP para estilos temáticos comuns. Por exemplo:

- estilo `função` → `função`
- `variável` com o modificador `constante` → estilo `constante`
- estilo `class` → `type.class`, `class` ou `type` (o primeiro encontrado)
- `comment` com o modificador `documentation` → estilo `comment.documentation` ou `comment.doc`

A configuração padrão completa pode ser exibida no Zed com o comando {#action zed::ShowDefaultSemanticTokenRules}.

## Tipos padrão de tokens

Os servidores de idiomas relatam tokens utilizando tipos padronizados. Entre os tipos mais comuns estão:

| Tipo            | Descrição                        |
| --------------- | ---------------------------------- |
| `namespace`     | Nomes de namespace ou de módulo          |
| `tipo`          | Nomes de tipos                         |
| `classe`         | Nomes das classes                        |
| `enum`          | Nomes de tipos de enumeração                    |
| `interface`     | Nomes de interfaces                    |
| `struct`        | Nomes de estruturas                       |
| `typeParameter` | Parâmetros de tipo genérico            |
| `parâmetro`     | Parâmetros de função/método         |
| `variável`      | Nomes de variáveis                     |
| `propriedade`      | Propriedades de objetos ou campos de estruturas |
| `enumMember`    | Variantes de enumeração                      |
| `função`      | Nomes de funções                     |
| `método`        | Nomes de métodos                       |
| `macro`         | Nomes de macros                        |
| `palavra-chave`       | Palavras-chave de linguagem                  |
| `comentário`       | Comentários                           |
| `string`        | Literais de string                    |
| `número`        | Literais numéricos                   |
| `operador`      | Operadores                          |

Os modificadores comuns incluem: `declaration`, `definition`, `readonly`, `static`, `deprecated`, `async`, `documentation`, `defaultLibrary` e modificadores específicos de cada linguagem, como `unsafe` (Rust) ou `abstract` (TypeScript).

Para consultar a especificação completa, veja a [documentação sobre tokens semânticos do LSP](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#semanticTokenTypes).

## Inspeção de tokens semânticos

Para ver os tokens semânticos aplicados ao seu código em tempo real, use o comando {#action dev::OpenHighlightsTreeView} da paleta de comandos. Isso abre um painel que mostra todos os destaques (incluindo tokens semânticos) do buffer atual, facilitando a compreensão de quais tokens estão sendo aplicados e a depuração de suas regras personalizadas.

## Solução de problemas

### O destaque semântico não está aparecendo

1. Certifique-se de que `semantic_tokens` esteja definido como `"combined"` ou `"full"` para o idioma
2. Verifique se o servidor de idiomas suporta tokens semânticos (nem todos suportam)
3. Tente reiniciar o servidor de idiomas com {#action editor::RestartLanguageServer}
4. Verifique se há erros nos logs do LSP ({#action dev::OpenLanguageServerLogs})

### As cores não são atualizadas após a alteração das configurações

Alterações no modo `semantic_tokens` podem exigir a reinicialização do servidor de linguagem. Use {#action editor::RestartLanguageServer} na paleta de comandos.

### Os estilos do tema não estão sendo aplicados

Certifique-se de que os nomes dos estilos em suas regras correspondam aos estilos definidos no seu tema. O array `style` oferece opções alternativas — se o primeiro estilo não for encontrado, o Zed tenta o próximo.
