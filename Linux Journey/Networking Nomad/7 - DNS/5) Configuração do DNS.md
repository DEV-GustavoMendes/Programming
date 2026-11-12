# Configuração do DNS

Aprenda a escolher, proteger, validar e operar serviços DNS autoritativos ou recursivos.

O software de DNS deve ser selecionado pela função e pelos requisitos operacionais, não por um “melhor servidor” universal. Um serviço autoritativo publica zonas; um serviço recursivo responde aos clientes resolvendo e armazenando em cache; um resolvedor encaminhador envia consultas a outro resolvedor. Combinar funções altera a superfície de ataque.

## Escolhendo uma função e uma implementação

- O BIND pode fornecer serviços autoritativos e recursivos com amplo suporte aos padrões.
- O Unbound é normalmente implantado como um resolvedor recursivo com validação.
- O dnsmasq fornece recursos leves de encaminhamento, cache e DHCP para redes controladas menores.
- O PowerDNS oferece produtos autoritativos e recursivos separados, com vários mecanismos de armazenamento.

Os recursos e o empacotamento mudam; portanto, consulte a documentação oficial da versão instalada. Implante apenas a função necessária e desabilite recursão ou serviço de zona não intencionais.

## Projetando antes de instalar

Defina zonas, clientes, volume de consultas, mecanismo de atualização, necessidades de DNSSEC, registros, monitoramento, backups e recuperação. Zonas autoritativas precisam de servidores redundantes e delegações registradas corretamente. O serviço recursivo precisa de controle explícito de acesso dos clientes, política de cache, acessibilidade dos servidores superiores ou iterativos e proteção contra abuso.

Nunca exponha recursão irrestrita à Internet. Resolvedores abertos podem ser usados indevidamente em ataques de reflexão e consumir recursos locais.

## Validando a configuração e os dados das zonas

Use as ferramentas da implementação para verificar a sintaxe e as zonas antes do reload. Para o BIND, alguns exemplos comuns são:

```bash
$ named-checkconf
$ named-checkzone example.com /etc/bind/zones/db.example.com
```

Execute com as permissões e os caminhos apropriados ao host. O sucesso do analisador não comprova a delegação, a propagação do serial, a cadeia DNSSEC, a acessibilidade pelo firewall nem a correção das respostas; portanto, prossiga com consultas controladas.

## Aplicando e testando com segurança

Preserve a configuração atual e o acesso de recuperação, valide e então faça reload em vez de restart, quando houver suporte. Consulte cada servidor autoritativo diretamente, com a recursão desabilitada, e compare o serial SOA, o conjunto NS, registros positivos, nomes inexistentes e o comportamento tanto por UDP quanto por TCP:

```bash
$ dig @192.0.2.53 example.com SOA +norecurse
$ dig @192.0.2.53 missing.example.com A +norecurse
$ dig @192.0.2.53 example.com SOA +norecurse +tcp
```

Para a recursão, teste redes de clientes permitidas e negadas, validação DNSSEC, comportamento do cache e falhas das dependências superiores.

## Operando o serviço

Monitore falhas de consultas, latência, comportamento do cache, uso de recursos, transferências de zonas, consistência do serial, expiração do DNSSEC e integridade da delegação. Faça backup seguro da configuração de origem e do material de assinatura, mas verifique se uma nova instância consegue carregar as zonas e servir respostas corretas. Aplique correções às versões com suporte e limite as interfaces de controle, as atualizações dinâmicas e o acesso a transferências.

