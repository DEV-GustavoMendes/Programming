# Tarefas Cron

Aprenda a criar, inspecionar, testar e operar com segurança tarefas recorrentes com cron.

O cron executa comandos em agendas recorrentes sem um shell interativo. A automação repete tanto os comportamentos corretos quanto os erros, portanto teste o comando, use caminhos explícitos, restrinja os privilégios e planeje o registro e a notificação de falhas antes de agendá-lo.

## Leitura de uma Entrada do Crontab

Uma entrada do crontab de um usuário contém cinco campos de tempo seguidos por um comando:

```cron
30 8 * * * /home/pete/scripts/change_wallpaper
```

Da esquerda para a direita, os campos são minuto, hora, dia do mês, mês e dia da semana. Esse exemplo é executado às 08:30 conforme o fuso horário aplicável ao daemon cron. Um asterisco significa todos os valores permitidos naquele campo.

Quando os campos de dia do mês e dia da semana estão restritos, muitas implementações do cron executam quando qualquer um deles corresponde. Confirme a semântica local antes de criar uma agenda que use os dois.

## Gerenciamento do Crontab de um Usuário

Edite o crontab do usuário atual com:

```bash
$ crontab -e
```

Liste as entradas instaladas antes e depois de uma alteração:

```bash
$ crontab -l
```

`crontab -r` remove todo o crontab do usuário e pode fazer isso sem abrir um editor. Não o use para remover uma única linha; edite o crontab e verifique as entradas restantes.

## Consideração do Ambiente do Cron

O cron normalmente fornece um ambiente limitado e um shell não interativo. Use caminhos absolutos de comandos e arquivos, defina explicitamente as variáveis necessárias e não dependa de aliases, do diretório atual de um terminal ou de arquivos de inicialização do shell.

Redirecione a saída e o erro padrão para um log controlado ou use um mecanismo de notificação adequado ao sistema. Proteja as credenciais com permissões restritivas e evite incorporar segredos diretamente a um comando do crontab.

## Testes e Prevenção de Sobreposição

Execute o script manualmente como o mesmo usuário, com um ambiente igualmente mínimo. Faça-o retornar status de saída úteis e registrar resultados com timestamps. Após a instalação, aguarde uma agenda de teste inofensiva ou uma execução controlada e verifique o efeito real e os logs.

Se uma execução puder durar mais que seu intervalo, projete-a para concorrência ou use um mecanismo de bloqueio, como `flock`, quando disponível:

```cron
*/5 * * * * /usr/bin/flock -n /run/user/1000/report.lock /home/pete/bin/report
```

Escolha um caminho de bloqueio que o usuário da tarefa possa criar com segurança e decida se execuções ignoradas são aceitáveis. O cron não garante automaticamente que apenas uma instância seja executada.

## Escolha do Agendador Adequado

O cron é apropriado para comandos recorrentes simples. Os timers do systemd podem oferecer integração de dependências, execução posterior persistente, atraso aleatório e registro no journal em hosts com systemd. Agendadores de aplicações ou clusters podem ser mais seguros quando uma tarefa precisa ser executada exatamente uma vez em várias máquinas.

