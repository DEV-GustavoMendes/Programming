# Registro de Autenticação

Aprenda a localizar, interpretar e correlacionar com segurança registros de autenticação do Linux.

Logs de autenticação ajudam a explicar tentativas de login, mudanças de privilégio e atividade de sessões. São evidências sensíveis, mas uma única linha raramente demonstra a intenção de uma pessoa ou prova comprometimento da conta.

## Localização dos registros de autenticação

Configurações syslog da família Debian normalmente enviam eventos a `/var/log/auth.log`; as da família Red Hat costumam usar `/var/log/secure`. O journal pode guardar os mesmos eventos com metadados, e um coletor central pode ter a cópia oficial.

Descubra o destino local e consulte o serviço relevante:

```bash
$ sudo journalctl -u ssh.service --since '1 hour ago'
$ sudo less /var/log/auth.log
```

A unit pode se chamar `ssh.service` ou `sshd.service`. O acesso costuma ser restrito porque os registros expõem contas e detalhes de acesso.

## Interpretação de um evento

Um registro tradicional pode conter:

```text
Jan 31 10:37:50 icebox pkexec: pam_unix(polkit-1:session): session opened for user root by (uid=1000)
```

Ele identifica horário, host, programa, módulo e serviço PAM, usuário solicitado e UID de origem. Sozinho, não identifica a pessoa por trás do UID 1000 nem prova malícia. Resolva o UID conforme as contas válidas na data e correlacione terminal, endereço remoto, sessão e eventos próximos.

## Investigação de sucesso e falha

Pesquise tentativas aceitas e rejeitadas num intervalo limitado. Para SSH, examine fonte, método, conta-alvo, abertura e fechamento da sessão e reinícios do serviço. Falhas repetidas podem ser erro do usuário, automação com credenciais antigas, varredura ou ataque; a frequência sozinha não decide.

`last` e `lastb` podem resumir `wtmp` e `btmp`, quando mantidos, mas esses bancos binários têm limites de retenção e integridade. Compare-os com journal, syslog e fontes centralizadas.

## Preservação e resposta

Se houver suspeita de incidente, registre hora e fuso do host, preserve logs originais e metadados e proteja as cópias. Não edite evidências. Bloquear contas, mudar firewall e encerrar sessões pode interromper acesso legítimo ou alertar um invasor; siga o processo de resposta e mantenha recuperação.
