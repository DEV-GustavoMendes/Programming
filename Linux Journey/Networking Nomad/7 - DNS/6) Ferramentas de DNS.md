# Ferramentas de DNS

Aprenda a comparar a resolução do sistema e consultas DNS diretas com getent, resolvectl e dig.

A solução de problemas de DNS começa pela identificação da camada que está sendo testada. Ferramentas do resolvedor do sistema incluem arquivos locais e políticas, enquanto `dig` e `nslookup` enviam consultas DNS e podem apontar diretamente para um servidor específico.

## Testando o resolvedor do sistema

Use o caminho normal de serviços de nomes do host com:

```bash
$ getent ahosts www.example.com
```

Em um host com systemd-resolved, inspecione servidores por enlace, domínios de pesquisa e estado dos protocolos com:

```bash
$ resolvectl status
$ resolvectl query www.example.com
```

Uma aplicação ainda pode usar uma biblioteca resolvedora ou um proxy próprio; portanto, reproduza o teste pela aplicação quando os resultados forem diferentes.

## Consultando com dig

Especifique um nome e um tipo de registro:

```bash
$ dig www.example.com A
$ dig www.example.com AAAA
$ dig example.com MX
```

A saída identifica o servidor que respondeu, o estado, os sinalizadores, a pergunta, a resposta, a autoridade, os dados adicionais, o tempo da consulta e os metadados do transporte. `+short` é conveniente para scripts, mas oculta evidências necessárias ao diagnóstico.

## Selecionando um servidor

Aponte explicitamente para um resolvedor ou servidor autoritativo:

```bash
$ dig @192.0.2.53 www.example.com A
```

Compare o resolvedor recursivo configurado, um segundo resolvedor aprovado e cada servidor autoritativo ao diferenciar cache de autoridade. Um estado `NOERROR` pode não conter a resposta solicitada; `NXDOMAIN` significa que o nome consultado não existe, enquanto `SERVFAIL` significa que o servidor não conseguiu concluir a consulta.

## Verificando recursão e autoridade

`rd` na consulta solicita recursão; `ra` em uma resposta indica que o servidor a oferece. `aa` significa que a resposta é autoritativa. Consulte um servidor autoritativo com `+norecurse` para não confundir o cache recursivo com os dados servidos pela zona.

`dig +trace NAME` realiza seu próprio percurso iterativo a partir das referências da raiz. Ele pode diferir de um resolvedor de produção porque ignora o cache, o encaminhamento, a política, a validação DNSSEC e a localização de rede desse resolvedor.

## Testando consultas reversas e por TCP

Use `-x` para construir uma consulta PTR reversa:

```bash
$ dig -x 192.0.2.25
```

Teste DNS sobre TCP ao investigar truncamento, transferências de zona ou diferenças no firewall:

```bash
$ dig +tcp @192.0.2.53 example.com SOA
```

O DNS moderno pode usar a porta 53 por UDP ou TCP; ambos devem ser permitidos onde forem necessários. Uma resposta UDP com o sinalizador de truncamento leva clientes compatíveis a repetir a consulta por um transporte apropriado.

