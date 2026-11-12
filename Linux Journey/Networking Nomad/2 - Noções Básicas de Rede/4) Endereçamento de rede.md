# Endereçamento de rede

Aprenda como endereços de enlace, endereços IP e nomes de host identificam diferentes partes da comunicação de rede.

A comunicação de rede usa identificadores diferentes em escopos diferentes. Endereços da camada de enlace entregam quadros em um enlace local, endereços IP permitem a entrega roteada, e nomes ajudam aplicações e pessoas a selecionar serviços.

## Endereços da camada de enlace

Um endereço MAC Ethernet possui 48 bits e normalmente é escrito como seis octetos hexadecimais, por exemplo, `00:c4:b5:45:b2:43`. Um endereço de origem identifica uma interface no enlace atual, enquanto o destino pode ser unicast, multicast ou broadcast.

Não há garantia de que endereços MAC sejam permanentes ou globalmente exclusivos. O software pode atribuir um endereço administrado localmente, interfaces virtuais geram endereços, e recursos de privacidade do Wi-Fi podem torná-los aleatórios. Normalmente, os roteadores substituem o enquadramento Ethernet em cada salto; portanto, um servidor remoto não recebe o endereço Ethernet de origem do enlace local original.

## Endereços IP e prefixos

Endereços IPv4 possuem 32 bits, ou quatro octetos, enquanto endereços IPv6 possuem 128 bits. Um endereço IP normalmente é atribuído a uma interface e interpretado com um comprimento de prefixo, como `192.0.2.10/24` ou `2001:db8::10/64`. O prefixo identifica quais bits iniciais descrevem a rede.

Uma interface pode ter vários endereços IP, e um endereço pode mudar por DHCP, endereçamento de privacidade, failover ou administração. Endereços IPv4 privados podem ser reutilizados em redes separadas; as políticas de roteamento público e NAT determinam a acessibilidade externa.

## Nomes de host e resolução de nomes

Um nome de host é um nome, não um endereço. A resolução de nomes pode consultar `/etc/hosts`, DNS, sistemas multicast ou outras fontes, de acordo com a configuração de serviços de nomes do host. Um nome pode ser resolvido para vários endereços, e vários nomes podem se referir a um serviço.

Use o caminho do resolvedor do sistema ao testar o que uma aplicação provavelmente verá:

```bash
$ getent ahosts example.com
```

As respostas do DNS podem mudar ou estar em cache, e uma resolução bem-sucedida não comprova que o serviço está acessível.

## Inspecionando um host Linux

Consulte separadamente as configurações de enlace e IP:

```bash
$ ip -brief link
$ ip -brief address
```

Em seguida, inspecione as rotas e o estado dos vizinhos ao diagnosticar a acessibilidade. Nunca deduza a interface ou o endereço de origem correto apenas pelo nome; a seleção de rotas, as regras de política, os namespaces e os túneis podem alterar o caminho.

