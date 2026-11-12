# Caminho de um pacote

Aprenda como rotas, descoberta de vizinhos, quadros e roteadores transportam um pacote IP por um caminho.

O caminho de um pacote é uma sequência de decisões locais. O host de origem, cada roteador e o destino aplicam seus próprios estados de roteamento, vizinhança, filtragem e protocolo; normalmente, nenhum ponto de extremidade conhece antecipadamente todas as decisões internas.

## Enviando para um destino no enlace

Para um destino abrangido por uma rota conectada, a origem seleciona uma interface e um IP de origem. Em seguida, resolve o endereço de enlace do destino — ARP para IPv4 sobre Ethernet ou Descoberta de Vizinhos para IPv6 — e envia um quadro que transporta o pacote IP. Um switch pode encaminhar o quadro sem se tornar um salto IP.

## Enviando por um gateway

Para um destino fora do enlace, a rota selecionada identifica um roteador de próximo salto. O destino IP continua sendo o ponto de extremidade remoto, enquanto o destino do quadro local é o endereço de enlace do gateway. O host resolve o gateway, não o servidor remoto, em seu enlace local.

## Processamento em cada roteador

Um roteador remove o enquadramento de enlace recebido, valida e processa o cabeçalho IP, reduz o TTL ou o Limite de Saltos, consulta o destino, aplica a política e cria um novo enquadramento para o enlace de saída. No IPv4, o processamento da soma de verificação do cabeçalho reflete a alteração do TTL. Se o campo de saltos chegar a zero, o roteador descarta o pacote e pode retornar uma mensagem ICMP de tempo excedido.

## Considerando dispositivos intermediários e MTU

O roteamento comum preserva os endereços IP de origem e destino, mas o NAT pode reescrevê-los, e túneis podem encapsular o pacote original. Firewalls podem descartar o tráfego silenciosamente ou rejeitá-lo. As MTUs dos enlaces também diferem; roteadores IPv4 às vezes podem fragmentar pacotes, enquanto roteadores IPv6 não fragmentam pacotes encaminhados e dependem da Descoberta da MTU do Caminho.

## Acompanhando o caminho de retorno

O destino realiza sua própria consulta de rota para a resposta. O caminho de retorno pode usar roteadores diferentes devido a políticas de roteamento, balanceamento de carga ou falhas. Firewalls com estado e NAT precisam considerar o fluxo observado; por isso, a assimetria pode importar operacionalmente mesmo quando é permitida pelo IP.

