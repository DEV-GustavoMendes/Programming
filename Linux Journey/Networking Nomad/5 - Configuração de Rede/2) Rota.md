# Rota

Aprenda a inspecionar, adicionar, substituir, excluir e verificar rotas Linux com segurança usando ip.

Rotas manuais alteram como o kernel escolhe a interface de saída e o próximo salto. Um erro pode desconectar o host ou redirecionar tráfego sensível; examine a rota efetiva, o proprietário da configuração e o caminho de recuperação antes de mudar o estado.

## Inspeção da decisão atual

Registre as rotas relevantes e pergunte ao kernel como ele alcança o destino:

```bash
$ ip -4 route show
$ ip route get 192.168.2.25
```

Examine também regras de política e tabelas alternativas quando existirem. A consulta é evidência local e não envia tráfego.

## Adição ou substituição de uma rota

Adicione uma rota para o prefixo canônico por um próximo salto alcançável:

```bash
$ sudo ip route add 192.168.2.0/23 via 10.11.12.3 dev enp1s0
```

O gateway deve ser alcançável pelo link relevante ou por um projeto on-link explícito. `add` falha quando já existe rota equivalente. `replace` cria ou altera, sendo útil para configuração idempotente, mas pode sobrescrever estado funcional; confira o alvo exato.

## Exclusão precisa

Exclua os atributos exatos quando houver mais de um candidato ou tabela:

```bash
$ sudo ip route del 192.168.2.0/23 via 10.11.12.3 dev enp1s0
```

Excluir somente pelo destino pode corresponder de forma ampla ou ambígua. Registre previamente o comando para restaurar a rota.

## Persistência e segurança remota

`ip route` altera somente o estado atual. NetworkManager, systemd-networkd, netplan, ifupdown, DHCP, daemons ou orquestração podem substituí-lo. Armazene a rota no proprietário ativo somente após testar o comportamento.

Em host remoto, preserve console independente e use rollback que não dependa da rota alterada. Verifique depois a consulta, vizinhos, os dois sentidos do tráfego e o serviço real.

