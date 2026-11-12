# Fundamentos de rede

Aprenda como hosts, enlaces, switches, roteadores e pacotes formam redes locais e de longa distância.

Uma rede conecta interfaces para que aplicações em hosts diferentes possam trocar dados. Entender qual dispositivo, endereço e enlace cuida de cada parte do caminho facilita a interpretação posterior dos comandos Linux.

## Hosts e interfaces

Um host é um ponto de extremidade ou sistema conectado à rede, como um laptop, servidor, telefone ou máquina virtual. Um host pode ter várias interfaces: Ethernet, Wi-Fi, loopback, túneis, pontes ou adaptadores virtuais. Cada interface pode ter configurações da camada de enlace e da camada de rede adequadas à sua tecnologia.

Inspecione as interfaces e os endereços de um host Linux com:

```bash
$ ip address show
```

A presença de uma interface ou seu estado administrativamente ativo não comprova a conectividade de ponta a ponta.

## Redes locais

Uma rede local, ou LAN, abrange um ambiente limitado, como uma residência, um escritório ou um segmento de data center. Switches Ethernet encaminham quadros entre portas em um enlace local. Uma LAN sem fio, ou WLAN, usa tecnologia de enlace sem fio. Interfaces com e sem fio ainda podem pertencer à mesma sub-rede IP quando uma ponte ou um ponto de acesso as conecta.

## Roteadores e redes mais amplas

Um roteador encaminha pacotes da camada de rede entre redes IP de acordo com sua tabela de roteamento. Um equipamento residencial costuma combinar roteamento, comutação, acesso Wi-Fi, firewall, NAT e DHCP, mas essas continuam sendo funções distintas.

Uma rede de longa distância, ou WAN, abrange limites geográficos ou administrativos maiores. Um provedor de serviços de Internet pode conectar a rede de um cliente a outras redes, mas “WAN” não significa simplesmente todo dispositivo fora de uma residência.

## Pacotes, quadros e fluxos

As aplicações produzem dados que as camadas de protocolo dividem e encapsulam para transmissão. O IP transporta pacotes entre redes; um enlace local transporta cada pacote dentro de um quadro específico da tecnologia. Normalmente, os roteadores substituem o enquadramento da camada de enlace a cada salto enquanto encaminham o pacote IP.

Uma conversa pode envolver muitos pacotes nas duas direções. Perda, reordenação, fragmentação, retransmissão e mudanças de caminho significam que um único pacote capturado raramente descreve toda a transação da aplicação.

