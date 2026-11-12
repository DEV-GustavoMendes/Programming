## Conversão decimal ↔ binário (para IPv4)

Um endereço IPv4 tem **32 bits**, divididos em **4 octetos de 8 bits**. Por isso cada número do IP vai de 0 a 255 (`00000000` a `11111111`).

### Tabela de referência

Cada posição do octeto vale uma potência de 2. Memorize esta linha:

|128|64|32|16|8|4|2|1|
|---|---|---|---|---|---|---|---|

### Decimal → Binário

**Método da tabela (o mais usado em redes):** da esquerda para a direita, pergunte se o valor da posição “cabe” no número que sobrou. Se couber, marque **1** e subtraia. Se não, marque **0**.

**Exemplo: 168**

- 128 cabe? Sim → 1, sobra 40
- 64 cabe em 40? Não → 0
- 32 cabe? Sim → 1, sobra 8
- 16 cabe em 8? Não → 0
- 8 cabe? Sim → 1, sobra 0
- 4, 2, 1 → 0, 0, 0

Resultado: **10101000**

**Método das divisões (alternativo):** divida por 2 sucessivamente e anote os restos, lendo de baixo para cima. Para 10: 10÷2=5 (resto 0), 5÷2=2 (resto 1), 2÷2=1 (resto 0), 1÷2=0 (resto 1). Lendo de baixo para cima: `1010`. Complete com zeros à esquerda até ter 8 bits: **00001010**.

### Binário → Decimal

Some os valores das posições onde há **1**.

**Exemplo: 11001010**

```
128 + 64 + 0 + 0 + 8 + 0 + 2 + 0 = 202
```

### Aplicando em um IP completo

`192.168.1.10`

```
192 → 11000000
168 → 10101000
  1 → 00000001
 10 → 00001010
```

Em binário: `11000000.10101000.00000001.00001010`

### Por que isso importa para endereçamento

A **máscara de sub-rede** é uma sequência de 1s seguida de 0s. O `/26` significa 26 bits em 1:

```
11111111.11111111.11111111.11000000
   255  .   255   .   255  .  192
```

- Os bits **1** identificam a **rede**.
- Os bits **0** identificam os **hosts**.
- Aqui sobram 6 bits de host, então são 2⁶ = 64 endereços, dos quais 62 são utilizáveis (2⁶ − 2, descontando o endereço de rede e o de broadcast).

### Atalhos úteis

Os únicos valores possíveis em um octeto de máscara são:

|Binário|Decimal|
|---|---|
|10000000|128|
|11000000|192|
|11100000|224|
|11110000|240|
|11111000|248|
|11111100|252|
|11111110|254|
|11111111|255|
