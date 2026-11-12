# Acompanhamento de Processos: top

Aprenda a usar top para interpretar a carga do sistema, CPU, memória e atividade por processo.

`top` fornece uma visualização atualizada repetidamente da atividade do sistema e dos processos em execução. Ele é útil para formular uma hipótese de desempenho, mas uma única amostra com muita atividade não comprova a causa de um problema. Compare várias atualizações e relacione-as aos logs e às métricas específicas da carga de trabalho.

## Leitura do Resumo do Sistema

Uma exibição comum começa com linhas de resumo seguidas por uma tabela de processos:

```text
top - 18:06:26 up 6 days, 4:07, 2 users, load average: 0.92, 0.62, 0.59
Tasks: 389 total, 1 running, 387 sleeping, 0 stopped, 1 zombie
%Cpu(s): 1.8 us, 0.4 sy, 0.0 ni, 97.6 id, 0.1 wa, 0.0 hi, 0.0 si, 0.0 st
MiB Mem : 32099.0 total, 5276.3 free, 7031.2 used, 19791.5 buff/cache
MiB Swap: 32700.0 total, 32661.0 free, 39.0 used
```

A primeira linha contém o horário atual, o tempo em atividade, a quantidade de usuários conectados e as médias de carga de 1, 5 e 15 minutos. A linha de tarefas conta os estados dos processos. A carga média não é uma porcentagem direta de CPU; no Linux, ela representa tarefas executáveis e tarefas em sono ininterruptível, portanto interprete-a junto com a quantidade de CPUs, a atividade de E/S e a latência.

## Interpretação do Tempo de CPU

Alguns campos comuns da CPU são:

- `us`: tempo de execução no espaço do usuário.
- `sy`: tempo de execução no kernel.
- `ni`: tempo no espaço do usuário para tarefas com nice ajustado.
- `id`: tempo ocioso.
- `wa`: tempo ocioso enquanto existe uma solicitação de E/S pendente.
- `hi` e `si`: tratamento de interrupções de hardware e software.
- `st`: tempo de CPU virtual usado pelo hipervisor para outros convidados.

Um valor alto de `wa` pode sustentar uma hipótese de espera por E/S, mas não identifica um dispositivo nem comprova que o armazenamento seja o único gargalo. Inspecione a latência do dispositivo e o comportamento da aplicação antes de concluir.

## Leitura da Tabela de Processos

As colunas importantes normalmente incluem:

- `PID`, `USER` e `COMMAND`: identidade e propriedade.
- `S`: estado, como em execução (`R`), dormindo (`S`), sono ininterruptível (`D`), interrompido (`T`) ou zumbi (`Z`).
- `%CPU` e `%MEM`: atividade amostrada da CPU e parcela da memória física.
- `TIME+`: tempo de CPU acumulado.
- `VIRT`: espaço total de endereços virtuais associado à tarefa.
- `RES`: memória física residente e não transferida para o swap atualmente atribuída à tarefa.
- `SHR`: memória residente que pode ser compartilhada com outros processos.

`VIRT` não é a quantidade de RAM física consumida. Ele pode incluir arquivos mapeados, bibliotecas compartilhadas, espaço de endereços reservado e páginas no swap. Até mesmo `RES` deve ser interpretado com cuidado, pois as páginas compartilhadas complicam a atribuição.

## Foco e Ordenação

Monitore diretamente PIDs conhecidos:

```bash
$ top -p 1234,5678
```

Dentro de `top`, pressione `P` para ordenar por CPU, `M` para ordenar por memória, `1` para alternar as linhas por CPU e `q` para sair nas implementações comuns do procps-ng. Pressione `h` para consultar a ajuda interativa local, pois as teclas e os campos podem variar entre implementações.

Registre o PID, o comando, o timestamp e várias amostras antes de agir. Um processo chegar brevemente ao topo pode ser normal, e encerrá-lo pode causar perda de dados ou uma indisponibilidade.

