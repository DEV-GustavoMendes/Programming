# Monitoramento de E/S

Aprenda a usar amostras de iostat para investigar a atividade da CPU e dos dispositivos de bloco.

`iostat`, normalmente fornecido pelo pacote `sysstat`, informa a atividade da CPU e dos dispositivos de bloco. Use amostras repetidas junto com a latência da aplicação: o throughput ou a utilização, sozinhos, não comprovam que o armazenamento esteja causando um problema perceptível ao usuário.

## Coleta de Amostras Úteis

Execute estatísticas detalhadas dos dispositivos em intervalos de um segundo:

```bash
$ iostat -xz 1
```

Nas implementações comuns, o primeiro relatório contém médias desde o boot, e os relatórios posteriores abrangem cada intervalo. A opção `-x` acrescenta campos detalhados, enquanto `-z` omite dispositivos inativos. Aguarde vários intervalos para capturar períodos normais e problemáticos.

## Leitura dos Campos da CPU

A seção da CPU normalmente inclui os tempos de usuário (`%user`), sistema (`%system`), ocioso (`%idle`), espera por E/S (`%iowait`) e tomado pela máquina virtual (`%steal`). A espera por E/S é o tempo ocioso da CPU durante o qual o sistema possui uma solicitação de E/S pendente; não é a porcentagem de ocupação de um disco.

## Leitura dos Campos dos Dispositivos

Os nomes dos campos variam conforme a versão do sysstat, mas alguns conceitos úteis são:

- As operações ou os dados de leitura e escrita por segundo mostram a taxa da carga de trabalho.
- `await` informa a latência média das solicitações, incluindo o tempo na fila e de atendimento.
- Os campos de tamanho médio da fila mostram as solicitações aguardando ou sendo atendidas.
- `%util` informa a porcentagem do tempo decorrido durante o qual o dispositivo possuía E/S em andamento.

Um `%util` alto pode indicar saturação em um dispositivo serial simples, mas não se traduz diretamente na capacidade de desempenho de um armazenamento paralelo, arranjo ou dispositivo virtual. Compare a latência com o projeto do dispositivo, o padrão da carga de trabalho e o objetivo de serviço.

## Relação entre as Evidências

Mapeie os nomes dos dispositivos para as montagens e os dispositivos subjacentes antes de tirar conclusões:

```bash
$ lsblk -o NAME,TYPE,SIZE,FSTYPE,MOUNTPOINTS
$ findmnt
```

Depois, relacione os intervalos de `iostat` ao tempo de resposta da aplicação, às métricas do banco de dados ou sistema de arquivos e à E/S no nível dos processos. Device mapper, RAID, contêineres e armazenamento apoiado por rede podem acrescentar camadas que exigem suas próprias ferramentas.

