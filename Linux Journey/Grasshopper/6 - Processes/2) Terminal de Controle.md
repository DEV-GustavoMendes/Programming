# Terminal de Controle

Aprenda como terminais de controle conectam sessões à entrada interativa, aos sinais e ao controle de tarefas do shell.

Uma sessão de login interativa pode possuir um terminal de controle: um dispositivo de terminal associado à sessão e usado pelo kernel para sinais gerados pelo terminal e para o controle de tarefas. O campo `TTY` nas listagens de processos ajuda a identificar essa associação.

## Dispositivos de Terminal e Pseudoterminal

O nome TTY vem dos teletipos históricos. No Linux moderno, as interfaces de terminal são abstrações de dispositivos e não necessariamente equipamentos físicos.

Um console virtual do sistema pode aparecer com um nome como `tty1`. Os atalhos da área de trabalho para alternar entre consoles variam conforme a distribuição e não devem ser presumidos. Um emulador de terminal, login remoto ou multiplexador normalmente usa um par de pseudoterminais, com o lado interativo mostrado por um nome como `pts/3`.

Exiba o terminal conectado à entrada padrão do comando atual com:

```bash
$ tty
/dev/pts/3
```

Esse resultado está relacionado ao conceito mais amplo de terminal de controle, mas não é idêntico a ele. Um processo pode redirecionar a entrada ou saída padrão e ainda permanecer em uma sessão que possui um terminal de controle.

## Sessões, Grupos de Processos e Controle de Tarefas

Um terminal de controle pertence a uma sessão, não apenas ao comando que abriu uma janela. Dentro dessa sessão, o terminal acompanha um grupo de processos em primeiro plano. O shell coloca um pipeline em primeiro plano nesse grupo para que ele possa ler entradas e receber sinais gerados pelo terminal.

Por exemplo, pressionar `Ctrl-C` normalmente faz o driver do terminal enviar `SIGINT` ao grupo de processos em primeiro plano. Um grupo em segundo plano que tenta ler do terminal pode receber `SIGTTIN`. Essas regras permitem que o shell coordene tarefas em primeiro e segundo plano.

## Leitura da Coluna `TTY`

Solicite explicitamente os campos de processos quando quiser uma visualização estável:

```bash
$ ps -o pid,tty,stat,cmd
```

Um nome de terminal como `pts/3` identifica o terminal de controle registrado para aquele processo. Um ponto de interrogação (`?`) normalmente significa que o processo não possui um terminal de controle.

Muitos processos de serviços não possuem um terminal de controle porque um gerenciador de serviços os inicia independentemente de uma sessão de login interativa. Contudo, a ausência de uma TTY não comprova por si só que um processo seja um daemon, e uma tarefa do shell em segundo plano ainda pode possuir um terminal de controle.

## Fechamento do Terminal e Hangups

Quando a conexão de um terminal desaparece, o kernel ou o software do terminal/sessão pode enviar `SIGHUP` aos processos associados. Um processo pode terminar, capturar o sinal, ignorá-lo ou já ter sido configurado para sobreviver a ele. Recursos do shell como `disown`, utilitários como `nohup`, multiplexadores e gerenciadores de serviços afetam o comportamento do ciclo de vida.

Portanto, fechar um terminal não garante que todos os comandos iniciados nele terminarão. Inspecione a sessão do processo, o tratamento dos sinais, os redirecionamentos e o supervisor quando a persistência for importante.

O laboratório [Gerenciamento e Monitoramento de Processos Linux](https://labex.io/labs/comptia-manage-and-monitor-linux-processes-590864) oferece um ambiente seguro para comparar tarefas em primeiro e segundo plano e seus campos `TTY`.
