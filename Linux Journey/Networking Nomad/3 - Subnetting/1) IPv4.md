# IPv4

Aprenda como endereços IPv4, prefixos, escopos e a saída de interfaces no Linux se relacionam.

O IPv4 fornece endereços de origem e destino de 32 bits para pacotes roteados. Um endereço possui significado junto com seu prefixo, interface, escopo, política de roteamento e duração — não como um identificador permanente de um dispositivo inteiro.

## Notação decimal pontuada

O IPv4 é exibido como quatro octetos de oito bits separados por pontos:

```text
192.0.2.165
```

Cada octeto varia de 0 a 255, portanto o endereço completo contém quatro bytes. O comprimento do prefixo identifica quantos bits iniciais pertencem ao prefixo de rede, como em `192.0.2.165/24`.

## Escopo e finalidade dos endereços

Nem todo endereço IPv4 é roteável globalmente. Alguns exemplos são loopback `127.0.0.0/8`, link-local `169.254.0.0/16`, intervalos privados como `10.0.0.0/8` e intervalos de documentação como `192.0.2.0/24`. Endereços multicast e de broadcast limitado possuem outras semânticas.

Endereços privados podem ser reutilizados em redes separadas. O NAT pode traduzi-los para comunicação externa, mas não é necessário para a comunicação dentro do domínio privado roteado.

## Inspecionando endereços IPv4 no Linux

Exiba as atribuições de IPv4 com:

```bash
$ ip -4 address show
```

Uma linha como esta informa mais do que o endereço:

```text
inet 192.0.2.165/24 brd 192.0.2.255 scope global dynamic eth0
```

Ela mostra prefixo, broadcast, escopo, marcador de origem dinâmica e interface. Linhas adicionais podem mostrar as durações válida e preferencial. Uma interface pode conter vários endereços IPv4.

## Determinando a origem selecionada

A presença de um endereço não comprova que o Linux o usará para um destino. Rotas, regras de política, métricas e a vinculação da aplicação influenciam a seleção da origem. Consulte a decisão atual de roteamento:

```bash
$ ip route get 198.51.100.20
```

Leia o próximo salto, a interface e a origem selecionados e depois teste o caminho real da aplicação. Não altere endereços em um host remoto sem acesso ao console e um plano de reversão.

