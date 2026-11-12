## IPv6 (Internet Protocol version 6)

### 1. Por que o IPv6 existe

O **IPv4** usa endereços de **32 bits**, o que dá cerca de 4,3 bilhões de endereços. Com a explosão de celulares, IoT (Internet of Things, Internet das Coisas) e servidores, eles acabaram. A **IANA** (Internet Assigned Numbers Authority) esgotou seu estoque em 2011. Os **RIRs** (Regional Internet Registries, registros regionais) também esgotaram os seus, e no Brasil o RIR é o **LACNIC** (para América Latina e Caribe).

A solução paliativa foi o **NAT** (Network Address Translation, tradução de endereços de rede), que permite que várias máquinas compartilhem um único IP público. Ele funciona, mas quebra o princípio de comunicação ponta a ponta e complica VoIP, jogos e VPNs.

O IPv6 usa **128 bits**, ou seja, 2¹²⁸ ≈ 3,4 × 10³⁸ endereços. Além do espaço, ele simplifica o cabeçalho, melhora a autoconfiguração e restaura a conectividade ponta a ponta. (O IPv5 existiu só como protocolo experimental de streaming, o ST-2, e nunca foi usado.)

### 2. Notação do endereço

Um endereço IPv6 tem **8 grupos de 16 bits**, escritos em **hexadecimal** e separados por `:`.

```
2001:0db8:0000:0000:0000:ff00:0042:8329
```

Há duas regras de abreviação:

1. **Zeros à esquerda** de cada grupo podem ser omitidos: `0db8` → `db8`, `0042` → `42`.
2. **Uma sequência** de grupos consecutivos só com zeros pode ser trocada por `::`, mas **apenas uma vez** por endereço, senão ficaria ambíguo.

```
2001:db8::ff00:42:8329
```

Exemplos: `::1` é o loopback, `::` é o endereço não especificado e `fe80::1` é um link-local.

O **prefixo** usa a notação CIDR (Classless Inter-Domain Routing), como `2001:db8:abcd::/48`, em que o número é a quantidade de bits fixos da rede. Numa URL, o endereço vai entre colchetes: `http://[2001:db8::1]:8080/`.

### 3. Estrutura de um endereço unicast global

```
|   48 bits    | 16 bits  |      64 bits       |
| Prefixo      | Subnet   | Interface ID       |
| global       | ID       | (IID)              |
|<--- Rede (/64) -------->|<--- Host --------->|
```

- **Prefixo global de roteamento** (geralmente /48): entregue pelo provedor (ISP, Internet Service Provider) ao cliente.
- **Subnet ID** (16 bits): permite criar até 65.536 sub-redes por site.
- **Interface ID** (64 bits): identifica o dispositivo dentro da sub-rede.

**Regra de ouro:** a sub-rede padrão é sempre **/64**. O SLAAC (explicado adiante) depende disso. Não se “economiza” endereços em IPv6 como se fazia em IPv4 com /30 ou /29. Em links ponto a ponto, usa-se /127 (RFC 6164) ou /64.

#### Como o Interface ID é gerado

- **EUI-64** (Extended Unique Identifier, 64 bits): deriva do endereço MAC (Media Access Control). Insere-se `ff:fe` no meio do MAC e inverte-se o 7º bit. Expõe o hardware, o que é ruim para privacidade.
- **Privacy Extensions** (RFC 4941): gera IIDs aleatórios e temporários, comuns em Windows, Android e iOS.
- **Stable Privacy** (RFC 7217): IID estável por rede, mas não derivado do MAC.
- **Manual ou DHCPv6**: usado em servidores e equipamentos de rede.

### 4. Tipos de endereço

O IPv6 **não tem broadcast**. Os tipos são **unicast**, **multicast** e **anycast**.

#### Unicast (um para um)

|Tipo|Prefixo|Descrição|
|---|---|---|
|**GUA** (Global Unicast Address)|`2000::/3`|Roteável na internet, equivalente ao IP público|
|**ULA** (Unique Local Address)|`fc00::/7` (na prática `fd00::/8`)|Privado, equivalente a 10.0.0.0/8 e similares, não roteável na internet|
|**Link-Local**|`fe80::/10`|Válido só no enlace local, obrigatório em toda interface IPv6 e usado por protocolos de roteamento e descoberta|
|**Loopback**|`::1/128`|Equivalente a 127.0.0.1|
|**Não especificado**|`::/128`|“Ainda não tenho endereço”|
|**Documentação**|`2001:db8::/32`|Reservado para exemplos e manuais|
|**IPv4-mapped**|`::ffff:0:0/96`|Representa um endereço IPv4 dentro de sockets IPv6|

#### Multicast (um para um grupo): `ff00::/8`

|Endereço|Significado|
|---|---|
|`ff02::1`|Todos os nós do enlace|
|`ff02::2`|Todos os roteadores do enlace|
|`ff02::5` / `ff02::6`|Roteadores OSPF / OSPF DR (Designated Router)|
|`ff02::1:2`|Servidores e relays DHCPv6|
|`ff02::1:ffXX:XXXX`|**Solicited-Node**, usado na descoberta de vizinhos, formado pelos últimos 24 bits do endereço|

O 4º dígito hexadecimal indica o **escopo**: 1 = nó, 2 = enlace, 5 = site, e = global.

#### Anycast (um para o mais próximo)

Vários dispositivos compartilham o mesmo endereço, e o pacote vai para o mais próximo, segundo o roteamento. É usado em DNS raiz e CDNs (Content Delivery Networks). Os endereços saem do espaço unicast, e a distinção é feita na configuração.

### 5. Cabeçalho IPv6

O cabeçalho tem **tamanho fixo de 40 bytes**, contra 20 a 60 bytes variáveis no IPv4.

|Campo|Tamanho|Função|
|---|---|---|
|**Version**|4 bits|Valor 6|
|**Traffic Class**|8 bits|QoS (Quality of Service) via DSCP (Differentiated Services Code Point) e ECN (Explicit Congestion Notification)|
|**Flow Label**|20 bits|Identifica fluxos para tratamento especial|
|**Payload Length**|16 bits|Tamanho dos dados após o cabeçalho|
|**Next Header**|8 bits|Tipo do próximo cabeçalho (TCP=6, UDP=17, ICMPv6=58)|
|**Hop Limit**|8 bits|Equivale ao TTL (Time To Live) do IPv4|
|**Source Address**|128 bits|Origem|
|**Destination Address**|128 bits|Destino|

**O que foi removido em relação ao IPv4:** o checksum do cabeçalho (camadas superiores já verificam), os campos de fragmentação (movidos para extensão) e o IHL (Internet Header Length), pois o tamanho é fixo. Os roteadores processam mais rápido.

**Extension Headers** (cabeçalhos de extensão) são encadeados via Next Header: Hop-by-Hop Options, Routing, Fragment, Destination Options, AH (Authentication Header) e ESP (Encapsulating Security Payload).

**Fragmentação:** só o **host de origem** fragmenta, nunca os roteadores. O **MTU** (Maximum Transmission Unit) mínimo é **1280 bytes**, e o **PMTUD** (Path MTU Discovery) descobre o MTU do caminho. Por isso, bloquear ICMPv6 “Packet Too Big” quebra conexões.

### 6. ICMPv6 e Neighbor Discovery

O **ICMPv6** (Internet Control Message Protocol for IPv6) é **essencial** no IPv6. Ele carrega erros, diagnóstico (ping) e o **NDP** (Neighbor Discovery Protocol), que substitui o ARP (Address Resolution Protocol).

|Mensagem|Tipo|Função|
|---|---|---|
|**RS** (Router Solicitation)|133|Host pergunta “tem roteador aí?”|
|**RA** (Router Advertisement)|134|Roteador anuncia prefixo, gateway, MTU e flags|
|**NS** (Neighbor Solicitation)|135|“Qual o MAC de tal IP?” (substitui o ARP request)|
|**NA** (Neighbor Advertisement)|136|Resposta com o MAC|
|**Redirect**|137|Indica um caminho melhor|

Funções do NDP: resolução de endereço, descoberta de roteadores, detecção de endereço duplicado (**DAD**, Duplicate Address Detection), detecção de inacessibilidade de vizinho (**NUD**, Neighbor Unreachability Detection) e descoberta de prefixos. O **MLD** (Multicast Listener Discovery) substitui o IGMP (Internet Group Management Protocol).

### 7. Como um host recebe configuração

#### SLAAC (Stateless Address Autoconfiguration)

O host escuta o **RA**, pega o prefixo /64, gera o próprio IID e monta o endereço sem servidor central. Ele faz o DAD antes de usar.

#### DHCPv6 (Dynamic Host Configuration Protocol for IPv6)

- **Stateful:** o servidor atribui endereços e controla concessões (leases).
- **Stateless:** o endereço vem do SLAAC e o DHCPv6 entrega só extras, como DNS e domínio.
- Usa os portas UDP 546 (cliente) e 547 (servidor), e a identificação é por **DUID** (DHCP Unique Identifier).
- **DHCPv6-PD** (Prefix Delegation): o ISP delega um prefixo (ex.: /56 ou /60) ao roteador do cliente.

#### Flags do RA

|Flag|Significado|Efeito|
|---|---|---|
|**A** (Autonomous)|Prefixo pode ser usado para SLAAC|Ligada por padrão|
|**M** (Managed)|Use DHCPv6 para endereço|DHCPv6 stateful|
|**O** (Other)|Use DHCPv6 para outras informações|DHCPv6 stateless|

O DNS também pode vir no RA via opção **RDNSS** (Recursive DNS Server, RFC 8106).

### 8. DNS e IPv6

- Registro **AAAA** (“quad-A”): mapeia nome → IPv6, como o registro A faz para IPv4.
- Zona reversa: **`ip6.arpa`**, com os nibbles (4 bits) do endereço invertidos, separados por pontos.
- **Happy Eyeballs** (RFC 8305): o cliente tenta IPv6 e IPv4 quase em paralelo e usa o que responder mais rápido.

### 9. Roteamento

|Protocolo|Versão IPv6|
|---|---|
|RIP (Routing Information Protocol)|**RIPng** (next generation)|
|OSPF (Open Shortest Path First)|**OSPFv3**|
|EIGRP (Enhanced Interior Gateway Routing Protocol)|EIGRP for IPv6|
|IS-IS (Intermediate System to Intermediate System)|Suporta IPv6 nativamente|
|BGP (Border Gateway Protocol)|**MP-BGP** (Multiprotocol BGP)|

Os protocolos usam o endereço **link-local** como next hop. A rota padrão é `::/0`. Rotas estáticas são análogas às do IPv4.

### 9.1 Comandos práticos

**Linux:**

bash

```bash
ip -6 addr show
ip -6 route
ping -6 2001:db8::1
traceroute -6 google.com
ip -6 neigh        # tabela de vizinhos (equivale ao ARP)
```

**Cisco IOS:**

```
ipv6 unicast-routing
interface GigabitEthernet0/0
 ipv6 address 2001:db8:acad:1::1/64
 ipv6 address fe80::1 link-local
 no shutdown
ipv6 route ::/0 2001:db8:acad:ffff::2
show ipv6 interface brief
show ipv6 neighbors
```

### 10. Convivência com o IPv4: mecanismos de transição

- **Dual Stack:** o dispositivo roda IPv4 e IPv6 ao mesmo tempo. É a abordagem mais comum e recomendada.
- **Tunelamento:** encapsula IPv6 dentro de IPv4. Exemplos: 6in4, 6to4 (`2002::/16`, obsoleto), Teredo (atravessa NAT, obsoleto), ISATAP (Intra-Site Automatic Tunnel Addressing Protocol) e GRE (Generic Routing Encapsulation).
- **Tradução:** **NAT64** converte IPv6 ↔ IPv4, e o **DNS64** sintetiza registros AAAA para destinos que só têm IPv4. O **464XLAT** é muito usado em redes móveis.
- **Redes IPv6-only:** operadoras móveis usam, com NAT64 para alcançar sites legados.

### 11. Segurança

- O **IPsec** (Internet Protocol Security) era obrigatório no projeto original, mas hoje é apenas suportado.
- **NAT não é segurança.** No IPv6 todo host pode ter endereço global, então o **firewall com estado** é indispensável. A política padrão típica é bloquear entrada não solicitada.
- Ameaças específicas: RAs falsos (rogue RA), DHCPv6 falso e spoofing de NDP.
- **Mitigações:** RA Guard, DHCPv6 Guard, ND Inspection e Source Guard nos switches.
- Não bloqueie ICMPv6 cegamente, porque o protocolo depende dele para funcionar.
- Varredura de uma /64 por força bruta é inviável (2⁶⁴ endereços), mas descobrir hosts via DNS, logs e multicast continua possível.
- **Privacidade:** IIDs baseados em MAC permitem rastrear dispositivos, e as Privacy Extensions resolvem isso.

### 12. Usos e adoção

- **IoT:** bilhões de sensores e dispositivos precisam de endereço próprio.
- **Operadoras móveis:** a maioria das grandes redes 4G/5G já usa IPv6 por padrão.
- **CDNs e grandes plataformas:** Google, Meta, Netflix e Cloudflare servem tráfego em IPv6.
- **Redes corporativas e nuvem:** AWS, Azure e GCP suportam dual stack e IPv6-only.
- **Eliminação de CGNAT** (Carrier-Grade NAT) em provedores, melhorando desempenho e rastreabilidade.
- A adoção global gira na casa dos 40-50% do tráfego em muitos países. O Brasil tem adoção alta, com tráfego IPv6 perto ou acima de 40-50% segundo medições do Google e da APNIC, que variam ao longo do tempo.

### 13. Resumo das diferenças IPv4 × IPv6

|Aspecto|IPv4|IPv6|
|---|---|---|
|Tamanho|32 bits|128 bits|
|Notação|Decimal com pontos|Hexadecimal com `:`|
|Broadcast|Sim|Não (usa multicast)|
|Resolução de endereço|ARP|NDP (ICMPv6)|
|Configuração|Manual / DHCP|SLAAC / DHCPv6 / manual|
|Cabeçalho|20-60 bytes, variável|40 bytes, fixo|
|Checksum no cabeçalho|Sim|Não|
|Fragmentação|Hosts e roteadores|Só no host de origem|
|NAT|Muito usado|Desnecessário|
|DNS|A / `in-addr.arpa`|AAAA / `ip6.arpa`|

### 14. Glossário de siglas

- **AAAA:** registro DNS para IPv6
- **AH / ESP:** cabeçalhos de autenticação / criptografia do IPsec
- **BGP:** protocolo de roteamento entre sistemas autônomos
- **CDN:** rede de distribuição de conteúdo
- **CGNAT:** NAT em escala de operadora
- **CIDR:** notação de prefixo com barra (/64)
- **DAD:** detecção de endereço duplicado
- **DHCPv6:** configuração dinâmica de hosts
- **DUID:** identificador único de cliente DHCP
- **EUI-64:** identificador de 64 bits derivado do MAC
- **GUA / ULA:** endereço global / local único
- **IANA / RIR / LACNIC:** órgãos que distribuem endereços IP
- **ICMPv6:** protocolo de controle e diagnóstico
- **IID:** identificador de interface (64 bits finais)
- **IoT:** Internet das Coisas
- **MLD:** descoberta de membros de grupo multicast
- **MTU / PMTUD:** unidade máxima de transmissão / descoberta do MTU do caminho
- **NAT / NAT64:** tradução de endereços
- **NDP / NS / NA / RS / RA:** descoberta de vizinhos e suas mensagens
- **NUD:** detecção de inacessibilidade de vizinho
- **PD:** delegação de prefixo
- **QoS / DSCP / ECN:** qualidade de serviço e campos relacionados
- **RDNSS:** opção de DNS dentro do RA
- **SLAAC:** autoconfiguração sem estado
- **TTL / Hop Limit:** limite de saltos do pacote