---
título: Multibuffers — Edite vários arquivos ao mesmo tempo no Zed
descrição: Edite vários arquivos simultaneamente no Zed usando multibuffers. Combine com o recurso de múltiplos cursores para uma refatoração rápida entre arquivos.
---

# Multibuferadores

Um dos superpoderes que o Zed oferece é a capacidade de editar vários arquivos ao mesmo tempo. Quando combinado com vários cursores, isso torna as refatorações de grande porte significativamente mais rápidas.

## Edição em multibuffer

<div class="video" style="position: relative; padding-top: 71,71314741035857%;">
  <iframe
    src="https://customer-snccc0j9v3kfzkif.cloudflarestream.com/bda0a6584c19f4b39e58a263c0ae4358/iframe?muted=true&preload=true&loop=true&autoplay=true&poster=https%3A%2F% 2Fcustomer-snccc0j9v3kfzkif.cloudflarestream.com%2Fbda0a6584c19f4b39e58a263c0ae4358%2Fthumbnails%2Fthumbnail.jpg%3Ftime%3D%26height%3D600&controls=false"
    style="border: none; position: absolute; top: 0; left: 0; height: 100%; width: 100%;"
    allow="accelerometer; gyroscope; autoplay; encrypted-media; picture-in-picture;"
    allowfullscreen="true"
  ></iframe>
</div>

A edição de um multibuffer funciona da mesma forma que a edição de um arquivo normal. As alterações feitas serão refletidas nas cópias abertas desse arquivo no restante do editor, e você pode salvar todos os arquivos com {#action workspace::Save} (associado a `cmd-s` no macOS, `ctrl-s` no Windows/Linux ou `:w` no modo Vim).

Ao trabalhar em um ambiente com vários buffers, muitas vezes é útil usar vários cursores para editar todos os arquivos simultaneamente. Se você quiser editar algumas instâncias, pode selecioná-las com o mouse (`option-clique` no macOS, `alt-clique` no Windows/Linux) ou com o teclado. `cmd-d` no macOS, `ctrl-d` no Windows/Linux ou `gl` no modo Vim selecionarão a próxima ocorrência da palavra sob o cursor.

Quando quiser editar todas as ocorrências, você pode selecioná-las executando o comando {#action editor::SelectAllMatches} (`cmd-shift-l` no macOS, `ctrl-shift-l` no Windows/Linux ou `g a` no modo Vim).

## Acessando o arquivo de origem

Embora seja possível editar arquivos facilmente em um multibuffer, muitas vezes é vantajoso navegar diretamente até o arquivo-fonte. Para isso, basta clicar em qualquer uma das linhas divisórias entre os trechos ou posicionar o cursor em um trecho e executar o comando {#action editor::OpenExcerpts}. É importante observar que, caso estejam sendo usados vários cursores, o comando abrirá o arquivo-fonte posicionado sob cada cursor dentro do multibuffer.

Além disso, se você preferir usar o mouse e quiser clicar duas vezes em um trecho para abri-lo, é possível ativar essa funcionalidade com a configuração: `"double_click_in_multibuffer": "open"`.

## Pesquisa de projetos

Para iniciar uma pesquisa, execute o comando {#action pane::DeploySearch} (`cmd-shift-f` no macOS, `ctrl-shift-f` no Windows/Linux ou `g/` no modo Vim). Após a conclusão da pesquisa, os resultados serão exibidos em um novo multibuffer. Haverá um trecho para cada linha correspondente em todo o projeto.

## Diagnósticos

Se você tiver um servidor de linguagem instalado, o painel de diagnóstico pode exibir todos os erros do seu projeto. Você pode abri-lo clicando no ícone na barra de status ou executando o comando {#action diagnostics::Deploy} (`cmd-shift-m` no macOS, `ctrl-shift-m` no Windows/Linux ou `:clist` no modo Vim).

## Encontrar referências

Se você tiver um servidor de linguagem instalado, poderá localizar todas as referências ao símbolo sob o cursor com o comando {#action editor::FindAllReferences} (`cmd-clique` no macOS, `ctrl-clique` no Windows/Linux ou `g A` no modo Vim).

Dependendo do seu servidor de linguagem, comandos como {#action editor::GoToDefinition} e {#action editor::GoToTypeDefinition} também abrirão um multibuffer caso haja várias definições possíveis.
