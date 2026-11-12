# Protocolos de vetor de distância

Aprenda como protocolos de vetor de distância derivam rotas dos anúncios de vizinhos e limitam loops.

O roteamento por vetor de distância informa aos vizinhos quais destinos estão acessíveis e uma métrica que descreve a distância. Um roteador combina o anúncio de um vizinho com o custo até esse vizinho para derivar seu próprio caminho candidato.

## Aprendendo por meio dos vizinhos

Se o Roteador A anuncia uma distância de três até um prefixo e o Roteador B alcança A com custo um, B pode derivar a distância quatro por A. A informação descreve uma direção e uma métrica, não um mapa completo da topologia; por isso, a abordagem às vezes é chamada de roteamento por rumores.

## Loops e contagem até o infinito

Depois de uma falha, vizinhos podem anunciar equivocadamente uma rota de volta um para o outro, aumentando gradualmente sua métrica. Os protocolos atenuam isso com valores finitos de infinito, horizonte dividido, envenenamento de rotas, retorno envenenado, atualizações acionadas e temporizadores. Esses mecanismos reduzem o problema, mas não transformam toda mudança de topologia em uma convergência instantânea.

## Métricas e limites do RIP

O RIP usa contagem de saltos. Uma rota com métrica 16 é inacessível, portanto a maior métrica utilizável é 15. Isso limita a escalada dos loops, mas também restringe o diâmetro da rede. Menos saltos não significam necessariamente menor latência ou maior largura de banda.

O RIPv2 usa atualizações periódicas e acionadas e oferece suporte a informações CIDR. Normalmente, ele envia atualizações por multicast em vez de transmitir uma tabela inteira por broadcast em todas as circunstâncias. Autenticação e filtragem ainda exigem configuração deliberada.

## Avaliando uma rota aprendida

Verifique o estado dos vizinhos, os prefixos recebidos e anunciados, a métrica, o próximo salto, a instalação da rota e a acessibilidade do plano de dados. Uma rota pode ser válida dentro do RIP, mas perder para outra origem de rota segundo a política local de preferência.

