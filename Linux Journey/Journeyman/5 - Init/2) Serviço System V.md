# Serviço System V

Aprenda a inspecionar e operar scripts legados de serviços SysV pela interface compatível do sistema ativo.

Os serviços SysV normalmente são representados por scripts executáveis em `/etc/init.d/`. Um script aceita ações como `start`, `stop`, `restart` ou `status`, conforme sua implementação e as convenções da distribuição. O comando `service` oferece uma interface que executa determinado script em um ambiente mais controlado.

## Descoberta dos Serviços e das Ações

Liste primeiro os nomes dos scripts:

```bash
$ ls -1 /etc/init.d/
```

Algumas implementações oferecem:

```bash
$ service --status-all
```

Seus marcadores entre colchetes e status de saída são específicos da interface, e um script pode informar um estado desconhecido. Para um serviço, inspecione a saída de uso do script ou sua documentação, em vez de presumir que todas as ações existam.

## Início e Interrupção

Em um host realmente gerenciado pelo SysV, estas formas são comuns:

```bash
$ sudo service SERVICE_NAME start
$ sudo service SERVICE_NAME stop
```

Substitua o marcador somente depois de identificar o serviço, seus dependentes, o estado atual e o impacto operacional. Interromper a rede, o acesso remoto, o armazenamento ou a autenticação a partir de uma sessão remota pode bloquear seu acesso ou corromper trabalhos ativos.

A forma direta `/etc/init.d/SERVICE_NAME ACTION` pode existir, mas, em um host cujo gerenciador ativo oferece compatibilidade, use o comando voltado ao gerenciador para que ele acompanhe o estado e as dependências.

## Recarga, Reinicialização e Estado

`restart` normalmente interrompe e depois inicia um serviço, causando uma indisponibilidade. `reload` pode solicitar que um serviço releia a configuração sem uma reinicialização completa, mas apenas quando o script e o daemon oferecerem suporte. Alguns scripts oferecem `force-reload`, com um comportamento alternativo definido pela distribuição.

Valide a configuração antes de qualquer recarga ou reinicialização, preserve uma segunda conexão administrativa ao alterar o acesso remoto e depois verifique o serviço por seu endpoint real e pelos logs — não apenas por um estado “running”.

```bash
$ sudo service SERVICE_NAME status
$ sudo service SERVICE_NAME reload
```

## Controle em Tempo de Execução e Ativação no Boot

Iniciar um serviço agora não necessariamente o habilita para runlevels futuros. A ativação no boot é representada pelos links de runlevels e gerenciada por ferramentas específicas da distribuição, como `update-rc.d`, `chkconfig` ou geradores de compatibilidade do gerenciador de serviços.

Não crie links `S` e `K` manualmente antes de compreender os metadados de dependências e a ferramenta de gerenciamento da distribuição; links manuais podem ser sobrescritos ou ordenados incorretamente.

