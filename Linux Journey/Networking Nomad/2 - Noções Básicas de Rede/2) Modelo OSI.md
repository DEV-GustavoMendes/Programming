# Modelo OSI

Aprenda como o modelo de referência OSI de sete camadas organiza as funções de rede e a linguagem de solução de problemas.

O modelo de Interconexão de Sistemas Abertos é uma estrutura de referência de sete camadas. Ele oferece aos engenheiros um vocabulário comum para localizar responsabilidades, interfaces e falhas; não é uma descrição literal de toda implementação.

## As sete camadas

Da mais baixa para a mais alta, as camadas OSI são:

1. Física: sinais, meios, conectores e transmissão de bits.
2. Enlace de Dados: quadros locais, endereçamento de enlace e acesso ao meio.
3. Rede: endereçamento lógico e encaminhamento entre redes.
4. Transporte: comunicação entre pontos de extremidade ou processos.
5. Sessão: gerenciamento de sessões de comunicação.
6. Apresentação: representação, transformação e codificação de dados.
7. Aplicação: serviços de rede usados pelas aplicações.

## Usando o modelo como vocabulário

Afirmações como “um loop de Camada 2” ou “uma porta de Camada 4” identificam uma área funcional sem explicar todos os detalhes da implementação. Um protocolo real pode atravessar limites, e criptografia, túneis, proxies ou sobreposições podem criar várias camadas aninhadas.

## Comparando OSI e TCP/IP

A suíte de protocolos da Internet e o modelo de referência OSI se desenvolveram por meio de histórias de padronização diferentes. O modelo TCP/IP prático geralmente agrupa as responsabilidades de sessão e apresentação do OSI em sua camada de aplicação e combina as questões físicas e de enlace de dados em uma camada de enlace ou de acesso à rede. Os mapeamentos são aproximados, não uma prova de que uma pilha foi implementada diretamente a partir da outra.

## Solucionando problemas entre camadas

Comece pelo sintoma e teste as suposições, em vez de verificar mecanicamente as camadas em ordem numérica. Uma falha na Web pode envolver o estado do enlace local, o roteamento IP, a acessibilidade do transporte, TLS, resolução de nomes, autenticação ou comportamento da aplicação. Evidências em uma camada podem orientar o próximo teste sem comprovar que as camadas superiores funcionam.

