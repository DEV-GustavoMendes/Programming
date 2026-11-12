# Visão geral do DHCP

Aprenda como o DHCPv4 concede endereços e opções de rede por meio de descoberta, seleção e renovação.

O Protocolo de Configuração Dinâmica de Hosts fornece aos clientes uma configuração de rede concedida por tempo limitado. No DHCPv4, ela pode incluir um endereço IPv4, máscara de sub-rede, roteadores padrão, servidores DNS, duração da concessão e outras opções selecionadas pela política local.

## Clientes, servidores e retransmissores

Um servidor DHCP gerencia escopos ou conjuntos de endereços e o estado das concessões. O servidor não precisa estar em todos os segmentos físicos: um retransmissor DHCP pode encaminhar as trocas do cliente entre uma sub-rede e um servidor centralizado. Redes que usam apenas configuração estática podem não oferecer DHCP.

O DHCP é um protocolo da camada de aplicação transportado sobre UDP. Servidores DHCPv4 normalmente usam a porta UDP 67, e clientes, a porta 68.

# Visão geral do DHCP

Aprenda como o DHCPv4 concede endereços e opções de rede por meio de descoberta, seleção e renovação.

O Protocolo de Configuração Dinâmica de Hosts fornece aos clientes uma configuração de rede concedida por tempo limitado. No DHCPv4, ela pode incluir um endereço IPv4, máscara de sub-rede, roteadores padrão, servidores DNS, duração da concessão e outras opções selecionadas pela política local.

## Clientes, servidores e retransmissores

Um servidor DHCP gerencia escopos ou conjuntos de endereços e o estado das concessões. O servidor não precisa estar em todos os segmentos físicos: um retransmissor DHCP pode encaminhar as trocas do cliente entre uma sub-rede e um servidor centralizado. Redes que usam apenas configuração estática podem não oferecer DHCP.

O DHCP é um protocolo da camada de aplicação transportado sobre UDP. Servidores DHCPv4 normalmente usam a porta UDP 67, e clientes, a porta 68.

## Renovação da concessão

Uma concessão expira se não for renovada. Normalmente, o cliente começa a renovação antes do vencimento, muitas vezes contatando primeiro o servidor original diretamente. Se a renovação não tiver sucesso, mais tarde ele amplia a tentativa de revinculação. Os temporizadores exatos são fornecidos ou derivados de acordo com o protocolo.

Um endereço exibido como atribuído dinamicamente não comprova que sua concessão permanecerá para sempre. Ao solucionar alterações, registre a concessão ativa, sua duração, o servidor e as opções.

## Inspecionando o resultado

Depois que um cliente configura o DHCP, verifique todo o estado necessário, não apenas o endereço:

```bash
$ ip address show
$ ip route show
$ resolvectl status
```

O comando do resolvedor varia conforme o sistema. Inspecione também os dados da concessão e os logs do gerenciador de rede ativo. Endereços duplicados ainda podem ocorrer por servidores não autorizados, atribuições estáticas dentro de um conjunto, estado obsoleto ou configuração manual; o DHCP reduz erros, mas não pode impedir todo conflito por si só.

## DHCPv6 e configuração IPv6

Hosts IPv6 podem usar Configuração Automática de Endereço sem Estado, DHCPv6, configuração estática ou combinações. O DHCPv6 não usa a troca DORA do IPv4, e as informações do roteador padrão normalmente vêm dos Anúncios de Roteador IPv6, não do DHCPv6.

