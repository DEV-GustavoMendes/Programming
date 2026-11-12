# Visão geral do systemd

Aprenda como o systemd carrega unidades, resolve dependências, ativa alvos e gerencia recursos do sistema e do usuário.

O systemd é o sistema de init com PID 1 e o gerenciador de serviços usado por muitas distribuições Linux atuais. O projeto systemd também fornece componentes de registro, dispositivos, login, rede, horário e outros, mas as distribuições podem escolher quais partes implantar.

## Confirmando o gerenciador em execução

Inspecione o estado ativo em vez de verificar a existência de diretórios instalados:

```bash
$ ps -p 1 -o pid,comm,args=
$ systemctl is-system-running
```

`/usr/lib/systemd/` pode existir em um sistema no qual outro programa é o PID 1, e um contêiner pode expor seu próprio namespace de PID. O `systemctl` também possui modos para gerenciador de usuário e para sistemas remotos ou contêineres; portanto, identifique qual gerenciador é o destino de uma operação.

## Unidades como objetos gerenciados

Uma unidade é o modelo nomeado do systemd para um recurso ou uma atividade. Entre os tipos comuns de unidade estão:

- `.service` para processos e daemons
- `.socket` para ativação por socket
- `.mount` e `.automount` para sistemas de arquivos
- `.timer` e `.path` para ativação orientada a eventos
- `.target` para agrupamento e sincronização
- `.device`, `.swap`, `.slice` e `.scope` para outros recursos gerenciados

O estado de uma unidade nem sempre é “em execução”. Uma montagem pode estar montada, um temporizador pode estar aguardando, um dispositivo pode estar presente e um alvo pode estar ativo depois que suas dependências são alcançadas.

## Caminhos de carregamento e substituições de unidades

Unidades do sistema podem ser carregadas de caminhos da distribuição e do administrador, como:

- `/usr/lib/systemd/system/` para unidades fornecidas por pacotes em muitas distribuições
- `/run/systemd/system/` para configurações geradas em tempo de execução ou transitórias
- `/etc/systemd/system/` para configurações e substituições locais persistentes do administrador

Os caminhos exatos dos fornecedores podem variar. Configurações locais de prioridade mais alta substituem arquivos de prioridade mais baixa com o mesmo nome de unidade. Prefira substituições complementares criadas com `systemctl edit UNIT` a copiar e modificar um arquivo completo do fornecedor, para que as atualizações de pacotes continuem visíveis.

## Dependências e ordenação

O systemd cria uma transação com base nos relacionamentos de dependência. `Wants=` e `Requires=` incluem outras unidades em uma transação com intensidades diferentes. `Before=` e `After=` especificam a ordem quando ambas as unidades estão agendadas; por si só, eles não fazem outra unidade iniciar.

Uma linha `After=network.target` não comprova que uma conectividade utilizável, o DNS ou um ponto de extremidade remoto específico estejam prontos. Os serviços devem usar a integração apropriada com o estado de rede online ou implementar seu próprio comportamento de repetição e prontidão.

## Alvos e a transação de inicialização padrão

`default.target` normalmente é um alias para um alvo como `multi-user.target` ou `graphical.target`. O systemd inicia uma transação para esse alvo e suas dependências, permitindo que trabalhos não relacionados prossigam simultaneamente enquanto aplica a ordenação explícita.

Os alvos se assemelham aos níveis de execução apenas em um sentido amplo de compatibilidade. Vários alvos podem estar ativos ao mesmo tempo, alvos personalizados podem ser criados, e a atividade de um alvo não significa que todos os serviços da máquina estejam íntegros.

