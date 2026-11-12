# Tabela de roteamento

Aprenda a ler rotas no Linux e inspecionar a rota selecionada para um destino.

O estado de roteamento do Linux determina quais próximos saltos, interfaces e origens são elegíveis para um destino IP. A visualização legada `route -n` ainda é encontrada, mas `ip route` expõe os conceitos modernos de roteamento do kernel de forma mais direta.

## Lendo rotas IPv4

Um exemplo de saída pode ser:

```text
$ ip -4 route show
default via 192.168.224.2 dev eth0 proto dhcp src 192.168.224.10 metric 100
192.168.224.0/24 dev eth0 proto kernel scope link src 192.168.224.10 metric 100
```

A rota `/24` conectada envia os destinos correspondentes diretamente por `eth0`. A rota padrão usa o gateway de próximo salto `192.168.224.2`. `proto` descreve como a rota foi instalada, `src` é uma origem preferida para o tráfego correspondente, e uma métrica ajuda a classificar rotas que, de outra forma, seriam comparáveis.

## Rotas conectadas e padrão

Uma rota com `scope link` e sem próximo salto `via` trata o prefixo como diretamente acessível pela interface. Uma rota padrão corresponde a todos os endereços, mas perde para qualquer rota elegível mais específica.

## Comprimento do prefixo e métrica

A seleção de rotas considera as regras de política e escolhe o prefixo elegível mais longo. As métricas classificam rotas dentro de conjuntos comparáveis apropriados; uma rota padrão de métrica baixa não substitui uma `/24` correspondente apenas porque seu número é menor.

## Regras de política e várias tabelas

O Linux pode consultar várias tabelas de roteamento de acordo com políticas de `ip rule` baseadas em origem, marca, interface ou outros seletores. Por isso, visualizar apenas a tabela principal pode omitir o caminho real:

```bash
$ ip rule show
$ ip route show table all
```

Namespaces de rede e VRFs também podem conter estados separados. Execute a inspeção no mesmo contexto do processo afetado.

## Consultando uma rota efetiva

Peça ao kernel para avaliar um destino e, opcionalmente, uma origem:

```bash
$ ip route get 203.0.113.10
$ ip route get 203.0.113.10 from 192.168.224.10
```

O resultado prevê a consulta local naquele momento. Ele não envia uma sondagem nem comprova a acessibilidade do vizinho, dos saltos seguintes, do firewall ou da aplicação.

