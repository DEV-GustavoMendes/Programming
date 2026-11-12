# Monitoramento da CPU

Aprenda a interpretar as médias de carga do Linux junto com a quantidade de CPUs, a utilização e o estado das tarefas.

A solução de problemas da CPU começa pela separação entre carga, utilização e capacidade de resposta. Nenhum número isolado comprova um gargalo, portanto compare vários intervalos de tempo e relacione as métricas do host à carga de trabalho realmente percebida pelos usuários.

## Leitura de uptime

`uptime` fornece um ponto de partida compacto:

```text
$ uptime
 17:23:35 up 1 day, 5:59, 2 users, load average: 0.00, 0.02, 0.05
```

Os três valores finais são as médias de carga durante aproximadamente 1, 5 e 15 minutos. Compará-los mostra a direção: um valor de 1 minuto muito maior pode indicar carga crescente, enquanto um valor de 15 minutos maior pode indicar uma carga em queda.

## Compreensão da Carga no Linux

A carga média do Linux conta as tarefas executáveis, incluindo as que usam ou aguardam CPU, e as tarefas em sono ininterruptível, normalmente associadas à E/S. Portanto, ela não é o mesmo que a utilização da CPU.

Uma carga de `4.0` possui implicações diferentes em sistemas com uma e dezesseis CPUs lógicas. Descubra a quantidade de unidades de processamento disponíveis para o sistema com:

```bash
$ nproc
```

Cotas de CPU, afinidade, virtualização e limites de contêineres podem reduzir a capacidade visível para determinada carga de trabalho, portanto a quantidade de CPUs do host é apenas um ponto de partida.

## Comparação da Carga com a Atividade da CPU

Colete várias amostras, em vez de depender de uma única saída. Algumas ferramentas complementares úteis são:

```bash
$ top
$ vmstat 1
$ mpstat -P ALL 1
```

`top` combina visualizações do host e dos processos. `vmstat` mostra as contagens de tarefas executáveis e bloqueadas junto com categorias da CPU. `mpstat`, fornecido pelo `sysstat` em muitas distribuições, mostra a atividade por CPU. A disponibilidade e os campos exatos variam, portanto consulte os manuais locais.

Uma carga alta com CPUs ocupadas pode indicar demanda de CPU. Uma carga alta com muitas tarefas bloqueadas, latência de E/S ou observações de espera por E/S aponta para outro recurso limitado. Uma utilização média baixa também pode ocultar uma única CPU saturada ou um pico breve de latência.

## Avaliação da Capacidade e do Impacto

Não existe uma regra universal de que a carga sempre deva permanecer abaixo da quantidade de CPUs. Sistemas em lote podem aceitar filas, enquanto serviços interativos podem violar metas de latência antes desse ponto. Estabeleça uma linha de base para o mesmo host e a mesma carga de trabalho e depois compare tempo de resposta, throughput, taxa de erros, saturação e uso de recursos.

