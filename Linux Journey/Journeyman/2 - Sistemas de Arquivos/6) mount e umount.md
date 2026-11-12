# mount e umount

Aprenda a anexar, inspecionar e desanexar sistemas de arquivos com segurança usando origens e pontos de montagem verificados.

A montagem anexa um sistema de arquivos a um diretório no namespace visível. A origem pode ser um dispositivo de bloco, uma exportação de rede, um sistema de arquivos virtual, uma origem de bind ou outro objeto específico da implementação. O diretório de destino é chamado de ponto de montagem.

## Preparação e Inspeção de um Ponto de Montagem

Crie um diretório com um nome deliberado quando a política local exigir:

```bash
$ sudo mkdir -p /mnt/mydrive
```

Inspecione-o antes da montagem:

```bash
$ findmnt --target /mnt/mydrive
$ sudo ls -la /mnt/mydrive
```

Montar sobre um diretório não vazio oculta suas entradas existentes atrás do novo sistema de arquivos até a desmontagem; isso não as exclui. Esse comportamento pode confundir aplicações e consumir espaço em disco de forma invisível, portanto use um ponto de montagem vazio e dedicado.

## Montagem de um Sistema de Arquivos Verificado

Após confirmar a identidade da origem, o tipo detectado e o conteúdo esperado, monte explicitamente:

```bash
$ sudo mount -t ext4 /dev/VERIFIED-PARTITION /mnt/mydrive
```

A opção `-t` especifica a implementação do sistema de arquivos. Mount muitas vezes consegue detectar o tipo, mas um tipo explícito e opções revisadas tornam a intenção mais clara. Para conteúdo não confiável ou removível, considere opções restritivas como `ro`, `nosuid`, `nodev` e `noexec` quando forem adequadas à carga de trabalho; cada uma possui limitações e não deve ser tratada como uma sandbox completa.

Verifique o que realmente foi montado:

```bash
$ findmnt --target /mnt/mydrive -o TARGET,SOURCE,FSTYPE,OPTIONS
```

As montagens são específicas do namespace. Uma montagem criada em um contêiner ou namespace privado de serviço pode não aparecer na visão de outro processo.

## Uso de UUIDs de Sistemas de Arquivos

Nomes de enumeração como `/dev/sdb2` podem mudar. Descubra os identificadores dos sistemas de arquivos com:

```bash
$ lsblk -f
$ sudo blkid
```

Depois, monte um sistema de arquivos verificado pelo UUID:

```bash
$ sudo mount UUID=130b882f-7d79-436d-a096-1e594c92bb76 /mnt/mydrive
```

Um UUID identifica o sistema de arquivos, não necessariamente o disco físico. A reformatação o altera, enquanto a clonagem pode duplicá-lo. Verifique a exclusividade antes de anexar o original e o clone ao mesmo sistema.

## Desmontagem Segura

Desanexe pelo ponto de montagem exato:

```bash
$ sudo umount /mnt/mydrive
```

O comando se chama `umount`, sem o primeiro `n`. Uma desmontagem bem-sucedida desanexa o sistema de arquivos depois que o kernel conclui as gravações necessárias e as referências permitem a operação. Confirme depois com `findmnt` antes de desconectar o armazenamento.

Uma desmontagem bem-sucedida nem sempre é a última operação necessária para remover uma mídia com segurança. As pilhas de armazenamento dos ambientes gráficos podem oferecer uma ação de ejeção ou desligamento que sincroniza os caches do dispositivo e desabilita um dispositivo USB. Siga o fluxo da plataforma e do hardware.

## Diagnóstico de um Sistema de Arquivos Ocupado

A desmontagem falha quando o namespace ainda possui referências ativas, como arquivos abertos, um diretório de trabalho de processo, montagens aninhadas, swap ou outras camadas de armazenamento. Investigue em vez de forçar a operação imediatamente:

```bash
$ findmnt --submounts /mnt/mydrive
$ sudo fuser -vm /mnt/mydrive
```

Mova os shells para fora da árvore, encerre de forma limpa a aplicação responsável e desmonte os filhos antes do pai. As opções de desmontagem preguiçosa e forçada possuem semânticas especializadas e podem deixar referências ativas ou causar perda de dados; use-as apenas com uma justificativa de recuperação documentada.

Use o laboratório [Gerenciamento de Partições e Sistemas de Arquivos Linux](https://labex.io/labs/comptia-manage-linux-partitions-and-filesystems-590845) para praticar no armazenamento descartável designado.

