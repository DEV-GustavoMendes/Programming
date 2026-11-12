# Camada de enlace

Aprenda como quadros Ethernet, descoberta de vizinhos, switches e roteadores entregam pacotes em um enlace local.

A camada de enlace transporta pacotes da camada de rede através de um meio local ou enlace virtual. Ethernet e Wi-Fi usam detalhes de enquadramento diferentes, mas ambos fornecem entrega local abaixo do IP.

## Quadros Ethernet

Um quadro Ethernet contém endereços MAC de destino e origem, um campo EtherType ou de comprimento, uma carga útil e um trailer de sequência de verificação do quadro. A transmissão física também usa um preâmbulo e um delimitador de início. A sequência de verificação detecta corrupção no enlace; ela não repara um quadro danificado nem o protege criptograficamente.

## Switches e entrega local

Um switch Ethernet aprende quais endereços MAC de origem aparecem em suas portas e encaminha quadros unicast conhecidos em direção à porta aprendida para o destino. Tráfego de broadcast e algum tráfego de destino desconhecido é inundado dentro do domínio de broadcast. VLANs podem dividir um sistema de comutação em domínios lógicos de enlace separados.

## Resolvendo o endereço do próximo salto

Para IPv4 sobre Ethernet, o Protocolo de Resolução de Endereços mapeia um endereço IPv4 de próximo salto no enlace para um endereço MAC. Primeiro, o host verifica seu cache de vizinhos. Se necessário, ele transmite uma solicitação ARP em broadcast, e o proprietário ou um proxy autorizado responde.

Para um destino IP fora do enlace, o host resolve o endereço MAC do gateway padrão ou selecionado — não o endereço MAC do destino remoto. O IPv6 usa a Descoberta de Vizinhos sobre ICMPv6 em vez de ARP.

## Inspecionando o estado dos vizinhos

Consulte as entradas ARP do IPv4 e de Descoberta de Vizinhos do IPv6 com:

```bash
$ ip neighbor show
```

Estados como `REACHABLE`, `STALE`, `DELAY`, `PROBE` e `FAILED` descrevem o processo de detecção de inacessibilidade dos vizinhos. `STALE` não significa falha; significa que a confirmação de acessibilidade em cache já não é recente e pode ser testada durante o uso.

## Encapsulamento através de um roteador

O remetente coloca um pacote IP dentro de um quadro endereçado ao próximo salto. O roteador valida e remove o quadro recebido, processa o cabeçalho IP, seleciona uma rota de saída e constrói um novo quadro para esse enlace. O receptor desfaz o encapsulamento e entrega a carga útil de transporte ao socket apropriado.

