# Objetivos do systemd

Aprenda a inspecionar, substituir, validar, iniciar, habilitar e solucionar problemas de unidades de serviço do systemd.

`systemctl` envia solicitações a um gerenciador systemd. Esta lição se concentra nas unidades de serviço do sistema. Confirme o nome exato da unidade, o escopo do gerenciador, as dependências e o impacto operacional antes de alterar o estado.

## Lendo uma unidade de serviço

Uma unidade ilustrativa mínima pode ter esta aparência:

```ini
[Unit]
Description=Example worker
Wants=network-online.target
After=network-online.target

[Service]
Type=exec
ExecStart=/usr/local/bin/example-worker
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

- `[Unit]` contém a descrição e os relacionamentos de dependência.
- `[Service]` define o ciclo de vida do processo e o comportamento específico do serviço.
- `[Install]` informa aos comandos de habilitação quais aliases ou links de dependência devem ser criados; ela não é automaticamente uma dependência ativa em tempo de execução.

`ExecStart=` não é passado por um shell por padrão. Pipelines, redirecionamentos, variáveis e aspas do shell não se comportam como em uma linha de comando interativa, a menos que um shell explícito seja invocado intencionalmente.

## Inspecionando a configuração efetiva

Liste as unidades carregadas com:

```bash
$ systemctl list-units --type=service
```

Liste os arquivos de unidade instalados e seus estados de habilitação com:

```bash
$ systemctl list-unit-files --type=service
```

Essas são visualizações diferentes: um arquivo de unidade pode estar habilitado, mas inativo; ativo, mas desabilitado; estático; gerado; transitório; mascarado; ou ausente em uma das listas. Inspecione o conteúdo combinado do fornecedor e dos complementos com:

```bash
$ systemctl cat UNIT.service
$ systemctl show UNIT.service
```

## Criando uma substituição local

Use um complemento em vez de editar uma unidade empacotada:

```bash
$ sudo systemctl edit UNIT.service
```

Após salvar, nas implementações atuais, o systemctl normalmente solicita que o gerenciador recarregue como parte desse fluxo de edição. Porém, quando os arquivos forem alterados por outro método, execute:

```bash
$ sudo systemctl daemon-reload
```

`daemon-reload` relê as definições de unidade e reconstrói as dependências. Ele não recarrega a configuração da aplicação nem reinicia os serviços em execução. Quando apropriado, valide a sintaxe e as dependências da unidade com `systemd-analyze verify` e depois revise a unidade efetiva combinada.

## Estado do serviço em tempo de execução

Depois de validar a configuração do serviço e preservar um acesso de recuperação:

```bash
$ sudo systemctl start peanut.service
$ sudo systemctl stop peanut.service
$ sudo systemctl restart peanut.service
$ sudo systemctl reload peanut.service
```

`reload` só funciona quando a unidade define ou oferece suporte a uma ação de recarregamento. `restart` interrompe o processo e pode não conseguir restaurar o serviço. Para acesso remoto, rede, armazenamento ou autenticação, mantenha um caminho de console separado e verifique a configuração antes de agir.

Verifique o estado e os logs com:

```bash
$ systemctl status peanut.service
$ systemctl is-active peanut.service
$ journalctl -u peanut.service -b
```

“Ativo” é um estado do gerenciador, não uma prova de que todos os pontos de extremidade da aplicação estão íntegros.

## Habilitação, desabilitação e mascaramento

Gerencie os links de dependência futuros com:

```bash
$ sudo systemctl enable peanut.service
$ sudo systemctl disable peanut.service
```

Enable não inicia a unidade, a menos que `--now` seja adicionado. Disable não para uma unidade em execução, a menos que `--now` seja adicionado. Uma unidade estática pode não ter metadados de instalação e ainda assim ser ativada como dependência de outra unidade.

O mascaramento vincula a unidade a `/dev/null` e bloqueia a ativação comum, inclusive por dependência, até que ela seja desmascarada. Ele é mais forte que disable e pode interromper dependentes; inspecione as dependências reversas antes de usá-lo.

## Verifique o resultado do serviço

Após uma alteração, verifique o estado do processo, os logs recentes, os pontos de extremidade em escuta, as unidades dependentes, a integridade da aplicação e o comportamento após uma reinicialização controlada caso a habilitação de inicialização tenha mudado. Use `systemctl is-failed`, `systemctl list-dependencies` e verificações próprias da aplicação conforme apropriado.