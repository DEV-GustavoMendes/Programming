# Protocolos de roteamento

Aprenda como protocolos de roteamento dinâmico trocam informações de acessibilidade e convergem para caminhos de encaminhamento utilizáveis.

Rotas estáticas são configuradas diretamente, enquanto protocolos de roteamento dinâmico trocam informações de acessibilidade e topologia para que os roteadores possam se adaptar. O aprendizado dinâmico reduz o trabalho manual, mas introduz estados de protocolo, limites de confiança, temporizadores e modos de falha que precisam ser monitorados.

## Plano de controle e plano de encaminhamento

Um protocolo de roteamento aprende candidatos em seu próprio banco de dados. O roteador seleciona rotas para uma base de informações de roteamento e instala próximos saltos utilizáveis em uma tabela de encaminhamento. O hardware ou o kernel então encaminha os pacotes a partir dessa tabela.

Uma adjacência de protocolo estabelecida não comprova que o prefixo desejado foi aprendido, selecionado, instalado ou permitido pela política de encaminhamento.

## Roteamento interior e exterior

Protocolos de gateway interior operam dentro de um domínio administrativo de roteamento. Alguns exemplos são RIP, OSPF e IS-IS. O BGP troca informações de acessibilidade controladas por política dentro e entre sistemas autônomos e é o protocolo de roteamento exterior da Internet.

As métricas possuem significados específicos de cada protocolo. Um custo OSPF, uma contagem de saltos RIP e um conjunto de atributos BGP não podem ser comparados como se compartilhassem uma escala numérica universal. As implementações usam preferência de rota ou distância administrativa para escolher entre origens antes ou em conjunto com a seleção específica do protocolo.

## Vetor de distância e estado de enlace

Protocolos de vetor de distância anunciam acessibilidade e distância por meio de vizinhos, derivando caminhos dos relatórios deles. Protocolos de estado de enlace formam adjacências, inundam informações sobre o estado dos enlaces dentro de um escopo, constroem um banco de dados da topologia e calculam árvores de caminhos mais curtos. Protocolos modernos incluem refinamentos que tornam incompletos os resumos simples por categoria.

## Convergência

Depois de uma mudança de topologia ou política, os roteadores a detectam, propagam informações de controle, calculam caminhos e atualizam o estado de encaminhamento. Convergência é o período e o resultado em que a rede alcança um roteamento estável e mutuamente utilizável para os destinos afetados. Ela não exige que todos os roteadores tenham uma tabela completa idêntica; funções e políticas podem diferir intencionalmente.

Durante a convergência, podem ocorrer perdas transitórias, loops ou buracos negros. Meça separadamente detecção, propagação, cálculo e instalação e verifique com sondagens do plano de dados.

