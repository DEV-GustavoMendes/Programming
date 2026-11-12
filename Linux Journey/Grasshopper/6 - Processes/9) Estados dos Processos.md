# Estados dos Processos

Aprenda a interpretar os códigos comuns de estados de processos Linux em snapshots de `ps`.

Uma tarefa Linux transita entre estados de execução enquanto trabalha, aguarda, é interrompida e termina. O campo `STAT` de `ps` captura um único momento, portanto observações repetidas são mais úteis que uma única letra ao diagnosticar um comportamento.

```bash
$ ps -o pid,ppid,stat,wchan:24,cmd
```

O primeiro caractere de `STAT` é o estado principal. Os caracteres adicionais são modificadores que descrevem propriedades como a liderança da sessão ou a participação no grupo de processos em primeiro plano. Consulte o manual local de `ps` para conhecer o conjunto completo.

## Execução e Sono Interrompível

- `R` significa em execução ou executável. A tarefa está sendo executada em uma CPU ou aguardando tempo de CPU em uma fila de execução.
- `S` significa sono interrompível. A tarefa aguarda um evento e pode ser despertada por um sinal ou evento apropriado.

Dormir é normal. Programas interativos e serviços passam grande parte do tempo aguardando entradas, temporizadores, tráfego de rede, bloqueios ou outros eventos, em vez de consumir CPU continuamente.

## Sono Ininterruptível

`D` significa sono ininterruptível, normalmente enquanto a tarefa aguarda em uma operação do kernel, como determinados tipos de E/S de armazenamento ou de sistemas de arquivos de rede. A tarefa não reage a sinais comuns até sair dessa espera; enquanto isso, um sinal pode permanecer pendente.

Um estado `D` breve pode ser normal. Tarefas persistentes ou numerosas em `D` podem indicar E/S lenta, indisponível ou com falhas, mas o estado sozinho não identifica a causa. Inspecione o canal de espera, os logs do kernel, a integridade do armazenamento e da rede e o subsistema relevante antes de tirar conclusões.

## Estados Interrompido e Zumbi

- `T` normalmente significa interrompido por uma ação de controle de tarefas, como `SIGTSTP`, ou por `SIGSTOP`. Algumas ferramentas usam `t` minúsculo para uma parada de rastreamento.
- `Z` significa zumbi: o processo terminou, mas seu pai ainda não coletou o registro de encerramento.

Retome uma parada por controle de tarefas com `SIGCONT` quando for apropriado. Um zumbi não pode ser retomado nem encerrado, pois já não está executando; seu pai ou um coletor adotante precisa coletá-lo.

## Leitura dos Estados em Contexto

Os códigos de estado são observações, não diagnósticos. Combine-os com tempo decorrido, uso da CPU, canais de espera, relações entre pais e filhos, logs e amostras repetidas. Uma tarefa pode mudar de estado entre o instante em que o kernel o informa e o momento em que você lê a tela.

O laboratório [Gerenciamento e Monitoramento de Processos Linux](https://labex.io/labs/comptia-manage-and-monitor-linux-processes-590864) oferece um ambiente seguro para observar tarefas em primeiro plano, dormindo, interrompidas e encerradas.

