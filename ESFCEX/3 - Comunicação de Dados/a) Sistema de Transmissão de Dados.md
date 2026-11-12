## PARTE A – SISTEMAS DE TRANSMISSÃO DE DADOS

### A.1 Conceitos fundamentais

Dado é a representação bruta de um fato, e informação é o dado interpretado em um contexto. A comunicação de dados trata de como transportar esses dados de um ponto a outro, e a unidade que realmente viaja pelo meio físico é o **sinal**, uma grandeza física (tensão, corrente, luz, onda eletromagnética) que varia no tempo e representa os dados.

Um sinal **analógico** assume infinitos valores dentro de um intervalo, como a voz captada por um microfone. Um sinal **digital** assume um conjunto finito de valores discretos, normalmente dois níveis, representando 0 e 1. Os sinais também podem ser periódicos, quando repetem um padrão em um intervalo fixo, ou não periódicos. Um ponto que a banca gosta de explorar é que sinais periódicos têm espectro discreto e sinais não periódicos têm espectro contínuo. Os sinais digitais, por terem transições abruptas, têm em teoria largura de banda infinita, e na prática se usa uma banda efetiva finita.

Os atributos de uma onda senoidal são amplitude, frequência e fase. A frequência é medida em hertz (ciclos por segundo), e o período é o inverso da frequência (T = 1/f). A **largura de banda** (bandwidth) tem dois sentidos que não devem ser confundidos. No sentido analógico, é a diferença entre a maior e a menor frequência que o meio ou sinal suporta, medida em Hz. No sentido digital, é a taxa de transmissão em bits por segundo.

A **taxa de bits** (bps) é a quantidade de bits transmitidos por segundo. A **taxa de sinalização ou baud rate** é a quantidade de elementos de sinal (símbolos) por segundo. A relação entre elas é:

> bps = baud × log₂(M), onde M é o número de níveis ou símbolos distintos.

Se cada símbolo carrega 4 bits (16 símbolos), a taxa de bits é 4 vezes o baud. Essa conta aparece com frequência em prova.

#### Limites teóricos de capacidade

O **teorema de Nyquist** vale para um canal sem ruído: C = 2 · B · log₂(M), onde B é a largura de banda e M o número de níveis. Para um canal de 3 kHz com 2 níveis, a capacidade máxima é 6 kbps. O mesmo autor deu o critério de amostragem: para reconstruir fielmente um sinal analógico, é preciso amostrá-lo a pelo menos o dobro da maior frequência contida nele.

O **teorema de Shannon** vale para um canal com ruído: C = B · log₂(1 + SNR), onde SNR é a relação sinal-ruído como razão linear, e não em dB. A conversão é SNR(dB) = 10 · log₁₀(SNR linear). Uma SNR de 30 dB corresponde a 1000 vezes. Shannon diz que, mesmo com infinitos níveis, o ruído limita a taxa, e que aumentar a potência ou a banda é o que eleva a capacidade. **Macete:** se a questão menciona ruído, é Shannon. Se não menciona, é Nyquist.

#### Deficiências de transmissão

A **atenuação** é a perda de potência do sinal com a distância, e é combatida com amplificadores (analógico) ou repetidores (digital). A **distorção** acontece porque cada componente de frequência se propaga a uma velocidade diferente, e a distorção por atraso é especialmente nociva em sinais digitais. O **ruído** inclui o térmico, o de intermodulação, a **diafonia (crosstalk)**, que é o acoplamento entre pares vizinhos, e o impulsivo. As métricas são NEXT e FEXT, e o par trançado reduz o crosstalk justamente por causa do trançamento. Atenuação e distorção de atraso são diferentes de ruído, pois o ruído é um sinal indesejado somado ao sinal útil.

### A.2 Componentes de um sistema de transmissão

O modelo clássico tem cinco elementos: **fonte** (gera os dados), **transmissor** (converte os dados em sinal adequado ao meio), **meio de transmissão**, **receptor** (converte o sinal de volta) e **destino**. Na terminologia de equipamentos, o **DTE** (Data Terminal Equipment) é o terminal ou computador que gera e consome dados, e o **DCE** (Data Circuit-terminating Equipment) é o equipamento que adapta o sinal ao meio, como modem, CSU/DSU ou placa de rede. A interface DTE–DCE foi padronizada por normas como RS-232 (EIA/TIA-232, equivalente à V.24 e V.28 do ITU-T), RS-449 e X.21.

#### Meios guiados

O **par trançado** vem em duas versões, UTP (sem blindagem) e STP/FTP/S-FTP (com blindagem). As categorias determinam banda e velocidade: a Cat5 opera a 100 MHz e é usada em 100 Mbps, a Cat5e suporta 1 Gbps, a Cat6 tem 250 MHz e atinge 10 Gbps em até 55 m, a Cat6a tem 500 MHz e atinge 10 Gbps em 100 m, e a Cat7 tem 600 MHz. A distância máxima de um segmento Ethernet em par trançado é **100 m**, sendo 90 m de cabo permanente mais 10 m de cordões de manobra. Os conectores são RJ-45 e as normas de pinagem são T568A e T568B. Cabo direto liga dispositivos diferentes (PC–switch), e cabo crossover liga dispositivos iguais, embora a função Auto-MDIX torne isso dispensável nos equipamentos atuais.

O **cabo coaxial** tem condutor central, dielétrico, malha e capa. Foi usado em 10BASE5 (grosso, 500 m) e 10BASE2 (fino, 185 m), e hoje aparece em TV a cabo e CFTV. A **fibra óptica** guia a luz por reflexão interna total. A **monomodo** tem núcleo de cerca de 9 µm, usa laser, tem baixa atenuação e atinge dezenas de quilômetros. A **multimodo** tem núcleo de 50 ou 62,5 µm, usa LED ou VCSEL e é usada em distâncias curtas, como um prédio ou campus, sofrendo com a dispersão modal. A fibra é imune a interferência eletromagnética, tem maior segurança contra interceptação e oferece a maior largura de banda, mas seus conectores e emendas são mais delicados (SC, LC, ST, FC).

#### Meios não guiados

Incluem rádio (omnidirecional, atravessa paredes), micro-ondas terrestres e por satélite (direcionais, exigem linha de visada), infravermelho (curto alcance, não atravessa paredes) e laser. Os satélites geoestacionários (GEO), a cerca de 36.000 km, têm latência de ida e volta na faixa de 500 a 600 ms. Os de órbita baixa (LEO), como a constelação Starlink, têm latência bem menor.

### A.3 Topologia de ponto de vista de transmissão

Do ponto de vista da linha, uma ligação pode ser **ponto a ponto**, com dois dispositivos compartilhando exclusivamente um enlace, ou **multiponto (multidrop)**, com vários dispositivos compartilhando o mesmo meio. As topologias de rede propriamente ditas (barramento, estrela, anel, malha etc.) estão no tópico de Redes, na Parte B, porque a banca costuma cobrá-las lá.

### A.4 Codificação de linha, PCM e controle de erros

#### Codificação de linha (dados digitais, sinal digital)

No **NRZ-L**, um nível de tensão representa 0 e outro representa 1. É simples, mas sofre com perda de sincronismo em sequências longas de bits iguais. O **NRZI** inverte o nível quando aparece um 1. O **RZ** retorna a zero no meio do bit. O **AMI (Alternate Mark Inversion)** usa três níveis, com o 0 em nível zero e os 1s alternando polaridade, o que elimina componente DC. O **Manchester** tem transição no meio de cada bit, que codifica o dado e o relógio ao mesmo tempo (autossincronismo), e dobra a banda necessária, com eficiência de 50%. É usado no Ethernet 10 Mbps (IEEE 802.3), com a transição de baixa para alta representando 1 e de alta para baixa representando 0. O **Manchester Diferencial** usa a presença ou ausência de transição no início do bit para codificar o dado, e foi usado no Token Ring (802.5). O **4B/5B** mapeia 4 bits em 5 para garantir transições suficientes e é usado no 100BASE-TX (combinado com MLT-3) e no FDDI. O **8B/10B** é usado em Gigabit Ethernet de fibra, Fibre Channel e SATA. O 1000BASE-T usa **PAM-5** nos quatro pares simultaneamente, em modo full duplex em cada par.

#### PCM (dados analógicos, sinal digital)

A modulação por código de pulso tem três etapas. Na **amostragem**, o sinal é medido a intervalos regulares. Na **quantização**, cada amostra é aproximada para um nível discreto, o que gera o ruído de quantização. Na **codificação**, o nível é convertido em palavras binárias. Na telefonia, a voz é limitada a 4 kHz, amostrada a **8.000 amostras/s**, com 8 bits por amostra, resultando em **64 kbps** por canal (DS0). Esse valor é a base do **G.711**, com compressão logarítmica μ-law (América do Norte e Japão) ou A-law (Europa e Brasil). Um E1 agrega 32 canais de 64 kbps (30 de voz, mais sinalização e sincronismo) e resulta em 2,048 Mbps. Um T1 agrega 24 canais e resulta em 1,544 Mbps.

#### Detecção e correção de erros

A **paridade** (par ou ímpar) detecta erros de um bit e não detecta erros em número par de bits. O **checksum** é usado em IP, TCP e UDP. O **CRC** (verificação de redundância cíclica) é baseado em divisão polinomial e é extremamente eficiente contra rajadas de erro. O Ethernet usa CRC-32 no campo FCS. O **código de Hamming** permite corrigir erros de um bit (FEC), com os bits de paridade nas posições potências de 2. A distância de Hamming entre duas palavras é o número de posições em que elas diferem: para detectar _d_ erros, é preciso distância mínima d + 1, e para corrigir _d_ erros, 2d + 1.

#### Controle de fluxo e retransmissão (camada de enlace)

O **Stop-and-Wait** envia um quadro e espera o ACK, com baixa eficiência em enlaces de alto produto banda × atraso. As técnicas de janela deslizante são o **Go-Back-N**, que retransmite o quadro com erro e todos os seguintes, e o **Selective Repeat**, que retransmite apenas o quadro com erro. O protocolo clássico de enlace orientado a bit é o **HDLC** (ISO), do qual derivam o LAPB do X.25 e o PPP.

### A.5 Modulação

A modulação altera uma portadora (normalmente senoidal) de acordo com o sinal de informação, para adaptar o sinal ao meio, permitir multiplexação por frequência e viabilizar antenas de tamanho prático.

**Dados digitais, sinal analógico (chaveamento):** o **ASK** varia a amplitude (o caso extremo é o OOK, on-off keying, usado em fibra); é sensível a ruído. O **FSK** varia a frequência (o BFSK usa duas frequências), com maior imunidade a ruído que o ASK, e foi usado em modems antigos (Bell 103). O **PSK** varia a fase: o BPSK usa duas fases (1 bit/símbolo), o QPSK usa quatro fases (2 bits/símbolo), o 8-PSK usa 3 bits/símbolo. O **QAM** combina amplitude e fase: 16-QAM (4 bits/símbolo), 64-QAM (6 bits), 256-QAM (8 bits), 1024-QAM (10 bits) e 4096-QAM (12 bits). Quanto maior a ordem, maior a taxa por símbolo e maior a exigência de SNR. A visualização é o **diagrama de constelação**. É o QAM que permite as altas taxas de ADSL (DMT), cable modem, Wi-Fi e LTE/5G.

**Dados analógicos, sinal analógico:** **AM** (amplitude), **FM** (frequência) e **PM** (fase). O FM tem melhor imunidade a ruído que o AM, ao custo de maior banda.

**Espalhamento espectral (spread spectrum):** o **FHSS** salta entre frequências segundo uma sequência pseudoaleatória (Bluetooth, 802.11 original). O **DSSS** multiplica cada bit por uma sequência de chips (802.11b, GPS, CDMA). **OFDM** divide a banda em muitas subportadoras ortogonais, com robustez a multipercurso (802.11a/g/n/ac/ax, ADSL, LTE, DVB-T, ISDB-Tb, o padrão brasileiro de TV digital). O **OFDMA**, usado em Wi-Fi 6 e 5G, atribui subconjuntos de subportadoras a usuários diferentes.

**Multiplexação** (compartilhamento do meio por vários canais): **FDM** (divisão em frequência, analógico), **TDM síncrono** (slots fixos por canal, com desperdício se o canal ficar ocioso), **TDM estatístico** (slots sob demanda), **WDM/DWDM/CWDM** (divisão em comprimento de onda, na fibra, a versão óptica do FDM) e **CDM/CDMA** (códigos ortogonais). Os sistemas de telefonia usam as hierarquias PDH (E1, E3) e **SDH/SONET** (STM-1 = 155,52 Mbps, STM-4 = 622 Mbps, STM-16 = 2,5 Gbps, STM-64 = 10 Gbps). SDH é padrão ITU-T (G.707), e SONET é o equivalente americano.

### A.6 Modos de transmissão

**Quanto ao sentido do fluxo:** **simplex** (unidirecional, como rádio e TV), **half-duplex** (bidirecional, mas não simultâneo, como o walkie-talkie e o Ethernet em hub com CSMA/CD) e **full-duplex** (bidirecional simultâneo, como telefone e Ethernet em switch).

**Quanto ao número de bits simultâneos:** **paralela** (vários bits ao mesmo tempo em várias linhas, para curtas distâncias, como o antigo barramento paralelo da impressora) e **serial** (um bit de cada vez, o padrão em comunicação de dados, como USB, SATA, PCIe e RS-232).

**Quanto à sincronização:** na **assíncrona**, cada caractere é enquadrado por bits de start e stop, e o relógio de transmissão e recepção não é compartilhado (RS-232, 8N1). Tem overhead alto. Na **síncrona**, os blocos de dados são enviados continuamente, com o sincronismo derivado do próprio sinal ou de linha de relógio separada (HDLC, Ethernet). É mais eficiente.

**Quanto ao endereçamento:** **unicast** (um para um), **multicast** (um para um grupo) e **broadcast** (um para todos), e o IPv6 adiciona o **anycast** (para o mais próximo de um grupo).

**Quanto à comutação:** na **comutação de circuitos**, um caminho dedicado é estabelecido antes da transmissão (telefonia clássica), com largura de banda garantida e atraso constante, mas desperdício em tráfego em rajadas. Na **comutação de pacotes**, a mensagem é dividida em pacotes que seguem pelo caminho independentemente (datagrama, como o IP) ou por um **circuito virtual** pré-estabelecido (X.25, Frame Relay, ATM, MPLS), com multiplexação estatística e uso eficiente do meio. Na **comutação de mensagens**, a mensagem inteira é armazenada e encaminhada (store-and-forward).

**Quanto à largura de banda do meio:** **banda base** (um único canal usa toda a banda, sinal digital sem modulação, como Ethernet) e **banda larga** (a banda é dividida em vários canais por FDM, como cable modem).