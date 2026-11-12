# Controle de Tarefas

Aprenda como um shell interativo gerencia tarefas em primeiro plano, segundo plano e interrompidas.

Os shells interativos usam o controle de tarefas para coordenar pipelines dentro de uma sessão de terminal. Uma tarefa pode conter um processo ou um pipeline inteiro, normalmente agrupado em um grupo de processos para que o terminal e o shell possam agir sobre ele como uma unidade.

## Início de uma Tarefa em Segundo Plano

Acrescente `&` para iniciar um pipeline de forma assíncrona:

```bash
$ sleep 1000 &
[1] 18420
```

O shell retorna um prompt sem aguardar o fim da tarefa. O estado em segundo plano não redireciona automaticamente a saída, não desanexa o terminal de controle nem faz a tarefa sobreviver ao logout. Redirecione a entrada e a saída explicitamente quando necessário e use um gerenciador de serviços, agendador ou multiplexador de terminais para trabalhos que precisam sobreviver ao shell interativo.

Uma tarefa em segundo plano que tenta ler do terminal de controle normalmente é interrompida com `SIGTTIN`, pois não pertence ao grupo de processos em primeiro plano do terminal.

## Listagem das Tarefas do Shell

O comando interno `jobs` lista as tarefas conhecidas pelo shell atual:

```text
$ jobs
[1]    Running    sleep 1000 &
[2]-   Running    sleep 1001 &
[3]+   Stopped    sleep 1002
```

O número entre colchetes é um ID de tarefa do shell, não um PID. Um prefixo `%` forma uma especificação de tarefa, como `%1`. O marcador `+` identifica a tarefa atual selecionada por muitos comandos quando nenhum operando é fornecido; `-` identifica a tarefa anterior.

Como a tabela de tarefas pertence a um único shell, o shell de outro terminal normalmente não consegue listar nem endereçar essas tarefas por seus próprios comandos internos `jobs`, `fg` ou `bg`.

## Interrupção e Continuação de uma Tarefa

Enquanto uma tarefa está em primeiro plano, pressionar `Ctrl-Z` normalmente faz o terminal enviar `SIGTSTP` ao grupo de processos em primeiro plano. O shell recupera o controle depois que a tarefa é interrompida:

```text
$ sleep 1002
^Z
[3]+  Stopped    sleep 1002
```

Continue a tarefa atual interrompida em segundo plano com:

```bash
$ bg
```

`bg` envia um sinal de continuação e mantém a tarefa fora do primeiro plano do terminal. Ele só é útil para uma tarefa interrompida; um comando que já está em execução em segundo plano não precisa ser retomado.

## Movimentação de uma Tarefa para o Primeiro Plano

Use `fg` com uma especificação de tarefa para torná-la o grupo de processos em primeiro plano do terminal e aguardá-la:

```bash
$ fg %1
```

Sem um operando, `fg` normalmente seleciona a tarefa atual marcada com `+`. Uma tarefa interrompida é retomada ao entrar no primeiro plano.

## Envio de Sinais a uma Tarefa

Os shells permitem que `kill` aceite uma especificação de tarefa:

```bash
$ kill -TERM %1
```

Normalmente, isso sinaliza o grupo de processos da tarefa, não apenas um membro do pipeline. Inspecione primeiro a tarefa selecionada e use `SIGTERM` antes de considerar um escalonamento forçado. As especificações de tarefas são sintaxe do shell; scripts e ferramentas externas normalmente trabalham com PIDs ou IDs de grupos de processos verificados.

Pratique essas operações com comandos inofensivos, como `sleep`, no laboratório [Gerenciamento e Monitoramento de Processos Linux](https://labex.io/labs/comptia-manage-and-monitor-linux-processes-590864).