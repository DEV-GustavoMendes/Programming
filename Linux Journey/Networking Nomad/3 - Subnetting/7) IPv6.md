# IPv6

Aprenda a ler endereços IPv6, prefixos, escopos, configuração automática e estado de roteamento no Linux.

O IPv6 usa endereços de 128 bits e foi projetado para oferecer um espaço de endereços muito maior, junto com comportamentos atualizados de pacotes e descoberta de vizinhos. IPv4 e IPv6 são protocolos separados; hosts de pilha dupla podem executar ambos durante a transição das redes.

## Lendo a notação IPv6

Um endereço IPv6 é escrito como oito grupos hexadecimais de 16 bits:

```text
2001:0db8:0000:0000:0000:0000:0000:0025
```

Os zeros iniciais de cada grupo podem ser omitidos, e uma sequência consecutiva de grupos formados por zeros pode ser comprimida com `::`:

```text
2001:db8::25
```

Apenas um `::` pode aparecer, pois, caso contrário, a quantidade de grupos omitidos seria ambígua. `2001:db8::/32` é reservado para exemplos de documentação.

## Tipos de endereço e escopo

Entre os endereços e intervalos importantes estão:

- `::1/128`: loopback no host local.
- `fe80::/10`: unicast link-local; normalmente presente nas interfaces IPv6.
- `2000::/3`: espaço unicast global atualmente alocado.
- `ff00::/8`: multicast.

O IPv6 não possui endereço de broadcast; multicast e Descoberta de Vizinhos atendem a casos de uso que o IPv4 costuma tratar com broadcast. Um destino link-local pode exigir uma zona de interface, como `fe80::1%eth0`, pois o mesmo prefixo existe em todos os enlaces.

## Prefixos e endereços de interface

A notação CIDR do IPv6 usa um comprimento de prefixo de `/0` a `/128`. Um `/64` é o tamanho padrão para a maioria das sub-redes LAN e permite a Configuração Automática de Endereço sem Estado. Uma interface pode conter simultaneamente endereços link-local, globais estáveis, temporários de privacidade e outros, cada um com durações preferencial e válida.

## Descoberta de vizinhos e roteadores

A Descoberta de Vizinhos do IPv6 usa ICMPv6 para resolução de endereços, detecção de endereços duplicados, descoberta de roteadores e informações de acessibilidade. Anúncios de Roteador podem fornecer prefixos e informações do roteador padrão. Os hosts podem combinar SLAAC com DHCPv6 para outras configurações; normalmente, o DHCPv6 não fornece o roteador padrão.

Bloquear todo o ICMPv6 interrompe comportamentos essenciais do protocolo. A política de firewall deve permitir os tipos de mensagem necessários com o escopo apropriado, em vez de tratar o ICMPv6 como opcional.

## Inspecionando e testando o IPv6

Inspecione endereços, rotas e vizinhos de forma independente:

```bash
$ ip -6 address show
$ ip -6 route show
$ ip -6 neighbor show
$ ping -6 -c 3 2001:db8::25
```

Use um endereço de teste realmente atribuído, não o endereço de documentação mostrado. Uma aplicação de pilha dupla pode funcionar por IPv4 enquanto o IPv6 está com falha, ou vice-versa; portanto, teste explicitamente cada família e seus registros DNS `A` ou `AAAA`.

