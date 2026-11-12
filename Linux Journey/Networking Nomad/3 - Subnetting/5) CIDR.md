# CIDR

Aprenda como prefixos CIDR representam intervalos de endereços, limites de sub-redes e rotas agregadas.

O Roteamento Entre Domínios sem Classes representa um intervalo de endereços com um comprimento de prefixo, em vez de depender das classes de endereços históricas. O CIDR permite alocações de tamanho variável, divisão em sub-redes e agregação de rotas para IPv4 e IPv6.

## Lendo a notação de prefixo

Em `10.42.3.17/24`, os primeiros 24 bits são o prefixo de rede, e restam oito bits para posições dentro do intervalo. A rede canônica é `10.42.3.0/24`; o endereço do host fornecido ainda pode ser escrito com o prefixo ao configurar uma interface.

## Calculando o tamanho do intervalo

O prefixo IPv4 `/23` deixa nove bits de host e, portanto, abrange `2^9 = 512` endereços no total. O prefixo alinhado `123.12.24.0/23` abrange:

```text
first: 123.12.24.0
last:  123.12.25.255
```

No uso tradicional de broadcast, o primeiro é o endereço de rede, e o último, o broadcast direcionado. Não aplique indiscriminadamente o atalho de “menos dois” hosts utilizáveis a enlaces ponto a ponto `/31` ou rotas de host `/32`.

## Verificando o alinhamento

Um prefixo deve começar em seu limite binário. Um `/23` avança em blocos de dois no terceiro octeto quando os octetos anteriores são fixos; portanto, `123.12.24.0/23` está alinhado, mas `123.12.25.0/23` é canonizado para o mesmo intervalo `123.12.24.0/23`.

## Agregando rotas

O CIDR pode anunciar um agregado para vários prefixos contíguos, de mesmo tamanho e corretamente alinhados. Por exemplo, `192.0.2.0/25` e `192.0.2.128/25` se combinam em `192.0.2.0/24`. A agregação só é segura quando o roteador anunciante consegue alcançar corretamente todo o agregado ou possui uma política para evitar loops e buracos negros.

## Roteamento por prefixo mais longo

Quando as rotas se sobrepõem, o encaminhamento normalmente seleciona a rota elegível com o prefixo correspondente mais longo. Uma rota `/24` é mais específica do que uma `/16` que a abrange, enquanto uma rota padrão `/0` só vence quando nenhuma rota elegível mais específica é escolhida.

