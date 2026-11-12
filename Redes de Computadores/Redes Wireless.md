## Redes Wireless (Wi-Fi): guia completo

### 1. Fundamentos

**O que é:** uma rede wireless (WLAN, _Wireless Local Area Network_) usa ondas de rádio em vez de cabos para transportar quadros de camada 2. O padrão que a define é o **IEEE 802.11**. **Wi-Fi** é a marca da **Wi-Fi Alliance**, que certifica a interoperabilidade entre fabricantes.

**Por que é diferente de Ethernet:**

- O meio é **compartilhado e half-duplex**: só um dispositivo transmite por vez em cada canal.
- Não existe detecção de colisão. Ethernet usa CSMA/CD, e o Wi-Fi usa **CSMA/CA** (_Collision Avoidance_): o dispositivo escuta o canal, espera um tempo aleatório (_backoff_) e só então transmite. O receptor responde com um **ACK** para cada quadro unicast.
- O meio é **não confiável**: sofre interferência, atenuação e obstáculos, e qualquer um ao alcance pode captar o sinal. Por isso a segurança é tão importante.

**Componentes:**

- **STA (estação):** qualquer cliente, como notebook, celular ou IoT.
- **AP (_Access Point_):** conecta clientes wireless à rede cabeada. Funciona como uma ponte de camada 2 entre 802.11 e 802.3.
- **SSID:** nome da rede (até 32 caracteres).
- **BSSID:** identificador único de um rádio do AP. É o endereço MAC do rádio, em formato de 48 bits.
- **BSS (_Basic Service Set_):** um AP e seus clientes. A área de cobertura é o **BSA**.
- **ESS (_Extended Service Set_):** vários APs com o mesmo SSID, interligados por um sistema de distribuição (DS, a rede cabeada). É o que permite o **roaming**.
- **IBSS (ad hoc):** clientes conversam diretamente, sem AP. É raro hoje.
- **Wi-Fi Direct:** conexão ponto a ponto entre dispositivos, usada em espelhamento de tela e impressoras.

**Tipos de associação do AP:**

1. **Probe/Scan:** o cliente descobre redes de forma passiva (ouve _beacons_) ou ativa (envia _probe requests_).
2. **Authentication:** é a autenticação 802.11 “aberta” (legada e quase simbólica). A autenticação real vem depois, com WPA.
3. **Association:** o cliente se associa ao AP e recebe um AID.

---

### 2. Física do rádio (conceitos que caem em prova e na prática)

- **Frequência e comprimento de onda:** frequências altas têm mais banda e menos alcance, porque atravessam pior os obstáculos. As baixas têm mais alcance e penetração.
- **Amplitude:** representa a potência do sinal.
- **Fase:** é a posição da onda no ciclo. É usada na modulação.
- **Potência de transmissão:** medida em **mW** ou **dBm**. Em dBm, +3 dB dobra a potência e -3 dB corta pela metade.
- **dBi:** ganho da antena em relação a uma antena isotrópica.
- **EIRP:** potência efetivamente irradiada, calculada como TX + ganho da antena - perdas do cabo. É o que os reguladores limitam (no Brasil, a Anatel).
- **RSSI:** indicador da força do sinal recebido. Valores típicos:
    - Acima de -67 dBm: bom para voz e vídeo.
    - Entre -67 e -70 dBm: aceitável.
    - Abaixo de -80 dBm: ruim.
- **Ruído (_noise floor_)** e **SNR:** relação sinal/ruído, calculada como RSSI - ruído. Uma SNR acima de 25 dB é boa.
- **Atenuação (_path loss_):** o sinal enfraquece com a distância (_Free Space Path Loss_) e com obstáculos como concreto, metal e água. Concreto e metal atenuam muito.
- **Fenômenos de propagação:**
    - Reflexão
    - Refração
    - Difração
    - Espalhamento
    - Absorção
- **Multipath:** o sinal chega por vários caminhos e causa interferência. O MIMO usa isso a favor.
- **Fontes de interferência:** micro-ondas, Bluetooth, babás eletrônicas, telefones sem fio e redes vizinhas.
    - **Co-channel:** APs no mesmo canal compartilham o meio.
    - **Adjacent channel:** canais sobrepostos, que é pior do que o co-channel.

---

### 3. Bandas e canais

#### 2,4 GHz

- Tem **mais alcance**, penetra melhor em paredes e tem **mais interferência**.
- Há 14 canais de 22 MHz de largura (no Brasil, 1 a 13), espaçados de 5 MHz entre si. Isso gera sobreposição.
- **Canais não sobrepostos: 1, 6 e 11.** Esse é o plano clássico para evitar interferência de canal adjacente.
- Banda lotada (Bluetooth, micro-ondas, IoT).

#### 5 GHz

- Tem **mais banda**, menos interferência e **menos alcance**, porque atenua mais.
- Há muito mais canais, todos não sobrepostos quando se usa 20 MHz.
- Canais de 20, 40, 80 e 160 MHz. Quanto mais largo o canal, maior a velocidade e menos canais sobram.
- **DFS (_Dynamic Frequency Selection_):** em parte do espectro de 5 GHz, o AP deve liberar o canal ao detectar radar (meteorológico ou militar). Isso pode causar quedas momentâneas.
- Funciona bem em ambientes de alta densidade.

#### 6 GHz (Wi-Fi 6E / Wi-Fi 7)

- É uma banda nova, com muito espectro limpo e canais de até 160 MHz (e 320 MHz no Wi-Fi 7).
- Só aceita dispositivos 6E ou mais novos. **WPA3 é obrigatório.**
- O alcance é ainda menor que o de 5 GHz.

#### Resumo

||2,4 GHz|5 GHz|6 GHz|
|---|---|---|---|
|Alcance|Maior|Médio|Menor|
|Velocidade|Menor|Alta|Muito alta|
|Interferência|Alta|Baixa|Mínima|
|Canais (sem sobreposição)|3|Muitos|Muitos|

**Boa prática:** use **2,4 GHz para IoT e alcance** e **5/6 GHz para desempenho**. Muitos APs oferecem **band steering** para empurrar clientes dual-band para 5 GHz.

---

### 4. Padrões IEEE 802.11

|Padrão|Nome Wi-Fi|Ano|Banda|Velocidade máx. teórica|Tecnologias-chave|
|---|---|---|---|---|---|
|802.11|(original)|1997|2,4|2 Mbps|DSSS/FHSS|
|802.11b|-|1999|2,4|11 Mbps|DSSS|
|802.11a|-|1999|5|54 Mbps|OFDM|
|802.11g|-|2003|2,4|54 Mbps|OFDM, compatível com b|
|**802.11n**|**Wi-Fi 4**|2009|2,4 e 5|600 Mbps|**MIMO**, canais de 40 MHz|
|**802.11ac**|**Wi-Fi 5**|2013|**5**|~3,5 Gbps (Wave 2)|**MU-MIMO (downlink)**, 80/160 MHz, 256-QAM|
|**802.11ax**|**Wi-Fi 6 / 6E**|2019/2020|2,4, 5 (6E: +6)|~9,6 Gbps|**OFDMA**, MU-MIMO bidirecional, 1024-QAM, TWT, BSS Coloring|
|802.11be|Wi-Fi 7|2024|2,4, 5, 6|~46 Gbps|320 MHz, 4096-QAM, MLO|

#### Tecnologias importantes

- **MIMO (_Multiple Input, Multiple Output_):** múltiplas antenas e _spatial streams_ (fluxos espaciais). A notação é **TxR:S**, por exemplo 3x3:3.
- **SU-MIMO vs MU-MIMO:**
    - SU atende um cliente por vez.
    - MU atende vários clientes ao mesmo tempo (a partir do Wi-Fi 5, no downlink).
- **Beamforming:** o AP concentra o sinal na direção do cliente.
- **Channel bonding:** juntar canais de 20 MHz para formar 40, 80 ou 160 MHz.
- **OFDM:** divide o canal em subportadoras ortogonais.
- **OFDMA (Wi-Fi 6):** divide o canal em _Resource Units_, de modo que vários clientes transmitem no mesmo quadro. Isso melhora a eficiência em alta densidade e com tráfego pequeno.
- **BSS Coloring (Wi-Fi 6):** identifica o BSS no cabeçalho, reduzindo a interferência co-channel.
- **TWT (_Target Wake Time_):** agenda quando o cliente acorda, o que economiza bateria (muito útil em IoT).
- **QAM:** modulação de amplitude e fase. Quanto mais alta (256, 1024, 4096), maior a taxa, mas exige sinal melhor.
- **MLO (_Multi-Link Operation_, Wi-Fi 7):** o cliente usa várias bandas ao mesmo tempo.
- **Retrocompatibilidade:** os padrões novos funcionam com os antigos, mas um cliente lento pode degradar o desempenho do canal inteiro.

**Observação sobre nomes:** o nome Wi-Fi 4/5/6 é comercial. Em prova, você precisa associar **802.11n = Wi-Fi 4, ac = Wi-Fi 5 e ax = Wi-Fi 6**.

---

### 5. Segurança wireless

#### Evolução

|Protocolo|Criptografia|Situação|
|---|---|---|
|**WEP**|RC4, chaves de 40/104 bits, IV de 24 bits|**Obsoleto e quebrável em minutos.** Nunca use.|
|**WPA**|RC4 + **TKIP**|Solução temporária, hoje também obsoleta|
|**WPA2**|**AES-CCMP**|Padrão por muitos anos, ainda aceitável|
|**WPA3**|**AES-CCMP/GCMP-256**|O mais atual, com autenticação **SAE**|

#### Modos de autenticação

- **Personal (PSK, _Pre-Shared Key_):** todos usam a mesma senha. É simples e comum em casa e em pequenos escritórios. O problema é que quem sai da empresa leva a senha.
- **Enterprise (802.1X/RADIUS):** cada usuário se autentica individualmente, com credenciais ou certificado.
    - **Suplicante:** o cliente.
    - **Autenticador:** o AP ou a WLC.
    - **Servidor de autenticação:** o **RADIUS** (por exemplo, Cisco ISE ou FreeRADIUS).
    - Usa **EAP** em métodos como **PEAP, EAP-TLS e EAP-FAST**.
    - Cada sessão recebe chaves únicas, o que permite revogar acessos individualmente. É o modelo correto para empresas.

#### Novidades do WPA3

- **SAE (_Simultaneous Authentication of Equals_):** substitui o handshake do PSK e protege contra ataques de dicionário offline.
- **Forward secrecy:** se a senha vazar depois, o tráfego capturado antes continua protegido.
- **PMF (_Protected Management Frames_, 802.11w):** protege quadros de gerenciamento, evitando ataques de _deauthentication_. É obrigatório no WPA3.
- **OWE (_Opportunistic Wireless Encryption_):** criptografa redes abertas, como Wi-Fi de visitantes, sem senha.
- **WPA3-Enterprise 192-bit:** modo de segurança elevada.
- O WPA3 tem **modo de transição**, que aceita clientes WPA2 e WPA3 no mesmo SSID.

#### Outras proteções e riscos

- **Filtro de MAC e ocultar SSID:** **não são segurança de verdade.** Ambos são facilmente contornáveis.
- **WPS:** conveniente, mas vulnerável a ataques de PIN. Recomenda-se desativar.
- **Ameaças comuns:**
    - **Rogue AP:** AP não autorizado ligado à rede.
    - **Evil Twin:** AP falso com o mesmo SSID para interceptar tráfego.
    - **Deauth attack:** derruba clientes.
    - **Eavesdropping/sniffing:** captura de tráfego sem criptografia.
    - **Man-in-the-Middle.**
    - **KRACK (WPA2)** e outras vulnerabilidades de handshake.
- **WIPS/WIDS:** sistemas que detectam e contêm rogues. Controladoras Cisco têm recursos de contenção.
- **Boas práticas:**
    - Use WPA3 (ou WPA2-AES) e senhas longas.
    - Use 802.1X em ambientes corporativos.
    - Separe visitantes em uma **VLAN/SSID dedicado** (_guest_), com isolamento de clientes e portal captivo.
    - Atualize o firmware.
    - Segmente por VLAN, mapeando cada **SSID para uma VLAN**.

---

### 6. Arquiteturas de redes wireless

#### 6.1 Autônoma (_Autonomous AP_)

- Cada AP é configurado e gerenciado **individualmente** (via GUI, SSH ou CLI). Cada um tem sua própria inteligência.
- **Vantagens:** simples e barato, sem controladora. Ótimo para casas e pequenas empresas.
- **Desvantagens:** não escala, a configuração é inconsistente e o gerenciamento é difícil. Falta roaming inteligente e gerenciamento de RF centralizado.
- O AP conecta a um **porta de acesso** do switch, em uma única VLAN. Com várias VLANs, usa uma porta **trunk**.

#### 6.2 Baseada em nuvem (_Cloud-based_)

- Os APs são gerenciados por um **painel na nuvem** (por exemplo, **Cisco Meraki**). O **plano de gerenciamento** fica na nuvem, e o **plano de dados** (tráfego dos usuários) continua local, passando direto pela rede.
- **Vantagens:** gerência centralizada sem hardware de controladora, provisionamento rápido (_zero-touch_), atualizações automáticas, bom para filiais e múltiplos sites.
- **Desvantagens:** depende de licenciamento recorrente. Sem internet, a gerência fica indisponível (os clientes costumam continuar funcionando).

#### 6.3 Baseada em controladora (_Split-MAC_)

- Uma **WLC (_Wireless LAN Controller_)** gerencia vários APs em modo **lightweight (LWAP)**. Os APs ficam “burros” e a WLC concentra a inteligência.
- **CAPWAP (_Control And Provisioning of Wireless Access Points_):** protocolo de túnel entre AP e WLC.
    - **Plano de controle:** **UDP 5246**, criptografado com DTLS.
    - **Plano de dados:** **UDP 5247**, com criptografia opcional.
- **Split-MAC:** divide as funções.
    - **No AP (tempo real):** transmissão e recepção de rádio, beacons, probe responses, criptografia dos quadros, ACK/retransmissão.
    - **Na WLC (gerenciamento):** autenticação, associação, roaming, gerenciamento de RF (canais e potência), políticas de segurança, QoS.
- **Processo de join do AP:** o AP descobre a WLC (por DHCP opção 43, DNS ou broadcast), estabelece o túnel CAPWAP e baixa configuração e firmware.
- **Vantagens:** gerenciamento centralizado, roaming transparente, RRM (_Radio Resource Management_) dinâmico, políticas consistentes, WIPS integrado. Escala bem para campus.
- **Desvantagens:** custo da controladora, ponto de falha (resolvido com redundância HA).

#### Modos de operação do AP lightweight (Cisco)

- **Local:** modo padrão. Serve clientes e cria o túnel CAPWAP.
- **FlexConnect:** permite comutar o tráfego localmente se o link com a WLC cair. É ideal para filiais.
- **Monitor:** não serve clientes, só detecta rogues e ameaças.
- **Sniffer:** captura tráfego para análise.
- **Rogue Detector, Bridge/Mesh, SE-Connect** e outros.

#### Variações da arquitetura

- **Embedded Wireless Controller:** a WLC roda dentro de um AP ou switch.
- **Mobility Express:** controladora virtual em um AP.
- **Mesh:** APs que se conectam entre si por rádio, usados em áreas amplas.
- **Controladora virtual ou em cloud:** para ambientes virtualizados.

#### Comparativo

||Autônoma|Cloud|Controladora|
|---|---|---|---|
|Gerência|Individual|Painel na nuvem|WLC central|
|Escala|Baixa|Alta|Alta|
|Custo inicial|Baixo|Médio|Mais alto|
|Ideal para|Casa e pequeno escritório|Multi-site e filiais|Campus e grande empresa|

---

### 7. Roaming, controle de RF e QoS

- **Roaming:** o cliente passa de um AP a outro no mesmo ESS sem perder a conexão.
    - **Layer 2 roaming:** os APs ficam na mesma VLAN/sub-rede, e o IP não muda.
    - **Layer 3 roaming:** a sub-rede muda, e a controladora usa túneis de mobilidade para manter o IP do cliente.
    - **Protocolos de roaming rápido:** **802.11r** (_Fast BSS Transition_), **802.11k** (relatórios de vizinhos) e **802.11v** (gerenciamento de transição). Importantes para voz.
- **Projeto de cobertura (_site survey_):**
    - Cobertura com **sobreposição de cerca de 15 a 20%** entre células vizinhas.
    - Planejamento de canais para evitar co-channel.
    - Potência adequada, sem exagero (o cliente também precisa “ouvir” o AP, então uma potência alta demais só gera interferência).
    - Considere a densidade de usuários, os materiais do prédio e as aplicações (voz exige RSSI maior).
- **RRM (_Radio Resource Management_):** ajusta canais (DCA) e potência (TPC) automaticamente e cobre falhas de cobertura.
- **QoS wireless:** **WMM** (_Wi-Fi Multimedia_) prioriza o tráfego em quatro categorias: **voz, vídeo, melhor esforço e background**.
- **Antenas:**
    - **Omnidirecional:** cobertura em 360°.
    - **Direcional (Yagi, patch, parabólica):** feixe focado, usado em enlaces ponto a ponto e corredores.
    - Maior ganho significa feixe mais estreito.

---

### 8. Conexão com a rede cabeada e acesso à gerência

- **APs** conectam a **portas de switch**, normalmente com **PoE** (802.3af/at/bt) para alimentação.
    - **Autônomo:** porta de acesso (uma VLAN) ou trunk.
    - **Lightweight:** porta de acesso na VLAN de gerência. Os dados do cliente viajam dentro do túnel CAPWAP até a WLC.
- **WLC:** conecta ao switch por portas **trunk**, que podem ser agregadas com **LAG**, e mapeia cada **SSID para uma interface dinâmica/VLAN**.
- **Interfaces da WLC (Cisco):**
    - **Management interface:** gerência e comunicação com os APs.
    - **Service port:** gerência fora de banda.
    - **Dynamic interfaces:** associadas às VLANs dos clientes.
    - **Virtual interface:** usada em roaming entre controladoras.
- **Acesso à gerência:** GUI (HTTPS), CLI (SSH), SNMP, e APIs/REST em soluções modernas.
- **Configuração de uma WLAN na WLC (visão geral):**
    1. Criar o **SSID**.
    2. Mapear para uma **interface/VLAN**.
    3. Definir a **segurança** (WPA2/WPA3, PSK ou 802.1X).
    4. Habilitar a WLAN e ajustar QoS e bandas.

---

### 9. Usos e contextos de aplicação

- **Residencial:** AP autônomo ou roteador Wi-Fi. Prefira WPA2/WPA3 e canais bem escolhidos.
- **Pequeno escritório:** AP autônomo ou um sistema simples gerenciado na nuvem.
- **Corporativo e campus:** controladora (WLC) ou cloud, com 802.1X, SSIDs para visitantes e roaming rápido.
- **Alta densidade (estádios, auditórios, escolas):** Wi-Fi 6/6E, OFDMA, planejamento fino de canais e células pequenas.
- **Filiais:** cloud (Meraki) ou FlexConnect.
- **Indústria e logística:** antenas diretas, mesh, coberturas resistentes, roaming de baixa latência.
- **Saúde:** voz sobre Wi-Fi, dispositivos críticos, segmentação estrita.
- **IoT:** 2,4 GHz, economia de energia (TWT), VLAN própria e segmentação.
- **Voz e vídeo:** exigem boa cobertura (RSSI forte), QoS/WMM e roaming rápido (802.11r/k/v).
- **Enlaces ponto a ponto (_backhaul_):** antenas direcionais conectam prédios.
- **Hotspots públicos:** portal captivo e OWE, com isolamento de clientes.

---

### 10. Troubleshooting

|Sintoma|Causas comuns|
|---|---|
|Lentidão|Interferência co-channel, cliente legado, canal saturado, sinal fraco|
|Quedas|DFS, roaming ruim, interferência, drivers antigos|
|Sem conexão|Senha incorreta, tipo de segurança incompatível, banda não suportada pelo cliente|
|Cliente “gruda” no AP distante|Roaming agressivo ausente, baixa potência no cliente (resolver com 802.11k/v/r)|
|Sem IP|VLAN errada, DHCP indisponível, mapeamento SSID↔VLAN incorreto|
|AP não junta à WLC|Falha no DHCP opção 43, DNS, firewall bloqueando UDP 5246/5247, hora incorreta (certificados)|

Ferramentas úteis incluem analisadores de espectro e Wi-Fi (por exemplo Ekahau e NetSpot), pesquisa de site (_site survey_) e os logs da WLC.

---

### 11. Resumo para memorizar

- **Padrões:** n=Wi-Fi 4, ac=Wi-Fi 5, ax=Wi-Fi 6. OFDMA, BSS Coloring e TWT são do **ax**.
- **2,4 GHz:** mais alcance e mais interferência. Canais sem sobreposição: **1, 6 e 11**. **5 GHz:** mais velocidade e menos alcance. **6 GHz:** Wi-Fi 6E, WPA3 obrigatório.
- **Segurança:** WEP (RC4, obsoleto) → WPA (TKIP) → WPA2 (AES-CCMP) → WPA3 (SAE). **Personal = PSK; Enterprise = 802.1X + EAP + RADIUS.**
- **Arquiteturas:** autônoma (individual), cloud (gerência na nuvem, dados locais) e controladora (WLC + APs lightweight com **CAPWAP**, UDP **5246 (controle)** e **5247 (dados)**, **Split-MAC**).
- **CSMA/CA** com ACK, meio half-duplex. **SSID** é o nome, **BSSID** é o MAC do rádio, **ESS** são vários APs com o mesmo SSID.
- **Roaming rápido:** 802.11r, k e v.
- Filtro de MAC e SSID oculto **não são segurança**.