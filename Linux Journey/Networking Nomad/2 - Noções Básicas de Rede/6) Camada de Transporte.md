# Camada de transporte

Aprenda como TCP e UDP usam portas e diferentes semânticas de entrega entre pontos de extremidade das aplicações.

A camada de transporte conecta pontos de extremidade das aplicações através de uma rede IP. Tanto o TCP quanto o UDP usam números de porta de 16 bits, mas expõem às aplicações modelos de comunicação e garantias diferentes.

## Portas e sockets

Uma porta de destino ajuda o sistema operacional a entregar o tráfego a um socket em escuta. Uma conexão ou fluxo é identificado por mais de uma porta: importam o protocolo, os endereços de origem e destino e as portas de origem e destino. Por isso, a mesma porta de servidor pode atender muitos clientes simultaneamente.

## Fluxos de bytes TCP

O TCP fornece um fluxo de bytes confiável e ordenado enquanto a conexão permanece viável. Ele usa números de sequência, confirmações, retransmissão, controle de fluxo e controle de congestionamento. O TCP não preserva os limites das mensagens da aplicação: uma gravação pode chegar por meio de várias leituras, ou uma leitura pode retornar várias gravações. As aplicações definem seu próprio enquadramento.

Confiabilidade não significa entrega absoluta. Uma conexão pode atingir o tempo limite, ser redefinida ou falhar, e uma confirmação não comprova que uma aplicação gravou os dados de forma durável.

## O handshake TCP

Uma conexão TCP normal começa com um handshake de três vias:

1. O iniciador envia `SYN` com suas informações iniciais de sequência.
2. O ouvinte responde com `SYN-ACK`, suas próprias informações de sequência e uma confirmação.
3. O iniciador retorna `ACK`.

Isso estabelece o estado do transporte nos dois pontos de extremidade. Não autentica o servidor da aplicação nem comprova que a operação solicitada da aplicação terá sucesso.

## Datagramas UDP

O UDP preserva os limites dos datagramas e fornece detecção de erros baseada em soma de verificação, mas não oferece o estado de conexão, a ordenação, a retransmissão, o controle de fluxo nem o controle de congestionamento do TCP. Uma aplicação pode acrescentar por conta própria qualquer comportamento necessário de confiabilidade ou congestionamento. O UDP não é automaticamente mais rápido; o desempenho depende do projeto do protocolo, da carga de trabalho, do caminho e da implementação.

## Inspecionando pontos de extremidade do transporte

Use `ss` para inspecionar sockets em escuta e conectados sem alterá-los:

```bash
$ ss -lntup
$ ss -tn state established
```

Os detalhes dos processos podem exigir privilégios. Um socket em escuta comprova a prontidão local apenas no limite do transporte; firewall, roteamento, família de endereços, TLS e integridade da aplicação ainda precisam dos testes adequados.

