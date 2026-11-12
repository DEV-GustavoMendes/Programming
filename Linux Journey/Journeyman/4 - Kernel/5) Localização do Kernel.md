# Localização do Kernel

Aprenda onde as distribuições armazenam imagens do kernel, initramfs, configurações, símbolos e módulos versionados.

As distribuições normalmente guardam artefatos inicializáveis em `/boot`, mas layouts UEFI e Boot Loader Specification também podem colocá-los em uma Partição de Sistema EFI ou partição de boot estendida montada em `/boot`, `/boot/efi` ou `/efi`. Examine montagens e a configuração do carregador, sem presumir um caminho universal.

## Arquivos versionados em `/boot`

Um layout tradicional pode conter:

- `vmlinuz-KERNEL_RELEASE`: imagem inicializável do kernel Linux
- `initrd.img-KERNEL_RELEASE` ou `initramfs-KERNEL_RELEASE.img`: imagem do espaço inicial do usuário
- `config-KERNEL_RELEASE`: configuração usada na compilação do kernel empacotado
- `System.map-KERNEL_RELEASE`: mapa de endereços de símbolos da compilação

Os nomes variam. Um arquivo chamado `initrd` em uma distribuição moderna costuma conter um initramfs. O nome `vmlinuz` não informa a compactação interna exata nem o formato de boot; use as ferramentas da distribuição para inspecioná-lo.

## Sistema de arquivos RAM inicial e metadados

O initramfs deve conter módulos e ferramentas iniciais exigidos pelo kernel correspondente e pelo projeto de armazenamento raiz. Um nome compatível não basta: uma geração antiga ou mal-sucedida ainda pode produzir uma entrada inutilizável.

`config-*` mostra recursos incorporados, modulares ou omitidos. `System.map-*` pode ajudar na simbolização e depuração, embora randomização, informações de debug separadas e ferramentas da distribuição influenciem seu uso. São artefatos de apoio, não kernels alternativos.

## Módulos versionados do kernel

Módulos carregáveis da versão em execução normalmente ficam em:

```bash
$ printf '/lib/modules/%s\n' "$(uname -r)"
```

Em layouts unificados, isso pode apontar para `/usr/lib/modules/KERNEL_RELEASE`. Cada kernel precisa de uma árvore compatível e índices de dependência. `modprobe` usa metadados específicos da versão, não procura arquivos `.ko` arbitrários pelo disco.

## Imagens unificadas e caminhos do firmware

Uma Unified Kernel Image (UKI) é um único executável EFI assinado que pode reunir kernel, initrd, linha de comando e metadados. UKIs ficam em um local acessível ao EFI, em vez de arquivos separados `vmlinuz` e initramfs.

Assim, um `/boot` tradicional aparentemente vazio não prova que nenhum kernel está instalado. Use `findmnt`, o banco de pacotes, ferramentas do gerenciador de boot e sua configuração para mapear os artefatos ativos.

## Gerenciamento seguro do espaço

Se o sistema de arquivos de boot estiver cheio, mapeie os caminhos montados e identifique o pacote proprietário de cada artefato. Use o fluxo de limpeza de kernels do gerenciador, preserve o kernel em execução e uma alternativa funcional, regenere ou confira as entradas e verifique o espaço.

Não apague manualmente `vmlinuz`, initramfs, UKI ou árvores de módulos apenas pela idade. Um arquivo pode ser a única entrada de recuperação inicializável.

