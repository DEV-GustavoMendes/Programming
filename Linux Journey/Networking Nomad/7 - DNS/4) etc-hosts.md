# /etc/hosts

Aprenda como os mapeamentos do arquivo hosts local participam da resolução de nomes no Linux e como testá-los com segurança.

`/etc/hosts` fornece entradas estáticas de endereço e nome para a pilha local de serviços de nomes do sistema. Ele é útil para nomes de loopback, dependências de inicialização e testes de escopo restrito, mas não publica registros para outros hosts nem atualiza o DNS.

## Lendo o arquivo

Uma linha começa com um endereço IPv4 ou IPv6, seguido por um ou mais nomes:

```text
127.0.0.1       localhost
192.0.2.25      app-test.example.net app-test
2001:db8::25    app-test-v6.example.net app-test-v6
```

Comentários começam com `#`. Por convenção, algumas ferramentas tratam o primeiro nome como canônico e os posteriores como aliases, mas o comportamento das aplicações e as APIs dos resolvedores variam. Evite entradas duplicadas ou conflitantes para o mesmo nome.

## Ordem do resolvedor

A configuração do Name Service Switch, normalmente em `/etc/nsswitch.conf`, determina como as funções do resolvedor do sistema combinam `files`, DNS, sistemas multicast e outras fontes. Uma linha comum é:

```text
hosts: files dns
```

Não presuma que os arquivos sempre vêm primeiro sem inspecionar a política. As aplicações também podem usar suas próprias bibliotecas DNS, caches, proxies ou resolvedores criptografados e talvez não sigam o caminho do sistema.

## Testando pelo resolvedor do sistema

Use `getent` para exercitar o caminho configurado dos serviços de nomes do sistema:

```bash
$ getent ahosts app-test.example.net
```

`dig` consulta o DNS diretamente e normalmente não informa os mapeamentos de `/etc/hosts`. Essa diferença é útil: se `getent` funcionar e `dig` não, isso pode indicar uma fonte local ou uma diferença na política do resolvedor.

## Editando com segurança

Preserve as entradas necessárias de localhost e identidade do host, valide o endereço pretendido e faça uma alteração recuperável com ferramentas de edição privilegiadas. Evite substituir um domínio público real em um teste casual; isso pode redirecionar credenciais ou tráfego de aplicações inesperadamente. Use um nome dedicado de teste e remova a entrada depois do experimento.

Após a edição, teste a aplicação exata, pois ela pode manter um cache ou usar outro resolvedor. Documente substituições persistentes para que elas não permaneçam silenciosamente além de sua finalidade.

## Configuração do servidor resolvedor

`/etc/resolv.conf` tradicionalmente lista as configurações do resolvedor DNS, mas muitas vezes é gerado pelo NetworkManager, systemd-resolved, DHCP ou outro gerenciador. Inspecione os links simbólicos e os comentários do arquivo e, depois, altere a fonte de configuração responsável em vez de editar uma saída gerada que será sobrescrita.

