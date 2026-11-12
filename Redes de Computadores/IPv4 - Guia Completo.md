## Endereçamento IPv4

### 1. O que é

**IPv4** (_Internet Protocol version 4_) é a versão do protocolo IP que identifica e localiza dispositivos em uma rede. Todo dispositivo que se comunica por IP precisa de um endereço, assim como uma casa precisa de um endereço postal para receber correspondência.

Um endereço IPv4 tem **32 bits**, o que dá 2³² = 4.294.967.296 endereços possíveis (cerca de 4,3 bilhões).

### 2. Formato do endereço

Os 32 bits são divididos em 4 grupos de 8 bits, chamados **octetos**. Cada octeto é escrito em decimal (0 a 255) e separado por ponto. Isso se chama **notação decimal pontuada**.

```
Binário:  11000000.10101000.00001010.00000001
Decimal:  192     .168     .10      .1
```

**Conversão binária → decimal:** cada posição do octeto vale uma potência de 2:

|128|64|32|16|8|4|2|1|
|---|---|---|---|---|---|---|---|
|1|1|0|0|0|0|0|0|

11000000 = 128 + 64 = **192**. Essa tabela é a base de todo o cálculo de sub-redes.

### 3. Rede e host: a ideia central

Todo endereço IP tem duas partes:

- **Porção de rede:** identifica _em qual rede_ o dispositivo está.
- **Porção de host:** identifica _qual dispositivo_ dentro dessa rede.

O que define onde termina uma parte e começa a outra é a **máscara de sub-rede**.

#### Máscara de sub-rede

Sequência de 32 bits em que os bits `1` marcam a porção de rede e os bits `0` marcam a porção de host:

```
IP:      192.168.10.77   = 11000000.10101000.00001010.01001101
Máscara: 255.255.255.0   = 11111111.11111111.11111111.00000000
                           |---- rede (24 bits) ----|-- host --|
```

Para descobrir a rede de um IP, o dispositivo faz um **AND lógico** bit a bit entre o IP e a máscara. Ele usa isso para decidir se o destino está na mesma rede (entrega direta) ou em outra (envia ao gateway).

#### Notação CIDR (barra)

Em vez de escrever a máscara inteira, conta-se quantos bits `1` ela tem: `192.168.10.77/24`. O **CIDR** (_Classless Inter-Domain Routing_) é o padrão atual de endereçamento sem classes.

### 4. Classes de endereço (histórico)

Originalmente o IPv4 era dividido em classes de tamanho fixo (_classful_):

|Classe|Faixa do 1º octeto|Máscara padrão|Uso original|
|---|---|---|---|
|A|1 – 126|/8 (255.0.0.0)|Redes enormes (~16,7 milhões de hosts)|
|B|128 – 191|/16 (255.255.0.0)|Redes médias (~65 mil hosts)|
|C|192 – 223|/24 (255.255.255.0)|Redes pequenas (254 hosts)|
|D|224 – 239|n/a|**Multicast**|
|E|240 – 255|n/a|Reservada/experimental|

A faixa 127 é reservada para loopback, e o 0 também é reservado.

As classes eram desperdiçadoras: uma empresa com 300 máquinas precisava de uma Classe B inteira (65 mil endereços). Por isso surgiu o **CIDR** (1993), que permite máscaras de qualquer tamanho (_classless_). Hoje as classes só aparecem em provas e na terminologia, e na prática se usa sempre prefixo/máscara.

### 5. Endereços especiais e reservados

|Endereço/Faixa|Função|
|---|---|
|**Endereço de rede** (todos os bits de host = 0)|Identifica a rede em si; não pode ser atribuído a um host|
|**Endereço de broadcast** (todos os bits de host = 1)|Envia para _todos_ os hosts da rede; não pode ser atribuído|
|**127.0.0.0/8** (ex.: 127.0.0.1)|**Loopback**: o próprio dispositivo (`localhost`)|
|**0.0.0.0**|“Este host”/”qualquer endereço”; também a rota padrão (`0.0.0.0/0`)|
|**255.255.255.255**|Broadcast limitado (rede local)|
|**169.254.0.0/16**|**APIPA/Link-Local**: autoatribuição quando o DHCP falha|
|**224.0.0.0/4**|**Multicast**|
|**100.64.0.0/10**|Espaço compartilhado usado por **CGNAT** das operadoras|

#### Endereços privados (RFC 1918)

Não são roteados na Internet pública e podem ser reutilizados livremente em redes internas:

|Faixa|Prefixo|Quantidade|
|---|---|---|
|10.0.0.0 – 10.255.255.255|10.0.0.0/8|~16,7 milhões|
|172.16.0.0 – 172.31.255.255|172.16.0.0/12|~1 milhão|
|192.168.0.0 – 192.168.255.255|192.168.0.0/16|65.536|

Todo o resto é **endereço público**, único globalmente e alocado por entidades reguladoras.

### 6. Tipos de comunicação

- **Unicast:** um para um (o tráfego normal).
- **Broadcast:** um para todos da mesma rede (ex.: requisições ARP).
- **Multicast:** um para um grupo específico que “se inscreveu” (ex.: streaming, protocolos de roteamento).
- **Anycast:** um endereço compartilhado por vários servidores, e a rede entrega ao mais próximo (ex.: DNS raiz, CDNs).

### 7. Protocolos e serviços que giram em torno do IP

- **DHCP** (_Dynamic Host Configuration Protocol_): distribui automaticamente IP, máscara, gateway e DNS aos dispositivos.
- **Gateway padrão** (_default gateway_): o roteador por onde sai todo tráfego destinado a outras redes.
- **DNS** (_Domain Name System_): traduz nomes (`google.com`) em endereços IP.
- **ARP** (_Address Resolution Protocol_): descobre o **MAC** (_Media Access Control_, endereço físico da placa de rede) associado a um IP na rede local.
- **NAT** (_Network Address Translation_): traduz IPs privados em um IP público ao sair para a Internet. Sua variante mais comum, o **PAT** (_Port Address Translation_), permite que vários dispositivos compartilhem um único IP público usando portas diferentes.
- **ICMP** (_Internet Control Message Protocol_): mensagens de diagnóstico e erro (é o que o `ping` usa).
- **CGNAT** (_Carrier-Grade NAT_): NAT em escala de operadora, usado por **ISPs** (_Internet Service Providers_) para compensar a falta de IPs públicos.

### 8. Sub-redes (subnetting)

**Subnetting** é dividir uma rede grande em redes menores, “emprestando” bits da porção de host para a porção de rede. Serve para:

- **Organizar** a rede (departamentos, andares, serviços)
- **Reduzir o tamanho dos domínios de broadcast** (menos tráfego desnecessário)
- **Segurança**: aplicar regras de firewall entre sub-redes
- **Economizar endereços**

#### Fórmulas essenciais

- **Hosts por sub-rede** = 2^(bits de host) − 2 (menos o endereço de rede e o de broadcast)
- **Número de sub-redes** = 2^(bits emprestados)
- **Tamanho do bloco** = 256 − (valor da máscara no octeto “interessante”)

#### Tabela de prefixos úteis

|CIDR|Máscara|Hosts úteis|Bloco|
|---|---|---|---|
|/24|255.255.255.0|254|256|
|/25|255.255.255.128|126|128|
|/26|255.255.255.192|62|64|
|/27|255.255.255.224|30|32|
|/28|255.255.255.240|14|16|
|/29|255.255.255.248|6|8|
|/30|255.255.255.252|2|4|
|/31|255.255.255.254|2*|2|
|/32|255.255.255.255|1 (host único)|1|

*O /31 é permitido em enlaces ponto a ponto (RFC 3021) sem rede/broadcast. O /32 identifica um único host, comum em rotas e loopbacks de roteadores.

#### Exemplo 1: dividir 192.168.10.0/24 em 4 sub-redes

Para 4 sub-redes, precisamos de 2 bits emprestados (2² = 4). A máscara vai de /24 para **/26** (255.255.255.192), com blocos de 64:

|Sub-rede|Rede|Faixa de hosts|Broadcast|
|---|---|---|---|
|1|192.168.10.0/26|.1 – .62|.63|
|2|192.168.10.64/26|.65 – .126|.127|
|3|192.168.10.128/26|.129 – .190|.191|
|4|192.168.10.192/26|.193 – .254|.255|

#### Exemplo 2: descobrir a rede de um IP qualquer

**Pergunta:** a que rede pertence `172.16.45.77/20`?

1. /20 = 255.255.**240**.0, então o octeto interessante é o 3º.
2. Bloco = 256 − 240 = **16**.
3. Múltiplos de 16 no 3º octeto: 0, 16, 32, 48... O 45 está entre 32 e 48, logo a rede é **172.16.32.0/20**.
4. Broadcast: o próximo bloco menos 1 → **172.16.47.255**.
5. Faixa de hosts: **172.16.32.1 a 172.16.47.254**.
6. Hosts úteis: 2¹² − 2 = **4.094**.

#### VLSM (máscara de tamanho variável)

O **VLSM** (_Variable Length Subnet Mask_) permite usar máscaras diferentes dentro da mesma rede, ajustando cada sub-rede à sua necessidade real em vez de cortar tudo em pedaços iguais.

**Exemplo:** de `192.168.1.0/24`, atender 100, 50, 20 e 2 hosts. A regra é começar pela maior necessidade:

|Necessidade|Prefixo|Rede|Hosts úteis|
|---|---|---|---|
|100 hosts|/25|192.168.1.0/25|126|
|50 hosts|/26|192.168.1.128/26|62|
|20 hosts|/27|192.168.1.192/27|30|
|2 hosts (enlace entre roteadores)|/30|192.168.1.224/30|2|

#### Sumarização de rotas (supernetting)

É o inverso do subnetting: agrupar várias redes contíguas em uma única rota, o que reduz o tamanho das tabelas de roteamento.

`192.168.0.0/24`, `192.168.1.0/24`, `192.168.2.0/24` e `192.168.3.0/24` viram **192.168.0.0/22**.

### 9. Esgotamento e IPv6

Os ~4,3 bilhões de endereços não bastam para o mundo atual. As soluções de contorno foram NAT, CGNAT e endereços privados. A solução definitiva é o **IPv6**, com endereços de 128 bits (~3,4 × 10³⁸ endereços), em formato hexadecimal. Os dois protocolos coexistem hoje (_dual stack_).

### 10. Glossário de siglas

|Sigla|Significado|
|---|---|
|**IP**|_Internet Protocol_ (Protocolo de Internet)|
|**IPv4 / IPv6**|Versões 4 e 6 do IP|
|**bit**|_Binary digit_, a menor unidade de informação (0 ou 1)|
|**CIDR**|_Classless Inter-Domain Routing_, endereçamento sem classes, com notação /prefixo|
|**VLSM**|_Variable Length Subnet Mask_, máscaras de tamanhos diferentes na mesma rede|
|**NAT**|_Network Address Translation_, tradução de endereços de rede|
|**PAT**|_Port Address Translation_, NAT com várias máquinas compartilhando um IP via portas|
|**CGNAT**|_Carrier-Grade NAT_, NAT de operadora|
|**DHCP**|_Dynamic Host Configuration Protocol_, configuração automática de IP|
|**DNS**|_Domain Name System_, sistema de nomes de domínio|
|**ARP**|_Address Resolution Protocol_, resolve IP → MAC|
|**MAC**|_Media Access Control_, endereço físico de 48 bits da placa de rede|
|**ICMP**|_Internet Control Message Protocol_, diagnóstico e erros (ping)|
|**APIPA**|_Automatic Private IP Addressing_, IP automático 169.254.x.x|
|**RFC**|_Request for Comments_, documentos que definem padrões da Internet|
|**ISP**|_Internet Service Provider_, provedor de acesso|
|**LAN / WAN**|_Local / Wide Area Network_, rede local / rede de longa distância|
|**VLAN**|_Virtual LAN_, rede local lógica separada dentro de um mesmo equipamento físico|

### 11. Dicas para fixar

1. **Decore as potências de 2** (1, 2, 4, 8, 16, 32, 64, 128, 256). Quase todo cálculo vem delas.
2. **Descubra sempre o “octeto interessante”**: aquele em que a máscara não é 255 nem 0. É nele que está o bloco.
3. **Rede = primeiro endereço do bloco; broadcast = último; hosts = tudo no meio.**
4. Pratique com IPs aleatórios: calcule rede, broadcast, faixa e quantidade de hosts, e confira com uma calculadora ou com `ipcalc` no Linux.

Se quiser, posso preparar uma lista de exercícios de subnetting com gabarito, ou aprofundar em VLSM, sumarização ou NAT.

explique mais afundo e com exemplos a Notação CIDR

## Notação CIDR em profundidade

### 1. O problema que o CIDR resolveu

Até o início dos anos 90, o IPv4 usava **classes fixas** (A, B e C). Isso causava dois problemas graves:

1. **Desperdício de endereços.** Uma empresa com 300 máquinas não cabia numa Classe C (254 hosts) e recebia uma Classe B inteira (65.534 hosts), desperdiçando mais de 65 mil endereços.
2. **Explosão das tabelas de roteamento.** Cada rede alocada virava uma entrada separada nos roteadores da Internet. Elas cresciam tão rápido que ameaçavam o funcionamento do backbone.

Em 1993 surgiu o **CIDR** (_Classless Inter-Domain Routing_, “roteamento entre domínios sem classes”, RFC 1518/1519, hoje atualizado pela RFC 4632). Ele fez duas coisas:

- Eliminou a ligação entre o primeiro octeto e o tamanho da rede. Qualquer tamanho de prefixo passou a ser possível.
- Permitiu **agregar** (resumir) várias redes em uma única rota.

### 2. Anatomia da notação

```
192.168.10.0 / 24
|___________|   |_|
  endereço      comprimento do prefixo
```

- **Endereço:** qualquer IP dentro do bloco (normalmente o primeiro, o endereço de rede).
- **Prefixo (`/n`):** quantidade de bits, **contando da esquerda**, que pertencem à porção de rede. Os `32 − n` bits restantes são de host.

Ou seja, `/24` significa “os primeiros 24 bits identificam a rede, e os 8 restantes identificam os hosts”. É exatamente o mesmo que a máscara 255.255.255.0, só que escrito de forma compacta.

### 3. Prefixo ↔ máscara

#### Prefixo → máscara

Preencha `n` bits com `1` da esquerda para a direita, complete com `0` e converta cada octeto para decimal.

**Atalho:** cada octeto completo vale 255. O octeto parcial usa esta tabela:

|Bits `1` no octeto|Binário|Decimal|
|---|---|---|
|0|00000000|0|
|1|10000000|128|
|2|11000000|192|
|3|11100000|224|
|4|11110000|240|
|5|11111000|248|
|6|11111100|252|
|7|11111110|254|
|8|11111111|255|

**Exemplos:**

- **/20** → 20 = 8 + 8 + 4 → `255.255.240.0`
- **/13** → 13 = 8 + 5 → `255.248.0.0`
- **/27** → 27 = 8 + 8 + 8 + 3 → `255.255.255.224`
- **/22** → 22 = 8 + 8 + 6 → `255.255.252.0`

#### Máscara → prefixo

Some os bits `1`: `255.255.254.0` → 8 + 8 + 7 + 0 = **/23**.

Uma máscara válida sempre tem todos os `1` seguidos, depois todos os `0`. Algo como `255.0.255.0` é inválido.

#### Máscara curinga (_wildcard_)

É o inverso da máscara (`255.255.255.255 − máscara`). Aparece em **ACLs** (_Access Control Lists_, listas de controle de acesso) e no **OSPF** (_Open Shortest Path First_, protocolo de roteamento) em equipamentos Cisco:

|Prefixo|Máscara|Wildcard|
|---|---|---|
|/24|255.255.255.0|0.0.0.255|
|/20|255.255.240.0|0.0.15.255|
|/27|255.255.255.224|0.0.0.31|

### 4. Quantos endereços cabem em cada prefixo

Total de endereços = **2^(32 − n)**. Hosts úteis = total − 2 (rede e broadcast).

|Prefixo|Total|Hosts úteis|Equivalência|
|---|---|---|---|
|/8|16.777.216|16.777.214|Antiga Classe A|
|/12|1.048.576|1.048.574|Faixa privada 172.16/12|
|/16|65.536|65.534|Antiga Classe B|
|/20|4.096|4.094|16 × /24|
|/22|1.024|1.022|4 × /24|
|/23|512|510|2 × /24|
|/24|256|254|Antiga Classe C|
|/25|128|126|½ × /24|
|/26|64|62|¼ × /24|
|/27|32|30||
|/28|16|14||
|/29|8|6||
|/30|4|2|Enlace ponto a ponto clássico|
|/31|2|2*|Ponto a ponto (RFC 3021)|
|/32|1|1|Um único host|

**Regra de ouro:** cada bit que você **adiciona** ao prefixo **divide o bloco ao meio**; cada bit que **remove** o **dobra**. De /24 para /25 são 2 blocos de 128, e de /24 para /23 é 1 bloco de 512.

### 5. Como analisar qualquer bloco CIDR

Método em 4 passos:

1. Encontre o **octeto interessante** (onde a máscara não é 255 nem 0).
2. **Tamanho do bloco** = 256 − valor da máscara nesse octeto.
3. **Rede** = maior múltiplo do bloco que seja ≤ ao valor do IP nesse octeto (octetos seguintes zerados).
4. **Broadcast** = rede + bloco − 1 nesse octeto (octetos seguintes em 255).

#### Exemplo A: 192.168.1.100/27

- Máscara: 255.255.255.**224**, então o octeto interessante é o 4º.
- Bloco = 256 − 224 = **32**. Múltiplos: 0, 32, 64, **96**, 128...
- 100 está entre 96 e 128, então a rede é **192.168.1.96/27**.
- Broadcast: 96 + 32 − 1 = **192.168.1.127**.
- Hosts: **.97 a .126**, ou seja, 2⁵ − 2 = **30**.

#### Exemplo B: 172.16.5.200/22

- Máscara: 255.255.**252**.0, então o octeto interessante é o 3º.
- Bloco = 256 − 252 = **4**. Múltiplos: 0, 4, 8...
- 5 está entre 4 e 8, então a rede é **172.16.4.0/22**.
- Broadcast: 4 + 4 − 1 = 7, logo **172.16.7.255**.
- Hosts: **172.16.4.1 a 172.16.7.254**, ou seja, 2¹⁰ − 2 = **1.022**.

Note que o `.200` do IP é irrelevante para achar a rede. O que importa é o 3º octeto.

#### Exemplo C: 10.43.7.9/13

- Máscara: 255.**248**.0.0, então o octeto interessante é o 2º.
- Bloco = 256 − 248 = **8**. Múltiplos: ..., 32, **40**, 48.
- 43 está entre 40 e 48, então a rede é **10.40.0.0/13**.
- Broadcast: 40 + 8 − 1 = 47, logo **10.47.255.255**.
- Hosts: **10.40.0.1 a 10.47.255.254**, ou seja, 2¹⁹ − 2 = **524.286**.

Este exemplo mostra que o CIDR não precisa respeitar fronteiras de octeto.

#### Conferindo em binário (Exemplo A)

```
IP        192.168.1.100 = 11000000.10101000.00000001.01100100
Máscara   /27           = 11111111.11111111.11111111.11100000
AND (rede)              = 11000000.10101000.00000001.01100000  → 192.168.1.96
```

### 6. Alinhamento: nem todo “endereço/prefixo” é válido como rede

O endereço de rede precisa ser um **múltiplo do tamanho do bloco**. Por isso:

- `192.168.0.0/23` é válido (bloco de 2 no 3º octeto, 0 é múltiplo de 2). Cobre 192.168.0.0 a 192.168.1.255.
- `192.168.1.0/23` **não é um endereço de rede válido**, porque 1 não é múltiplo de 2. O bloco real que contém esse endereço é `192.168.0.0/23`.
- `192.168.2.0/23` é válido e cobre 192.168.2.0 a 192.168.3.255.

Escrever `192.168.1.0/23` não gera erro em muitos sistemas, mas o endereço é tratado como “um IP dentro do bloco 192.168.0.0/23”.

### 7. Alocação hierárquica (como se usa na prática)

Imagine uma empresa com o bloco `10.20.0.0/16` (65.536 endereços):

```
10.20.0.0/16                    ← bloco da empresa
├── 10.20.1.0/24   Matriz
├── 10.20.2.0/24   Filial SP
├── 10.20.3.0/24   Filial MG
└── 10.20.10.0/24  Servidores
        ├── 10.20.10.0/27     Web      (30 hosts)
        ├── 10.20.10.32/27    Banco    (30 hosts)
        ├── 10.20.10.64/27    Backup   (30 hosts)
        └── 10.20.10.96/27    Reserva
```

Cada nível usa prefixos mais longos, e isso permite resumir para cima. Os roteadores da matriz só precisam anunciar `10.20.0.0/16` ao mundo, sem listar cada sub-rede.

### 8. Agregação de rotas (supernetting)

Redes **contíguas e alinhadas** podem ser resumidas em um prefixo menor.

#### Exemplo 1: funciona

```
192.168.4.0/24   → 192.168.00000100.0
192.168.5.0/24   → 192.168.00000101.0
192.168.6.0/24   → 192.168.00000110.0
192.168.7.0/24   → 192.168.00000111.0
                           ^^^^^^ 6 bits em comum
```

16 + 6 = 22, então o resumo é **192.168.4.0/22**. São 4 redes, e o 4 é múltiplo de 4, então está alinhado.

#### Exemplo 2: outro resumo válido

`10.10.8.0/24` até `10.10.15.0/24` são 8 redes, e o 8 é múltiplo de 8. Resumo: **10.10.8.0/21**.

#### Exemplo 3: não funciona de forma exata

`10.1.1.0/24` e `10.1.2.0/24` são vizinhas, mas desalinhadas (1 = `00000001`, 2 = `00000010`). O menor prefixo que cobre as duas é `10.1.0.0/22`, que também inclui `10.1.0.0/24` e `10.1.3.0/24`. Se essas duas não forem suas, o resumo anuncia rotas que você não possui. Por isso o planejamento de endereços deve nascer já pensando em agregação.

### 9. CIDR no roteamento: _longest prefix match_

Quando uma tabela de rotas tem várias entradas que casam com o destino, o roteador escolhe a de **prefixo mais longo** (a mais específica).

```
Rota                 Próximo salto
10.0.0.0/8       →   R1
10.1.0.0/16      →   R2
10.1.2.0/24      →   R3
0.0.0.0/0        →   R4  (rota padrão, Internet)
```

|Destino|Rotas que casam|Escolhida|
|---|---|---|
|10.1.2.5|/8, /16, /24, /0|**/24 → R3**|
|10.1.9.9|/8, /16, /0|**/16 → R2**|
|10.200.1.1|/8, /0|**/8 → R1**|
|8.8.8.8|só /0|**/0 → R4**|

A **rota padrão** `0.0.0.0/0` tem prefixo zero: casa com tudo, mas só vence quando nada mais específico existe.

### 10. Onde você encontra CIDR no dia a dia

- **Configurar interface no Linux:** `ip addr add 192.168.1.10/24 dev eth0`
- **Tabela de rotas:** `ip route` mostra linhas como `192.168.1.0/24 dev eth0` e `default via 192.168.1.1`.
- **Firewalls e nuvem** (security groups da AWS, regras de firewall): `0.0.0.0/0` significa “qualquer origem” e `203.0.113.25/32` significa “somente este IP”.
- **VPCs** (_Virtual Private Clouds_, redes privadas virtuais na nuvem): você escolhe um bloco como `10.0.0.0/16` e o divide em sub-redes.
- **Docker:** a rede padrão (bridge) costuma usar `172.17.0.0/16`.
- **ISPs:** recebem blocos de entidades regionais de registro e os repartem em prefixos para os clientes.
- **Calcular rápido:** `ipcalc 172.16.5.200/22` mostra rede, broadcast, faixa de hosts e máscara.

### 11. Erros comuns

1. **Confundir o IP com o endereço de rede.** `192.168.1.100/27` é um host, e a rede é `192.168.1.96/27`.
2. **Esquecer de subtrair 2** ao contar hosts (rede e broadcast).
3. **Achar que o prefixo precisa terminar no limite de um octeto.** /13, /19 e /27 são perfeitamente normais.
4. **Ignorar o alinhamento** ao agregar ou criar sub-redes.
5. **Trocar prefixo maior por rede maior.** É o contrário: **/8 é uma rede enorme, /30 é minúscula**. Quanto maior o número depois da barra, menor o bloco.

### 12. Exercícios

Tente resolver antes de olhar as respostas:

1. Qual a rede, o broadcast e a faixa de hosts de `192.168.50.130/26`?
2. Qual a rede, o broadcast e quantos hosts úteis em `172.20.100.5/21`?
3. Qual o prefixo de `255.255.255.248` e quantos hosts úteis ele comporta?
4. Resuma `10.10.8.0/24` até `10.10.15.0/24` em uma só rota.

**Respostas:**

1. Bloco 64; 130 cai no bloco de 128. Rede **192.168.50.128/26**, broadcast **192.168.50.191**, hosts **.129 a .190** (62 hosts).
2. Máscara 255.255.248.0, bloco 8; 100 cai no bloco de 96. Rede **172.20.96.0/21**, broadcast **172.20.103.255**, **2.046** hosts.
3. 8 + 8 + 8 + 5 = **/29**, com 2³ − 2 = **6** hosts.
4. 8 redes alinhadas a partir do 8, então **10.10.8.0/21**.