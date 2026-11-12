# Registro de Sistema

Aprenda como fontes, coletores, armazenamento e ferramentas de consulta de logs Linux se relacionam.

Logs registram eventos emitidos pelo kernel, serviços, aplicativos e componentes de segurança. Eles ajudam no diagnóstico e na auditoria, mas apenas quando a coleta funciona, os horários são compreendidos e a fonte relevante está incluída.

## Caminho de uma mensagem de log

Um caminho de logging tem partes distintas:

1. Uma fonte emite um evento.
2. Um coletor o recebe e enriquece.
3. Regras de roteamento e retenção escolhem armazenamento ou encaminhamento.
4. Uma ferramenta consulta os registros armazenados.

Em hosts systemd, `systemd-journald` costuma coletar saída de serviços, mensagens do kernel e mensagens nativas do journal ou syslog. Um daemon como rsyslog também pode receber mensagens, gravar arquivos de texto ou encaminhá-las. Aplicativos podem manter arquivos próprios ou telemetria externa.

## Descoberta dos logs disponíveis

Não presuma que todo host tenha os mesmos arquivos. Examine serviços ativos e configuração local:

```bash
$ systemctl --type=service --state=running | grep -E 'journal|syslog'
$ ls -la /var/log
$ journalctl --disk-usage
```

`/var/log/syslog` é comum na família Debian com roteamento compatível, enquanto `/var/log/messages` aparece em outros sistemas. Ambos podem faltar em um host apenas com journal. A documentação do aplicativo e a unit podem revelar destinos adicionais.

## Consulta do journal

Comece por uma consulta limitada:

```bash
$ journalctl -b -p warning
$ journalctl -u ssh.service --since '1 hour ago'
```

`-b` seleciona o boot atual, `-p` filtra prioridade e `-u` filtra uma unit. Nomes e boots retidos variam. Use `journalctl --list-boots` para listar boots e `journalctl -f` para acompanhar novos registros ao reproduzir um problema.

## Leitura dos registros em contexto

Uma linha tradicional pode ser:

```text
Jan 27 07:41:32 icebox anacron[4650]: Job `cron.weekly' started
```

Ela contém horário, host, programa e PID, depois a mensagem. Trate o texto como saída do aplicativo, não como fato estruturado garantido. Confira fuso, sincronização do relógio, ID do boot, reutilização do PID e eventos próximos. Campos do journal podem fornecer identificadores mais fortes.

Logs podem conter usuários, endereços, caminhos, tokens e outros dados sensíveis. Aplique privilégio mínimo, remova dados de exportações e preserve originais e horários numa investigação.

