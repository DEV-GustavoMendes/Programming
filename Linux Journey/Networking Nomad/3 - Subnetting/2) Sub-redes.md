# Sub-redes

Aprenda como os prefixos definem sub-redes IPv4 e influenciam a entrega no enlace, o roteamento e as políticas.

Uma sub-rede é um intervalo de endereços IP definido por um prefixo de rede. Hosts em uma sub-rede costumam estar no mesmo enlace local, mas a proximidade física não é a definição: VLANs, túneis, sobreposições e enlaces roteados podem alterar a topologia.

## Prefixos e máscaras

O IPv4 pode expressar um prefixo de 24 bits como `/24` ou como a máscara `255.255.255.0`. Em binário, uma máscara de sub-rede convencional válida possui uns contíguos seguidos por zeros:

```text
11111111.11111111.11111111.00000000
```

Para o endereço `192.168.1.8/24`, o prefixo de rede é `192.168.1.0/24`. A escrita `192.168.1.0/255.255.255.0` é entendida em alguns contextos, mas a notação de prefixo CIDR é a forma compacta padrão.

## Decidindo se um destino está no enlace

O Linux instala rotas conectadas a partir dos endereços e prefixos das interfaces. Ele compara um destino com as rotas elegíveis, em vez de apenas comparar os três primeiros octetos decimais. Em limites que não coincidem com octetos, como `/20`, a divisão ocorre dentro de um octeto.

Inspecione as rotas conectadas e a decisão para um endereço:

```bash
$ ip route show
$ ip route get 192.168.1.50
```

## Roteamento entre sub-redes

Um roteador com interfaces e rotas adequadas pode encaminhar tráfego entre sub-redes. Um gateway padrão é simplesmente um próximo salto selecionado por uma rota padrão; ele não precisa usar o primeiro endereço utilizável nem terminar em `.1`.

A separação em sub-redes cria um ponto para aplicar políticas de roteamento e filtragem, mas não constitui automaticamente um limite de segurança. Se o encaminhamento for permitido sem uma política restritiva, hosts em sub-redes diferentes ainda poderão se comunicar.

## Motivos para criar sub-redes

A divisão em sub-redes pode organizar a alocação de endereços, limitar o escopo de broadcast da camada de enlace, separar domínios de falha e fornecer limites para políticas. Ela também pode acrescentar complexidade de roteamento, firewall, DHCP, monitoramento e documentação. Projete os prefixos de acordo com requisitos reais de escala, crescimento, redundância e segurança, em vez de presumir que menor sempre significa mais rápido.

