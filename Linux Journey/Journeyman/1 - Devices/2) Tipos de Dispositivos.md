# Tipos de Dispositivos

Aprenda a diferenciar nós de dispositivos de caractere e bloco de pipes, sockets e objetos comuns do sistema de arquivos.

O primeiro caractere de um modo exibido por `ls -l` identifica o tipo do objeto no sistema de arquivos. Em `/dev`, os arquivos especiais de caractere e de bloco são nós de dispositivos. Nós de pipes e sockets de domínio Unix também podem aparecer ali, mas são objetos de comunicação entre processos, não nós de dispositivos de hardware.

```text
$ ls -l /dev/null /dev/sda /run/systemd/journal/dev-log /tmp/example-fifo
crw-rw-rw- 1 root root 1, 3 ... /dev/null
brw-rw---- 1 root disk 8, 0 ... /dev/sda
srw-rw-rw- 1 root root      ... /run/systemd/journal/dev-log
prw------- 1 user user      ... /tmp/example-fifo
```

As entradas e permissões variam conforme o sistema; o exemplo ilustra apenas os caracteres de tipo.

## Nós de Dispositivos de Caractere

Um `c` identifica um dispositivo de caractere. Ele normalmente expõe uma interface orientada a fluxo ou específica do dispositivo, em vez de blocos de armazenamento de tamanho fixo endereçáveis. Alguns exemplos são os terminais e pseudodispositivos como `/dev/null`.

“Caractere” não exige que cada chamada de sistema transfira exatamente um caractere. As aplicações podem ler ou gravar buffers, enquanto o driver define o bloqueio, o enquadramento e o comportamento de controle.

## Nós de Dispositivos de Bloco

Um `b` identifica um dispositivo de bloco. Os dispositivos de bloco fornecem armazenamento endereçável em blocos por meio da camada de blocos do kernel e podem oferecer suporte a operações como E/S com buffer, particionamento e sistemas de arquivos. Discos, partições e volumes lógicos normalmente possuem nós de bloco.

Um nó de bloco não é um sistema de arquivos montado. Ele representa um dispositivo de armazenamento ou uma região lógica; um sistema de arquivos pode ser criado nele e montado separadamente. Gravar dados brutos no nó de bloco errado pode destruir tabelas de partições, sistemas de arquivos ou dados dos usuários.

## FIFOs e Nós de Sockets

Um `p` identifica um FIFO, também chamado de pipe nomeado. Ele fornece um fluxo de bytes nomeado por meio do qual os processos podem se comunicar. Os dados não ficam armazenados de forma persistente no nó FIFO depois de serem consumidos.

Um `s` identifica um nó de socket de domínio Unix. Ele nomeia um endpoint de socket local e pode oferecer comunicação orientada a conexões ou por datagramas, passagem de descritores e recursos de credenciais dos pares. Sockets de rede que usam endereços da Internet não necessariamente possuem nós no sistema de arquivos.

Nem um FIFO nem um nó de socket Unix usa números maiores e menores de dispositivos para selecionar um driver de hardware.

## Números Maiores e Menores de Dispositivos

Os nós de dispositivos de caractere e bloco armazenam um número de dispositivo dividido em componentes maior e menor. Em uma listagem longa, eles substituem a coluna comum de tamanho do arquivo:

```text
brw-rw---- 1 root disk 8, 0 ... /dev/sda
```

O par informa ao kernel qual interface e instância de dispositivo registradas o nó endereça. Um número maior está associado a um driver ou classe de dispositivos, enquanto o driver interpreta o número menor. Não codifique suposições como “o número menor zero sempre significa a primeira unidade”; os mapeamentos dependem do subsistema e das interfaces do kernel.

Exiba explicitamente o tipo e os números do dispositivo com:

```bash
$ stat -c 'type=%F major=%t minor=%T path=%n' /dev/null
```

Os valores `%t` e `%T` são mostrados em hexadecimal pelo `stat` do GNU.



