### 1. Fundamentos de Redes

#### 1.1 Tipos de rede por escala

![[Pasted image 20261111142027.png]]

|Tipo|Alcance|Exemplo|
|---|---|---|
|**LAN**|Local (prédio, andar)|Rede do escritório|
|**WAN**|Geográfico amplo|Links entre filiais, Internet|
|**MAN**|Cidade/campus|Rede metropolitana|
|**WLAN**|LAN sem fio|Wi-Fi corporativo|
|**SAN**|Armazenamento|Storage em datacenter|

#### 1.2 Topologias

- **Física** (como os cabos estão): barramento, anel, estrela, estrela estendida, malha (mesh) completa ou parcial.
- **Lógica** (como os dados fluem): independe do cabeamento físico.

> **Por que isso importa?** A estrela dominou porque isola falhas: se um cabo cai, só aquele host perde conexão. Em barramento ou anel, um problema derrubava a rede inteira.

#### 1.3 Arquiteturas

- **Modelo hierárquico de 3 camadas:** _Acesso_ (conecta hosts) → _Distribuição_ (políticas, agregação, roteamento entre VLANs) → _Core_ (backbone rápido, sem políticas pesadas).
- **Collapsed Core (2 camadas):** core e distribuição fundidos, comum em redes menores.
- **Spine-Leaf:** usado em datacenters. Todo leaf conecta a todo spine, o que dá latência previsível e boa escalabilidade horizontal.
- **Cliente-servidor vs. peer-to-peer.**

#### 1.4 Modelos de referência

**Modelo OSI (7 camadas):**

|#|Camada|PDU|Exemplos|
|---|---|---|---|
|7|Aplicação|Dados|HTTP, DNS, SMTP|
|6|Apresentação|Dados|TLS, codificação, compressão|
|5|Sessão|Dados|Controle de sessões|
|4|Transporte|**Segmento** (TCP) / **Datagrama** (UDP)|TCP, UDP|
|3|Rede|**Pacote**|IP, ICMP, roteadores|
|2|Enlace|**Quadro (Frame)**|Ethernet, MAC, switches|
|1|Física|**Bits**|Cabos, hubs, sinais|

**Modelo TCP/IP (4 camadas):** Aplicação (OSI 5-7) · Transporte · Internet · Acesso à Rede (OSI 1-2).

> **Dica de prova:** o OSI é o modelo _conceitual_ usado para troubleshooting e comunicação (“problema na camada 2”); o TCP/IP é o que _realmente_ roda. Memorize a PDU de cada camada, a Cisco adora perguntar isso.

**Encapsulamento:** cada camada adiciona seu cabeçalho ao descer (Dados → Segmento → Pacote → Quadro → Bits). No destino ocorre o **desencapsulamento**.

#### 1.5 Meios físicos

|Meio|Características|
|---|---|
|**UTP (cobre)**|Cat5e/Cat6, até 100 m, sujeito a EMI. Conector RJ-45|
|**Fibra multimodo**|Distâncias curtas (centenas de metros), LED/laser, núcleo grosso|
|**Fibra monomodo**|Longas distâncias (km), laser, núcleo fino|
|**Wireless**|Sem cabos, sujeito a interferência|

- **Straight-through:** dispositivos _diferentes_ (PC↔switch, switch↔roteador).
- **Crossover:** dispositivos _iguais_ (switch↔switch, PC↔PC). Hoje a maioria das interfaces tem **Auto-MDIX**, que detecta isso sozinha.
- **Rollover (console):** acesso de gerenciamento ao dispositivo.

#### 1.6 Ethernet: Conceitos essenciais

- **CSMA/CD:** usado em half-duplex (hubs). Detecta colisões. Em **full-duplex** (switches modernos) não há colisões, então o CSMA/CD é desativado.
- **Domínio de colisão:** cada porta de switch é seu próprio domínio. Hub = um único domínio compartilhado.
- **Domínio de broadcast:** delimitado por **roteadores** (ou VLANs). Switches não o separam.
- **Endereço MAC:** 48 bits em hexadecimal (ex.: `AA:BB:CC:DD:EE:FF`). Os 24 primeiros bits são o **OUI** (fabricante).
- **Tipos de transmissão:** unicast, multicast, broadcast (`FF:FF:FF:FF:FF:FF`).
- **Quadro Ethernet:** Preâmbulo · MAC destino · MAC origem · Tipo · Dados (46-1500 bytes) · FCS. **MTU = 1500 bytes.**

---

### 2. Dispositivos de Rede

|Dispositivo|Camada|O que faz|Domínios|
|---|---|---|---|
|**Hub**|1|Repete bits por todas as portas|1 colisão, 1 broadcast|
|**Switch L2**|2|Encaminha por MAC, aprende endereços|Colisão por porta, 1 broadcast (por VLAN)|
|**Switch L3**|2 e 3|Switching + roteamento (SVIs)|Idem + roteia entre VLANs|
|**Roteador**|3|Encaminha por IP entre redes|Separa broadcast|
|**Firewall**|3-7|Filtra tráfego por políticas (NGFW inspeciona aplicações)|n/a|
|**Access Point**|1-2|Conecta clientes wireless à rede cabeada|n/a|
|**WLC**|n/a|Controla vários APs centralmente|n/a|
|**IPS**|3-7|Detecta e **bloqueia** ameaças em linha|n/a|

#### Como o switch aprende (essencial para a prova)

1. **Aprende** o MAC de origem de cada quadro recebido e associa à porta (tabela CAM).
2. **Decide**: se o MAC de destino está na tabela, encaminha só pela porta correta (_forward_). Se não está (_unknown unicast_) ou é broadcast, faz **flooding** por todas as portas, exceto a de entrada.
3. **Filtra**: se o destino está na mesma porta de origem, descarta.

#### Planos de operação

- **Data plane:** encaminha os pacotes (tabela de roteamento, CAM).
- **Control plane:** constrói as tabelas (protocolos de roteamento, STP).
- **Management plane:** gerência do dispositivo (SSH, SNMP).

> **Contexto moderno:** o CCNA atual cobre **SDN e redes controladas por controladora** (ex.: Cisco DNA Center). Ali o control plane é centralizado, com APIs **northbound** (aplicações → controlador) e **southbound** (controlador → dispositivos). Também aparece automação (REST, JSON, Ansible, Terraform).

---

### 3. Endereçamento IPv4

#### 3.1 Estrutura

- 32 bits, divididos em 4 octetos (ex.: `192.168.10.25`).
- Dois componentes: **parte de rede** + **parte de host**, separadas pela **máscara**.
- Máscara em decimal (`255.255.255.0`) ou **CIDR** (`/24`: 24 bits “1”).

#### 3.2 Classes (histórico, mas cai em prova)

|Classe|Faixa 1º octeto|Máscara padrão|Uso|
|---|---|---|---|
|A|1-126|/8|Redes enormes|
|B|128-191|/16|Médias|
|C|192-223|/24|Pequenas|
|D|224-239|n/a|**Multicast**|
|E|240-255|n/a|Experimental|

> `127.0.0.0/8` é **loopback** (teste local). O endereçamento _classless_ (CIDR) substituiu as classes para evitar desperdício.

#### 3.3 Endereços privados (RFC 1918) e especiais

- **Privados:** `10.0.0.0/8` · `172.16.0.0/12` (172.16 a 172.31) · `192.168.0.0/16`. Não são roteados na Internet; precisam de **NAT**.
- **APIPA/link-local:** `169.254.0.0/16` (o host não achou DHCP).
- **Loopback:** `127.0.0.0/8`.
- **Broadcast:** `255.255.255.255` (limitado) ou último endereço da sub-rede (dirigido).
- **Default route:** `0.0.0.0/0`.

#### 3.4 Endereços especiais dentro de cada rede

- **Endereço de rede:** todos os bits de host = 0 (não atribuível).
- **Endereço de broadcast:** todos os bits de host = 1 (não atribuível).
- **Hosts utilizáveis = 2ⁿ − 2** (n = bits de host).

---

### 4. Sub-redes (Subnetting)

#### 4.1 Por que fazer subnetting?

Dividir uma rede grande em partes menores **reduz o tamanho dos domínios de broadcast**, melhora a segurança, organiza o endereçamento e evita desperdício de IPs.

#### 4.2 Método prático (o “salto” / magic number)

1. Identifique o octeto “interessante” (onde a máscara é parcial).
2. **Salto = 256 − valor da máscara nesse octeto.**
3. As sub-redes começam em múltiplos do salto.
4. **Broadcast** = próxima sub-rede − 1. Hosts = entre rede e broadcast.

**Tabela de máscaras (decore!):**

|CIDR|Máscara|Salto|Hosts/sub-rede|
|---|---|---|---|
|/24|255.255.255.0|n/a|254|
|/25|255.255.255.128|128|126|
|/26|255.255.255.192|64|62|
|/27|255.255.255.224|32|30|
|/28|255.255.255.240|16|14|
|/29|255.255.255.248|8|6|
|/30|255.255.255.252|4|2|
|/31|255.255.255.254|n/a|2 (links ponto a ponto, RFC 3021)|
|/32|255.255.255.255|n/a|1 (host único)|

#### 4.3 Exemplo resolvido

**Pergunta:** qual a rede, o broadcast e a faixa de hosts de `192.168.10.77/26`?

- /26 → máscara `255.255.255.192` → salto = 256 − 192 = **64**.
- Sub-redes: `.0`, `.64`, `.128`, `.192`.
- O `.77` está entre `.64` e `.128`, então:
    - **Rede:** `192.168.10.64`
    - **Broadcast:** `192.168.10.127`
    - **Hosts:** `192.168.10.65` a `192.168.10.126` (62 hosts)

#### 4.4 Quantas sub-redes eu crio?

- **Sub-redes = 2ⁿ**, onde _n_ = bits emprestados da parte de host.
- Ex.: `192.168.1.0/24` → `/26` empresta 2 bits → 2² = **4 sub-redes** de 62 hosts.

#### 4.5 VLSM (Variable Length Subnet Mask)

Usar máscaras **diferentes** na mesma rede, ajustadas à necessidade de cada segmento.

**Procedimento:**

1. Ordene as sub-redes da **maior para a menor** demanda de hosts.
2. Atribua a menor máscara que comporte cada uma, em sequência, sem sobreposição.
3. Links ponto a ponto entre roteadores usam `/30` (ou `/31`).

> **Por que maior primeiro?** Isso mantém os blocos alinhados nas fronteiras corretas e evita fragmentar o espaço. Se começar pelas pequenas, você bloqueia o alinhamento das grandes.

#### 4.6 Sumarização de rotas

Agrupar várias redes contíguas em **uma rota só**. Ex.: `192.168.0.0/24` a `192.168.3.0/24` → `192.168.0.0/22`. Isso reduz o tamanho das tabelas de roteamento.

---

### 5. Protocolos Essenciais

#### 5.1 Camada de Transporte: TCP vs. UDP

||**TCP**|**UDP**|
|---|---|---|
|Conexão|Orientado a conexão|Sem conexão|
|Confiabilidade|Sim (ACK, retransmissão)|Não|
|Controle de fluxo|Sim (janela deslizante)|Não|
|Overhead|Maior|Menor|
|Usos|HTTP/S, SSH, FTP, SMTP|DNS (consultas), DHCP, TFTP, VoIP, SNMP|

**Three-way handshake:** `SYN` → `SYN-ACK` → `ACK`. Encerramento com `FIN`.

**Portas importantes (decore):**

|Porta|Protocolo|Transporte|
|---|---|---|
|20/21|FTP (dados/controle)|TCP|
|22|SSH|TCP|
|23|Telnet (inseguro)|TCP|
|25|SMTP|TCP|
|53|DNS|UDP/TCP|
|67/68|DHCP (servidor/cliente)|UDP|
|69|TFTP|UDP|
|80|HTTP|TCP|
|110|POP3|TCP|
|123|NTP|UDP|
|143|IMAP|TCP|
|161/162|SNMP (consulta/trap)|UDP|
|443|HTTPS|TCP|
|514|Syslog|UDP|

#### 5.2 Camada de Rede

- **IP:** endereçamento e roteamento _best-effort_, sem garantias.
- **ICMP:** mensagens de controle e erro (`ping` = echo request/reply; `traceroute` usa TTL expirado). O **TTL** evita loops infinitos.
- **ARP:** descobre o **MAC a partir do IP** na rede local. Funciona assim: _ARP request_ (broadcast: “quem tem o IP X?”) → _ARP reply_ (unicast com o MAC). Para destinos em outra rede, o host faz ARP do **default gateway**.

#### 5.3 Serviços de infraestrutura

- **DHCP (processo DORA):** **D**iscover (broadcast) → **O**ffer → **R**equest → **A**cknowledge. Entrega IP, máscara, gateway e DNS. Em redes com **DHCP Relay** (`ip helper-address`), o roteador repassa os broadcasts ao servidor em outra sub-rede.
- **DNS:** traduz nomes em IPs. Registros: A (IPv4), AAAA (IPv6), CNAME (alias), MX (e-mail), PTR (reverso).
- **NAT/PAT:**
    - _NAT estático_ (1:1), _NAT dinâmico_ (pool) e **PAT/overload** (muitos hosts → 1 IP público, diferenciados por porta).
    - Terminologia Cisco: **inside local**, **inside global**, **outside local**, **outside global**.
- **NTP:** sincroniza relógios (essencial para logs coerentes).
- **Syslog:** envia logs a um servidor. Níveis 0 (Emergency) a 7 (Debug).
- **SNMP:** monitoramento e gerência (v1, v2c, **v3** com autenticação e criptografia).
- **TFTP/FTP:** transferência de arquivos (backup de configs, imagens IOS).
- **SSH vs. Telnet:** SSH é criptografado; Telnet trafega em texto puro.
- **QoS:** prioriza tráfego sensível (voz/vídeo). Conceitos: classificação, marcação (DSCP/CoS), filas, policing/shaping.

#### 5.4 Protocolos de Camada 2

- **STP (802.1D):** evita loops em topologias redundantes. Elege uma **root bridge** (menor Bridge ID = prioridade + MAC). Papéis de porta: root, designated, blocked/alternate. **RSTP (802.1w)** converge muito mais rápido.
- **VLANs (802.1Q):** segmentam a rede logicamente. Portas **access** (uma VLAN) vs. **trunk** (várias VLANs, com tag 802.1Q). A **VLAN nativa** trafega sem tag.
- **Inter-VLAN routing:** _router-on-a-stick_ (subinterfaces) ou switch L3 (SVI).
- **EtherChannel (LACP/PAgP):** agrupa links físicos em um lógico, aumentando banda e redundância sem que o STP bloqueie.
- **CDP/LLDP:** descoberta de vizinhos (CDP é proprietário Cisco; LLDP é padrão aberto).

#### 5.5 Roteamento

- **Estático:** manual, simples, sem overhead. Inclui a **rota default** e a **rota flutuante** (distância administrativa maior, funciona como backup).
- **Dinâmico:** protocolos trocam rotas automaticamente.

**Distância Administrativa (AD)**: confiabilidade da _fonte_ da rota (menor = preferida):

|Fonte|AD|
|---|---|
|Conectada|0|
|Estática|1|
|EIGRP|90|
|OSPF|110|
|RIP|120|

**Seleção de rota:** 1º **prefixo mais longo** (longest match), 2º menor AD, 3º menor métrica.

**OSPF (foco do CCNA):** protocolo link-state, usa **custo** baseado em largura de banda e algoritmo SPF (Dijkstra). Conceitos: áreas (Área 0 = backbone), vizinhança (Hello/Dead timers), eleição de **DR/BDR** em redes multiacesso, router-ID.

#### 5.6 Wireless

- **Padrões:** 802.11a/b/g/n/ac/ax (Wi-Fi 6). Bandas de **2,4 GHz** (mais alcance, mais interferência, canais não sobrepostos 1/6/11) e **5 GHz** (mais banda, menos alcance).
- **Segurança:** WEP (obsoleto) → WPA → **WPA2** (AES) → **WPA3**. Modos Personal (PSK) e Enterprise (802.1X/RADIUS).
- **Arquiteturas:** autônoma, baseada em nuvem, **controladora (WLC)** com APs em modo lightweight (CAPWAP).

#### 5.7 Segurança básica

- **ACLs:** _standard_ (filtra só por origem) e _extended_ (origem, destino, protocolo, porta). Há **deny implícito** no final. Processadas **de cima para baixo**.
- **Port Security, DHCP Snooping, DAI (Dynamic ARP Inspection)** protegem a camada 2 contra spoofing e ataques de MAC/DHCP.
- **AAA:** Authentication, Authorization, Accounting (RADIUS/TACACS+).
- **VPNs:** site-to-site e acesso remoto (IPsec).
- Pilares: **CIA** (Confidencialidade, Integridade, Disponibilidade).

---

### 6. IPv6 (cai bastante no CCNA)

- 128 bits, em hexadecimal, 8 grupos de 16 bits. Compressão: `::` (uma vez só) e omissão de zeros à esquerda.
- **Sem broadcast:** usa unicast, multicast e **anycast**.
- Tipos: **Global Unicast** (`2000::/3`), **Link-local** (`FE80::/10`), **Unique Local** (`FC00::/7`), **Multicast** (`FF00::/8`).
- **ND (Neighbor Discovery)** substitui o ARP (via ICMPv6). **SLAAC** permite autoconfiguração; DHCPv6 também existe.
- Endereço de rede típico: prefixo `/64` para LANs (os últimos 64 bits são o _interface ID_, podendo usar EUI-64).

---

### 7. Troubleshooting: Roteiro Rápido

Abordagem **bottom-up** (camada 1 → 7):

1. **Física:** cabo, link, LEDs, `show interfaces` (up/up?).
2. **Enlace:** VLAN, duplex mismatch, MAC (`show mac address-table`).
3. **Rede:** IP/máscara/gateway corretos? `ping`, `traceroute`, `show ip route`.
4. **Transporte/Aplicação:** portas, ACLs, DNS.

**Comandos de ouro:** `show ip interface brief` · `show running-config` · `show vlan brief` · `show ip route` · `show interfaces trunk` · `show spanning-tree` · `show ip ospf neighbor` · `ping` · `traceroute`.