# Sistema de Arquivos /proc

Aprenda como o Linux expõe informações ativas de processos e do kernel por meio do sistema de arquivos virtual `/proc`.

O Linux normalmente monta `procfs` em `/proc`. Esse sistema de arquivos virtual apresenta interfaces geradas pelo kernel como arquivos e diretórios; seu conteúdo não é formado por arquivos persistentes comuns armazenados no disco. Ele expõe o estado dos processos e determinadas informações do kernel para todo o sistema.

## Localização dos Diretórios de Processos

Liste a montagem e as entradas do nível superior com:

```bash
$ findmnt /proc
$ ls /proc
```

Os nomes numéricos dos diretórios correspondem aos IDs de processos visíveis no namespace de PIDs do solicitante. Por exemplo, `/proc/12345` representa o PID 12345 no instante em que ele existe. `/proc/self` é um link simbólico que aponta para o diretório do próprio processo observador, e `/proc/thread-self` identifica a thread atual.

A visibilidade e o acesso dependem das credenciais, dos namespaces, da política de segurança e das opções de montagem do procfs, como `hidepid`. Um processo pode terminar entre a listagem de um diretório e a abertura de um de seus arquivos, portanto o desaparecimento é uma condição de corrida normal que as ferramentas de inspeção precisam tratar.

## Leitura das Informações de Processos

Inspecione o arquivo de estado de um processo quando as permissões permitirem:

```bash
$ less /proc/12345/status
```

Ele inclui campos como nome do processo, estado, IDs, credenciais, contadores de memória, capacidades e máscaras de sinais. Outras entradas úteis são:

- `/proc/12345/cmdline`: argumentos da linha de comando separados por bytes nulos
- `/proc/12345/environ`: entradas do ambiente, com controle de acesso e potencialmente confidenciais
- `/proc/12345/fd/`: links simbólicos que representam descritores de arquivos abertos
- `/proc/12345/maps`: mapeamentos atuais de memória
- `/proc/12345/cwd`: link simbólico para o diretório de trabalho atual

Trate essas informações como observações mutáveis. Os campos podem variar conforme a versão do kernel, um processo pode mudar de estado durante a leitura de vários arquivos, e alguns contadores possuem detalhes que seus nomes, sozinhos, não revelam.

## Leitura de Interfaces de Todo o Sistema

Nem toda entrada de `/proc` pertence a um processo. Alguns exemplos são:

- `/proc/cpuinfo` para informações de CPU fornecidas pelo kernel
- `/proc/meminfo` para contadores de memória do sistema
- `/proc/mounts` para a visão das montagens do processo atual
- `/proc/loadavg` para informações de carga média e tarefas executáveis
- `/proc/sys/` para parâmetros do kernel em tempo de execução

Alguns arquivos, especialmente em `/proc/sys`, são interfaces de configuração graváveis. Não grave neles apenas porque parecem arquivos comuns. Compreenda o parâmetro, o escopo, o mecanismo de persistência e a reversão antes de realizar uma alteração autorizada no sistema.

## Uso de `/proc` por Meio de Ferramentas

As implementações Linux de ferramentas como `ps`, `top` e `free` obtêm grande parte de seus dados do procfs e de outras interfaces do kernel e então os rotulam, calculam e formatam. Prefira essas ferramentas no trabalho comum quando elas fornecerem o campo necessário; leia `/proc` diretamente para detalhes específicos ou scripts somente depois de estudar a documentação da interface.

Leitores diretos precisam interpretar os formatos corretamente, tolerar processos ausentes, proteger saídas confidenciais e evitar presumir que uma leitura seja um snapshot atômico do sistema.

