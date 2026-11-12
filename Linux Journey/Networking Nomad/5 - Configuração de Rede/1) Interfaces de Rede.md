# Interfaces de Rede

Aprenda a inspecionar estado, endereços, estatísticas e propriedade da configuração persistente de interfaces Linux.

Uma interface de rede Linux conecta um namespace de rede a um dispositivo físico, loopback, bridge, túnel, dispositivo virtual ou outro link. Estado, endereços, rotas, DNS e configuração persistente são relacionados, mas distintos.

## Descoberta das interfaces

Use as ferramentas modernas do iproute2:

```bash
$ ip -brief link show
$ ip -brief address show
```

Os nomes podem ser derivados do hardware, como `enp1s0`, tradicionais, como `eth0`, ou definidos pelo administrador. Nunca presuma que `eth0` exista ou identifique um adaptador específico.

## Estado administrativo e operacional

`UP` significa que a interface está administrativamente habilitada. `LOWER_UP` costuma indicar que a camada inferior relata prontidão, como portadora Ethernet. Nenhuma flag isolada prova que endereço IP, rota, DNS, firewall ou aplicativo funcionam.

```bash
$ ip -details link show dev enp1s0
$ ip -s link show dev enp1s0
```

As estatísticas revelam erros, descartes e contadores, mas precisam de intervalo e linha de base para ter significado.

## Alteração do estado em execução

Comandos de runtime incluem:

```bash
$ sudo ip link set dev enp1s0 up
$ sudo ip address add 192.0.2.10/24 dev enp1s0
```

Eles afetam o estado atual e podem conflitar com um gerenciador que reaplique seu perfil. Derrubar a interface de acesso remoto pode encerrar a conexão. Antes de mudar, confirme o dispositivo, preserve console, registre o estado e prepare rollback testado ou temporizado.

## Identificação do proprietário da configuração

Os caminhos persistentes variam entre distribuições e instalações. Entre as possibilidades estão perfis do NetworkManager, unidades do systemd-networkd, entradas do netplan, `/etc/network/interfaces`, cloud-init ou orquestração. Determine qual serviço gerencia o dispositivo antes de editar arquivos:

```bash
$ systemctl --type=service --state=running | grep -E 'NetworkManager|networkd|networking'
$ networkctl status
$ nmcli device status
```

Use somente comandos do gerenciador identificado. Dois gerenciadores no mesmo link podem competir e sobrescrever estados.

## Verificação da mudança

Verifique link, endereços e validade, rotas escolhidas, resolver, vizinhos e o aplicativo real. Para mudança persistente, teste reinício controlado do serviço ou do sistema somente com recuperação disponível.
