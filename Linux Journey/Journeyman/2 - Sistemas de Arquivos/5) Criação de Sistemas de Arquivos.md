# Criação de Sistemas de Arquivos

Aprenda a verificar o dispositivo de bloco de destino e criar um sistema de arquivos com ferramentas específicas do formato.

Criar um sistema de arquivos grava novas estruturas de alocação e metadados em um dispositivo de bloco. Essa é uma etapa destrutiva de inicialização, não apenas uma alteração de rótulo. Use somente armazenamento descartável para praticar e mantenha um backup testado antes de formatar um dispositivo que já tenha contido dados valiosos.

## Compreensão de `mkfs`

`mkfs` normalmente é uma interface que encaminha a operação a um programa específico do sistema de arquivos, como `mkfs.ext4`, `mkfs.xfs` ou `mkfs.btrfs`. Um comando genérico possui este formato:

```bash
$ sudo mkfs -t ext4 /dev/VERIFIED-PARTITION
```

O marcador só deve ser substituído após a verificação. A sintaxe equivalente específica do formato normalmente é:

```bash
$ sudo mkfs.ext4 /dev/VERIFIED-PARTITION
```

As opções compatíveis, os padrões, os conjuntos de recursos e as solicitações de confirmação de sobrescrita diferem entre as implementações. Leia o manual local do formatador exato, em vez de presumir que todos os backends de `mkfs` se comportem da mesma forma.

## Verificação de Todas as Camadas de Armazenamento

Antes de formatar, identifique o destino por modelo, número de série, tamanho, topologia, link persistente e função pretendida:

```bash
$ lsblk -o NAME,PATH,TYPE,SIZE,MODEL,SERIAL,FSTYPE,UUID,MOUNTPOINTS
$ findmnt --real
$ sudo wipefs --no-act /dev/VERIFIED-PARTITION
```

`wipefs --no-act` informa as assinaturas reconhecidas sem apagá-las. Verifique também o uso por swap, LVM, RAID, criptografia, máquinas virtuais, contêineres e aplicações. Um dispositivo pode estar ativo mesmo que `MOUNTPOINTS` esteja vazio.

Desmonte ou desative todas as camadas relevantes por meio de suas próprias ferramentas. Verifique novamente a identidade imediatamente antes de executar o formatador, pois os nomes de enumeração podem mudar.

## Seleção Deliberada do Sistema de Arquivos

Escolha um tipo compatível com a distribuição, o ambiente de boot, as ferramentas de backup, as ferramentas de reparo e a carga de trabalho. Considere os limites necessários, snapshots, checksums, cotas, camadas de criptografia, comportamento de expansão ou redução e acesso entre plataformas.

Não selecione um formato apenas por ser popular. Por exemplo, ext4, XFS e Btrfs possuem diferentes recursos operacionais e procedimentos de recuperação. Um dispositivo removível destinado à interoperabilidade pode exigir outro formato, com semânticas diferentes para permissões Unix.

## Rótulos, UUIDs e Verificação

Os formatadores normalmente geram um UUID de sistema de arquivos e muitas vezes podem definir um rótulo legível. Use rótulos suficientemente exclusivos para o ambiente e garanta que sistemas de arquivos clonados não mantenham identificadores conflitantes quando forem montados juntos.

Após a criação bem-sucedida, inspecione sem montar:

```bash
$ lsblk -f /dev/VERIFIED-PARTITION
$ sudo blkid /dev/VERIFIED-PARTITION
```

Registre o UUID para a configuração posterior da montagem. Criar um sistema de arquivos não o monta, não cria diretórios de aplicações, não restaura backups nem o torna persistente após a inicialização.

Use o laboratório [Gerenciamento de Partições e Sistemas de Arquivos Linux](https://labex.io/labs/comptia-manage-linux-partitions-and-filesystems-590845) somente no disco secundário descartável do laboratório.

