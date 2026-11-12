# ps (Processos)

Aprenda a obter snapshots de processos com `ps` e monitorar atividades em mudança com `top`.

Um processo é uma instância em execução de um programa, junto com sua memória, suas credenciais, seus recursos abertos e seu estado de execução. O Linux identifica cada processo ativo com um ID numérico de processo, ou PID. Um PID é exclusivo entre os processos existentes ao mesmo tempo, mas o kernel pode reutilizá-lo depois que um processo termina.

## Obtenção de um Snapshot Básico

Execute `ps` sem opções para ver um snapshot selecionado pelos padrões da implementação, normalmente os processos associados ao seu terminal e usuário atuais:

```text
$ ps
    PID TTY          TIME CMD
  41230 pts/4    00:00:00 bash
  51224 pts/4    00:00:00 ps
```

Os campos comuns incluem:

- `PID`: ID do processo
- `TTY`: terminal de controle, ou `?` quando nenhum está associado
- `TIME`: tempo de CPU acumulado, não a duração decorrida no relógio
- `CMD`: nome ou linha de comando, conforme o formato selecionado

As colunas exatas e os padrões de seleção variam entre implementações de `ps` e ambientes.

## Listagem de Processos com Opções no Estilo BSD

O `ps` do Linux aceita vários estilos de opções. As opções no estilo BSD normalmente são escritas sem um hífen inicial:

```bash
$ ps aux
```

Nessa combinação:

- `a` amplia a seleção para processos de outros usuários que possuem terminais.
- `x` também inclui processos sem terminais de controle e amplia a seleção quando combinado com `a`.
- `u` seleciona um formato de saída voltado ao usuário, com campos como `USER`, `%CPU`, `%MEM`, `VSZ` e `RSS`.

Como os significados das opções podem interagir, interprete a combinação completa em vez de tratar cada letra como um comando independente.

## Uso de Opções no Estilo Padrão

O comando amplamente usado `ps -ef`, no estilo padrão, escreve as opções com um hífen inicial:

```bash
$ ps -ef
```

- `-e` seleciona todos os processos visíveis ao solicitante.
- `-f` solicita uma listagem em formato completo.

A saída normalmente inclui `UID`, `PID`, `PPID`, horário de início e informações do comando. `PPID` é o ID do processo pai. Essa listagem não é inerentemente hierárquica; use uma opção como `--forest` quando disponível ou um visualizador de árvores dedicado, como `pstree`, quando a disposição entre pais e filhos for importante.

## Monitoramento da Atividade ao Longo do Tempo

`ps` termina depois de produzir um snapshot. Use `top` para obter uma visualização interativa atualizada periodicamente:

```bash
$ top
```

`top` ajuda a identificar consumidores variáveis de CPU e memória, mas seus valores são amostras e podem oscilar. Confirme um possível problema em várias observações e relacione as porcentagens à quantidade de CPUs da máquina, à contabilidade de memória e à carga de trabalho.

Para praticar, use o laboratório [Gerenciamento e Monitoramento de Processos Linux](https://labex.io/labs/comptia-manage-and-monitor-linux-processes-590864) para comparar snapshots com um monitor interativo ou explore a ordenação e filtragem no laboratório [Comando `top` do Linux](https://labex.io/labs/linux-linux-top-command-real-time-system-monitoring-388500).
