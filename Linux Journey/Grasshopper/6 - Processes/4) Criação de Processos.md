# Criação de Processos

Aprenda como fork, exec, PIDs e relações de parentesco participam da criação de processos no Linux.

Os processos Linux formam relações entre pais e filhos. Um shell normalmente inicia um comando externo criando um processo filho e fazendo esse filho executar o programa solicitado. A explicação clássica separa esse trabalho nas operações `fork` e `exec`.

## Criação de um Filho com `fork`

A chamada de sistema `fork()` cria um processo filho com base no processo solicitante. O pai e o filho continuam a partir do ponto de retorno de `fork`, mas recebem valores de retorno diferentes e possuem PIDs distintos.

O filho recebe um estado de processo logicamente separado. Inicialmente, o Linux pode compartilhar páginas de memória física usando copy-on-write, copiando uma página somente quando um dos processos a modifica. Os descritores de arquivos abertos são herdados e apontam para as mesmas descrições de arquivos abertos subjacentes, portanto detalhes como os offsets dos arquivos podem continuar compartilhados.

## Substituição de um Programa com `execve`

Uma chamada `execve()` carrega um novo programa no processo solicitante. Quando bem-sucedida, ela substitui a imagem do processo e não retorna ao programa antigo. O PID permanece o mesmo porque `execve()` não cria um novo processo.

Por isso, muitos comandos do shell seguem um padrão fork-exec:

1. O shell cria um processo filho.
2. O filho prepara redirecionamentos e outros estados de execução.
3. O filho executa o programa solicitado.
4. O shell aguarda ou continua, dependendo da execução em primeiro ou segundo plano.

Bibliotecas e aplicações podem expor interfaces de nível superior, como `posix_spawn()`, e o Linux possui primitivas adicionais, como `clone()`. O conhecido modelo fork-exec continua sendo útil sem ser a única interface possível.

## Inspeção dos IDs de Pais e Filhos

`PID` identifica o processo, enquanto `PPID` identifica seu pai. Solicite esses campos explicitamente:

```bash
$ ps -o pid,ppid,stat,cmd
```

Se um shell iniciar `ps`, o PID do shell normalmente aparecerá como `PPID` desse processo `ps`. O momento importa: processos de curta duração podem terminar antes que uma observação separada os capture.

## PID 1 e Reparentalização

O kernel inicia o primeiro processo do espaço do usuário com o PID 1. Dependendo do sistema, ele pode ser `systemd`, outra implementação de init ou um init pequeno dentro de um contêiner ou namespace de PIDs. O PID 1 inicia e supervisiona partes do ambiente do espaço do usuário e possui responsabilidades especiais de sinais e coleta de processos órfãos.

Quando um pai termina antes de seu filho, o filho é reparentalizado para um subreaper apropriado ou para o processo init de seu namespace de PIDs. Ele não precisa terminar apenas porque seu pai original foi encerrado.

O laboratório [Gerenciamento e Monitoramento de Processos Linux](https://labex.io/labs/comptia-manage-and-monitor-linux-processes-590864) permite observar os IDs de pais e filhos durante a execução de comandos em primeiro e segundo plano.

