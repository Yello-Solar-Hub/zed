---
título: Autocompletamento de código - Zed
descrição: Autocompletamento de código do Zed a partir de servidores de linguagem e previsões de edição. Configure o comportamento do autocompletamento, os trechos de código e a exibição da documentação.
---

# Conclusões

O Zed oferece suporte a duas fontes para sugestões de autocompletar:

1. "Autocompletamento de código" fornecido por servidores de linguagem (LSPs) instalados automaticamente pelo Zed ou por meio das [Extensões de Linguagem do Zed](languages.md).
2. "Editar previsões" fornecidas pelo próprio modelo Zeta da Zed ou por provedores externos, como o [GitHub Copilot](#github-copilot).

## Autocompletamento de código do servidor de linguagem {#code-completions}

Quando houver um servidor de linguagem adequado disponível, o Zed fornecerá sugestões de nomes de variáveis, funções e outros símbolos no arquivo atual. Você pode desativar essas sugestões adicionando o seguinte ao seu arquivo `settings.json` do Zed:

```json [settings]
"show_completions_on_input": false
```

Você pode ativar manualmente as sugestões de autocompletamento com `ctrl-space` ou acionando a ação `editor::ShowCompletions` na paleta de comandos.

> Observação: para usar `ctrl-space` no Zed, é necessário desativar o atalho global do macOS.
> Abra **Configurações do sistema** > **Teclado** > **Atalhos de teclado** >
> **Fontes de entrada** e desmarque a opção **Selecionar a fonte de entrada anterior**.

Para mais informações, consulte:

- [Configurando os idiomas suportados](./configuring-languages.md)
- [Lista de idiomas suportados pelo Zed](./languages.md)

## Editar previsões {#edit-predictions}

O Zed possui suporte integrado para prever várias edições ao mesmo tempo [por meio do Zeta](https://huggingface.co/zed-industries/zeta), o modelo de código aberto e dados abertos do Zed.
As sugestões de edição aparecem à medida que você digita e, na maioria das vezes, você pode aceitá-las pressionando a tecla `tab`.

Consulte a [documentação sobre previsões de edição](./ai/edit-prediction.md) para obter mais informações sobre como instalar e configurar as previsões de edição do Zed.
