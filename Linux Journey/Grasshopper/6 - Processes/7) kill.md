# kill (Encerrar)

Aprenda a identificar um processo e enviar um sinal adequado com `kill` usando uma sequência segura de escalonamento.

O comando `kill` envia um sinal a um processo ou grupo de processos. Seu nome é histórico: o sinal solicitado pode encerrar, interromper, continuar ou pedir alguma ação definida pela aplicação. Sempre confirme o destino exato e compreenda o comportamento documentado dos sinais do programa antes de enviar um.

## Solicitação de um Encerramento Ordenado

Com apenas um PID, `kill` envia `SIGTERM` por padrão:

```bash
$ kill 12445
```

Prefira o nome simbólico ao especificar um sinal explicitamente:

```bash
$ kill -TERM 12445
```

`SIGTERM` possui como ação padrão o encerramento, mas um programa pode capturá-lo ou ignorá-lo. Um serviço bem projetado pode usar um manipulador para parar de aceitar trabalhos, salvar o estado apropriado e liberar recursos da aplicação. Essa é uma possibilidade, não uma garantia de limpeza imediata ou bem-sucedida.

## Verificação do Destino

Os PIDs podem ser reutilizados, portanto um PID antigo pode identificar outro processo mais tarde. Inspecione o destino ativo imediatamente antes de agir:

```bash
$ ps -p 12445 -o pid,ppid,user,lstart,stat,cmd
```

Verifique seu usuário, horário de início, comando, pai, serviço proprietário e função operacional. Se um gerenciador de serviços controlar o processo, use o comando de parada ou recarga desse gerenciador quando possível, para que ele mantenha o estado correto e não reinicie imediatamente o filho.

Você pode sinalizar os processos que possui, sujeito às regras de credenciais. Sinalizar o processo de outro usuário normalmente exige privilégios adequados. Não use um comando amplo baseado em nomes antes de revisar todas as correspondências.

## Verificação da Permissão de Sinal com o Sinal Zero

O sinal de número zero realiza verificações de erros sem entregar um sinal real:

```bash
$ kill -0 12445
```

Um resultado bem-sucedido significa que um processo com esse PID existe e que o solicitante tem permissão para sinalizá-lo naquele instante. Uma falha é ambígua: o processo pode não existir ou o solicitante pode não ter permissão. Examine o erro e o status de saída, em vez de traduzir toda falha como “não está em execução”. Essa também é apenas uma verificação momentânea e não elimina uma condição de corrida posterior por reutilização do PID.

## Escalonamento Somente Quando Necessário

Se um destino autorizado não terminar após `SIGTERM`, aguarde um tempo adequado à carga de trabalho e investigue o motivo. Então, quando o encerramento forçado for justificável, envie:

```bash
$ kill -KILL 12445
```

`SIGKILL` não pode ser capturado, ignorado nem bloqueado, portanto o programa não consegue realizar uma limpeza no nível da aplicação. Ele pode deixar transações incompletas, estados temporários ou trabalho de recuperação para outros componentes. Use-o como escalonamento, não como primeira medida rotineira.

Outros sinais só possuem significado conforme o contrato do programa receptor. `SIGHUP` costuma solicitar a recarga da configuração, mas alguns programas mantêm seu comportamento padrão de encerramento. `SIGSTOP` pausa sem limpeza, e `SIGCONT` retoma um processo interrompido.

Pratique a seleção de sinais somente em processos que você iniciou em um ambiente isolado. O laboratório [Gerenciamento e Monitoramento de Processos Linux](https://labex.io/labs/comptia-manage-and-monitor-linux-processes-590864) oferece um fluxo controlado de inspeção e encerramento.