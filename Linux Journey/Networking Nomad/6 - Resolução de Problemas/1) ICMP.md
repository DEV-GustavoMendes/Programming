# ICMP

Aprenda como o ICMP relata erros IP, apoia diagnósticos e permite comportamentos essenciais de IPv4 e IPv6.

Internet Control Message Protocol transporta informações de controle, erro e diagnóstico junto ao IP. ICMP para IPv4 e ICMPv6 são protocolos relacionados, mas distintos, com números de tipos e responsabilidades diferentes.

## Tipos, códigos e checksums

Uma mensagem ICMP tem tipo, um código mais específico quando aplicável e checksum. Mensagens de erro normalmente incluem parte do pacote causador para que o remetente associe o erro a um fluxo.

## Mensagens de eco e erro

No ICMPv4, Echo Request é tipo 8 e Echo Reply tipo 0. Destination Unreachable é tipo 3 e Time Exceeded tipo 11. ICMPv6 usa outros números; identifique a família antes de interpretar uma captura.

## Path MTU e ICMP essencial

ICMP não é apenas tráfego opcional de ping. Erros IPv4 de fragmentação necessária e mensagens ICMPv6 Packet Too Big sustentam Path MTU Discovery. ICMPv6 também transporta Neighbor Discovery e Router Advertisements. Bloquear tudo pode criar black holes e quebrar IPv6.

Filtre por tipo necessário, direção, taxa e escopo, não por regra total. Atacantes podem falsificar ICMP; valide o pacote citado e compare rotas e capturas locais.

## Interpretação do silêncio

Ausência de resposta pode significar filtro, rate limit, rota assimétrica, falta de rota de retorno, host desligado ou dispositivo que ignora a mensagem. Um erro também pode vir de um dispositivo intermediário.

