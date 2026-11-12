# Threads de Processos

Aprenda como as threads Linux compartilham recursos de processos e como inspecioná-las com ps.

Uma thread é um fluxo de execução escalonado dentro de um processo. Todo processo em execução possui pelo menos uma thread, e um processo multithread possui vários fluxos capazes de progredir simultaneamente.

## Processos e Threads

As threads de um mesmo processo compartilham recursos como o espaço de endereços virtual e os descritores de arquivos abertos. Cada thread ainda possui seu próprio estado de execução, incluindo registradores e uma pilha. O compartilhamento torna a comunicação eficiente, mas também significa que uma alteração não sincronizada por uma thread pode afetar as outras.

Processos separados normalmente possuem espaços de endereços distintos e se comunicam por mecanismos explícitos entre processos. Nenhum dos projetos é automaticamente mais rápido ou seguro; a carga de trabalho e a implementação determinam as compensações.

## Identificadores de Threads

O Linux representa cada thread como uma tarefa escalonável com seu próprio ID de thread. O ID do líder do grupo de threads normalmente é apresentado como o ID do processo, enquanto todos os membros compartilham um ID de grupo de threads. As ferramentas usam rótulos como `PID`, `TID`, `LWP` e `SPID`; verifique as definições dos campos da ferramenta, em vez de presumir que todos os rótulos signifiquem a mesma coisa.

## Listagem de Threads com ps

Use campos explícitos de saída para evitar layouts padrão ambíguos:

```bash
$ ps -eLo pid,tid,psr,stat,comm
```

No `ps` do procps, `-L` mostra as threads, e `-e` seleciona todos os processos. `pid` identifica o grupo de threads, `tid` identifica uma thread individual, `psr` mostra a CPU em que ela foi executada pela última vez e `stat` informa o estado. Para inspecionar um processo:

```bash
$ ps -L -p 1234 -o pid,tid,stat,pcpu,comm
```

As listagens de threads são snapshots. Uma thread pode terminar ou mudar de estado imediatamente depois.

## Interpretação da Atividade das Threads

O uso elevado de CPU em uma thread pode ficar oculto por uma média de todo o processo. Combine amostras de CPU no nível das threads com logs da aplicação, rastreamentos de pilha e ferramentas de profiling. Não anexe depuradores nem envie sinais a tarefas de produção sem compreender os impactos sobre pausas, permissões e serviços.

