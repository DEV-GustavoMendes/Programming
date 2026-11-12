Aqui estão os fundamentos de redes, explicados com o “porquê” de cada conceito.

### Conceitos base

- **Rede de computadores:** conjunto de dispositivos interligados que trocam dados seguindo regras comuns (protocolos). O objetivo é compartilhar recursos: arquivos, internet, impressoras, serviços.
- **Host / nó:** qualquer dispositivo conectado à rede (PC, celular, servidor, IoT).
- **Protocolo:** conjunto de regras de comunicação (ex.: IP, TCP, HTTP). Sem protocolo comum, dois dispositivos não se entendem.
- **Pacote / quadro:** unidade de dados. _Quadro (frame)_ na camada 2, _pacote_ na camada 3, _segmento_ na camada 4.

### Dispositivos

|Dispositivo|Camada OSI|Função|Endereçamento usado|
|---|---|---|---|
|**Hub**|1 (Física)|Repete o sinal recebido para todas as portas|Nenhum|
|**Switch**|2 (Enlace)|Encaminha quadros só para a porta do destino|MAC|
|**Roteador**|3 (Rede)|Conecta redes diferentes e escolhe o caminho|IP|
|**Access Point**|1 e 2|Estende a rede cabeada para Wi-Fi|MAC|

**Hub:** burro por design. Tudo que entra sai em todas as portas, gerando colisões e um único _domínio de colisão_. Está obsoleto.

**Switch:** aprende os MACs em uma _tabela CAM_ e envia o quadro só à porta correta. Cada porta é seu próprio domínio de colisão, mas todas continuam no mesmo _domínio de broadcast_ (a menos que haja VLANs).

**Roteador:** separa domínios de broadcast e liga redes distintas (ex.: sua LAN e a internet). Consulta a _tabela de roteamento_ para decidir o próximo salto. Costuma fazer também NAT e DHCP em casa.

**Access Point:** cria uma célula Wi-Fi conectada à rede cabeada. Difere do “roteador Wi-Fi” doméstico, que junta roteador + switch + AP em uma caixa só.

### Endereçamento

- **Endereço MAC:** identificador físico da placa de rede (48 bits, ex.: `AA:BB:CC:11:22:33`). Vale na rede local.
- **Endereço IP:** identificador lógico, hierárquico. **IPv4** tem 32 bits; **IPv6** tem 128 bits.
- **Máscara de sub-rede / CIDR:** define qual parte do IP é rede e qual é host (ex.: `192.168.1.0/24`).
- **Gateway padrão:** o roteador para onde vão os pacotes destinados a outras redes.
- **IP público x privado:** privados (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) não são roteados na internet.
- **Porta:** número (0-65535) que identifica o serviço/aplicação no host (ex.: 80 HTTP, 443 HTTPS, 22 SSH).

### Serviços essenciais

- **DHCP:** atribui IP, máscara, gateway e DNS automaticamente.
- **DNS:** traduz nomes (`google.com`) em IPs.
- **NAT:** traduz IPs privados em um IP público, economizando endereços IPv4.
- **ARP:** descobre o MAC a partir de um IP na rede local.
- **ICMP:** mensagens de controle e diagnóstico (`ping`, `traceroute`).

### Classificação por alcance

- **PAN:** pessoal (Bluetooth).
- **LAN:** local (casa, escritório).
- **MAN:** metropolitana (cidade).
- **WAN:** longa distância (a internet é a maior WAN).
- **WLAN:** LAN sem fio.

### Topologias

Barramento, anel, estrela (a mais comum hoje, com um switch central), malha (mesh) e híbrida.

### Modelos de camadas

**Modelo OSI (7 camadas):**

1. Física: bits, cabos, sinais
2. Enlace: quadros, MAC, switches
3. Rede: IP, roteamento
4. Transporte: TCP/UDP, portas
5. Sessão: controle de diálogo
6. Apresentação: formato, criptografia, compressão
7. Aplicação: HTTP, DNS, SMTP

![[Pasted image 20261111142240.png]]

**Modelo TCP/IP (4 camadas):** Acesso à rede, Internet, Transporte, Aplicação. É o modelo real usado na prática; o OSI é referência didática.

**Por que camadas?** Cada uma resolve um problema e esconde a complexidade das outras, o que permite trocar uma tecnologia (ex.: Wi-Fi por cabo) sem mexer no resto.

### TCP x UDP

- **TCP:** orientado à conexão, confiável, ordenado, com controle de fluxo. Usado em web, e-mail, SSH.
- **UDP:** sem conexão, rápido, sem garantia de entrega. Usado em streaming, VoIP, jogos, DNS.

### Meios de transmissão

- **Cabo par trançado** (Cat5e, Cat6): Ethernet comum, até 100 m por segmento.
- **Fibra óptica:** luz, longas distâncias e alta velocidade.
- **Wireless:** Wi-Fi (802.11), Bluetooth, celular.

### Outros conceitos importantes

- **VLAN:** divide logicamente um switch em redes separadas.
- **Firewall:** filtra tráfego conforme regras.
- **Largura de banda x latência:** capacidade (Mbps) x tempo de resposta (ms).
- **Domínio de colisão x broadcast:** colisão é limitado por switches; broadcast é limitado por roteadores (ou VLANs).
- **Cliente-servidor x P2P:** quem pede e quem serve x pares com papéis iguais.

Como você está na trilha de certificações (CCNA), os pontos mais cobrados são: tabela de domínios de colisão/broadcast, OSI x TCP/IP, sub-redes (CIDR) e o funcionamento de ARP/DHCP/DNS/NAT. Se quiser, posso aprofundar qualquer um deles, começando por sub-redes, que costuma ser o tema mais difícil.