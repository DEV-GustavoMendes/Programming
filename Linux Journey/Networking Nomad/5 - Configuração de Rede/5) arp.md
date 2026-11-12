# arp

Aprenda a inspecionar e interpretar o estado do cache de vizinhos ARP IPv4 e IPv6 no Linux.

O Linux guarda endereços de enlace de próximos saltos resolvidos recentemente na tabela de vizinhos. Para IPv4 sobre Ethernet, as entradas são aprendidas por ARP; IPv6 usa Neighbor Discovery. O comando legado `arp` mostra apenas parte do estado, enquanto `ip neighbor` cobre ambas as famílias.

## Visualização das entradas

Examine todas as entradas ou uma interface:

```bash
$ ip neighbor show
$ ip neighbor show dev enp1s0
```

Uma entrada inclui IP, endereço da camada de enlace, dispositivo e estado de alcance. A tabela pode estar vazia após o boot e ser preenchida conforme o tráfego exige próximos saltos locais.

## Resolução de um vizinho IPv4

Quando falta o mapeamento de um IPv4 no mesmo link, o host transmite uma solicitação ARP perguntando quem possui o endereço. O alvo, ou um roteador com proxy ARP explícito, responde. O remetente armazena o mapeamento e envia o quadro aguardando.

Para um destino IP remoto, o host resolve o endereço do gateway escolhido, não o MAC do host remoto.

## Interpretação dos estados

Estados comuns incluem `REACHABLE`, `STALE`, `DELAY`, `PROBE`, `INCOMPLETE` e `FAILED`. `STALE` significa que a confirmação recente expirou; o endereço armazenado ainda pode ser usado enquanto a pilha testa conforme necessário. `FAILED` indica que a resolução ou detecção não teve êxito, com causas possíveis em link, VLAN, endereço, rota, filtro ou peer desligado.

## Alteração cuidadosa do estado

Entradas estáticas e limpezas mudam o estado, podem interromper tráfego e apagar evidências. Capture primeiro rotas, contadores e vizinhos. Prefira uma sondagem direcionada e captura de pacotes em rede de teste autorizada antes de limpar toda uma interface.

ARP não tem autenticação embutida; endereços duplicados ou respostas falsas podem envenenar mapeamentos. Proteções do switch, segmentação, monitoramento e autenticação em camadas superiores reduzem o impacto.

