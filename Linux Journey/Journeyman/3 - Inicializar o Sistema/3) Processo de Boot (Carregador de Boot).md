# Processo de Boot: Carregador de Boot

Aprenda como um carregador de boot seleciona artefatos Linux, constrói a linha de comando do kernel e transfere o controle.

Um carregador de boot faz a ponte entre a descoberta realizada pelo firmware e a execução do kernel. O GRUB é comum em PCs Linux, mas systemd-boot, U-Boot, o carregamento de um kernel EFI-stub pelo firmware e outros projetos implementam partes diferentes desse papel.

## Seleção dos artefatos de boot

Uma entrada do carregador pode identificar:

- uma imagem do kernel Linux
- uma imagem initramfs opcional ou um initrd legado
- uma linha de comando do kernel
- metadados específicos da plataforma ou o carregador de outro sistema operacional

O GRUB pode apresentar vários kernels e entradas de recuperação. Um kernel alternativo só é útil quando seus módulos e initramfs correspondentes continuam disponíveis e testados. O carregador lê arquivos por seus próprios módulos de armazenamento e sistema de arquivos; ele não depende do VFS do Linux, que ainda não está em execução.

## Parâmetros da linha de comando do kernel

O carregador passa uma linha de texto que o kernel e o espaço inicial do usuário interpretam. Exemplos comuns:

- `root=...` identifica o sistema de arquivos raiz pretendido ou uma especificação para o espaço inicial
- `ro` ou `rw` solicita o modo inicial de montagem da raiz
- `quiet` reduz as mensagens do kernel no console
- `init=...` solicita outro primeiro programa de espaço do usuário para recuperação especializada
- parâmetros `rd.*` específicos da distribuição são interpretados pelas ferramentas do initramfs

`initrd` normalmente é uma diretiva do carregador que nomeia uma imagem, não um parâmetro genérico do kernel. `BOOT_IMAGE=` pode aparecer em linhas produzidas por algumas configurações do GRUB, mas não é o mecanismo que carrega o kernel.

Veja a linha usada no boot atual com:

```bash
$ cat /proc/cmdline
```

## Edição temporária e recuperação

O GRUB normalmente permite que um usuário autorizado no console edite uma entrada por um único boot, usando uma tecla indicada no menu. Isso é útil para remover `quiet`, escolher parâmetros de recuperação ou corrigir um identificador de raiz. A interface e a autorização variam, sobretudo com Secure Boot e GRUB protegido por senha.

Parâmetros podem expor texto sensível em `/proc/cmdline`, logs de boot e relatórios de falha. Também podem enfraquecer a segurança ou impedir o boot. Nunca coloque segredos ali; preserve uma entrada funcional e um caminho de recuperação pelo console.

## Configuração persistente do GRUB

As distribuições costumam gerar a configuração final do GRUB a partir de modelos, padrões, scripts e kernels encontrados. Não edite diretamente o `grub.cfg` gerado, salvo quando a distribuição documentar esse fluxo; uma nova geração pode sobrescrevê-lo.

Faça uma alteração limitada na fonte, execute o comando de geração documentado pela distribuição, confira a saída e teste mantendo uma entrada antiga funcional e uma mídia de recuperação. Comando e caminho de saída variam entre Debian, Fedora, instalações UEFI e BIOS.

Use [Personalizar o Menu de Inicialização GRUB2 no Linux](https://labex.io/labs/comptia-customize-the-grub2-boot-menu-in-linux-590859) somente no ambiente de laboratório com recursos de recuperação.