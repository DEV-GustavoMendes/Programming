# lsof e fuser

Aprenda a identificar processos que usam arquivos, diretórios, pontos de montagem e sockets de rede.

Um sistema de arquivos pode continuar ocupado porque um processo possui um arquivo aberto, mapeia um arquivo na memória ou usa um diretório como seu diretório de trabalho atual. `lsof` e `fuser` ajudam a identificar essas relações. Inspecione primeiro; interromper processos é uma decisão separada, com consequências operacionais.

## Listagem de Arquivos Abertos com lsof

`lsof` significa “list open files”. Consulte um caminho para ver os registros correspondentes de arquivos abertos:

```bash
$ sudo lsof -- /mnt/usb
```

Para uma árvore inteira de diretórios no mesmo sistema de arquivos, as implementações normalmente oferecem `+D`, mas as verificações recursivas podem ser caras:

```bash
$ sudo lsof +D /mnt/usb
```

As colunas úteis incluem `COMMAND`, `PID`, `USER`, descritor de arquivo (`FD`), tipo, dispositivo e `NAME`. Um registro cujo `FD` seja `cwd` indica que o processo usa o diretório como seu diretório de trabalho atual. A saída sem privilégios pode ser incompleta para processos de outros usuários.

## Identificação dos Usuários com fuser

`fuser` informa os IDs dos processos que usam determinado arquivo ou sistema de arquivos. A saída detalhada acrescenta usuários, tipos de acesso e nomes de comandos:

```bash
$ sudo fuser -v /mnt/usb
```

Para tratar o argumento como um sistema de arquivos montado e localizar processos que acessam arquivos dentro dele, use a opção de montagem oferecida pelo `fuser` do procps:

```bash
$ sudo fuser -vm /mnt/usb
```

Verifique se o caminho é o ponto de montagem pretendido com ferramentas como `findmnt --target /mnt/usb`. Montagens bind, namespaces, permissões e condições de corrida podem afetar o que uma única consulta revela.

## Tratamento de um Sistema de Arquivos Ocupado

Use uma sequência deliberada, em vez de encerrar imediatamente todos os PIDs correspondentes:

1. Confirme o host, o caminho, a origem da montagem e a manutenção pretendida.
2. Identifique os processos com as duas ferramentas quando for viável.
3. Determine se cada processo pode ser interrompido, movido para fora do diretório ou deixado terminar.
4. Interrompa-o pelo gerenciador de serviços ou pela interface da aplicação quando disponível.
5. Consulte novamente, desmonte e verifique o resultado.

`fuser -k` envia um sinal aos processos correspondentes. Seu sinal padrão é `SIGKILL` nas implementações comuns do procps, portanto ele não oferece um encerramento ordenado. Se um encerramento explicitamente aprovado for necessário, selecione um sinal adequado, verifique o PID e o proprietário e considere que o conjunto de processos pode mudar entre a inspeção e a ação.

## Escolha da Ferramenta

Use `lsof` quando precisar de registros detalhados de arquivos abertos, descritores ou informações de sockets. Use `fuser` para uma visão orientada a caminhos dos PIDs e tipos de acesso correspondentes. Nenhum resultado, sozinho, informa se é seguro encerrar um processo.

Para sockets de rede, use um namespace de protocolo explícito com `fuser` ou uma ferramenta voltada a sockets, como `ss`:

```bash
$ sudo fuser -v 22/tcp
$ sudo ss -lntp
```

