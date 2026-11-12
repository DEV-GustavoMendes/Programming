# Protocolos de estado de enlace

Aprenda como protocolos de estado de enlace formam adjacências, inundam informações de topologia e calculam caminhos.

Protocolos de estado de enlace descrevem enlaces e prefixos locais, distribuem essas descrições por um escopo de roteamento e permitem que cada roteador calcule caminhos a partir de um banco de dados da topologia. OSPF e IS-IS são exemplos comuns.

## Formando adjacências

Os roteadores descobrem vizinhos compatíveis e formam adjacências de protocolo de acordo com o tipo de interface, a área, os temporizadores, a autenticação e outros parâmetros. Ver pacotes hello não garante uma adjacência completa; configurações incompatíveis podem interromper a máquina de estados antes.

## Inundando informações de estado de enlace

Cada roteador origina anúncios sobre seu estado relevante. Os vizinhos inundam de forma confiável as informações mais recentes por toda a área ou domínio definido, em vez de manter as atualizações apenas entre o par de vizinhos original. Mecanismos de sequência e envelhecimento diferenciam informações atuais e removem estados obsoletos.

## Calculando os caminhos mais curtos

Depois de construir um banco de dados de estado de enlace, um roteador executa um algoritmo de caminho mais curto primeiro, normalmente o algoritmo de Dijkstra, tendo a si próprio como raiz. O OSPF soma os custos das interfaces; a política e as regras de custos iguais influenciam quais resultados são instalados.

“Mais curto” significa o menor custo do protocolo, não necessariamente menos roteadores nem a menor latência medida pela aplicação. O projeto dos custos deve refletir a intenção operacional.

## Áreas e convergência

As áreas OSPF limitam o escopo da inundação de topologia e dos cálculos, e a Área 0 atua como backbone no projeto normal entre áreas. A sumarização e os tipos de área podem fazer intencionalmente com que roteadores diferentes tenham detalhes distintos no banco de dados.

Depois de uma mudança de enlace, a detecção, a inundação de anúncios, o cálculo SPF, a instalação das rotas e a recuperação do encaminhamento levam tempo individualmente. Uma convergência mais rápida do que em um projeto simples de vetor de distância é possível, mas não automática em toda falha ou configuração.

