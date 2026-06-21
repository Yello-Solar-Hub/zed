---
título: Zed no Linux
descrição: "O script de instalação disponível na página de download é a maneira mais rápida de instalar o Zed:"
---

# Zed no Linux

## Instalação padrão

O script de instalação disponível na página [download](https://zed.dev/download) é a maneira mais rápida de instalar o Zed:

```sh
curl -f https://zed.dev/install.sh | sh
```

Também oferecemos uma versão prévia do Zed, que recebe atualizações cerca de uma semana antes da versão estável. Você pode instalá-la com:

```sh
curl -f https://zed.dev/install.sh | ZED_CHANNEL=preview sh
```

O Zed instalado pelo script funciona melhor em sistemas que:

- ter uma GPU compatível com Vulkan disponível (por exemplo, no Linux em um MacBook da série M)
- ter uma glibc para todo o sistema
  - x86_64 (Intel/AMD): versão da glibc >= 2.31 (Ubuntu 20 e versões mais recentes)
  - aarch64 (ARM): versão da glibc >= 2.35 (Ubuntu 22 e versões mais recentes)

O NixOS não possui uma glibc para todo o sistema por padrão. Se você quiser usar nossas compilações no NixOS, elas podem funcionar se você instalar uma camada de compatibilidade com a glibc, como o [nix-ld](https://github.com/Mic92/nix-ld).

Você precisará compilar a partir do código-fonte para:

- arquiteturas diferentes das de 64 bits Intel ou 64 bits ARM (por exemplo, uma máquina de 32 bits ou RISC-V)
- Red Hat Enterprise Linux 8.x, Rocky Linux 8, AlmaLinux 8 e Amazon Linux 2 em todas as arquiteturas
- Red Hat Enterprise Linux 9.x, Rocky Linux 9.3, AlmaLinux 8, Amazon Linux 2023 na arquitetura aarch64 (x86_x64 compatível)

## Outras formas de instalar o Zed no Linux

O Zed é um software de código aberto, e [você pode instalá-lo a partir do código-fonte](./development/linux.md).

### Instalação por meio de um gerenciador de pacotes

Existem vários pacotes do Zed de terceiros para diversas distribuições Linux e gerenciadores de pacotes, às vezes com o nome `zed-editor`. A disponibilidade varia de acordo com a distribuição, mas talvez você consiga instalar o Zed usando um desses pacotes:

- Arch: [`zed`](https://archlinux.org/packages/extra/x86_64/zed/)
- Arch (AUR): [`zed-git`](https://aur.archlinux.org/packages/zed-git), [`zed-preview`](https://aur.archlinux.org/packages/zed-preview), [`zed-preview-bin`](https://aur.archlinux.org/packages/zed-preview-bin)
- Fedora/Ultramarine (Terra): [`zed`](https://github.com/terrapkg/packages/tree/frawhide/anda/devs/zed/stable), [`zed-preview`](https://github.com/terrapkg/packages/tree/frawhide/anda/devs/zed/preview), [`zed-nightly`](https://github.com/terrapkg/packages/tree/frawhide/anda/devs/zed/nightly)
- Manjaro: [`zed`](https://manjaristas.org/branch_compare?q=zed)
- Conda: [`zed`](https://anaconda.org/conda-forge/zed)
- Nix: `zed-editor` ([instável](https://search.nixos.org/packages?channel=unstable&show=zed-editor))
- Solus: [`zed`](https://github.com/getsolus/packages/tree/main/packages/z/zed)
- Parabola: [`zed`](https://www.parabola.nu/packages/extra/x86_64/zed/)
- ALT Linux (Sisyphus): [`zed`](https://packages.altlinux.org/en/sisyphus/srpms/zed/)
- AOSC OS: [`zed`](https://packages.aosc.io/packages/zed)
- Flathub: [`dev.zed.Zed`](https://flathub.org/apps/dev.zed.Zed)

Consulte [Repology](https://repology.org/project/zed-editor/versions) para obter uma lista atualizada dos pacotes Zed disponíveis em vários repositórios.

### Comunidade

Ao instalar um pacote de terceiros, esteja ciente de que ele pode não estar totalmente atualizado e pode apresentar pequenas diferenças em relação ao Zed que nós fornecemos (uma alteração comum é renomear o binário para `zedit` ou `zeditor` para evitar conflitos com outros pacotes).

Adoraríamos contar com a sua ajuda para tornar o Zed disponível para todos. Se o Zed ainda não estiver disponível para o seu gerenciador de pacotes e você quiser resolver isso, temos algumas orientações sobre [como fazer isso](./development/linux.md#notes-for-packaging-zed).

Os pacotes desta seção oferecem instalações binárias do Zed, mas não são pacotes oficiais das distribuições associadas. Esses pacotes são mantidos por membros da comunidade e, por isso, é preciso ter um maior cuidado ao instalá-los.

#### Debian e Ubuntu

O Zed está disponível neste [repositório mantido pela comunidade](https://debian.griffo.io/).

As instruções para cada versão estão disponíveis no arquivo README do repositório onde os pacotes são compilados.
As instruções de compilação, empacotamento e uso para cada versão estão disponíveis no arquivo README do [repositório](https://github.com/dariogriffo/zed-debian)

### Download manual

Se preferir, você pode instalar o Zed baixando nosso arquivo .tar.gz pré-compilado. Trata-se do mesmo pacote que nosso script de instalação utiliza, mas você pode personalizar o local da instalação seguindo as instruções abaixo:

Baixe o arquivo `.tar.gz`:

- [zed-linux-x86_64.tar.gz](https://cloud.zed.dev/releases/stable/latest/download?asset=zed&arch=x86_64&os=linux&source=docs)
  ([visualização](https://cloud.zed.dev/releases/preview/latest/download?asset=zed&arch=x86_64&os=linux&source=docs))
- [zed-linux-aarch64.tar.gz](https://cloud.zed.dev/releases/stable/latest/download?asset=zed&arch=aarch64&os=linux&source=docs)
  ([visualização](https://cloud.zed.dev/releases/preview/latest/download?asset=zed&arch=aarch64&os=linux&source=docs))

Em seguida, certifique-se de que o binário `zed` contido no arquivo tar esteja no seu caminho. A maneira mais fácil é descompactar o arquivo tar e criar um link simbólico:

```sh
mkdir -p ~/.local
# extract zed to ~/.local/zed.app/
tar -xvf <path/to/download>.tar.gz -C ~/.local
# link the zed binary to ~/.local/bin (or another directory in your $PATH)
ln -sf ~/.local/zed.app/bin/zed ~/.local/bin/zed
```

Se você deseja integrar o programa a um ambiente de área de trabalho compatível com XDG, também precisará instalar o arquivo `.desktop`:

```sh
install -D ~/.local/zed.app/share/applications/dev.zed.Zed.desktop -t ~/.local/share/applications
sed -i "s|Icon=zed|Icon=$HOME/.local/zed.app/share/icons/hicolor/512x512/apps/zed.png|g" ~/.local/share/applications/dev.zed.Zed.desktop
sed -i "s|Exec=zed|Exec=$HOME/.local/zed.app/bin/zed|g" ~/.local/share/applications/dev.zed.Zed.desktop
```

## Desinstalando o Zed

### Desinstalação padrão

Se o Zed tiver sido instalado usando o script de instalação padrão, ele pode ser desinstalado fornecendo o parâmetro `--uninstall` ao comando `zed` do shell

```sh
zed --uninstall
```

Se não houver erros, o shell perguntará se você deseja manter suas preferências ou excluí-las. Após fazer sua escolha, você deverá ver uma mensagem informando que o Zed foi desinstalado com sucesso.

Caso o comando do shell `zed` não tenha sido encontrado no seu PATH, você pode tentar um dos seguintes comandos

```sh
$HOME/.local/bin/zed --uninstall
```

ou

```sh
$HOME/.local/zed.app/bin.zed --uninstall
```

O primeiro caso pode não funcionar se um link simbólico não tiver sido criado corretamente entre `$HOME/.local/bin/zed` e `$HOME/.local/zed.app/bin.zed`. Mas o segundo caso deve funcionar, desde que o Zed tenha sido instalado no local padrão.

Se o Zed tiver sido instalado em um local diferente, você deverá executar o binário `zed` armazenado nesse diretório de instalação e passar a opção `--uninstall` a ele, no mesmo formato dos comandos anteriores.

### Gerenciador de pacotes

Se o Zed tiver sido instalado por meio de um gerenciador de pacotes, consulte a documentação desse gerenciador para saber como desinstalar um pacote.

## Solução de problemas

O Linux funciona em uma grande variedade de sistemas configurados de diversas maneiras. Testamos o Zed principalmente em uma instalação padrão do Ubuntu, já que essa é a distribuição mais comum entre nossos usuários; no entanto, esperamos que ele funcione em uma ampla variedade de máquinas.

### Zed não consegue dar a partida

Se você encontrar um erro como “/lib64/libc.so.6: versão 'GLIBC_2.29' não encontrada”, isso significa que a versão da glibc da sua distribuição está muito desatualizada. Você pode atualizar seu sistema ou [instalar o Zed a partir do código-fonte](./development/linux.md).

### Problemas gráficos

#### O Zed não consegue abrir janelas

O Zed requer uma GPU para funcionar de maneira eficaz. Nos bastidores, usamos o [Vulkan](https://www.vulkan.org/) para nos comunicarmos com a sua GPU. Se você estiver enfrentando problemas de desempenho ou se o Zed não carregar, é possível que o Vulkan seja o culpado.

Se você receber uma notificação informando `Zed failed to open a window: NoSupportedDeviceFound`, isso significa que o Vulkan não consegue encontrar uma GPU compatível. Você pode tentar executar o [vkcube](https://github.com/krh/vkcube) (geralmente disponível como parte do pacote `vulkaninfo` ou `vulkan-tools` em várias distribuições) para tentar identificar a origem do problema, da seguinte maneira:

```
vkcube
```

> **_Observação_**: Tente executar o programa nos modos X11 e Wayland, digitando `vkcube -m [x11|wayland]`. Algumas versões do `vkcube` usam o `vkcube` para rodar no X11 e o `vkcube-wayland` para rodar no Wayland.

Isso deve exibir uma linha descrevendo sua configuração gráfica atual e mostrar um cubo girando. Se isso não funcionar, você deve conseguir resolver o problema instalando drivers de GPU compatíveis com o Vulkan; no entanto, em alguns casos, ainda não há suporte para o Vulkan.

Você pode descobrir qual placa de vídeo o Zed está usando verificando no log do Zed (`~/.local/share/zed/logs/Zed.log`) se há a menção `Using GPU: ...`.

Se você encontrar erros como `ERROR_INITIALIZATION_FAILED`, `GPU Crashed` ou `ERROR_SURFACE_LOST_KHR`, talvez seja possível contornar o problema instalando drivers diferentes para sua GPU ou selecionando uma GPU diferente para a execução. (Consulte [#14225](https://github.com/zed-industries/zed/issues/14225))

Em alguns sistemas, o arquivo `/etc/prime-discrete` pode ser usado para forçar o uso de uma GPU discreta por meio do [PRIME](https://wiki.archlinux.org/title/PRIME). Dependendo dos detalhes da sua configuração, talvez seja necessário alterar o conteúdo desse arquivo para “on” (para forçar o uso da placa de vídeo discreta) ou “off” (para forçar o uso da placa de vídeo integrada).

Em outros casos, talvez seja possível definir a variável de ambiente `DRI_PRIME=1` ao executar o Zed para forçar o uso da GPU discreta.

Se você estiver usando uma GPU AMD, poderá receber um erro do tipo “Broken Pipe”. Tente usar os drivers RADV ou Mesa. (Consulte [#13880](https://github.com/zed-industries/zed/issues/13880))

Se você estiver usando o `amdvlk`, o driver gráfico AMD de código aberto padrão, poderá perceber que o Zed falha constantemente ao ser iniciado. Esse é um problema conhecido por alguns usuários, por exemplo, no Omarchy (consulte a issue [#28851](https://github.com/zed-industries/zed/issues/28851)). Para corrigir isso, você precisará usar um driver diferente. Recomendamos remover os pacotes `amdvlk` e `lib32-amdvlk` e instalar o `vulkan-radeon` em seu lugar (consulte o issue [#14141](https://github.com/zed-industries/zed/issues/14141)).

Para mais informações, o [Guia do Arch sobre o Vulkan](https://wiki.archlinux.org/title/Vulkan) traz algumas orientações úteis que se aplicam bem à maioria das distribuições.

#### Forçar o Zed a usar uma GPU específica

Existem algumas maneiras diferentes de forçar o Zed a usar uma GPU específica:

##### Opção A

Você pode usar a variável de ambiente `ZED_DEVICE_ID={device_id}` para especificar o ID do dispositivo da GPU que deseja que o Zed utilize.

Você pode obter o ID do dispositivo da sua GPU executando o comando `lspci -nn | grep VGA`, que exibirá cada GPU em uma linha, como:

```
08:00.0 VGA compatible controller [0300]: NVIDIA Corporation GA104 [GeForce RTX 3070] [10de:2484] (rev a1)
```

onde o ID do dispositivo aqui é `2484`. Esse valor está no formato hexadecimal; portanto, para forçar o Zed a usar essa GPU específica, você deve definir a variável de ambiente da seguinte forma:

```
ZED_DEVICE_ID=0x2484 zed
```

Certifique-se de exportar a variável caso opte por defini-la globalmente em um arquivo `.bashrc` ou similar.

##### Opção B

Se você estiver usando o Mesa, pode executar `MESA_VK_DEVICE_SELECT=list zed --foreground` para obter uma lista das GPUs disponíveis e, em seguida, exportar `MESA_VK_DEVICE_SELECT=xxxx:yyyy` para escolher um dispositivo específico. Além disso, você pode recorrer ao xwayland com uma exportação adicional de `WAYLAND_DISPLAY=""`.

##### Opção C

Usando o [vkdevicechooser](https://github.com/jiriks74/vkdevicechooser).

#### Como relatar problemas gráficos

Se o Vulkan estiver configurado corretamente e o Zed ainda não estiver funcionando para você, por favor, [relate o problema](https://github.com/zed-industries/zed) fornecendo o máximo de informações possível.

Ao relatar problemas em que o Zed não inicia devido a erros de inicialização dos gráficos no GitHub, pode ser impossível executar o comando {#action zed::CopySystemSpecsIntoClipboard}, conforme orientamos em nosso modelo de relatório. Oferecemos uma maneira alternativa de coletar as especificações do sistema especificamente para essa situação.

Passar o parâmetro `--system-specs` ao Zed da seguinte forma:

```sh
zed --system-specs
```

exibirá as especificações do sistema no terminal desta forma. Recomenda-se enfaticamente que você copie a saída literalmente na abertura do issue no GitHub, pois ela usa formatação Markdown para garantir que a saída seja legível.

Além disso, é extremamente útil fornecer o conteúdo do seu log do Zed ao relatar esses problemas. O log geralmente está localizado em `~/.local/share/zed/logs/Zed.log`. O procedimento recomendado para gerar um arquivo de log útil é o seguinte:

```sh
truncate -s 0 ~/.local/share/zed/logs/Zed.log # Clear the log file
ZED_LOG=wgpu=info zed .
cat ~/.local/share/zed/logs/Zed.log
# copy the output
```

Ou, se você tiver o Zed cli configurado, pode fazer o seguinte:

```sh
ZED_LOG=wgpu=info /path/to/zed/cli --foreground .
# copy the output
```

Também é altamente recomendável que, ao colar o log em uma issue do GitHub, você utilize o seguinte modelo:

> **_Observação_**: Os espaços em branco no modelo são importantes e, se não forem preservados, causarão uma formatação incorreta.

````
<details><summary>Zed Log</summary>

```
{conteúdo do log zed}
```

</details>
````

Isso fará com que os registros fiquem ocultos por padrão, facilitando a leitura do problema.

### Não consigo abrir nenhum arquivo

### Não está funcionando clicar nos links

Esses recursos são fornecidos pelos portais de área de trabalho XDG, especificamente:

- `org.freedesktop.portal.FileChooser`
- `org.freedesktop.portal.OpenURI`

Alguns gerenciadores de janelas, como o `Hyprland`, não oferecem um seletor de arquivos por padrão. Consulte [esta lista](https://wiki.archlinux.org/title/XDG_Desktop_Portal#List_of_backends_and_interfaces) como ponto de partida para alternativas.

### O Zed não está lembrando das minhas chaves de API

### O Zed não está lembrando do meu login

Esse recurso também requer os portais de área de trabalho XDG, especificamente:

- `org.freedesktop.portal.Secret` ou
- `org.freedesktop.Secrets`

O Zed precisa de um local para armazenar com segurança informações confidenciais, como seu cookie de login do Zed ou suas chaves de API da OpenAI, e usamos um gerenciador de chaves fornecido pelo sistema para fazer isso. Exemplos de pacotes que oferecem esse recurso são o `gnome-keyring`, o `KWallet` e o `keepassxc`, entre outros.

### Não foi possível iniciar o inotify

O Zed depende do inotify para monitorar as alterações no seu sistema de arquivos. Se você não conseguir iniciar o inotify, o Zed não funcionará de maneira confiável.

Se você estiver vendo a mensagem “muitos arquivos abertos”, tente primeiro `sysctl fs.inotify`.

- Verifique se o valor de `max_user_instances` é 128 ou superior (você pode alterar o limite com `sudo sysctl fs.inotify.max_user_instances=1024`). O Zed precisa de apenas uma instância do inotify.
- Verifique se o valor de `max_user_watches` é 8.000 ou superior (você pode alterar o limite com o comando `sudo sysctl fs.inotify.max_user_watches=64000`). O Zed precisa de um monitoramento por diretório em todos os seus projetos abertos + um por repositório Git + mais alguns para configurações, temas, mapas de teclado e extensões.

Também é possível que você esteja ficando sem descritores de arquivo. Você pode verificar os limites com o comando `ulimit` e atualizá-los editando o arquivo `/etc/security/limits.conf`.

### Sem som ou dispositivo de saída incorreto

Se você não estiver ouvindo nenhum som no Zed ou se o áudio estiver sendo direcionado para o dispositivo errado, isso pode ser causado por uma incompatibilidade entre os sistemas de áudio. O Zed utiliza o ALSA, enquanto seu sistema pode estar usando o PipeWire ou o PulseAudio. Para resolver isso, é necessário configurar o ALSA para direcionar o áudio através do PipeWire/PulseAudio.

Se o seu sistema usa o PipeWire:

1. **Instale o plug-in PipeWire para ALSA**

   Em sistemas baseados no Debian, execute:

   ```bash
   sudo apt install pipewire-alsa
   ```

2. **Configurar o ALSA para usar o PipeWire**

   Adicione a seguinte configuração ao seu arquivo de configurações do ALSA. Você pode usar o `~/.asoundrc` (nível do usuário) ou o `/etc/asound.conf` (em todo o sistema):

   ```bash
   pcm.!default {
       type pipewire
   }

   ctl.!default {
       type pipewire
   }
   ```

3. **Reinicie o sistema**

### Forçar o fator de escala do X11

Em sistemas X11, o Zed detecta automaticamente o fator de escala adequado para monitores com alta resolução (DPI). O fator de escala é determinado seguindo a seguinte ordem de prioridade:

1. Variável de ambiente `GPUI_X11_SCALE_FACTOR` (se definida)
2. `Xft.dpi` do banco de dados de recursos do X (xrdb)
3. Detecção automática via RandR com base na resolução e no tamanho físico do monitor

Se você quiser personalizar o fator de escala além do que o Zed detecta automaticamente, há várias opções disponíveis:

#### Verifique seu fator de escala atual

Você pode verificar se a variável `Xft.dpi` está definida:

```sh
xrdb -query | grep Xft.dpi
```

Se esse comando não retornar nenhuma saída, o Zed está usando o RandR (extensão de gerenciamento de monitores do X11) para calcular automaticamente o fator de escala com base na resolução e nas dimensões físicas informadas pelo seu monitor.

#### Opção 1: Definir Xft.dpi (banco de dados de recursos do X)

`Xft.dpi` é uma configuração padrão do X11 que muitos aplicativos utilizam para garantir um dimensionamento consistente das fontes e da interface do usuário. Definir esse parâmetro garante que o Zed seja dimensionado da mesma forma que outros aplicativos do X11 que respeitam essa configuração.

Edite ou crie o arquivo `~/.Xresources`:

```sh
vim ~/.Xresources
```

Adicione esta linha com o DPI desejado:

```sh
Xft.dpi: 96
```

Valores comuns de DPI:

- `96` para escala padrão de 1x
- `144` para ampliação de 1,5x
- `192` para ampliação de 2x
- `288` para ampliação de 3x

Carregar a configuração:

```sh
xrdb -merge ~/.Xresources
```

Reinicie o Zed para que as alterações entrem em vigor.

#### Opção 2: Use a variável de ambiente GPUI_X11_SCALE_FACTOR

Essa variável de ambiente específica do Zed define diretamente o fator de escala, ignorando toda a detecção automática.

```sh
GPUI_X11_SCALE_FACTOR=1.5 zed
```

Você pode usar valores decimais (por exemplo, `1,25`, `1,5`, `2,0`) ou definir `GPUI_X11_SCALE_FACTOR=randr` para forçar a detecção baseada em RandR, mesmo quando `Xft.dpi` estiver definido.

Para tornar essa configuração permanente, adicione-a ao seu perfil do shell ou à entrada da área de trabalho.

#### Opção 3: Ajustar o DPI do RandR em todo o sistema

Isso altera o DPI informado para toda a sua sessão X11, afetando a forma como o RandR calcula o redimensionamento para todos os aplicativos que o utilizam.

Adicione isto ao seu `.xprofile` ou `.xinitrc`:

```sh
xrandr --dpi 192
```

Substitua `192` pelo valor de DPI desejado. Isso afeta o sistema como um todo e será utilizado pela detecção automática de RandR do Zed quando `Xft.dpi` não estiver definido.

### Parâmetros de renderização de fontes

No Linux, o Zed lê as variáveis de ambiente `ZED_FONTS_GAMMA` e `ZED_FONTS_GRAYSCALE_ENHANCED_CONTRAST` para determinar os valores a serem usados na renderização das fontes.

`ZED_FONTS_GAMMA` corresponde aos valores de [getgamma](https://learn.microsoft.com/en-us/windows/win32/api/dwrite/nf-dwrite-idwriterenderingparams-getgamma).
Intervalo permitido [1,0; 2,2]; outros valores são cortados.
Padrão: 1,8

`ZED_FONTS_GRAYSCALE_ENHANCED_CONTRAST` corresponde aos valores de [getgrayscaleenhancedcontrast](https://learn.microsoft.com/en-us/windows/win32/api/dwrite_1/nf-dwrite_1-idwriterenderingparams1-getgrayscaleenhancedcontrast).
Intervalo permitido: [0,0, ..), outros valores são cortados.
Padrão: 1,0
