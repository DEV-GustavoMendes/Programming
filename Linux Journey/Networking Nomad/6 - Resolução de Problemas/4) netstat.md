# netstat

Aprenda a inspecionar sockets, listeners, filas e estados TCP do Linux usando ss.

O legado `netstat` exibe sockets, rotas e estatísticas. No Linux moderno, `ss` é preferível para inspecionar sockets, pois expõe eficientemente o estado do kernel e é mantido com iproute2.

## Listagem de sockets em escuta

Mostre listeners TCP e UDP numericamente, com processos quando permitido:

```bash
$ sudo ss -lntup
```

`-l` seleciona listeners, `-n` evita resolução de nomes, `-t` e `-u` escolhem TCP e UDP, e `-p` solicita processos. UDP não tem conexão, portanto sockets vinculados não realizam handshake `LISTEN` como TCP.

## Portas, endpoints e serviços

Um endpoint local combina endereço, protocolo de transporte e porta. Uma conexão TCP é distinguida por protocolo e endereços e portas de origem e destino. `/etc/services` mapeia nomes convencionais, mas não prova qual processo possui uma porta nem qual protocolo de aplicação fala.

## Leitura dos estados TCP

Estados comuns:

- `SYN-SENT`: o endpoint local enviou pedido e aguarda progresso.
- `ESTAB`: a conexão está estabelecida.
- `CLOSE-WAIT`: o peer fechou seu envio, mas o aplicativo local ainda não fechou o socket.
- `TIME-WAIT`: o ponto de extremidade que fechou ativamente espera os segmentos atrasados expirarem e a troca final ser tratada com segurança.

Populações grandes ou crescentes de `CLOSE-WAIT` costumam apontar para o comportamento de limpeza da aplicação local. `TIME-WAIT` é um estado normal do protocolo; a quantidade e o impacto sobre os recursos determinam se há uma preocupação operacional.

## Interpretação das filas

O significado de `Recv-Q` e `Send-Q` depende do estado e do protocolo. Em sockets TCP estabelecidos, eles podem indicar dados na fila para recebimento pela aplicação ou para confirmação da transmissão. Em sockets em escuta, os campos de fila descrevem o estado do backlog de conexões, e não bytes da carga útil da aplicação da mesma forma.

Um snapshot não demonstra vazamento ou gargalo. Observe ao longo do tempo e correlacione processo, latência, retransmissões e limites.

## Filtragem da investigação

Limite a saída ao protocolo, estado ou endpoint:

```bash
$ ss -tn state established
$ ss -ltn 'sport = :443'
```

Um listener prova prontidão local do transporte, não alcance remoto nem saúde do aplicativo. Continue com testes de rota, firewall, pacotes, TLS e aplicação.

