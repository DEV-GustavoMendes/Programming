# Monitoramento Contínuo

Aprenda como a coleta do sysstat e os relatórios de sar apoiam a análise histórica do desempenho do Linux.

As ferramentas interativas mostram o que acontece enquanto você as observa. O monitoramento histórico é necessário quando uma lentidão já terminou. O conjunto `sysstat` coleta periodicamente contadores do sistema, e `sar` lê os contadores atuais ou arquivos de atividade salvos.

## Ativação da Coleta de Dados

Instale o pacote `sysstat` da distribuição e depois confirme que seu coletor e seu mecanismo de retenção estejam ativados. Os serviços, temporizadores e caminhos de configuração exatos variam conforme a distribuição; instalar o pacote não garante que a coleta tenha começado.

Em um host com systemd, inspecione as unidades fornecidas pelo pacote, em vez de adivinhar seus nomes:

```bash
$ systemctl list-unit-files | grep sysstat
$ systemctl list-timers --all | grep sysstat
```

Verifique se novos arquivos de atividade estão sendo criados no diretório de dados do sysstat da distribuição e revise suas permissões e política de retenção.

## Leitura de Amostras Atuais

Solicite a `sar` três relatórios da CPU em intervalos de um segundo:

```bash
$ sar -u 1 3
```

Outros relatórios comuns incluem fila de execução e carga (`-q`), memória (`-r`), paginação (`-B`), dispositivos de bloco (`-d`) e atividade por CPU (`-P ALL`). As opções e os campos variam conforme a versão do sysstat, portanto consulte `sar --help` ou o manual local.

## Leitura de Arquivos Históricos

Os locais e nomes dos arquivos salvos variam, muitas vezes ficando em `/var/log/sysstat` ou `/var/log/sa`. Forneça um arquivo de atividade selecionado com `-f`:

```bash
$ sar -q -f /var/log/sysstat/sa02
```

Confirme a data completa do arquivo nos cabeçalhos do relatório; um sufixo de dois dígitos normalmente indica o dia do mês e pode ser ambíguo entre períodos de retenção. Os formatos binários salvos também podem exigir uma versão compatível do sysstat.

## Relação com um Incidente

Determine o horário e o fuso do incidente e depois compare vários sinais no mesmo intervalo. Procure alterações na carga, CPU, fila de execução, paginação, atividade dos dispositivos, tráfego de rede e latência da aplicação. As alterações nos contadores mostram correlação, não necessariamente causalidade; os registros de implantações e logs da aplicação podem explicar o fator desencadeante.

As lacunas podem significar que o host estava desligado, que o coletor falhou ou que a retenção removeu os dados. Monitore o próprio pipeline de monitoramento para que as evidências ausentes sejam percebidas antes de um incidente.

