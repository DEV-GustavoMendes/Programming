# syslog

Aprenda como funcionam facilities, níveis de gravidade, regras de roteamento e o comando logger do syslog.

Syslog define um modelo de mensagens e convenções de transporte usadas por muitos sistemas semelhantes ao Unix. Rsyslog é uma implementação que recebe, filtra, transforma, armazena e encaminha mensagens. Ele pode coexistir com `systemd-journald`; nenhum dos nomes significa que todo aplicativo usa esse caminho.

## Facilities e níveis de gravidade

Uma mensagem syslog carrega uma facility que descreve a categoria geral da fonte e uma gravidade de emergency a debug. Facilities comuns incluem `auth`, `cron`, `daemon`, `kern`, `mail`, `user` e `local0` a `local7`.

Os níveis são ordenados. Na sintaxe clássica, `daemon.warning` normalmente seleciona mensagens warning e todas as mais graves da facility daemon, não apenas warning. Em implementações compatíveis, a correspondência exata usa `daemon.=warning`.

## Leitura das regras do rsyslog

Rsyslog costuma carregar um arquivo principal e trechos de `/etc/rsyslog.d/`. Uma regra tradicional tem seletor e ação:

```text
auth,authpriv.*          /var/log/auth.log
*.*;auth,authpriv.none  -/var/log/syslog
kern.*                  /var/log/kern.log
```

A primeira envia todas as prioridades de duas facilities de autenticação. A segunda seleciona amplamente e exclui essas facilities. A terceira envia mensagens do kernel. Um `-` antes do arquivo normalmente solicita escritas assíncronas; não indica exclusão.

Examine todos os arquivos incluídos e valide a sintaxe da versão instalada antes de mudar o roteamento de produção.

## Envio de uma mensagem de teste

Use `logger` para enviar um teste controlado com uma tag e uma prioridade identificáveis:

```bash
$ logger -p user.notice -t lesson-test 'routing check 2026-08-31T10:00'
```

Em seguida, consulte o destino esperado, por exemplo:

```bash
$ journalctl -t lesson-test --since '5 minutes ago'
```

O mesmo evento pode aparecer no journal e em um arquivo de texto, dependendo do encaminhamento e do roteamento. `logger -s` também copia a mensagem para a saída de erro padrão; isso não comprova o armazenamento durável.

## Alteração e verificação do roteamento

Antes de uma mudança, salve a configuração atual e identifique os consumidores posteriores. Valide a sintaxe com o modo de verificação de configuração da implementação, normalmente:

```bash
$ sudo rsyslogd -N1
```

Somente depois da validação recarregue o serviço por meio de seu gerenciador. Envie uma nova mensagem marcada, verifique todos os destinos necessários e confira o estado do serviço e os logs de erros internos. Uma regra sintaticamente válida ainda pode rotear de forma ampla demais, duplicar registros ou expor dados confidenciais.

Encaminhamento remoto deve usar transporte autenticado e criptografado em redes não confiáveis. UDP não oferece confirmação de ponta a ponta; auditoria crítica precisa considerar filas, perdas, integridade, acesso e indisponibilidade do receptor.

