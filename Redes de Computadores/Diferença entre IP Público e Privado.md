**IP público** é o endereço visível na internet. É único globalmente, atribuído pelo provedor (ISP) e roteável entre redes diferentes. Qualquer servidor na internet pode te alcançar (ou ver) por ele.

**IP privado** é o endereço usado dentro de uma rede local (casa, empresa). Não é roteável na internet e pode se repetir em redes diferentes sem conflito.

**Faixas privadas (RFC 1918):**

- `10.0.0.0/8` (10.0.0.0 – 10.255.255.255)
- `172.16.0.0/12` (172.16.0.0 – 172.31.255.255)
- `192.168.0.0/16` (192.168.0.0 – 192.168.255.255)

Qualquer endereço fora dessas faixas (e de outras reservadas, como `127.0.0.0/8` e `169.254.0.0/16`) é público.

**Como se conectam:** o roteador faz **NAT** (Network Address Translation). Vários dispositivos com IPs privados saem para a internet compartilhando um único IP público. O roteador mantém uma tabela de traduções para saber a qual dispositivo devolver cada resposta.

**Diferenças práticas:**

- **Visibilidade:** o público é acessível de fora; o privado fica “escondido” atrás do NAT.
- **Custo/escassez:** IPv4 público é limitado e pago; privado é gratuito e abundante.
- **Segurança:** o NAT dá uma proteção básica contra conexões de entrada, mas não substitui firewall.
- **Acesso externo:** para expor um serviço interno, é preciso port forwarding, VPN ou túnel.

Nota: com **CGNAT** (comum em provedores brasileiros), o IP da WAN do seu roteador pode até ser privado/compartilhado (faixa `100.64.0.0/10`), e vários clientes dividem o mesmo IP público.

Para ver os dois: o privado aparece com `ip a` (Linux) ou `ipconfig` (Windows); o público, em sites como `ifconfig.me` ou via `curl ifconfig.me`.