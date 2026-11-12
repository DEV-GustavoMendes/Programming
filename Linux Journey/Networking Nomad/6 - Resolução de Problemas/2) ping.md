# ping

Aprenda a executar testes de ping limitados e interpretar respostas, perdas, RTT, TTL e limitações.

`ping` envia ICMP Echo Requests e informa respostas observadas. Ele testa um caminho de mensagem de controle até um endereço; não prova que TCP, UDP, DNS, autenticação ou um aplicativo funcionem.

## Execução de um teste limitado

Envie três solicitações IPv4 com timeout de dois segundos em implementações iputils comuns:

```bash
$ ping -4 -c 3 -W 2 example.com
```

Use `-6` para IPv6. Registre o endereço resolvido, pois um nome pode retornar vários e execuções posteriores escolherem outro.

## Sequência e perda

`icmp_seq` identifica solicitações na execução. Respostas ausentes contribuem para a perda observada; respostas fora de ordem podem indicar atrasos diferentes. Amostras pequenas são ruidosas; compare intervalos limitados e a taxa de erros do aplicativo.

A perda pode ocorrer em qualquer direção, e rate limiting ICMP pode diferir da perda do aplicativo.

## Tempo de ida e volta

O campo `time` é o round-trip time em milissegundos, do envio à resposta. Combina atraso de ida, processamento remoto e retorno. Não revela latência de uma direção sem medições sincronizadas nas pontas.

## TTL ou Hop Limit

O TTL IPv4 ou Hop Limit IPv6 exibido é o valor restante na resposta recebida. Sem conhecer o valor inicial e a rota de retorno, subtraí-lo não dá a contagem exata de saltos. Uma mudança pode refletir outro respondedor, valor inicial ou caminho.

## Teste da camada correta

Se ping funciona e o serviço falha, teste porta, TLS, protocolo e solicitação reais. Se ping falha, examine resolução, `ip route get`, vizinhos, firewall e capturas antes de declarar o host desligado.

