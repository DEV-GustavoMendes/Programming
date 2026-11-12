# NAT

Aprenda como a tradução de origem, destino e portas modifica fluxos IPv4 e o estado das conexões.

A Tradução de Endereços de Rede reescreve campos de endereço e, frequentemente, portas de transporte à medida que os pacotes atravessam um dispositivo de tradução. Ela é amplamente usada para conectar redes IPv4 com endereços privados por meio de um conjunto menor de endereços roteáveis externamente.

## Tradução de origem

O NAT de origem substitui o endereço de origem de um pacote quando ele sai de uma rede. Implantações de muitos para um também traduzem portas de origem para que vários fluxos internos possam compartilhar um endereço externo. Essa forma que considera portas costuma ser chamada de NAPT, PAT ou mascaramento quando o endereço externo pode mudar.

O tradutor acompanha os mapeamentos para que os pacotes de resposta possam ser reescritos de volta ao ponto de extremidade interno original. Normalmente, ele encaminha o mesmo fluxo de transporte; não precisa abrir uma conexão de proxy separada como faria um proxy de aplicação.

## Tradução de destino

O NAT de destino reescreve o endereço ou a porta de destino, normalmente para publicar um serviço interno por meio de um ponto de extremidade externo. Uma regra de encaminhamento de portas pode mapear uma porta TCP externa para um endereço e uma porta internos diferentes. O tráfego de retorno precisa de uma tradução reversa consistente.

## NAT e política de firewall

NAT não é um firewall. Um tradutor com estado pode não ter um mapeamento para tráfego de entrada não solicitado, mas o encaminhamento explícito, a tradução de destino, a filtragem e a exposição da aplicação determinam o que está acessível. A política de segurança deve ser expressa e auditada com regras de firewall, serviços de privilégio mínimo e controles de ponta a ponta, em vez de ser deduzida da reescrita de endereços.

## Consequências operacionais

O NAT pode esgotar os mapeamentos de endereços e portas, complicar protocolos ponto a ponto, ocultar das aplicações as origens reais e exigir tratamento especial para protocolos que incorporam endereços. Se for necessário rastrear fluxos, os logs devem preservar os horários das traduções e os detalhes dos mapeamentos.

No Linux, as políticas modernas costumam ser configuradas com nftables e rastreamento de conexões. Inspecione o conjunto de regras real antes de alterá-lo:

```bash
$ sudo nft list ruleset
$ sudo conntrack -L
```

O segundo comando exige as ferramentas conntrack e privilégios. Alterações no conjunto de regras podem desconectar o acesso remoto; portanto, use recuperação por console, configuração atômica, validação e reversão.

