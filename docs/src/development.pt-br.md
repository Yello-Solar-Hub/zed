---
título: Desenvolvendo o Zed
descrição: "Guia para compilar e desenvolver o Zed a partir do código-fonte."
---

# Desenvolvendo o Zed

Consulte as instruções específicas para cada plataforma sobre como compilar o Zed a partir do código-fonte:

- [macOS](./development/macos.md)
- [Linux](./development/linux.md)
- [Windows](./development/windows.md)

## Acesso ao Keychain

O Zed armazena segredos no keychain do sistema.

No entanto, ao executar uma versão de desenvolvimento do Zed no macOS (e talvez em outros
(plataformas) que tentam acessar o keychain geram uma série de solicitações do keychain
que exigem que você digite sua senha repetidamente.

No macOS, isso ocorre porque a versão de desenvolvimento não possui uma identidade estável.
Mesmo que você escolha a opção “Sempre permitir”, o sistema operacional ainda solicitará que você
digite sua senha novamente na próxima vez que houver alguma alteração no arquivo binário.

Isso logo se torna irritante e prejudica a velocidade de desenvolvimento.

É por isso que, por padrão, ao executar uma compilação de desenvolvimento do Zed, uma alternativa
O provedor de credenciais é usado para contornar o keychain do sistema.

> **Observação:** Isso se aplica **apenas** às compilações de desenvolvimento. Para todas as compilações que não sejam de desenvolvimento
> Nos canais de lançamento, o keychain do sistema é sempre utilizado.

Se você precisar testar algo usando o keychain do sistema real em um
Na compilação de desenvolvimento, execute o Zed com a seguinte variável de ambiente definida:

```
ZED_DEVELOPMENT_USE_KEYCHAIN=1
```

## Medidas de desempenho

O Zed inclui um sistema de medição do tempo de renderização de quadros que pode ser usado para analisar quanto tempo leva para renderizar cada quadro. Isso é particularmente útil ao comparar o desempenho de renderização entre diferentes versões ou ao otimizar o código de renderização de quadros.

### Como usar o ZED_MEASUREMENTS

Para habilitar as medições de desempenho, defina a variável de ambiente `ZED_MEASUREMENTS`:

```sh
export ZED_MEASUREMENTS=1
```

Quando ativado, o Zed exibirá informações sobre o tempo de renderização de cada quadro no stderr, mostrando quanto tempo cada quadro leva para ser renderizado.

### Fluxo de trabalho para comparação de desempenho

Aqui está um fluxo de trabalho típico para comparar o desempenho de renderização de quadros entre diferentes versões:

1. **Ativar medidas:**

   ```sh
   export ZED_MEASUREMENTS=1
   ```

2. **Teste a primeira versão:**

   - Confira o commit que você deseja avaliar
   - Execute o Zed no modo de lançamento e utilize-o por 5 a 10 segundos: `cargo run --release &> version-a`

3. **Teste a segunda versão:**

   - Faça o checkout de outro commit que você queira comparar
   - Execute o Zed no modo de lançamento e utilize-o por 5 a 10 segundos: `cargo run --release &> version-b`

4. **Gerar comparação:**

   ```sh
   script/histogram version-a version-b
   ```

A ferramenta `script/histogram` pode aceitar quantos arquivos de medição você quiser e irá gerar uma visualização em histograma comparando os dados de desempenho de renderização de quadros entre as versões fornecidas.

### Usando `util_macros::perf`

Para realizar testes de desempenho em testes unitários, anote-os com o atributo `#[perf]` do crate `util_macros`. Em seguida, execute `cargo
`perf-test -p $CRATE` para avaliar seu desempenho. Consulte a documentação do rustdoc sobre `crates/util_macros` e `tooling/perf` para
exemplos e explicações detalhadas.

## Perfilagem ETW no Windows

O Zed oferece suporte à análise de desempenho com o Event Tracing for Windows (ETW) para capturar dados detalhados de desempenho, incluindo atividades da CPU, da GPU, da memória, do disco e de E/S de arquivos. Os dados são salvos em um arquivo `.etl`, que pode ser aberto em ferramentas padrão de análise de desempenho para análise.

As gravações do ETW podem conter informações de identificação pessoal ou sensíveis do ponto de vista da segurança, como caminhos para arquivos e chaves do Registro acessadas, bem como nomes de processos. Tenha isso em mente ao compartilhar rastreamentos com outras pessoas.

### Gravação de um traço

Abra a paleta de comandos e execute uma das seguintes opções:

- `zed: gravar rastreamento etw`: grava a atividade da CPU, da GPU, da memória e de E/S
- `zed: gravar rastreamento etw com rastreamento de heap`: inclui dados de alocação de heap para o processo Zed

O Zed solicitará que você escolha um local para salvar o arquivo `.etl` e, em seguida, pedirá permissão de administrador. Assim que a permissão for concedida, a gravação terá início.

### Salvar ou cancelar

Enquanto um rastreamento estiver sendo gravado, abra a paleta de comandos e execute uma das seguintes opções:

- `zed: salvar rastreamento etw`: interrompe a gravação e salva o rastreamento no disco
- `zed: cancelar rastreamento etw`: interrompe a gravação sem salvar

As gravações são salvas automaticamente após 60 segundos, caso não sejam interrompidas manualmente.

## Links dos colaboradores

- [CONTRIBUTING.md](https://github.com/zed-industries/zed/blob/main/CONTRIBUTING.md)
- [Depuração de falhas](./development/debugging-crashes.md)
- [Código de Conduta](https://zed.dev/code-of-conduct)
- [Licença de Colaborador do Zed](https://zed.dev/cla)
