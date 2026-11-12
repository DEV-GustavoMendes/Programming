### Nível 1: Fundamentos

**1. Rede do escritório pequeno**

- **Cenário:** 3 PCs e 1 switch em `192.168.1.0/24`, com IPs estáticos.
- **Tarefa:** configure os IPs, teste o `ping` entre os PCs, depois veja `show mac address-table` no switch.
- **Desafio:** no Simulation Mode, acompanhe o ARP request (broadcast) e o ARP reply (unicast).
- **Por quê:** é aqui que o aprendizado do switch (learn, flood, forward) deixa de ser teoria.

**2. Hardening básico de um switch**

- **Cenário:** um switch novo chegou e precisa de configuração segura antes de entrar na rede.
- **Tarefa:** configure `hostname`, `enable secret`, `banner motd`, senha de console, `service password-encryption`, SSH (domain-name + `crypto key generate rsa`), `transport input ssh` nas VTYs e salve com `copy running-config startup-config`.
- **Por quê:** quase toda auditoria de segurança cobra isso, e a prova também.

**3. Duas redes, um roteador**

- **Cenário:** o escritório tem as redes Administrativo e Suporte, separadas.
- **Tarefa:** conecte duas LANs a um roteador, configure o gateway em cada host e faça os dois lados se comunicarem.
- **Desafio:** erre de propósito o gateway de um PC e observe o sintoma.
- **Por quê:** o erro de gateway é uma das causas mais comuns de “não sai da rede” na vida real.

**4. DHCP no roteador**

- **Cenário:** chega de configurar IP em cada PC.
- **Tarefa:** crie um pool com `ip dhcp pool`, `ip dhcp excluded-address` (reserve o gateway e os IPs de servidores), defina `default-router` e `dns-server`, e coloque os PCs em DHCP.
- **Por quê:** confirma o DORA na prática (use o Simulation Mode para ver as 4 mensagens).

---

### Nível 2: Camada 2 e segmentação

**5. Empresa com VLANs**

- **Cenário:** Vendas, TI e Financeiro estão na mesma rede e a diretoria quer separação.
- **Tarefa:** com 2 switches, crie as VLANs 10/20/30, configure portas _access_ e um _trunk_ entre os switches, com VLAN nativa 99.
- **Validação:** hosts da mesma VLAN se comunicam entre os switches, e VLANs diferentes não se enxergam. Use `show vlan brief` e `show interfaces trunk`.
- **Por quê:** a VLAN reduz o domínio de broadcast e isola departamentos sem comprar mais equipamentos.

**6. Router-on-a-stick**

- **Cenário:** agora as VLANs precisam conversar de forma controlada.
- **Tarefa:** crie subinterfaces no roteador (`encapsulation dot1Q <vlan>`), uma por VLAN, cada uma com o gateway da sua VLAN.
- **Por quê:** mostra que a VLAN separa na camada 2, mas só a camada 3 liga as redes.

**7. DHCP centralizado com Relay**

- **Cenário:** o servidor DHCP fica numa VLAN de servidores, e os usuários estão em outras VLANs.
- **Tarefa:** configure o servidor e use `ip helper-address` nas subinterfaces.
- **Por quê:** broadcast não atravessa roteador. É a razão do relay existir e uma pergunta clássica.

**8. Planejamento com VLSM**

- **Cenário:** você recebeu `192.168.10.0/24` para uma empresa com TI (50 hosts), Vendas (25), Financeiro (10), Gerência (5) e 2 links ponto a ponto entre roteadores.
- **Tarefa:** divida o bloco em VLSM **no papel primeiro**, com a tabela completa, e só depois implemente.
- **Por quê:** é a habilidade que mais cai na prova, e aqui você vê o resultado funcionando.

**9. Controle do STP**

- **Cenário:** 3 switches em triângulo (redundância) e ninguém sabe qual é o root.
- **Tarefa:** descubra o root com `show spanning-tree`, force um switch de core como root (`spanning-tree vlan X priority 4096` ou `root primary`), e ative **PortFast + BPDU Guard** nas portas de usuário.
- **Por quê:** um switch qualquer virando root por ter o menor MAC é um problema real e comum. O BPDU Guard protege contra alguém plugar um switch na tomada da parede.

**10. EtherChannel**

- **Cenário:** o link entre dois switches está saturado.
- **Tarefa:** agregue 2 ou 3 links com LACP (`channel-group X mode active`) e confirme com `show etherchannel summary`.
- **Desafio:** derrube um dos links físicos e veja o tráfego continuar.

---

### Nível 3: Roteamento

**11. Matriz e filiais com rotas estáticas**

- **Cenário:** uma matriz e duas filiais ligadas por roteadores.
- **Tarefa:** configure rotas estáticas para todas as redes e uma **rota default** na filial que sai para a Internet.
- **Desafio:** substitua várias rotas por uma **rota sumarizada**.
- **Por quê:** estática é simples e previsível, mas não escala. Sentir isso ao digitar a 15ª rota é o motivo de estudar protocolos dinâmicos.

**12. Link de backup com rota flutuante**

- **Cenário:** a filial tem um link principal e um 4G de reserva.
- **Tarefa:** configure duas rotas default, a de backup com distância administrativa maior (ex.: `ip route 0.0.0.0 0.0.0.0 <next-hop> 200`). Desligue a interface principal (`shutdown`) e veja a rota de backup assumir em `show ip route`.
- **Por quê:** é redundância barata e muito usada em filiais pequenas.

**13. OSPF de área única**

- **Cenário:** a rede cresceu e a manutenção manual virou um problema.
- **Tarefa:** com 4 roteadores, configure OSPF (Área 0), defina `router-id`, use `passive-interface` nas LANs e `default-information originate` no roteador de borda.
- **Validação:** `show ip ospf neighbor`, `show ip route ospf`.
- **Por quê:** é o protocolo principal do CCNA. Passive-interface evita fazer vizinhança com PCs.

**14. OSPF em rede multiacesso**

- **Cenário:** 3 roteadores ligados ao mesmo switch.
- **Tarefa:** observe a eleição de DR/BDR, force o resultado com `ip ospf priority`, e ajuste o custo com `auto-cost reference-bandwidth` para refletir links de 1 Gbps ou mais.
- **Por quê:** o custo padrão trata Fast Ethernet e Gigabit como iguais, o que gera caminhos ruins.

**15. Core com switch L3**

- **Cenário:** o router-on-a-stick virou gargalo.
- **Tarefa:** troque pelo roteamento inter-VLAN em um **switch L3** (`ip routing`, SVIs) e anuncie as redes em OSPF para o roteador de borda.
- **Por quê:** é como as redes de campus reais funcionam hoje.

---

### Nível 4: Serviços e segurança

**16. NAT e PAT**

- **Cenário:** a empresa tem 1 IP público e 100 máquinas internas, e o servidor web interno precisa ser acessível de fora.
- **Tarefa:** configure **PAT (overload)** para a LAN e **NAT estático** para o servidor web. Defina `ip nat inside` e `outside` e confira em `show ip nat translations`.
- **Desafio:** inverta inside/outside de propósito e analise a falha.
- **Por quê:** é o erro mais comum de iniciantes.

**17. ACLs na prática**

- **Cenário:** a rede de visitantes não pode acessar a rede de TI, mas precisa de Internet.
- **Tarefa:**
    - Uma ACL **extended** que bloqueia Visitantes → TI e libera o resto.
    - Uma ACL **standard** limitando o acesso SSH aos roteadores a um único IP de gerência (`access-class` na VTY).
- **Desafio:** coloque a regra _permit_ genérica antes da _deny_ e veja a ordem importar, depois lembre do **deny implícito**.
- **Por quê:** ACL na ordem errada ou na interface errada é o erro mais frequente.

**18. Defesa de camada 2**

- **Cenário:** um funcionário plugou um roteador doméstico na rede e passou a distribuir IPs falsos.
- **Tarefa:** ative **DHCP Snooping** (porta do servidor como _trusted_), **Port Security** (limite de MACs, violação `shutdown`) e **DAI**.
- **Validação:** conecte um “servidor DHCP falso” e veja o bloqueio.
- **Por quê:** é um ataque real e comum. Você treina detecção e contenção.

**19. Wi-Fi corporativo e de visitantes**

- **Cenário:** a empresa quer Wi-Fi para funcionários e outro para visitantes.
- **Tarefa:** configure dois SSIDs com **WPA2** em APs e mapeie cada um para uma VLAN diferente, com a ACL do exercício 17 isolando a de visitantes.
- **Por quê:** junta VLANs, trunk, segurança e wireless num caso só.

**20. Monitoramento: NTP, Syslog e SNMP**

- **Cenário:** um incidente aconteceu e os logs dos equipamentos têm horários diferentes.
- **Tarefa:** configure um servidor NTP, envie logs ao servidor Syslog (`logging host`) e habilite SNMP com comunidade somente leitura.
- **Por quê:** sem relógio sincronizado não se correlacionam eventos entre dispositivos.

---

### Nível 5: Projeto integrado e troubleshooting

**21. Projeto final: matriz e duas filiais**

- **Cenário:** você é o responsável pela rede de uma empresa com sede e 2 filiais.
- **Requisitos:**
    - Sede com topologia de 3 camadas, VLANs por departamento e STP com root definido.
    - Servidor DHCP centralizado com relay, e DNS interno.
    - OSPF entre todas as localidades, e uma saída para a Internet com PAT.
    - ACLs de segurança, SSH em todos os equipamentos e Wi-Fi com rede de visitantes.
    - **Plano de endereçamento VLSM** e diagrama documentados.
- **Por quê:** a configuração isolada é a parte fácil. Projetar e integrar é o que diferencia quem decorou de quem entendeu.

**22. Troubleshooting às cegas**

- **Cenário:** alguém configurou uma rede “quebrada” e você precisa consertar sem saber onde.
- **Tarefa:** pegue uma topologia pronta (um dos seus labs) e peça que outra pessoa introduza falhas. Você diagnostica **de baixo para cima** (camada 1 a 7). Falhas boas para praticar:
    - VLAN criada, mas ausente na lista permitida do trunk.
    - VLAN nativa diferente nas duas pontas do trunk.
    - Máscara ou gateway errados em um host.
    - Falta de `ip helper-address`.
    - Hello/Dead timers diferentes entre vizinhos OSPF.
    - ACL aplicada na interface ou direção errada.
    - NAT com inside/outside invertidos.
    - Duplex mismatch entre duas portas.
- **Por quê:** a prova e o trabalho cobram diagnóstico, não só configuração. Quebrar e consertar fixa mais do que acertar de primeira.

**23. Dual-stack com IPv6**

- **Cenário:** o provedor passou a entregar IPv6 e a rede precisa conviver com os dois protocolos.
- **Tarefa:** habilite `ipv6 unicast-routing`, atribua endereços globais e link-local, configure **SLAAC** nos hosts e teste com `ping` IPv6. Opcionalmente, rode OSPFv3.
- **Por quê:** o IPv6 aparece bastante na prova, e ver o ND substituindo o ARP ajuda a fixar a diferença.