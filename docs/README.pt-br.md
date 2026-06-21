# Zed Docs

Bem-vindo à documentação do Zed.

Isso é gerado ao enviar o código para o branch `main` e publicado automaticamente em [https://zed.dev/docs](https://zed.dev/docs).

Para visualizar a documentação localmente, você precisará instalar o [mdBook](https://rust-lang.github.io/mdBook/) (`cargo install mdbook@0.4.40`), gerar os metadados da ação e, em seguida, disponibilizá-la:

```sh
script/generate-action-metadata
mdbook serve docs
```

O primeiro comando gera um manifesto de ações no arquivo `crates/docs_preprocessor/actions.json`. Sem ele, o pré-processador não consegue validar as referências a atalhos de teclado e ações na documentação e exibirá erros. Você só precisa executá-lo novamente quando houver alterações nas ações.

É importante observar o número da versão acima. Por alguma razão desconhecida, a partir de 23/04/2025, a execução da versão 0.4.48 causará um comportamento anômalo nas URLs, o que pode causar falhas no sistema.

Antes de fazer o commit, verifique se os documentos estão formatados da maneira que o Prettier espera, usando:

```
cd docs && pnpm dlx prettier@3.5.0 . --write && cd ..
```

## Pré-processador

Temos um pré-processador mdBook personalizado para fazer a interface com nossos crates (`crates/docs_preprocessor`).

Se, por algum motivo, você precisar ignorar o pré-processador de documentação, pode comentar a linha `[preprocessor.zed_docs_preprocessor]` no arquivo `book.toml`.

## Imagens e vídeos

Para adicionar imagens ou vídeos aos documentos, faça o upload deles para outro local (por exemplo, zed.dev, o armazenamento de recursos do GitHub) e, em seguida, insira os links para eles nos documentos.

Colocar ativos binários, como imagens, no repositório Git aumentará o tamanho do repositório com o passar do tempo.

## Notas internas:

- Temos um roteador do Cloudflare chamado `docs-proxy` que intercepta as solicitações para `zed.dev/docs` e as encaminha para o projeto “docs” do Cloudflare Pages.
- O CI faz o upload de uma nova versão para o projeto Cloudflare Pages a partir do arquivo `.github/workflows/deploy_docs.yml` sempre que houver um push para o branch `main`.

### Índice

Os arquivos do índice (`theme/page-toc.js` e `theme/page-doc.css`) foram gerados inicialmente pelo [`mdbook-pagetoc`](https://crates.io/crates/mdbook-pagetoc).

Como tudo o que esse pré-processador faz é gerar os recursos estáticos, não precisamos mantê-lo ativo depois que eles forem gerados.

## Referências a atalhos de teclado e ações

Ao se referir a combinações de teclas ou ações, use os seguintes formatos:

### Atribuições de teclas

{#kb scope::Action} — por exemplo, {#kb zed::OpenSettings}.

Isso gerará um elemento de código semelhante a: `<code>Cmd + , | Ctrl + ,</code>`. Em seguida, usamos um plug-in do lado do cliente para exibir a combinação de teclas real, de acordo com a plataforma do usuário.

Ao usar o nome da ação, podemos garantir que a combinação de teclas esteja sempre atualizada, em vez de codificá-la de forma rígida.

#### Sobreposições de mapa de teclas

`{#kb:keymap_name scope::Action}` — por exemplo, `{#kb:jetbrains editor::GoToDefinition}`.

Isso resolve primeiro a combinação de teclas a partir de uma sobreposição de mapa de teclas (por exemplo, JetBrains) e, caso a sobreposição não defina uma combinação para essa ação, recorre ao mapa de teclas padrão. Isso é útil para seções em que a documentação espera que um mapa de teclas básico específico esteja configurado.

Overlays compatíveis: `jetbrains`.

### Ações

{#action scope::Action} — por exemplo, {#action zed::OpenSettings}.

Isso exibirá uma versão legível para o usuário do nome da ação, por exemplo, “zed: abrir configurações”, e nos permitirá implementar recursos como contexto adicional ao passar o mouse, etc.

### Criação de novos modelos

Os modelos são funções que modificam o código-fonte das páginas da documentação (geralmente por meio de uma correspondência e substituição com expressões regulares).
Você pode ver como as ações e as combinações de teclas estão definidas em modelos no arquivo `crates/docs_preprocessor/src/main.rs` para ter uma referência sobre como criar novos modelos.

## Banner de consentimento

Nós pré-empacotamos o pacote `c15t` porque o pipeline de documentação não inclui um empacotador de JS. Se você precisar atualizar o `c15t` e recompilar o pacote, use:

```
mkdir c15t-bundle && cd c15t-bundle
npm init -y
npm install c15t@<version> esbuild
echo "import { getOrCreateConsentRuntime } from 'c15t'; window.c15t = { getOrCreateConsentRuntime };" > entry.js
npx esbuild entry.js --bundle --format=iife --minify --outfile=c15t@<version>.js
cp c15t@<version>.js ../theme/c15t@<version>.js
cd .. && rm -rf c15t-bundle
```

Substitua `<version>` pela nova versão do `c15t` que você está instalando. Em seguida, atualize o arquivo `book.toml` para referenciar o novo nome do pacote.

### Referências

- Característica do modelo: `crates/docs_preprocessor/src/templates.rs`
- Modelo de exemplo: `crates/docs_preprocessor/src/templates/keybinding.rs`
- Plug-ins do lado do cliente: `docs/theme/plugins.js`

## Pós-processador

Um pós-processador é implementado como um subcomando do `docs_preprocessor` que envolve o renderizador HTML integrado e aplica pós-processamento aos arquivos HTML, a fim de adicionar suporte a valores de título e descrição da tag `meta` específicos para cada página.

Um exemplo da sintaxe pode ser encontrado no arquivo `git.md`, bem como a seguir:

```md
---
title: Some more detailed title for this page
description: A page-specific description
---

# Editor
```

O código acima será transformado em (com as tags irrelevantes removidas):

```html
<head>
  <title>Editor | Some more detailed title for this page</title>
  <meta name="description" contents="A page-specific description" />
</head>
<body>
  <h1>Editor</h1>
</body>
```

Se não forem fornecidos dados preliminares, ou se uma ou ambas as chaves não forem fornecidas, os campos `title` e `description` serão definidos com base nas chaves `default-title` e `default-description` no arquivo `book.toml`, respectivamente.

### Detalhes da implementação

Infelizmente, o mdBook não oferece suporte ao pós-processamento da mesma forma que ao pré-processamento, e permite definir apenas uma descrição para ser inserida na tag `meta` por livro, e não por arquivo.
Portanto, para aplicar o pós-processamento (necessário para modificar as tags `head` do HTML), a descrição global do livro é definida como um valor marcador `#description#` e o renderizador de HTML é substituído por um subcomando do `docs_preprocessor` que envolve o renderizador de HTML integrado e aplica o pós-processamento aos arquivos HTML, substituindo o valor marcador e o `<title>(.*) </title> pelo conteúdo da seção introdutória, caso exista.

### Limitações conhecidas

A análise do cabeçalho é extremamente simples, o que evita a necessidade de incluir uma dependência adicional ou de implementar uma análise completa de YAML.

- Aspas duplas e valores com várias linhas não são permitidos; ou seja, as chaves e os valores devem estar inteiramente na mesma linha, sem aspas duplas ao redor do valor.

O seguinte não funcionará:

```md
---
title: Some
  Multi-line
  Title
---
```

nem isto:

```md
---
title: "Some title"
---
```

- As páginas preliminares devem estar no início do arquivo, precedidas apenas por espaços em branco.
- O conteúdo dos campos `title` e `description` não passará por escapamento HTML. Eles devem conter apenas texto ASCII simples, sem caracteres Unicode ou emojis.
