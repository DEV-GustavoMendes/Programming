# Monitoramento da Memória

Aprenda a interpretar amostras de memória, paginação, processos, E/S e CPU de vmstat.

O Linux usa intencionalmente a memória que estaria ociosa para caches, portanto um valor pequeno em `free`, sozinho, não comprova pressão de memória. `vmstat` ajuda a relacionar a memória às tarefas executáveis, à paginação, à E/S e à atividade da CPU.

## Amostragem com vmstat

Colete uma amostra por segundo:

```bash
$ vmstat 1
```

A primeira linha de dados normalmente informa médias desde o boot; as linhas posteriores abrangem cada intervalo. Interrompa com `Ctrl-C` depois de capturar um período representativo. As unidades e os campos disponíveis variam, portanto consulte `vmstat --unit` e o manual local.

## Processos e Memória

Os campos comuns de processos são `r`, tarefas executáveis, e `b`, tarefas bloqueadas em sono ininterruptível. Os campos de memória incluem swap usado (`swpd`), memória ociosa (`free`), buffers (`buff`) e cache (`cache`). Esses são valores de todo o sistema, não o consumo por processo.

Para obter uma visão mais simples da memória atualmente disponível, compare com:

```bash
$ free -h
```

A estimativa `available` normalmente é mais útil que apenas `free`, pois o cache recuperável pode atender a novas alocações.

## Paginação e E/S

`si` e `so` mostram as taxas de entrada e saída do swap. Uma paginação contínua combinada com latência e atividade de recuperação de memória pode indicar pressão, mas o uso de swap (`swpd`) diferente de zero não comprova por si só um problema atual. `bi` e `bo` informam as taxas de entrada e saída de blocos e não se limitam ao tráfego de swap.

## Atividade da CPU e do Sistema

As colunas da CPU normalmente incluem as porcentagens de usuário (`us`), sistema (`sy`), ocioso (`id`), espera por E/S (`wa`) e tempo tomado (`st`). As colunas do sistema incluem interrupções (`in`) e trocas de contexto (`cs`) por segundo. Interprete os picos em relação a uma linha de base; taxas altas de troca de contexto podem ser normais para algumas cargas.

