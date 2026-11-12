# swap

Aprenda como o Linux usa, inicializa, ativa, dimensiona e desativa com segurança o espaço de swap.

O Linux pode mover páginas selecionadas de memória anônima entre a RAM e um armazenamento apoiado por swap. Isso permite manter memória inativa enquanto libera RAM para cargas de trabalho ativas e para o cache do sistema de arquivos, mas o armazenamento é muito mais lento que a RAM. O swap é uma ferramenta de capacidade e gerenciamento de memória, não um substituto para uma quantidade suficiente de memória nem um limite para a memória das aplicações.

## Participação do Swap no Gerenciamento de Memória

O kernel pode usar o swap antes que a RAM se esgote completamente, dependendo da carga de trabalho, da pressão de memória, dos cgroups e de parâmetros ajustáveis como swappiness. Páginas limpas apoiadas por arquivos muitas vezes podem ser descartadas e relidas de seus arquivos, enquanto páginas anônimas precisam do swap ou devem permanecer na RAM.

Uma atividade intensa e contínua de swap pode causar grande latência ou thrashing. Diagnostique a demanda de memória, os conjuntos de trabalho, a pressão e os limites das aplicações, em vez de tratar uma área de swap maior como uma solução universal de desempenho.

## Inspeção do Swap Ativo

Use primeiro comandos somente para leitura:

```bash
$ swapon --show
$ cat /proc/swaps
$ free -h
```

Eles mostram o swap ativo configurado e os valores agregados de memória. Um valor “used” diferente de zero não representa automaticamente um problema; relacione-o às taxas de entrada e saída do swap, à pressão de memória, à latência e ao comportamento da carga de trabalho.

## Inicialização e Ativação de um Dispositivo de Swap

`mkswap` grava uma assinatura de swap e destrói os metadados utilizáveis anteriores do destino. Pratique somente em um destino descartável verificado:

```bash
$ sudo mkswap /dev/VERIFIED-SWAP-TARGET
$ sudo swapon /dev/VERIFIED-SWAP-TARGET
```

Antes de executar `mkswap`, verifique modelo, número de série, tamanho, identidade persistente, assinaturas existentes, montagens, RAID, LVM, criptografia e backups, assim como faria antes de `mkfs`. Após a ativação, confirme a origem exata com `swapon --show`.

Para persistência, use o UUID do swap em `/etc/fstab` com o tipo e as opções adequados à política local:

```text
UUID=VERIFIED-SWAP-UUID none swap sw 0 0
```

## Arquivos de Swap e Outros Backends

Um arquivo de swap pode fornecer capacidade flexível sem reparticionar, mas os requisitos de criação dependem do sistema de arquivos. O arquivo deve ter permissões restritivas, uma alocação apropriada sem lacunas nem comportamentos copy-on-write incompatíveis, uma assinatura de swap e ativação. Siga a documentação do sistema de arquivos e da distribuição, em vez de copiar em todos os ambientes uma receita genérica com `fallocate`.

Dispositivos de RAM comprimida, como zram, podem fornecer outra camada de swap, com diferentes compensações entre CPU e capacidade. O swap criptografado pode proteger as páginas armazenadas, enquanto a hibernação exige uma configuração de retomada e armazenamento adequado suficiente. Esses objetivos afetam o dimensionamento e o projeto.

Não existe uma regra universal de que o swap deve ter o dobro da RAM. Dimensione-o a partir dos picos da carga de trabalho, do comportamento desejado em caso de falha, das necessidades de hibernação, da latência e durabilidade do armazenamento, do projeto de dumps de falhas e do monitoramento operacional.

## Desativação Segura do Swap

Desative uma área específica verificada com:

```bash
$ sudo swapoff /dev/VERIFIED-SWAP-TARGET
```

O kernel precisa mover para outro lugar as páginas residentes naquela área. Se a RAM e o swap restante não puderem acomodá-las, a operação pode falhar ou criar uma pressão de memória perigosa. Interrompa ou limite primeiro as cargas de trabalho, monitore a memória, remova a entrada persistente do fstab somente após verificar o destino correto e confirme a desativação com `swapon --show` antes de reutilizar o armazenamento.

Use o laboratório [Criação e Ativação de um Arquivo de Swap no Linux](https://labex.io/labs/comptia-create-and-activate-a-swap-file-in-linux-590858) em um ambiente controlado para praticar permissões de arquivos, ativação e persistência.

