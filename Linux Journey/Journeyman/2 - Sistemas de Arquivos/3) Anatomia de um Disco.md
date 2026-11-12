# Anatomia de um Disco

Aprenda como dispositivos de bloco, tabelas de partições, partições e sistemas de arquivos formam camadas distintas de armazenamento.

Um dispositivo de armazenamento é exposto como um dispositivo de bloco, como `/dev/sda` ou `/dev/nvme0n1`. Ele pode conter uma tabela de partições, cujas entradas descrevem regiões expostas como dispositivos de bloco filhos. Uma partição pode então conter um sistema de arquivos, uma assinatura de swap, um membro de RAID, um contêiner de criptografia, um volume físico de volumes lógicos ou outro formato de dados.

Essas camadas são independentes: nem todo disco possui uma tabela de partições, nem toda partição contém um sistema de arquivos e um sistema de arquivos pode residir em um volume lógico ou em um dispositivo inteiro.

## Tabelas de Partições e Limites

Uma tabela de partições registra posições iniciais, comprimentos, identificadores de tipos e atributos específicos do esquema. O kernel a lê para criar dispositivos de bloco de partições, como `/dev/sda1` ou `/dev/nvme0n1p1`.

Em layouts comuns, os limites das partições não devem se sobrepor. O espaço fora de todas as entradas é considerado não alocado pela tabela de partições, embora ainda possa conter assinaturas ou dados antigos. Alterar uma tabela não move automaticamente o conteúdo dos sistemas de arquivos para corresponder aos novos limites.

## Particionamento MBR

O esquema legado DOS/MBR armazena sua tabela principal no primeiro setor lógico. Ele possui quatro entradas principais. Uma delas pode descrever uma partição estendida, que funciona como contêiner para uma série encadeada de partições lógicas, permitindo mais de quatro regiões utilizáveis.

Com endereços de setores de 32 bits e setores lógicos de 512 bytes, o MBR alcança um limite frequentemente citado de cerca de 2 TiB. O endereçamento exato depende do tamanho do setor e do suporte das ferramentas. O MBR também não possui as cópias redundantes do cabeçalho e da tabela nem os GUIDs por partição do GPT.

## Particionamento GPT

A GUID Partition Table, ou GPT, usa endereços de blocos lógicos de 64 bits e normalmente armazena um cabeçalho e uma matriz de entradas principais perto do início, além de cópias de backup perto do fim do disco. Um MBR protetor ajuda a impedir que softwares antigos compatíveis apenas com MBR tratem o disco como vazio.

Cada entrada GPT inclui um GUID de tipo de partição e um GUID exclusivo da partição; portanto, o GPT não possui apenas um tipo de partição. A quantidade de entradas disponíveis é determinada pela tabela alocada e pelas ferramentas, geralmente muito superior a quatro, sem partições estendidas ou lógicas.

O GPT normalmente é usado em discos de boot UEFI, mas o particionamento e o modo de inicialização do firmware são conceitos distintos. Um sistema UEFI também precisa dos arquivos de boot apropriados e de uma EFI System Partition; somente o GPT não torna um disco inicializável.

## Estruturas de Sistemas de Arquivos São Específicas do Formato

Após o particionamento, uma ferramenta de criação de sistemas de arquivos grava as estruturas definidas por aquele sistema. Muitos formatos possuem conceitos como superblocos, metadados de alocação, registros de diretórios e extensões ou blocos de dados, mas seu layout, sua redundância e sua terminologia são diferentes.

Por exemplo, os sistemas de arquivos ext usam inodes e grupos de blocos, enquanto outros sistemas organizam metadados por diferentes árvores ou estruturas de alocação. Não aplique um diagrama simplificado de “bloco de boot, um superbloco, tabela de inodes e blocos de dados” a todos os sistemas de arquivos.

## Inspeção do Layout Atual

Use visualizações somente para leitura antes de qualquer modificação:

```bash
$ lsblk -o NAME,PATH,TYPE,SIZE,PTTYPE,PARTTYPE,FSTYPE,MOUNTPOINTS
$ sudo parted --list
```

`PTTYPE` descreve o esquema detectado da tabela de partições, `PARTTYPE` identifica um tipo de partição e `FSTYPE` informa uma assinatura de conteúdo detectada. A detecção é um indício, não uma garantia de que o conteúdo esteja íntegro ou seja seguro montá-lo.

Os nomes dos dispositivos podem mudar, e assinaturas antigas podem confundir a detecção. Confirme modelo, número de série, tamanho, transporte, links persistentes, montagens ativas, swap, RAID, LVM, criptografia e backups antes de abrir qualquer ferramenta de particionamento no modo de escrita.

Use o laboratório [Gerenciamento de Partições e Sistemas de Arquivos Linux](https://labex.io/labs/comptia-manage-linux-partitions-and-filesystems-590845) somente em armazenamento descartável para praticar essas camadas.
