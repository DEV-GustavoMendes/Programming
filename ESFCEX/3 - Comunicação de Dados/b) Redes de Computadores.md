## PARTE B – REDES DE COMPUTADORES

### B.1 Conceitos e classificações

Uma rede de computadores é um conjunto de dispositivos autônomos interligados que trocam dados segundo protocolos. Pela abrangência, as classificações são **PAN** (alguns metros: Bluetooth, USB), **LAN** (prédio ou campus), **MAN** (cidade) e **WAN** (países, continentes). Pelo modelo de interação, existe o **cliente-servidor**, com papéis assimétricos, e o **peer-to-peer (P2P)**, em que cada nó atua como cliente e servidor. A **Internet** é a rede mundial de redes que usa TCP/IP, a **Intranet** é uma rede privada que usa as mesmas tecnologias, e a **Extranet** é a extensão controlada de uma intranet a parceiros.

### B.2 Topologias

A topologia pode ser **física** (como os cabos e dispositivos estão dispostos) ou **lógica** (como os dados fluem). Um Ethernet 10BASE-T com hub é fisicamente em estrela e logicamente em barramento.

Na topologia em **barramento**, todos compartilham um único meio. O custo é baixo, mas uma falha no cabo derruba toda a rede, a colisão é frequente e o desempenho degrada com a carga (coaxial, Ethernet antigo). Na topologia em **anel**, cada nó se liga a dois vizinhos e o dado circula em um sentido (Token Ring 802.5, FDDI com anel duplo contrarrotativo, SDH). Em **estrela**, todos se ligam a um ponto central (hub ou switch): é a mais usada hoje, de fácil gerência e isolamento de falhas, mas o nó central é ponto único de falha. A **estrela estendida** (ou árvore) é uma hierarquia de estrelas. Na **malha (mesh)**, cada nó se liga a vários outros: na malha completa, são n(n−1)/2 enlaces para n nós. Oferece alta redundância e alto custo, e a malha parcial é comum em backbones e na Internet. A **híbrida** combina as anteriores. A topologia **barramento** é a que apresenta maior vulnerabilidade a falha de cabo, e a **estrela** é a de melhor equilíbrio custo-benefício.

### B.3 Componentes de rede

A **placa de rede (NIC)** tem endereço MAC de 48 bits, com os primeiros 24 bits identificando o fabricante (OUI). O **repetidor** regenera o sinal na camada 1. O **hub** é um repetidor multiporta: um único domínio de colisão e um único de broadcast. A **bridge** e o **switch** operam na camada 2 e encaminham por MAC. O switch constrói uma tabela MAC (CAM table) aprendendo pelo endereço de **origem** e encaminha pelo de **destino**. Se o destino é desconhecido (unknown unicast) ou broadcast, ele inunda todas as portas, exceto a de entrada. Cada porta de switch é um domínio de colisão, e todo o switch (sem VLANs) é um único domínio de broadcast. Os métodos de encaminhamento são **store-and-forward** (recebe o quadro inteiro, verifica o FCS, é o mais confiável), **cut-through** (encaminha assim que lê o MAC de destino, com menor latência) e **fragment-free**.

O **roteador** opera na camada 3, interliga redes distintas, decide por endereço IP e **separa domínios de broadcast**. O **switch camada 3** combina comutação em hardware com roteamento entre VLANs. O **gateway** é um termo genérico para dispositivo que interliga redes com protocolos diferentes (inclusive, no jargão, o roteador padrão). O **access point** liga clientes sem fio à rede cabeada. O **firewall**, o **proxy**, o **balanceador de carga** e o **modem** completam o conjunto. No **cabeamento estruturado** (ANSI/TIA-568, ABNT NBR 14565), os subsistemas são: área de trabalho, cabeamento horizontal (até 90 m), cabeamento vertical (backbone), sala de telecomunicações, sala de equipamentos e entrada de facilidades.

### B.4 Ethernet, VLANs e STP

O **IEEE 802.3** define o Ethernet e o **CSMA/CD** (Carrier Sense Multiple Access with Collision Detection): a estação escuta o meio, transmite se estiver livre, detecta colisões durante a transmissão, emite um sinal de jam, aguarda um tempo aleatório (backoff exponencial binário) e retransmite. Só se aplica a **half-duplex**. Em full-duplex em switch não há colisão e o CSMA/CD é desativado.

O **quadro Ethernet II** tem: preâmbulo (7 bytes) + SFD (1 byte), MAC de destino (6), MAC de origem (6), EtherType (2) (ou comprimento no 802.3), dados (46 a 1500 bytes) e FCS (4 bytes, CRC-32). O tamanho mínimo é **64 bytes** e o máximo é **1518 bytes** (1522 com tag 802.1Q). O MTU de 1500 bytes é o limite do campo de dados. Quadros “jumbo” chegam a cerca de 9000 bytes.

Os padrões mais cobrados são: 10BASE-T (par trançado Cat3+, 10 Mbps, 100 m), 100BASE-TX (Cat5, 100 Mbps), 100BASE-FX (fibra multimodo), 1000BASE-T (Cat5e, 4 pares), 1000BASE-SX (multimodo, 850 nm), 1000BASE-LX (monomodo ou multimodo, 1310 nm), 10GBASE-T (Cat6a), 10GBASE-SR/LR/ER (fibra) e 802.3ae (10 Gbps em fibra). O **PoE** (802.3af = 15,4 W, 802.3at = 30 W, 802.3bt = até 60 ou 90 W) alimenta dispositivos pelo cabo de rede. A agregação de links é o **802.3ad/802.1AX (LACP)**.

As **VLANs** segmentam logicamente um switch em vários domínios de broadcast. O **IEEE 802.1Q** acrescenta uma tag de 4 bytes ao quadro: TPID 0x8100 (16 bits), PCP (3 bits, prioridade 802.1p), DEI (1 bit) e VID (12 bits, de 1 a 4094 utilizáveis). Portas de acesso pertencem a uma VLAN, e portas **trunk** transportam várias VLANs; a VLAN nativa trafega sem tag. A comunicação entre VLANs exige roteamento (roteador com subinterfaces, o “router on a stick”, ou switch L3).

O **STP (802.1D)** evita loops de camada 2 em topologias redundantes: elege uma **root bridge** (menor Bridge ID = prioridade + MAC; prioridade padrão 32768), calcula o menor caminho até ela e bloqueia as portas redundantes. Os estados clássicos são blocking, listening, learning e forwarding (mais disabled). O **RSTP (802.1w)** converge em segundos, com os estados discarding, learning e forwarding. O **MSTP (802.1s)** mapeia várias VLANs em instâncias. Outras proteções são PortFast, BPDU Guard e Root Guard.

### B.5 Modelos de referência: OSI/ISO, TCP/IP e ITU-T

#### Modelo OSI (ISO 7498)

|Camada|Função|PDU|Exemplos|
|---|---|---|---|
|7 Aplicação|interface com o usuário/aplicativo|dados|HTTP, SMTP, DNS, FTP|
|6 Apresentação|formato, criptografia, compressão|dados|TLS (em parte), ASCII, JPEG, MPEG|
|5 Sessão|controle de diálogo, sincronização|dados|RPC, NetBIOS, SIP (em parte)|
|4 Transporte|fim a fim, confiabilidade, portas|segmento (TCP) / datagrama (UDP)|TCP, UDP|
|3 Rede|endereçamento lógico e roteamento|pacote|IP, ICMP, OSPF|
|2 Enlace|acesso ao meio, MAC, quadros|quadro|Ethernet, PPP, 802.11, HDLC|
|1 Física|bits, sinais, conectores|bit|cabos, hubs, repetidores|

**Macete (de baixo para cima):** _Fábio Entregou Rosas Todo Santo Ano Passado_ (Física, Enlace, Rede, Transporte, Sessão, Apresentação, Aplicação). O processo de **encapsulamento** vai de cima para baixo, com cada camada adicionando cabeçalho (e o enlace também um trailer). A **desencapsulação** ocorre no sentido inverso. Cada camada conversa logicamente com a camada de mesmo nível (comunicação par a par) e fisicamente com a camada adjacente.

#### Modelo TCP/IP (RFC 1122)

Tem quatro camadas: **Acesso à rede (ou interface de rede)** (equivale a OSI 1 e 2), **Internet** (OSI 3: IP, ICMP, ARP, IGMP), **Transporte** (OSI 4: TCP, UDP) e **Aplicação** (OSI 5, 6 e 7). Alguns autores (Kurose, Tanenbaum) adotam 5 camadas, separando Física e Enlace. Na prova, olhe o enunciado para saber qual versão é usada. Diferenças essenciais em relação ao OSI: o TCP/IP surgiu da prática (ARPANET) e depois foi formalizado, enquanto o OSI foi um modelo teórico anterior aos protocolos; o OSI distingue claramente serviço, interface e protocolo; o TCP/IP é o que domina na prática.

#### ITU-T

O ITU-T (setor de padronização de telecomunicações da União Internacional de Telecomunicações) emite recomendações organizadas em séries por letra. As cobradas com mais frequência: **série V** (modems e comunicação de dados sobre rede telefônica: V.24/V.28 interfaces RS-232, V.90/V.92 modems de 56 kbps), **série X** (redes de dados: **X.25** comutação de pacotes, **X.500** serviços de diretório, base do LDAP, **X.509** certificados digitais, X.21 interface), **série G** (transmissão: **G.711**, **G.729**, **G.723.1**, G.722 de codecs, G.707 SDH, **G.992** ADSL, G.114 atraso em voz), **série H** (sistemas audiovisuais: **H.323** sinalização multimídia, **H.264/AVC** e **H.265/HEVC** codecs de vídeo, H.248), **série I** (RDSI/ISDN) e **série Q** (sinalização, como a Q.931 e o SS7).

Tecnologias WAN do ITU-T: **X.25** opera nas camadas 1 a 3, usa circuitos virtuais comutados (SVC) e permanentes (PVC), com controle de erro em cada salto, o que o torna lento em relação aos sucessores. O **Frame Relay** opera nas camadas 1 e 2, com PVCs identificados por **DLCI**, sem controle de erro por salto (apenas detecção e descarte), com CIR (Committed Information Rate), bit DE (Discard Eligibility) e notificação de congestionamento FECN/BECN. A **RDSI (ISDN)** oferece o acesso básico BRI (2B + D, com B = 64 kbps e D = 16 kbps, total 144 kbps) e o primário PRI (30B + D no padrão europeu e brasileiro, 23B + D nos EUA). O **ATM** usa **células de 53 bytes** (5 de cabeçalho e 48 de payload), identifica conexões por VPI/VCI e oferece classes de serviço CBR, VBR, ABR e UBR, o que o tornava adequado a voz e vídeo.

### B.6 Protocolos da camada de Rede

#### IPv4

O endereço tem 32 bits, escritos em quatro octetos decimais. As **classes** (históricas) são: A (1–126, máscara /8), B (128–191, /16), C (192–223, /24), D (224–239, multicast) e E (240–255, experimental). O bloco 127.0.0.0/8 é loopback. Os **endereços privados** (RFC 1918) são **10.0.0.0/8**, **172.16.0.0/12** (172.16 a 172.31) e **192.168.0.0/16**. Outros especiais: 169.254.0.0/16 (APIPA/link-local), 100.64.0.0/10 (CGNAT), 0.0.0.0 (rota padrão ou “este host”), 255.255.255.255 (broadcast limitado).

O **CIDR** (classless) descarta as classes e usa prefixo variável (/n). O **VLSM** permite máscaras diferentes dentro de uma mesma rede. O cálculo básico: com prefixo /n, o número de hosts utilizáveis é 2^(32−n) − 2. Exemplos: /24 = 254 hosts, /26 = 62, /27 = 30, /28 = 14, /30 = 2 (enlaces ponto a ponto), /31 permite 2 hosts sem broadcast (RFC 3021). A máscara /26 é 255.255.255.192, e o salto (bloco) é 64. **Macete:** o “salto” = 256 − valor do octeto da máscara. Para saber a rede de um IP, faça AND bit a bit com a máscara.

O **cabeçalho IPv4** tem 20 bytes (mínimo) a 60 bytes e inclui versão, IHL, DSCP/ECN, comprimento total, identificação, flags (DF, MF), deslocamento de fragmento, **TTL**, protocolo (1 = ICMP, 6 = TCP, 17 = UDP, 50 = ESP, 51 = AH, 89 = OSPF), checksum do cabeçalho e endereços. O TTL é decrementado a cada roteador e, ao chegar a zero, o pacote é descartado e um ICMP Time Exceeded é enviado (base do traceroute). A **fragmentação** ocorre quando o pacote excede o MTU do enlace, e a remontagem é feita **só no destino**. O **Path MTU Discovery** usa o bit DF.

O **NAT** traduz endereços privados em públicos. Suas variações são NAT estático (1 para 1), dinâmico (pool) e **PAT/NAPT/overload** (muitos para um, usando portas). O NAT quebra a transparência fim a fim e dificulta protocolos como IPsec AH, SIP e FTP ativo.

O **ARP** resolve IP em MAC por broadcast e resposta unicast. O ARP gratuito anuncia a própria associação e é a base do ARP spoofing. O **ICMP** carrega mensagens de controle e erro: echo request (tipo 8) e reply (tipo 0), destination unreachable (tipo 3), time exceeded (tipo 11) e redirect. O **IGMP** gerencia a participação em grupos multicast.

#### IPv6

Tem 128 bits em notação hexadecimal com dois-pontos, com a abreviação de zeros consecutivos por “::” (uma única vez no endereço). O cabeçalho tem **40 bytes fixos**, sem checksum e sem fragmentação nos roteadores intermediários (só a origem fragmenta), com **cabeçalhos de extensão** opcionais. Os tipos de endereço são **unicast**, **multicast** e **anycast**, e **não existe broadcast**. Os prefixos mais cobrados: **2000::/3** (unicast global), **FE80::/10** (link-local), **FC00::/7** (ULA, equivalente aos privados), **FF00::/8** (multicast), **::1** (loopback) e **::/128** (não especificado). O **NDP** (Neighbor Discovery Protocol, sobre ICMPv6) substitui o ARP e também faz descoberta de roteadores. A configuração automática pode ser por **SLAAC** (stateless) ou **DHCPv6** (stateful). A transição IPv4→IPv6 usa **dual stack**, **tunelamento** (6to4, Teredo, 6in4) e **tradução** (NAT64/DNS64). O IPsec é parte nativa do projeto do IPv6.

### B.7 Protocolos da camada de Transporte

O **UDP** é não orientado a conexão, sem garantia de entrega ou de ordem, com cabeçalho de 8 bytes (portas de origem e de destino, comprimento e checksum), de baixa latência e usado por DNS, DHCP, SNMP, TFTP, VoIP e streaming em tempo real. O **TCP** é orientado a conexão, confiável, ordenado, com controle de fluxo e de congestionamento, cabeçalho de 20 a 60 bytes, e usa números de sequência e confirmação.

A **conexão TCP** é aberta com o **three-way handshake**: SYN → SYN/ACK → ACK. Seu encerramento é em quatro passos (FIN/ACK de cada lado), e o estado TIME_WAIT protege contra segmentos atrasados. As flags são SYN, ACK, FIN, RST, PSH e URG. O **controle de fluxo** usa a janela de recepção (rwnd), anunciada pelo receptor. O **controle de congestionamento** usa a janela de congestionamento (cwnd): **slow start** (crescimento exponencial até o limiar ssthresh), **congestion avoidance** (crescimento linear, AIMD), **fast retransmit** (3 ACKs duplicados disparam a retransmissão sem esperar o timeout) e **fast recovery**. O timeout dispara a reinicialização em slow start. As variantes incluem Tahoe, Reno, NewReno, CUBIC (padrão no Linux) e BBR. As **portas** vão de 0 a 65535: bem conhecidas (0–1023), registradas (1024–49151) e dinâmicas (49152–65535). O **QUIC** (base do HTTP/3) usa UDP e implementa confiabilidade e TLS no espaço do usuário.

### B.8 Protocolos e serviços de aplicação

Tabela de portas que mais cai em prova:

|Serviço|Porta|Transporte|
|---|---|---|
|FTP|20 (dados) / 21 (controle)|TCP|
|SSH / SFTP|22|TCP|
|Telnet|23|TCP|
|SMTP|25 (587 submissão, 465 SMTPS)|TCP|
|DNS|53|UDP (consulta) e TCP (zona/respostas grandes)|
|DHCP|67 (servidor) / 68 (cliente)|UDP|
|TFTP|69|UDP|
|HTTP|80|TCP|
|POP3 / POP3S|110 / 995|TCP|
|NTP|123|UDP|
|IMAP / IMAPS|143 / 993|TCP|
|SNMP|161 (agente) / 162 (traps)|UDP|
|BGP|179|TCP|
|LDAP / LDAPS|389 / 636|TCP|
|HTTPS|443|TCP (e UDP no HTTP/3)|
|SMB|445|TCP|
|Syslog|514|UDP|
|RDP|3389|TCP|
|SIP|5060 (5061 TLS)|UDP/TCP|

**DNS:** sistema hierárquico e distribuído de nomes (raiz → TLD → domínio). A resolução pode ser recursiva (o servidor resolve pelo cliente) ou iterativa (o servidor responde com a indicação do próximo). Registros: **A** (IPv4), **AAAA** (IPv6), **CNAME** (alias), **MX** (e-mail), **NS** (servidor de nomes), **PTR** (reverso), **SOA** (autoridade da zona), **TXT**. O DNS usa UDP para consultas curtas e TCP para transferência de zona (AXFR) e respostas grandes. **DNSSEC** adiciona assinatura digital às respostas (autenticidade e integridade, sem confidencialidade), enquanto DoH e DoT cifram a consulta.

**DHCP:** atribui configuração IP automaticamente em quatro mensagens, o **DORA** (Discover, Offer, Request, Acknowledge), por UDP, com o concessionário por tempo de lease. **HTTP:** protocolo sem estado, com métodos GET, POST, PUT, DELETE, HEAD, OPTIONS, PATCH e códigos de status 1xx a 5xx (200 OK, 301/302 redirecionamento, 404 não encontrado, 500 erro de servidor). O HTTP/2 multiplexa sobre uma conexão TCP, e o HTTP/3 usa QUIC. **E-mail:** o **SMTP** envia (cliente→servidor e servidor→servidor), o **POP3** baixa mensagens (em geral removendo-as do servidor) e o **IMAP** mantém e sincroniza as mensagens no servidor. **FTP:** usa duas conexões (controle e dados), com modos ativo e passivo, e transfere credenciais em texto claro, sendo suas alternativas seguras o SFTP (sobre SSH) e o FTPS (FTP sobre TLS). **SNMP**, **NTP** e **LDAP** completam os mais cobrados.

### B.9 Roteamento

O roteamento decide o caminho dos pacotes pela **tabela de roteamento**, com seleção pela correspondência de **prefixo mais longo** (longest prefix match). O roteamento **estático** é configurado manualmente (simples, sem overhead, sem adaptação a falhas), e o **dinâmico** usa protocolos de roteamento. Os algoritmos são de **vetor de distância** (cada roteador informa aos vizinhos a sua tabela, “rumores”, Bellman-Ford) e de **estado de enlace** (cada roteador conhece a topologia completa e calcula caminhos com Dijkstra, SPF). Um **IGP** opera dentro de um sistema autônomo (RIP, OSPF, EIGRP, IS-IS), e um **EGP** entre sistemas autônomos (BGP).

O **RIP** é de vetor de distância, métrica = número de saltos, **máximo 15** (16 = infinito), atualizações a cada 30 s, versão 1 (classful, broadcast) e versão 2 (classless, multicast 224.0.0.9, autenticação). Os problemas de convergência lenta e **contagem ao infinito** são mitigados por **split horizon**, **poison reverse**, hold-down e triggered updates. O **OSPF** é de estado de enlace, usa a métrica “custo” (inversamente proporcional à largura de banda), roda direto sobre IP (protocolo 89), usa multicast 224.0.0.5 (todos os roteadores OSPF) e 224.0.0.6 (DR/BDR), divide a rede em **áreas** com a **área 0 (backbone)** como centro, e em redes multiacesso elege **DR e BDR** para reduzir a quantidade de adjacências. O **EIGRP** (Cisco, algoritmo DUAL, híbrido) usa métrica composta (largura de banda e atraso por padrão). O **IS-IS** é de estado de enlace e muito usado em provedores. O **BGP** é um protocolo de **vetor de caminho** (path vector), usa TCP 179, troca informações de prefixos entre ASes com atributos como AS_PATH, LOCAL_PREF e MED, e distingue eBGP (entre ASes) e iBGP (dentro do AS). O BGP é a cola da Internet e prioriza política sobre desempenho.

A **distância administrativa (AD)** escolhe entre fontes de rota diferentes (menor vence): conectada 0, estática 1, eBGP 20, EIGRP 90, OSPF 110, IS-IS 115, RIP 120, iBGP 200. **Macete:** “Conectada, Estática, eBGP, EIGRP, OSPF, IS-IS, RIP” — 0, 1, 20, 90, 110, 115, 120. O **MPLS** (RFC 3031) insere um rótulo de 32 bits (20 de label, 3 de EXP/TC, 1 de bottom-of-stack e 8 de TTL) entre as camadas 2 e 3 (por isso “camada 2,5”), e o encaminhamento é por rótulos em LSPs, com LERs na borda e LSRs no núcleo. Suporta engenharia de tráfego, QoS e VPNs.

### B.10 Qualidade de Serviço (QoS)

Os parâmetros fundamentais são **largura de banda** (vazão), **atraso (latência)**, **jitter** (variação do atraso) e **perda de pacotes**. As aplicações em tempo real (voz e vídeo interativo) são sensíveis a atraso, jitter e perda; as elásticas (e-mail, FTP) toleram atrasos mas exigem integridade.

**Modelos de QoS:** **Best Effort** (padrão da Internet, sem garantias), **IntServ** (Integrated Services, reserva de recursos por fluxo, sinalizada pelo **RSVP**, com garantias fortes e escalabilidade ruim) e **DiffServ** (Differentiated Services, classifica o tráfego em classes marcadas no campo DSCP do cabeçalho IP, tratadas por salto, com **PHB**, Per-Hop Behavior, sendo escalável). O campo DS tem 8 bits: 6 de **DSCP** + 2 de ECN. Os PHBs principais são **EF** (Expedited Forwarding, DSCP 46, baixa perda, latência e jitter, usada para voz), **AF** (Assured Forwarding, 4 classes × 3 precedências de descarte, ex.: AF11 a AF43), **CS** (compatível com o IP Precedence) e **BE/Default** (DSCP 0). Na camada 2, o **802.1p** usa 3 bits (PCP) na tag VLAN.

**Ferramentas:** classificação e marcação (identificar e rotular os pacotes), **policing** (descarta ou remarca o que excede a taxa contratada) e **shaping** (armazena em buffer o excesso para suavizar o tráfego, introduzindo atraso). Os algoritmos de balde são o **leaky bucket** (saída em taxa constante, suaviza rajadas) e o **token bucket** (permite rajadas até o tamanho do balde, com taxa média controlada). **Filas:** FIFO, **PQ** (priority queuing, que pode causar starvation das filas de baixa prioridade), **CQ**, **WFQ** (weighted fair queuing), **CBWFQ** (por classes) e **LLQ** (CBWFQ + fila estrita de prioridade para voz). Para o controle de congestionamento, **RED/WRED** descartam pacotes de forma aleatória e antecipada, evitando a sincronização global do TCP (tail drop).

### B.11 Redes sem fio

O **IEEE 802.11** (Wi-Fi) usa o **CSMA/CA** (evitação de colisão), porque em redes sem fio não é possível detectar colisão durante a transmissão. A estação escuta o meio, aguarda um intervalo (DIFS e backoff aleatório), transmite e espera o **ACK**. O **NAV** é o temporizador de reserva virtual do meio. O mecanismo opcional **RTS/CTS** mitiga o problema do **nó oculto** (hidden node).

|Padrão|Nome|Faixa|Taxa teórica|Observação|
|---|---|---|---|---|
|802.11b|–|2,4 GHz|11 Mbps|DSSS|
|802.11a|–|5 GHz|54 Mbps|OFDM|
|802.11g|–|2,4 GHz|54 Mbps|OFDM, compatível com b|
|802.11n|Wi-Fi 4|2,4 e 5 GHz|até 600 Mbps|MIMO, canais de 40 MHz|
|802.11ac|Wi-Fi 5|5 GHz|vários Gbps|256-QAM, MU-MIMO (downlink), até 160 MHz|
|802.11ax|Wi-Fi 6/6E|2,4, 5 (e 6 GHz no 6E)|~9,6 Gbps|OFDMA, 1024-QAM, TWT|
|802.11be|Wi-Fi 7|2,4, 5 e 6 GHz|~46 Gbps|4096-QAM, 320 MHz, MLO|

**Macete:** 2,4 GHz tem maior alcance e atravessa melhor obstáculos, mas é mais congestionada e tem poucos canais sem sobreposição (**1, 6 e 11**). 5 GHz tem mais canais e velocidade, mas alcance menor.

Os conceitos de arquitetura são **BSS** (um AP e seus clientes), **ESS** (vários BSS ligados por um sistema de distribuição, com o mesmo SSID, permitindo roaming), **IBSS** (ad hoc) e **SSID** (nome da rede). Os quadros do 802.11 são de gerenciamento (beacon, probe, autenticação, associação), de controle (RTS, CTS, ACK) e de dados.

**Segurança:** o **WEP** usa RC4 com vetor de inicialização de 24 bits e é completamente quebrável. O **WPA** usou TKIP (ainda RC4) como correção provisória. O **WPA2 (802.11i)** usa **AES-CCMP** e é o padrão consolidado. O **WPA3** introduz o **SAE** (Simultaneous Authentication of Equals), que substitui o handshake PSK e protege contra ataques de dicionário offline, e torna obrigatório o PMF (proteção de quadros de gerência). A variante **Enterprise** usa **802.1X/EAP** com servidor **RADIUS**, e a **Personal** usa chave pré-compartilhada (PSK). O **WPS** (PIN) é vulnerável. Outros ataques: evil twin, rogue AP, deauth, KRACK (contra o handshake de 4 vias do WPA2).

Outras tecnologias sem fio: **Bluetooth** (IEEE 802.15.1, 2,4 GHz, FHSS, piconet de 1 mestre e até 7 escravos ativos, scatternet), **Zigbee** (802.15.4, baixa taxa e baixo consumo, IoT), **NFC** (curtíssimo alcance), **WiMAX** (802.16, banda larga metropolitana), **LoRaWAN** (LPWAN de longo alcance) e a telefonia celular (2G GSM, 3G UMTS/HSPA, 4G LTE, 5G NR com eMBB, URLLC e mMTC). As gerações de celular mudam principalmente de comutação de circuitos para pacotes (3G → 4G, totalmente IP) e de voz para dados.

### B.12 VoIP e streaming de áudio e vídeo

#### VoIP (Voz sobre IP)

O fluxo básico é: captura analógica → digitalização → codificação/compressão (codec) → empacotamento em **RTP/UDP/IP** → transmissão → remontagem com **jitter buffer** → decodificação → reprodução. Os **codecs** mais cobrados: **G.711** (PCM, 64 kbps, sem compressão, qualidade de telefonia fixa), **G.729** (8 kbps, CS-ACELP), **G.723.1** (5,3 ou 6,3 kbps), **G.722** (banda larga, 7 kHz, 64 kbps), **G.726** (ADPCM) e **Opus** (RFC 6716, adaptativo, usado em WebRTC). O consumo real de banda é maior que a taxa do codec por causa dos cabeçalhos: com G.711 e pacotes a cada 20 ms, são 160 bytes de voz + 40 bytes de cabeçalhos (IP 20 + UDP 8 + RTP 12), ou 200 bytes a 50 pacotes/s, resultando em **80 kbps** (sem contar o enlace). As **métricas de qualidade** são atraso unidirecional (a recomendação **ITU-T G.114** indica até **150 ms** para boa qualidade), jitter, perda de pacotes (idealmente abaixo de 1%) e o **MOS** (Mean Opinion Score, de 1 a 5). O **E-model (G.107)** calcula o fator R.

**Protocolos de sinalização:** o **SIP** (RFC 3261) é baseado em texto, parecido com o HTTP, e usa os métodos **INVITE, ACK, BYE, CANCEL, REGISTER, OPTIONS** e respostas com códigos 1xx (provisórias), 2xx (sucesso), 3xx a 6xx. Seus elementos são o **User Agent** (cliente/servidor), o **Proxy**, o **Registrar** e o **Redirect Server**. O **SDP** descreve a sessão (codecs, portas, endereços). O **H.323** (ITU-T) é um conjunto de recomendações que usa terminais, **gatekeeper** (controle de admissão e tradução de endereços), **gateway** e **MCU** (conferência), com os subprotocolos H.225 (RAS e Q.931) e H.245 (controle de mídia). O **MGCP** e o **H.248/Megaco** controlam gateways de mídia. A **mídia** trafega por **RTP** (transporte de áudio e vídeo em tempo real, com número de sequência e timestamp) e o **RTCP** fornece estatísticas e feedback de qualidade (e usa a porta ímpar seguinte à da RTP). O **SRTP** protege a mídia com criptografia. Para atravessar NAT: **STUN, TURN e ICE**. O **Asterisk** é um PBX IP de código aberto muito cobrado, com **ATA** (adaptador para telefone analógico) e as interfaces **FXS** (fornece tom e energia ao telefone) e **FXO** (conecta-se à linha da operadora ou ao tronco analógico). O **IAX2** é o protocolo do Asterisk que usa uma única porta UDP (4569) para sinalização e mídia.

#### Streaming de áudio e vídeo

O **streaming** entrega mídia de forma contínua enquanto ela é reproduzida, com um buffer no cliente, diferentemente do download completo. Pode ser **sob demanda** (VoD) ou **ao vivo**. Os protocolos clássicos para tempo real são o **RTP/RTCP** e o **RTSP** (controle remoto da reprodução, tipo play/pause, porta 554). Os modernos usam **streaming adaptativo sobre HTTP**: **HLS** (Apple, listas .m3u8 e segmentos), **MPEG-DASH** (padrão ISO) e **Smooth Streaming**, nos quais o vídeo é fragmentado em segmentos de poucos segundos em várias qualidades e o cliente escolhe a taxa conforme a banda disponível (ABR). Isso atravessa firewalls, usa CDNs e se beneficia de caches HTTP. O **RTMP** é usado hoje principalmente na ingestão de transmissões ao vivo, o **WebRTC** (SRTP, DTLS, ICE, UDP) é usado para comunicação interativa de baixíssima latência, e o **SRT** é usado na contribuição profissional. Os codecs de vídeo mais comuns são **H.264/AVC**, **H.265/HEVC**, **VP9** e **AV1**, e os de áudio, **AAC**, **MP3** e **Opus**. O **multicast IP** (com IGMP e PIM) entrega um fluxo a muitos receptores economizando banda, e o **CDN** (rede de distribuição de conteúdo) aproxima o conteúdo do usuário.