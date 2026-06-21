---
título: Análise rápida e aproximada do desempenho da CPU (Flamechart)
descrição: “Análise de desempenho e otimização para o desenvolvimento do Zed.”
---

Como usar nossas ferramentas internas para otimizar e manter o Zed rápido.

# Análise rápida e aproximada do desempenho da CPU (Flamechart)

Veja em que a CPU gasta mais tempo. Recomendamos fortemente que você use
[samply](https://github.com/mstange/samply). Isso abre um perfil interativo em
o navegador (especificamente uma instância local do [firefox_profiler](https://profiler.firefox.com/)).

Consulte o arquivo README do [samply](https://github.com/mstange/samply) para saber como instalar e executar o programa.

O arquivo profile.json não contém nenhum símbolo. O profiler do Firefox pode adicionar os símbolos locais ao perfil. Para isso, clique no botão “Carregar perfil local”, no canto superior direito.

<img width="851" height="auto" alt="imagem" src="https://github.com/user-attachments/assets/cbef2b51-0442-4ee9-bc5c-95f6ccf9be2c" style="display: block; margin: 0 auto;" />

# Análise detalhada do desempenho da CPU (rastreamento)

Veja quanto tempo levou cada chamada de função anotada e quais foram seus argumentos (se
(configurado).

Anote qualquer função que você queira que apareça no perfil usando o instrumento. Para mais informações
Para mais detalhes, consulte
[tracing-instrument](https://docs.rs/tracing/latest/tracing/attr.instrument.html):

```rust
#[instrument(skip_all)]
fn should_appear_in_profile(kitty: Cat) {
    sleep(QUITE_LONG)
}
```

Em seguida, compile o Zed com `ZTRACING=1 cargo r --features tracy --release`. A compilação de versão de lançamento é opcional, mas altamente recomendada, pois, como em qualquer programa, as características de desempenho do Zed mudam drasticamente com as otimizações. Você não vai querer ficar tentando resolver lentidões que não existem na versão de lançamento.

## Configuração única/Criação do profiler:

Baixe o profiler:
[linux x86_64](https://zed-tracy-import-miniprofiler.nyc3.digitaloceanspaces.com/tracy-profiler-linux-x86_64)
[macos aarch64](https://zed-tracy-import-miniprofiler.nyc3.digitaloceanspaces.com/tracy-profiler-0.13.0-macos-aarch64)

### Alternativa: Montar você mesmo

- Clone o repositório em git@github.com:wolfpld/tracy.git
- `cd profiler && mkdir build && cd build`
- Execute o cmake para gerar os arquivos de compilação: `cmake -G Ninja -DCMAKE_BUILD_TYPE=Release ..`
- Compilar o profiler: `ninja`
- [Opcional] mova o profiler para um local adequado, como ~/.local/bin no Linux

## Uso

Abra o profiler (tracy-profiler); você deverá ver “zed” na lista de `Clientes detectados`. Clique nele.

<img width="392" height="auto" alt="imagem" src="https://github.com/user-attachments/assets/b6f06fc3-6b25-41c7-ade9-558cc93d6033" style="display: block; margin: 0 auto;"/>

O Tracy é um profiler incrivelmente poderoso, capaz de realizar diversas tarefas, mas sua interface de usuário não é muito intuitiva. Este não é o lugar para um guia detalhado sobre o Tracy; no entanto, gostaria de destacar um fluxo de trabalho específico que é útil para descobrir por que um trecho de código fica _às vezes_ lento.

Aqui estão os passos:

1. Clique no botão “flamechart” na parte superior.

<img width="1815" height="auto" alt="Clique no flamechart" src="https://github.com/user-attachments/assets/9b488c60-90fa-4013-a663-f4e35ea753d2" />

2. Clique em uma função que demora muito para ser executada.

<img width="2001" height="auto" alt="Clique na imagem" src="https://github.com/user-attachments/assets/ddb838ed-2c83-4dba-a750-b8a2d4ac6202" />

3. Expanda a lista de chamadas de função clicando na thread principal.

<img width="2313" height="auto" alt="Clique no tópico principal" src="https://github.com/user-attachments/assets/465dd883-9d3c-4384-a396-fce68b872d1a" />

4. Filtre essa lista para exibir apenas as chamadas mais lentas e, em seguida, clique em uma das chamadas lentas da lista

<img width="2264" height="auto" alt="Selecione as chamadas de cauda no histograma para filtrar a lista de chamadas e, em seguida, clique em uma chamada" src="https://github.com/user-attachments/assets/a8fddc7c-f40a-4f11-a648-ca7cc193ff6f" />

5. Clique em “zoom to zone” para ir até essa chamada de função específica na linha do tempo

<img width="1822" height="auto" alt="Clique para ampliar a área" src="https://github.com/user-attachments/assets/3391664d-7297-41d4-be17-ac9b2e2c85d1" />

6. Role a tela para ampliar e ver mais detalhes sobre quem está ligando

<img width="1964" height="auto" alt="Role para ampliar" src="https://github.com/user-attachments/assets/625c2bf4-a68d-40c4-becb-ade16bc9a8bc" />

7. Clique em um participante para obter estatísticas sobre _ele_.

<img width="1888" height="auto" alt="Clique em qualquer uma das zonas para ver as estatísticas" src="https://github.com/user-attachments/assets/7e578825-2b63-4b7f-88f7-0cb16b8a3387" />

Embora, normalmente, as barras azuis na linha do tempo do Tracy correspondam a chamadas de função, elas podem cronometrar qualquer parte de uma base de código. No exemplo abaixo, adicionamos um intervalo extra “for block in edits” e incluímos metadados nele: o block_height. Você pode fazer isso da seguinte maneira:

```rust
let span = ztracing::debug_span!("for block in edits", block_height = block.height());
let _enter = span.enter(); // span guard, when this is dropped the span ends (and its duration is recorded)
```

# Análise de desempenho de tarefas/processos assíncronos

Obtenha um perfil do executor em primeiro plano e dos executores em segundo plano do zed. Verifique se
qualquer coisa que esteja bloqueando o primeiro plano por muito tempo ou consumindo muito tempo (de processamento) em
o contexto.

O profiler fica sempre em execução em segundo plano. Você pode salvar um rastreamento a partir da interface do usuário dele ou
veja os resultados ao vivo.

## Configuração/Criação do importador:

Baixe o importador
[linux x86_64](https://zed-tracy-import-miniprofiler.nyc3.digitaloceanspaces.com/tracy-import-miniprofiler-linux-x86_64)
[mac aarch64](https://zed-tracy-import-miniprofiler.nyc3.digitaloceanspaces.com/tracy-import-miniprofiler-macos-aarch64)

### Alternativa: Montar você mesmo

- Clone o repositório em git@github.com:zed-industries/tracy.git no branch v0.12.2
- `cd import && mkdir build && cd build`
- Execute o cmake para gerar os arquivos de compilação: `cmake -G Ninja -DCMAKE_BUILD_TYPE=Release ..`
- Compilar o importador: `ninja`
- Execute o importador no arquivo de rastreamento: `./tracy-import-miniprofiler /caminho/para/trace.miniprof.json /caminho/para/output.tracy`
- Abra o traçado no Tracy:
  - Se você estiver usando o Windows, baixe a versão v0.12.2 na seção de lançamentos do repositório original
  - Se você estiver em outras plataformas, acesse o site: https://tracy.nereid.pl/ (a versão pode não ser a mesma, então o resultado pode variar; o ideal seria hospedarmos nosso próprio site...)

## Para salvar um rastrinho:

- Execute a ação: `zed open performance profiler`
- Clique no botão “Salvar”. Isso abre uma caixa de diálogo de salvamento; caso ela não seja exibida, o rastreamento será salvo no seu diretório de trabalho.
- Converta o perfil para que ele possa ser importado no Tracy usando o importador: `./tracy-import-miniprofiler <caminho para performance_profile.miniprof.json> output.tracy`
- Acesse <https://tracy.nereid.pl/>, clique no “botão liga/desliga” no canto superior esquerdo e, em seguida, abra o traçado salvo.
- Agora amplie a imagem para ver as tarefas e quanto tempo elas levaram

# Avisar se a função estiver lenta

```rust
let _timer = zlog::time!("my_function_name").warn_if_gt(std::time::Duration::from_millis(100));
```
