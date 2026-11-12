## PARTE D – MACETES E TRUQUES PARA A PROVA

Em **camadas OSI**, se a questão fala em “bits, sinais, conectores”, é Física. “MAC, quadro, switch”, Enlace. “IP, pacote, roteador”, Rede. “Portas, fim a fim, segmento”, Transporte. Hub é L1, switch é L2, roteador é L3, e firewall de aplicação é L7. Cuidado com “switch L3” e “roteador”: ambos roteiam.

Em **TCP × UDP**, voz, vídeo ao vivo, DNS e DHCP são tipicamente UDP, e web, e-mail, FTP e SSH são TCP. O handshake do TCP é de três vias, e o encerramento é de quatro. Sempre que a questão disser “confiável”, pense em TCP.

Em **endereçamento**, memorize as faixas privadas (10/8, 172.16/12, 192.168/16) e use o macete do “salto” para sub-redes. Lembre que a quantidade de hosts é 2ⁿ − 2, e em IPv6 não existe broadcast nem checksum no cabeçalho.

Em **segurança**, o AH não cifra e o ESP cifra. O modo túnel do IPsec encapsula o pacote inteiro. O L2TP sozinho não cifra, e o PPTP é inseguro. O RADIUS cifra só a senha, e o TACACS+ cifra todo o pacote. O IDS detecta e o IPS bloqueia. O firewall stateful lembra das conexões. A assinatura digital garante integridade, autenticidade e não repúdio, mas **não** confidencialidade. A confidencialidade vem da criptografia.

Em **Wi-Fi**, o 802.11 usa CSMA/**CA** e o Ethernet cabeado usa CSMA/**CD**. WEP é quebrado, WPA2 usa AES-CCMP e WPA3 usa SAE. Os canais sem sobreposição em 2,4 GHz são 1, 6 e 11.

Em **VoIP**, o G.711 é 64 kbps, o G.729 é 8 kbps, o SIP sinaliza, o RTP leva a mídia e o RTCP leva o feedback. A recomendação de atraso é de até 150 ms.

Em **modulação**, o que cresce a eficiência por símbolo é o aumento do número de níveis, mas isso exige mais SNR. Em Nyquist, a taxa máxima é 2B·log₂M, e em Shannon, B·log₂(1+SNR).

Uma **estratégia de leitura** que ajuda muito: cuidado com palavras absolutas (“sempre”, “nunca”, “somente”, “apenas”) nas assertivas, que costumam tornar a afirmação falsa. Leia as alternativas “troca de conceito”, em que a banca troca uma propriedade (por exemplo, atribui a AH a confidencialidade, ou diz que o switch separa domínios de broadcast).

---

## PARTE E – ANÁLISE DO QUE MAIS CAI (2020–2026)

Primeiro, os fatos que consegui confirmar. O concurso da ESFCEx para o Quadro Complementar de Oficiais inclui a especialidade de Informática, e o mais recente teve as provas aplicadas em 12 de julho de 2026. As provas de Informática de 2025 e 2026 estão catalogadas no QConcursos com a Vunesp como banca. As provas de 2021, 2022 e 2023 também estão publicadas no próprio site da ESFCEx, em formato de caderno de 60 questões objetivas, no caso de 2022. Um cuidado: não consegui confirmar qual foi a banca em 2020 e nos anos anteriores a 2025, nem a distribuição exata de questões por tema. **Por isso, o que segue é uma estimativa qualitativa**, e eu recomendo que você a valide. [Exército: concurso da ESFCEx com 227 vagas encerra inscrições nesta sexta (12) +2](https://www.diariodepernambuco.com.br/economia/2026/06/11716252-exercito-concurso-da-esfcex-com-227-vagas-encerra-inscricoes-nesta-sexta-12.html)

**Como validar rapidamente:** baixe os cadernos de 2021, 2022 e 2023 no site da ESFCEx e os de 2025 e 2026 no QConcursos, e faça uma planilha simples marcando, para cada questão de redes e segurança, o subtópico. Em duas ou três horas você terá a distribuição real, e vale mais do que qualquer ranking genérico.

**Estimativa de incidência, do mais para o menos provável:**

1. **Modelos OSI e TCP/IP e protocolos de aplicação e transporte** (TCP, UDP, DNS, DHCP, HTTP, SMTP/POP/IMAP, portas): é o núcleo que nenhum candidato pode errar. É o tema mais recorrente em qualquer prova de redes de nível superior.
2. **Endereçamento IP, sub-redes, máscaras, CIDR, NAT e IPv6:** costuma aparecer em questão de cálculo ou de conceito. Treine a conta do salto.
3. **Segurança de redes: firewall, IDS/IPS, VPN, IPsec, TLS, criptografia e ataques:** o volume de segurança em provas de informática militares costuma ser alto, e na prova da ESFCEx o tópico de segurança também aparece em outro item do edital (certificação digital, criptografia, normas ABNT), o que **reforça** a incidência. Os subtópicos mais prováveis são firewall (tipos e DMZ), VPN/IPsec (AH, ESP, túnel e transporte), ataques de negação de serviço, ARP/DNS spoofing e tipos de IDS.
4. **Roteamento:** diferenças entre vetor de distância e estado de enlace, RIP, OSPF e BGP, e a distância administrativa.
5. **Camada de enlace e redes locais:** Ethernet, CSMA/CD, switch, VLAN 802.1Q, STP e topologias.
6. **Redes sem fio:** padrões 802.11, CSMA/CA, segurança (WEP, WPA2, WPA3) e Bluetooth.
7. **Transmissão e meios físicos:** modos simplex, half e full duplex, tipos de cabos, fibras, categorias de UTP, multiplexação e codificação.
8. **VoIP e streaming:** SIP, H.323, RTP, codecs e QoS para voz. Costuma ter uma questão por prova e, quando cai, é conceitual.
9. **QoS:** DiffServ, IntServ, filas e parâmetros. Ocorrência menor, mas com cobrança de detalhe.
10. **Modulação e teoria de sinais:** Nyquist e Shannon, QAM e PCM. É o tema mais “teórico” e o menos frequente, mas rende pontos de quem se prepara, justamente porque muitos candidatos o ignoram.
11. **Monitoramento e análise de tráfego:** SNMP, NetFlow, Wireshark e SPAN.

**Recomendação de estudo:** se o seu tempo é limitado, dedique cerca de metade ao bloco dos itens 1 a 3, um quarto aos itens 4 a 6 e o restante ao resto. Resolva questões por tópico (o QConcursos permite filtrar por assunto e por banca) e priorize as da própria Vunesp, para se habituar ao estilo, que costuma privilegiar enunciados conceituais com alternativas longas, em vez de questões do tipo certo/errado.