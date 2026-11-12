# O que é um roteador?

Aprenda como os roteadores selecionam próximos saltos e encaminham pacotes IP entre redes.

Um roteador conecta domínios da camada de rede e encaminha pacotes IP entre eles. Um host Linux pode atuar como roteador quando o encaminhamento está habilitado e suas interfaces, rotas, descoberta de vizinhos e política de filtragem estão configuradas adequadamente.

## Roteamento e encaminhamento

O roteamento cria ou seleciona informações sobre prefixos acessíveis. O encaminhamento aplica essas informações a cada pacote: examina o destino, escolhe uma rota elegível e o próximo salto, reduz o limite de saltos e transmite por uma interface de saída.

Essas são questões separadas do plano de controle e do plano de dados. Uma rota pode existir enquanto a política de firewall bloqueia o encaminhamento, ou uma interface de encaminhamento pode estar ativa enquanto não existe uma rota válida.

## Tabelas de roteamento e rotas padrão

Uma rota associa um prefixo de destino a uma interface de saída, próximo salto, métrica, preferência de origem ou outros atributos. A correspondência de prefixo mais longo favorece uma rota elegível mais específica. Uma rota padrão, IPv4 `/0` ou IPv6 `::/0`, é a correspondência menos específica e só é usada quando nenhuma rota mais específica vence.

Se nenhuma rota elegível existir, o roteador descarta o pacote e pode gerar uma mensagem ICMP de destino inacessível. Uma rota padrão é opcional e não precisa apontar diretamente para a Internet pública.

## Tráfego local e roteado

Dois hosts na mesma sub-rede no enlace normalmente trocam quadros sem enviar o pacote IP por um roteador. Um roteador participa quando a seleção de rota o escolhe como próximo salto ou quando a topologia e a política impõem deliberadamente a passagem roteada.

Um “roteador” residencial geralmente combina roteador IP, switch Ethernet, ponto de acesso Wi-Fi, serviço DHCP, NAT e firewall. Cada função deve ser diagnosticada separadamente.

## Saltos e prevenção de loops

Um salto roteado é uma etapa de encaminhamento da camada de rede. O TTL do IPv4 e o Limite de Saltos do IPv6 são reduzidos em cada roteador, limitando os loops. A contagem de saltos não é uma métrica completa de distância nem de qualidade: os enlaces diferem em largura de banda, latência, perda, política e congestionamento.

