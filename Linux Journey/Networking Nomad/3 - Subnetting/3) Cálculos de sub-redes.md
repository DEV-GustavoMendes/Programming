# Cálculos de sub-redes

Aprenda a calcular rede, broadcast, intervalo e quantidade de endereços IPv4 a partir de um prefixo.

Os cálculos de sub-redes aplicam um comprimento de prefixo aos 32 bits de um endereço IPv4. O raciocínio binário evita erros em limites de prefixo que não coincidem com octetos decimais.

## Encontrando o endereço de rede

Use o endereço `192.168.1.165/24`:

```text
address  11000000.10101000.00000001.10100101
mask     11111111.11111111.11111111.00000000
network  11000000.10101000.00000001.00000000
```

Uma operação AND bit a bit mantém os bits do endereço onde a máscara é um e zera os bits do host. O resultado é `192.168.1.0/24`.

## Contando endereços

Para o prefixo `/p`, a parte do host contém `32 - p` bits. A quantidade total de endereços é:

```text
2^(32 - p)
```

Portanto, um `/24` contém `2^8 = 256` endereços. Em uma sub-rede de broadcast tradicional, o valor de host formado apenas por zeros é o endereço de rede, e o valor formado apenas por uns é o broadcast direcionado, restando 254 endereços de host unicast comuns.

## Encontrando o limite de um bloco

Para `/26`, a máscara é `255.255.255.192`. O tamanho do bloco no último octeto é `256 - 192 = 64`, portanto os limites das sub-redes são 0, 64, 128 e 192. O endereço `192.168.1.165/26` pertence a:

```text
network:   192.168.1.128
broadcast: 192.168.1.191
range:     192.168.1.129 through 192.168.1.190
```

## Considerando exceções de prefixos

O atalho `2^host_bits - 2` não é universal. Prefixos IPv4 `/31` são definidos para enlaces ponto a ponto nos quais ambos os endereços podem ser pontos de extremidade e nenhum broadcast direcionado é necessário. Um `/32` identifica uma rota de host ou um endereço de interface. A tecnologia de rede e o uso do protocolo determinam quais endereços podem ser atribuídos.

## Verificando os cálculos

Use uma ferramenta ou biblioteca independente para conferir o trabalho manual e depois compare com a configuração real das interfaces e rotas. Um prefixo matematicamente válido ainda pode entrar em conflito com outra sub-rede ou violar um plano de alocação.

