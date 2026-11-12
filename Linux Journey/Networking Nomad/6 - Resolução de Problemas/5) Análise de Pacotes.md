# Análise de Pacotes

Aprenda a capturar um trace de pacotes limitado e filtrado e analisá-lo com segurança usando tcpdump.

A captura registra tráfego visível em um ponto de observação. Ela revela trocas e tempos, mas também pode coletar credenciais, dados pessoais e tráfego de terceiros. Obtenha autorização, reduza o escopo, proteja arquivos e siga a política de retenção.

## Escolha do ponto de observação

Capture na interface e no namespace pelos quais o fluxo afetado realmente passa. Bridges, containers, VPNs, bonds, VLANs e offload mudam o que uma interface mostra. Use `ip route get` e `ip link` para identificar candidatas.

## Captura de um fluxo limitado

Capture até 100 pacotes sem resolver nomes, restritos a host e porta TCP:

```bash
$ sudo tcpdump -i enp1s0 -n -c 100 -w incident.pcap \
    'host 192.0.2.25 and tcp port 443'
```

`-i` seleciona interface, `-n` mantém números, `-c` limita pacotes, `-w` grava pcap e a expressão final filtra. Defina também um limite externo de tempo quando puder não haver tráfego.

## Leitura dos pacotes capturados

Analise o arquivo salvo sem modificá-lo:

```bash
$ tcpdump -n -tttt -r incident.pcap
```

Leia horários, protocolo, origem, destino, flags, sequência, acknowledgements e tamanho conforme o protocolo. O horário marca a observação neste host, não necessariamente o envio exato em outro. Sincronização importa ao correlacionar vários sistemas.

## Interpretação de ausência e criptografia

Nenhum pacote pode significar interface ou namespace errado, perda de captura, filtro estreito, offload, outra rota ou nenhum tráfego. Confira os contadores recebidos e descartados e reproduza um evento conhecido.

TLS e outras criptografias ocultam payloads, mas deixam metadados como endpoints, tempos, tamanhos, comportamento TCP e partes do handshake. Não tente descriptografia não autorizada nem colete chaves privadas casualmente.

## Proteção e compartilhamento da evidência

Guarde pcaps com permissões restritas, registre comando, host, interface, fuso, filtro e janela e calcule hash quando a integridade importar. Antes de compartilhar, minimize ou sanitize preservando campos necessários; payload e metadados podem identificar pessoas e sistemas.

