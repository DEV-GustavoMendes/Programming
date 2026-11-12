# Modelo TCP/IP

Aprenda como as camadas de aplicação, transporte, Internet e enlace cooperam no modelo TCP/IP.

O modelo TCP/IP organiza os protocolos usados pelos hosts da Internet em camadas funcionais. Uma forma comum de quatro camadas usa Aplicação, Transporte, Internet e Enlace. Alguns modelos didáticos separam o meio físico da camada de enlace e, por isso, apresentam cinco camadas.

## Camada de aplicação

Os protocolos de aplicação definem mensagens e comportamentos para serviços como HTTP, DNS, SSH e SMTP. Essa camada também inclui muitas responsabilidades de representação e sessão que o modelo OSI apresenta separadamente.

## Camada de transporte

Os protocolos de transporte fornecem comunicação entre pontos de extremidade das aplicações. O TCP oferece um fluxo de bytes confiável e ordenado, com controle de congestionamento e de fluxo. O UDP fornece datagramas independentes, sem as garantias de conexão, ordenação ou retransmissão do TCP. Os números de porta ajudam a identificar pontos de extremidade do transporte, mas um número de porta por si só não comprova qual aplicação está escutando.

## Camada de Internet

O Protocolo de Internet transporta pacotes usando endereços IP de origem e destino. Os roteadores examinam as informações de roteamento e reduzem os limites de saltos enquanto encaminham os pacotes em direção ao destino. O ICMP comunica informações de controle e erro para a operação do IP. A entrega continua sendo de melhor esforço; as camadas superiores ou as aplicações cuidam de qualquer recuperação necessária.

## Camada de enlace e encapsulamento

A camada de enlace envia um pacote IP através de um enlace local usando Ethernet, Wi-Fi, um protocolo ponto a ponto ou outra tecnologia. À medida que os dados da aplicação descem, cada camada acrescenta as informações necessárias ao seu escopo. No receptor, as camadas validam e removem seu próprio encapsulamento antes de entregar os dados para cima.

Os cabeçalhos de enlace normalmente mudam em cada salto roteado; as conversas de transporte e aplicação são de ponta a ponta, a menos que um dispositivo intermediário as encerre ou transforme.

