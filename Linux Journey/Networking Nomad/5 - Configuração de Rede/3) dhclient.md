# dhclient

Aprenda quando e como usar dhclient sem entrar em conflito com o gerenciador de rede do sistema.

`dhclient` é um cliente ISC DHCP presente em alguns sistemas Linux. Muitas instalações atuais deixam NetworkManager, systemd-networkd ou outro serviço executar seu próprio cliente. Iniciar um segundo cliente numa interface gerenciada pode criar endereços, rotas, DNS e concessões concorrentes.

## Identificação do cliente ativo

Antes de invocar `dhclient`, examine o proprietário e os processos:

```bash
$ nmcli device status
$ networkctl status
$ ps -ef | grep '[d]hclient'
```

Use as ferramentas disponíveis no host. Se um gerenciador controla a interface, solicite DHCP por ele, em vez de iniciar outro cliente.

## Solicitação explícita de uma concessão

Em uma interface de teste não gerenciada, quando `dhclient` é o proprietário pretendido, especifique a interface e use saída detalhada:

```bash
$ sudo dhclient -v enp1s0
```

Sem uma interface, o comando pode agir em várias interfaces elegíveis. Caminhos de configuração e concessão variam; nomes comuns incluem `dhclient.conf` e `dhclient.leases`, mas não presuma uma localização fixa.

## Liberação de uma concessão

`dhclient -r INTERFACE` solicita liberação e pode remover a configuração utilizável. É disruptivo e não garante que o servidor esteja acessível para receber o pedido. Não libere apenas para inspecionar, sobretudo no caminho de gerenciamento remoto.

## Verificação da concessão aplicada

Depois de uma solicitação controlada, verifique mais que o endereço:

```bash
$ ip address show dev enp1s0
$ ip route show
$ resolvectl status
```

Examine os logs do gerenciador ou do cliente e a duração da concessão e, em seguida, teste a resolução de nomes e a aplicação pretendidas. Um DHCPACK pode trazer opções incorretas, e a atribuição bem-sucedida de um endereço não comprova a acessibilidade do gateway nem do DNS.

