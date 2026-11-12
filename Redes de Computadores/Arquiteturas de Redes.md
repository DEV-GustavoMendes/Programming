Vou cobrir as arquiteturas mais cobradas e usadas na prática, do nível lógico ao físico.

![[Pasted image 20261111141745.png]]

**1. Cliente-servidor**  
Clientes pedem serviços e servidores centralizados respondem.

- **Prós:** gestão, segurança e backup centralizados; fácil de escalar o servidor.
- **Contras:** ponto único de falha; o servidor pode virar gargalo.
- **Usos:** web, e-mail, bancos de dados, APIs.

**2. Peer-to-peer (P2P)**  
Cada nó é cliente e servidor ao mesmo tempo.

- **Prós:** sem ponto central de falha; escala bem com mais participantes.
- **Contras:** segurança e controle difíceis; desempenho irregular.
- **Usos:** BitTorrent, blockchain, redes pequenas domésticas.

**3. Hierárquica de 3 camadas (núcleo, distribuição, acesso)**  
Modelo clássico da Cisco: o acesso conecta os dispositivos, a distribuição aplica políticas e roteamento, e o núcleo transporta tráfego em alta velocidade.

- **Prós:** modular, previsível, fácil de isolar problemas e aplicar políticas.
- **Contras:** custo alto; o tráfego leste-oeste (entre servidores) percorre caminhos longos.
- **Usos:** campi, redes corporativas médias e grandes.

**4. Spine-leaf**  
Todo switch _leaf_ (que liga os servidores) se conecta a todos os _spines_. Assim, qualquer par de servidores fica a no máximo dois saltos de distância.

- **Prós:** latência baixa e previsível; ótimo para tráfego leste-oeste; cresce adicionando switches.
- **Contras:** muitos cabos e portas; complexidade de projeto.
- **Usos:** data centers, nuvem, virtualização.

**5. Mesh (malha) e SD-WAN**  
Mesh conecta nós com múltiplos caminhos. SD-WAN usa software para escolher dinamicamente o melhor link entre filiais (MPLS, internet, 4G/5G).

- **Prós:** alta redundância e resiliência; SD-WAN reduz custo de links dedicados.
- **Contras:** mesh total é caro e complexo; SD-WAN depende de bom controle de políticas.
- **Usos:** backbones, redes com várias filiais, ambientes críticos.

**Como escolher:** pense em três eixos: _onde está o tráfego_ (usuário→servidor ou servidor↔servidor), _quanto de falha você tolera_ (redundância custa dinheiro) e _quanto precisa crescer_. Por isso campus costuma usar 3 camadas, data center usa spine-leaf, e filiais usam SD-WAN.