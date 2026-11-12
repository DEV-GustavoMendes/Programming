# Nomes de Dispositivos

Aprenda como o Linux nomeia dispositivos de armazenamento, partições, dispositivos lógicos e links persistentes comuns.

Os nomes de dispositivos Linux refletem o subsistema e o driver do kernel que apresentam uma interface, nem sempre o conector físico indicado no hardware. Aprenda os padrões comuns, mas descubra o mapeamento real do sistema atual antes de alterar o armazenamento.

## Nomes de Discos da Camada SCSI

Os discos apresentados pela camada de discos SCSI normalmente usam nomes `sd`. Isso inclui muitos discos SCSI, SATA, de armazenamento USB e virtuais:

- `/dev/sda`: um disco inteiro
- `/dev/sdb`: outro disco inteiro
- `/dev/sda3`: partição 3 de `/dev/sda`
- `/dev/sdb1`: partição 1 de `/dev/sdb`

As letras refletem a enumeração, não uma identidade permanente. Adicionar um controlador, alterar a ordem do firmware ou conectar um dispositivo pode mudar qual disco recebe determinada letra.

## Nomes que Terminam em Dígitos

Alguns nomes de dispositivos inteiros já contêm dígitos, portanto os nomes de suas partições usam `p` como separador:

- `/dev/nvme0n1`: namespace NVMe 1 no controlador 0
- `/dev/nvme0n1p2`: partição 2 desse namespace
- `/dev/mmcblk0`: um dispositivo de bloco MMC
- `/dev/mmcblk0p1`: partição 1 desse dispositivo

Os dispositivos NVMe normalmente não são chamados `/dev/sdX`; eles usam a convenção de nomes do subsistema NVMe.

## Dispositivos de Bloco Lógicos e Virtuais

O Linux também cria dispositivos de bloco que não correspondem individualmente a um disco físico:

- `/dev/dm-N` para dispositivos do device mapper, muitas vezes acompanhados de links descritivos em `/dev/mapper/`
- `/dev/mdN` para arranjos RAID de software do Linux
- `/dev/loopN` para arquivos comuns associados como dispositivos de bloco loop

Partições, camadas de criptografia, RAID, volumes lógicos e sistemas de arquivos formam uma pilha. Use ferramentas como `lsblk` para visualizar as relações entre pais e filhos, em vez de deduzir a pilha apenas pelo nome.

## Links Persistentes de Armazenamento

O gerenciamento de dispositivos no espaço do usuário cria links em `/dev/disk/`, normalmente agrupados como:

- `by-id` para identificadores de hardware ou transporte
- `by-uuid` para UUIDs de sistemas de arquivos
- `by-label` para rótulos de sistemas de arquivos
- `by-partuuid` para UUIDs de tabelas de partições
- `by-path` para caminhos dependentes da topologia

Escolha um identificador que corresponda ao que precisa permanecer estável. O UUID de um sistema de arquivos identifica o sistema de arquivos, não necessariamente o disco físico abaixo dele. Clonar um sistema de arquivos pode duplicar seu UUID, portanto verifique a exclusividade antes de depender dele.

## Nomes de Pseudodispositivos

Nomes como `/dev/null`, `/dev/zero` e `/dev/urandom` descrevem pseudodispositivos do kernel, não armazenamento físico. `/dev/null` descarta gravações e retorna fim de arquivo nas leituras; `/dev/zero` fornece bytes zero; `/dev/urandom` fornece bytes do gerador de números aleatórios do kernel.

Use o laboratório [Exploração de Dispositivos de Hardware no Linux](https://labex.io/labs/comptia-explore-hardware-devices-in-linux-590861) para comparar nomes, links persistentes e relações exibidas por `lsblk` antes de tentar trabalhar com partições.
