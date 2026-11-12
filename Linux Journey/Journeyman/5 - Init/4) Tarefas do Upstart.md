# Tarefas do Upstart

Aprenda a inspecionar e controlar tarefas em um sistema legado Upstart confirmado com `initctl`.

`initctl` se comunica com um daemon init Upstart em execução. Use-o somente depois de confirmar que o namespace de PIDs relevante realmente executa o Upstart; em um host atual com systemd, use as ferramentas nativas do systemd.

## Listagem e Leitura do Estado das Tarefas

Liste as tarefas e instâncias conhecidas:

```bash
$ initctl list
```

Inspecione uma tarefa:

```bash
$ initctl status networking
networking start/running
```

O Upstart informa tanto um **objetivo**, como `start` ou `stop`, quanto um **estado** atual, como `running` ou `waiting`. `stop/waiting` significa que a tarefa não está em execução e aguarda uma condição de início ou uma solicitação manual; isso não indica necessariamente um erro.

## Início e Interrupção de uma Tarefa

Depois de revisar as dependências e o impacto:

```bash
$ sudo initctl start JOB_NAME
$ sudo initctl stop JOB_NAME
```

As tarefas podem definir várias instâncias identificadas por variáveis de ambiente. Nesse caso, forneça as variáveis exatas exigidas pela configuração e inclua-as de forma consistente ao consultar ou interromper uma instância. Iniciar tarefas de rede, armazenamento, autenticação ou acesso remoto pode prejudicar a sessão, portanto preserve o acesso de recuperação pelo console.

## Reinicialização e Alterações de Configuração

Solicite a reinicialização de uma tarefa já em execução com:

```bash
$ sudo initctl restart peanuts
```

No Upstart, `restart` nem sempre equivale a uma nova sequência de `stop` e `start` após a edição de um arquivo de tarefa: a configuração existente da tarefa em execução pode continuar sendo a autoridade. Valide o `.conf` alterado, peça ao Upstart que recarregue a configuração conforme a versão instalada e siga o procedimento documentado de parada/início quando a nova configuração precisar entrar em vigor.

Uma reinicialização causa interrupção e pode não devolver o serviço à operação. Verifique depois o endpoint real e os logs.

## Validação da Configuração da Tarefa

Antes de instalar um arquivo de tarefa modificado, use a ferramenta de validação fornecida pela distribuição legada, normalmente `init-checkconf`, e revise os scripts incluídos, o ambiente, as configurações de usuário/grupo, a política de respawn e as expressões de eventos. Depois, recarregue as definições com o fluxo de `initctl reload-configuration` apropriado à versão.

A validação da sintaxe não comprova que os caminhos existam, as credenciais permitam a execução, os eventos ocorram ou o processo fique pronto. Teste em um ambiente com capacidade de recuperação.

## Emissão Cuidadosa de Eventos

O Upstart pode emitir um evento nomeado:

```bash
$ sudo initctl emit EVENT_NAME
```

Todas as tarefas cujas expressões de início ou parada correspondam podem reagir. Um evento não é endereçado a uma única tarefa, e seus efeitos podem se propagar por outros eventos. Inspecione todas as configurações correspondentes antes de emitir um evento personalizado ou do sistema; não repita casualmente eventos essenciais de boot em um host de produção.

