# Gerenciador de Rede

Aprenda como o NetworkManager separa dispositivos, perfis persistentes e estado ativo em tempo de execução.

O NetworkManager gerencia dispositivos e ativa perfis de conexão em muitos desktops e servidores Linux. Ele não é universal; confirme que controla a interface desejada antes de usar `nmcli` para mudar a configuração.

## Dispositivos e conexões

Um dispositivo é uma interface do kernel como `enp1s0` ou `wlan0`. Uma conexão é um perfil armazenado com IPv4, IPv6, DNS, Wi-Fi, roteamento e outras definições. Um dispositivo pode ter vários perfis, mas normalmente apenas um aplicável fica ativo por vez.

```bash
$ nmcli device status
$ nmcli connection show
$ nmcli connection show --active
```

## Inspeção do estado efetivo

Mostre o perfil ativo e os detalhes do dispositivo:

```bash
$ nmcli -f GENERAL,IP4,IP6 device show enp1s0
$ nmcli connection show 'Wired connection 1'
```

Definições do perfil, resultados DHCP e estado do kernel podem diferir. Compare com `ip address`, `ip route` e o resolver. O obsoleto `nm-tool` não deve fundamentar um fluxo atual.

## Modificação e ativação de um perfil

Modifique explicitamente um perfil e ative-o numa janela de manutenção:

```bash
$ sudo nmcli connection modify 'Wired connection 1' ipv4.method auto
$ sudo nmcli connection up 'Wired connection 1'
```

Modificar muda dados persistentes; ativar pode substituir endereços, rotas e DNS ativos. Uma mudança remota exige console, definições originais salvas e rollback temporizado independente. Não dependa da conexão alterada para transportar sua própria recuperação.

## Verificação e proteção de segredos

Depois da ativação, verifique perfil, endereços e rotas do kernel, DNS, ambas as famílias e o aplicativo. Perfis Wi-Fi, VPN, 802.1X e móveis podem conter segredos. Limite permissões e não imprima campos secretos em logs ou transcrições compartilhadas.

