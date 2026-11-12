# Registro do Kernel

Aprenda a consultar mensagens atuais e retidas do kernel Linux com dmesg e journalctl.

O kernel emite mensagens sobre boot, drivers, dispositivos, sistemas de arquivos, rede, memória e falhas. Esses registros explicam sintomas de baixo nível, mas um aviso isolado não prova defeito no hardware.

## Leitura do buffer circular do kernel

`dmesg` lê mensagens do buffer circular:

```bash
$ dmesg --human
```

O buffer tem capacidade finita, portanto mensagens novas podem sobrescrever antigas. O acesso também pode exigir privilégio. `dmesg --follow` acompanha novas mensagens quando compatível; interrompa depois de uma reprodução limitada.

## Uso de horários legíveis

Horários brutos do kernel costumam ser relativos ao boot. `dmesg --ctime` ou `--human` pode convertê-los para hora do relógio, mas o resultado depende do histórico do relógio e pode ficar impreciso após mudanças. Preserve o tempo relativo quando a sequência exata importar.

## Consulta de registros persistentes

Em systemd, consulte o kernel do boot atual com:

```bash
$ journalctl -k -b
```

Se boots anteriores foram retidos:

```bash
$ journalctl --list-boots
$ journalctl -k -b -1
```

O roteamento tradicional do syslog pode criar `/var/log/kern.log` ou outro arquivo, mas isso depende da configuração. Um arquivo `/var/log/dmesg` salvo também não é universal e pode representar apenas um snapshot do momento da inicialização.

## Investigação de um evento

Identifique a inicialização, o registro de horário, o dispositivo, o subsistema e a ação que ocorria naquele momento. Consulte os registros próximos do kernel e dos serviços e, em seguida, compare o inventário de hardware e o estado atual:

```bash
$ journalctl -k -b --since '10 minutes ago'
$ lspci -k
$ lsblk
```

Use apenas ferramentas relevantes. Antes de recarregar driver, desvincular dispositivo ou reiniciar, avalie impacto em armazenamento, rede, console e serviços e preserve o acesso de recuperação.


