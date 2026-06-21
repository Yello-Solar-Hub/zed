---
título: REPL - Kernels do Jupyter em Zed
descrição: Execute código de forma interativa no Zed com suporte integrado ao kernel do Jupyter. Execute Python, TypeScript, R e outras linguagens diretamente no código.
---

# REPL

## Introdução

O REPL integrado do Zed utiliza [kernels do Jupyter](https://docs.jupyter.org/en/latest/projects/kernels.html), permitindo que você execute código de forma interativa em arquivos comuns de editor.

<figure style="width: 100%; margin: 0; overflow: hidden; border-top-left-radius: 2px; border-top-right-radius: 2px;">
    <controles de repetição do vídeo playsinline>
        <fonte
            src="https://customer-snccc0j9v3kfzkif.cloudflarestream.com/aec66e79f23d6d1a0bee5e388a3f17cc/downloads/default.mp4"
            type='video/webm; codecs="vp8.0, vorbis"'
        />
        <fonte
            src="https://customer-snccc0j9v3kfzkif.cloudflarestream.com/aec66e79f23d6d1a0bee5e388a3f17cc/downloads/default.mp4"
            type='video/mp4; codecs="avc1.4D401E, mp4a.40.2"'
        />
        <fonte
          src="https://zed.dev/img/post/repl/typescript-deno-kernel-markdown.png"
          type="image/png"
        />
    </video>
</figure>

## Instalação

O Zed permite a execução de código em várias linguagens. Para começar, é necessário instalar um kernel para a linguagem que você deseja usar.

**Idiomas atualmente suportados:**

- [Python (ipykernel)](#python)
- [TypeScript (Deno)](#typescript-deno)
- [R (Ark)](#r-ark)
- [R (Xeus)](#r-xeus)
- [Julia](#julia)
- [Scala (Almond)](#scala)

Depois de instalados, você pode começar a usar o REPL nos arquivos da respectiva linguagem ou em outros locais onde essas linguagens sejam compatíveis, como no Markdown. Se você adicionou os kernels recentemente, execute o comando {#action repl::RefreshKernelspecs} para disponibilizá-los no editor.

## Como usar o REPL

Para iniciar o REPL, abra um arquivo com a linguagem que deseja usar e utilize o comando {#action repl::Run} (padrão: `ctrl-shift-enter` no macOS) para executar um bloco, uma seleção ou uma linha. Você também pode clicar no ícone do REPL na barra de ferramentas.

O comando {#action repl::Run} será executado na(s) sua(s) seleção(ões), e o resultado será exibido abaixo da seleção.

As saídas podem ser apagadas com o comando {#action repl::ClearOutputs} ou pelo menu do REPL na barra de ferramentas.

### Modo celular

O Zed oferece suporte a [notebooks como scripts](https://jupytext.readthedocs.io/en/latest/formats-scripts.html) usando o separador de células `# %%` em Python e `// %%` em TypeScript. Isso permite que você escreva código em um único arquivo e o execute como se fosse um notebook, célula por célula.

O comando {#action repl::Run} executará cada bloco de código entre os marcadores `# %%` como uma célula separada.

```python
# %% Cell 1
import time
import numpy as np

# %% Cell 2
import matplotlib.pyplot as plt
import matplotlib.pyplot as plt
from matplotlib import style
style.use('ggplot')
```

## Instruções específicas para cada idioma

### Python

#### Contexto global

<div class="warning">

No macOS, o Python do sistema _não_ funcionará. Configure o [pyenv](https://github.com/pyenv/pyenv?tab=readme-ov-file#installation) ou use um ambiente virtual.

</div>

Para configurar sua versão atual do Python de modo que um kernel fique disponível, execute:

```sh
pip install ipykernel
python -m ipykernel install --user
```

#### Ambiente Conda

```sh
source activate myenv
conda install ipykernel
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

#### Virtualenv com o pip

```sh
source activate myenv
pip install ipykernel
python -m ipykernel install --user --name myenv --display-name "Python (myenv)"
```

### R (Ark Kernel) {#r-ark}

Instale o [Ark](https://github.com/posit-dev/ark/releases) baixando a versão compatível com o seu sistema operacional. Por exemplo, no macOS, basta descompactar o binário `ark` e colocá-lo em `/usr/local/bin`. Em seguida, execute:

```sh
ark --install
```

### R (Xeus Kernel) {#r-xeus}

- Instale o [Xeus-R](https://github.com/jupyter-xeus/xeus-r)
- Instale a extensão R para o Zed (procure por `R` nas extensões do Zed)

<!--
A definir: Melhorar as instruções do R REPL (Ark Kernel)
-->

### TypeScript: Deno {#typescript-deno}

- [Instale o Deno](https://docs.deno.com/runtime/manual/getting_started/installation/) e, em seguida, instale o kernel Jupyter do Deno:

```sh
deno jupyter --install
```

<!--
A definir: Melhorar as instruções do R REPL (Ark Kernel)
-->

### Júlia

- Baixe e instale o Julia no [site oficial](https://julialang.org/downloads/).
- Instale a extensão Julia para o Zed (procure por `Julia` nas extensões do Zed)

<!--
A definir: Melhorar as instruções do REPL do Julia
-->

### Scala

- [Instale o Scala](https://www.scala-lang.org/download/) com o comando `cs setup` (Coursier):
  - `brew install coursier/formulas/coursier && cs setup`
- REPL (Almond) [instruções de configuração](https://almond.sh/docs/quick-start-install):
  - `brew install --cask temurin` (binários oficiais do OpenJDK da Eclipse Foundation)
  - `brew install coursier/formulas/coursier && cs setup`
  - `coursier launch --use-bootstrap almond -- --install`

## Alterar o kernel utilizado por idioma {#changing-kernels}

O Zed detecta automaticamente os kernels disponíveis e os organiza no seletor de kernels:

- **Recomendado**: O ambiente Python compatível com sua cadeia de ferramentas ativa (se detectado)
- **Ambientes Python**: Ambientes virtuais (venv, virtualenv, Poetry, Pipenv, Conda, uv, etc.)
- **Kernels do Jupyter**: Especificações dos kernels do Jupyter instalados
- **Servidores remotos**: Servidores Jupyter remotos conectados

### Instalando o ipykernel

Os ambientes Python aparecem no seletor mesmo que o ipykernel não esteja instalado. Os ambientes que não possuem o ipykernel aparecem esmaecidos e com a mensagem “ipykernel não instalado”. Ao selecionar um deles, o Zed executa automaticamente o comando `pip install ipykernel` nesse ambiente e o ativa assim que a instalação for concluída.

### Como o Zed recomenda kernels

Quando você executa um código, o Zed seleciona um kernel automaticamente:

1. **Correspondência com a cadeia de ferramentas ativa**: Se um ambiente Python corresponder à sua cadeia de ferramentas ativa e tiver o ipykernel, o Zed o utilizará
2. **Primeiro ambiente Python disponível**: Caso contrário, o primeiro ambiente Python com o ipykernel
3. **Solução alternativa com base na linguagem**: Se não houver ambientes Python disponíveis, o Zed seleciona um kernel do Jupyter compatível com a linguagem do bloco de código

Você pode ignorar essa configuração selecionando explicitamente um kernel no seletor.

### Definição de kernels padrão

Para configurar um kernel padrão diferente para um idioma, você pode atribuir um kernel a qualquer idioma compatível no seu arquivo `settings.json`:

```json [settings]
{
  "jupyter": {
    "kernel_selections": {
      "python": "conda-env",
      "typescript": "deno",
      "javascript": "deno",
      "r": "ark"
    }
  }
}
```

## Entrada interativa

Quando a execução do código requer uma entrada do usuário (como a função `input()` do Python), o REPL exibe um prompt de entrada abaixo da saída da célula.

Digite sua resposta no campo de texto e pressione `Enter` para enviar. O kernel recebe sua entrada e continua a execução.

Ao digitar senhas, os caracteres aparecem ocultos por asteriscos por motivos de segurança.

Se a execução for interrompida enquanto um prompt de entrada estiver ativo, o prompt será automaticamente apagado quando o kernel retornar ao estado de inatividade.

## Depuração de Kernelspecs

Os kernels disponíveis são exibidos por meio do comando {#action repl::Sessions}. Para atualizar os kernels que você pode executar, use o comando {#action repl::RefreshKernelspecs}.

Se você tiver o `jupyter` instalado, pode executar o comando `jupyter kernelspec list` para ver os kernels disponíveis.

```sh
$ jupyter kernelspec list
Available kernels:
  ark                   /Users/z/Library/Jupyter/kernels/ark
  conda-base            /Users/z/Library/Jupyter/kernels/conda-base
  deno                  /Users/z/Library/Jupyter/kernels/deno
  python-chatlab-dev    /Users/z/Library/Jupyter/kernels/python-chatlab-dev
  python3               /Users/z/Library/Jupyter/kernels/python3
  ruby                  /Users/z/Library/Jupyter/kernels/ruby
  rust                  /Users/z/Library/Jupyter/kernels/rust
```

> Observação: O Zed utiliza, na medida do possível, `sys.prefix` e `CONDA_PREFIX` para localizar kernels em ambientes Python. Se você quiser controlar isso explicitamente, execute `python -m ipykernel install --user --name myenv --display-name "Python (myenv)"` para instalar o kernel diretamente no ambiente.
