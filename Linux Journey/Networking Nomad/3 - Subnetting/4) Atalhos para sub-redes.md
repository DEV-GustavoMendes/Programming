# Atalhos para sub-redes

Aprenda métodos binários compactos e de tamanho de bloco para conferir cálculos de sub-redes IPv4.

Calculadoras de sub-redes são úteis, mas um pequeno conjunto de padrões binários facilita a conferência de seus resultados. Esses métodos servem para verificação, não substituem a confirmação da alocação real e da política de roteamento.

## Valores dos bits de um octeto

Um octeto IPv4 usa estes valores posicionais:

```text
bit:    1   1   1   1   1  1  1  1
value: 128  64  32  16   8  4  2  1
```

A soma dos oito valores produz 255. O decimal 192 é `128 + 64`, portanto sua representação binária é `11000000`.

## Máscaras comuns em octetos parciais

Bits de prefixo contíguos produzem uma pequena sequência de máscaras:

```text
bits set: 0    1    2    3    4    5    6    7    8
decimal:  0  128  192  224  240  248  252  254  255
```

Por exemplo, `/19` contém 16 bits completos de prefixo e mais três bits no terceiro octeto, portanto sua máscara é `255.255.224.0`.

## Tamanhos de bloco

No primeiro octeto da máscara que não seja 255, subtraia o valor da máscara de 256 para obter o incremento da sub-rede. Uma máscara `/27` termina em 224, gerando o tamanho de bloco `256 - 224 = 32`. Assim, os limites no último octeto são 0, 32, 64, 96, 128, 160, 192 e 224.

O endereço `198.51.100.77/27` pertence ao bloco de 64 a 95.

## Convertendo um octeto arbitrário

Para converter o decimal 123, selecione os maiores valores restantes sem ultrapassá-lo:

```text
123 = 64 + 32 + 16 + 8 + 2 + 1
    = 01111011
```

Para converter de volta, some apenas os valores posicionais cujos bits são um. Sempre mantenha as oito posições ao trabalhar dentro de um octeto IPv4.

