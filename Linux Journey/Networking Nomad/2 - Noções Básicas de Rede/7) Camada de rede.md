# Camada de rede

Aprenda como endereçamento IP, prefixos, tabelas de roteamento e limites de saltos transportam pacotes entre redes.

A camada de rede fornece endereçamento lógico e entrega de pacotes por melhor esforço através de redes interconectadas. Na suíte de protocolos da Internet, IPv4 e IPv6 transportam pacotes enquanto os roteadores escolhem o próximo salto em direção a cada destino.

## Pacotes IP

Um cabeçalho IP inclui endereços de origem e destino, além dos campos necessários ao encaminhamento e ao processamento do protocolo. A carga útil geralmente contém um segmento TCP, um datagrama UDP ou uma mensagem ICMP. O IP não garante chegada, ordem nem ausência de duplicatas.

## Prefixos e sub-redes

Um endereço e um comprimento de prefixo definem quais bits iniciais formam um prefixo de rede. Os hosts usam essas informações e suas rotas para decidir se um destino está no enlace ou exige um roteador de próximo salto. Uma sub-rede é um intervalo de endereços sob um prefixo e uma política; sub-redes não estão automaticamente conectadas a todas as outras sub-redes.

## Decisões de roteamento

O Linux consulta as políticas e as tabelas de roteamento para selecionar uma interface de saída, um próximo salto e as informações de origem preferidas. Entre rotas igualmente elegíveis, normalmente é preferido o prefixo correspondente mais específico. Inspecione a decisão real para um destino com:

```bash
$ ip route get 203.0.113.10
```

Essa é uma consulta de rota local, não uma prova de que todos os roteadores seguintes possuem uma rota funcional nem de que o destino aceita tráfego.

## Limites de saltos e alterações no encaminhamento

Cada pacote IPv4 possui um TTL e cada pacote IPv6, um Limite de Saltos. Um roteador reduz esse valor; quando ele chega a zero, o roteador descarta o pacote e pode enviar um erro ICMP. Isso impede que loops de encaminhamento circulem indefinidamente.

Normalmente, os roteadores preservam os endereços IP de ponta a ponta, mas NAT, túneis, proxies e outros dispositivos intermediários podem transformar ou encapsular os pacotes. Os cabeçalhos da camada de enlace mudam em cada salto roteado de qualquer forma.

