# Estados de energia

Aprenda a agendar, cancelar e verificar com segurança operações de desligamento e reinicialização do Linux.

Desligar ou reinicializar altera a disponibilidade de todo o sistema. Antes de agir, confirme o host de destino, obtenha autorização, avise os usuários conectados e garanta que gravações, backups e tarefas de manutenção importantes possam ser concluídos. Em um sistema remoto, preserve um console independente ou um caminho de recuperação caso a máquina não volte.

## Desligando com segurança

Em uma distribuição baseada no systemd, solicite um desligamento ordenado com:

```bash
$ sudo systemctl poweroff
```

A interface tradicional `shutdown` também está amplamente disponível:

```bash
$ sudo shutdown -h now
```

Um desligamento ordenado solicita que os serviços parem, desmonta os sistemas de arquivos e então altera o estado de energia da máquina. Não trate uma reinicialização forçada nem o botão físico de energia como um atalho comum; ambos podem interromper gravações e deixar dados ou serviços inconsistentes.

## Agendando e cancelando um desligamento

Dê aos usuários e às cargas de trabalho tempo para se preparar agendando a operação. A forma `+m` representa uma quantidade de minutos a partir de agora:

```bash
$ sudo shutdown -h +4
```

Isso agenda uma parada ou um desligamento para daqui a quatro minutos e envia avisos aos usuários conectados. Se a manutenção for adiada, cancele o desligamento pendente antes do prazo:

```bash
$ sudo shutdown -c
```

Não presuma que um aviso torna a operação segura. Verifique as sessões ativas e as cargas de trabalho específicas do sistema e siga o procedimento documentado de drenagem do serviço ou cluster, quando houver.

## Reinicializando o sistema

Use uma reinicialização ordenada quando a máquina precisar parar e iniciar novamente:

```bash
$ sudo systemctl reboot
```

Entre os comandos de compatibilidade equivalentes normalmente estão:

```bash
$ sudo shutdown -r now
$ sudo reboot
```

Antes de reinicializar, verifique se discos criptografados, a configuração de inicialização, a rede e os serviços necessários podem se recuperar sem a sessão interativa atual. Coordene primeiro o failover ou a migração da carga de trabalho quando outros sistemas dependerem do host.

## Diferenciando parada e desligamento

`halt`, `poweroff` e `reboot` podem ser interfaces de compatibilidade para o sistema de init, mas seus estados finais solicitados são diferentes. Uma parada encerra a operação normal do sistema; dependendo da plataforma e da implementação, ela pode manter a alimentação elétrica. Um desligamento também solicita que o hardware compatível corte a energia. Prefira o comando que nomeia o resultado pretendido e consulte o manual local, pois o comportamento de compatibilidade pode variar.

## Verificando o resultado

Para uma operação agendada, confirme que os usuários receberam o aviso e que o trabalho crítico foi drenado. Depois de uma reinicialização, verifique o kernel e o estado de inicialização esperados, as unidades com falha, a integridade da aplicação, as montagens de armazenamento, a conectividade de rede e os logs recentes da inicialização. Apenas conseguir iniciar uma sessão não comprova que todo o serviço se recuperou.

```bash
$ uptime
$ systemctl --failed
$ journalctl -b -p warning
```

Esses são pontos de partida; use as verificações de integridade próprias da aplicação para a carga de trabalho real.

