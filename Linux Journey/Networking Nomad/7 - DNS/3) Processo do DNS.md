# Processo do DNS

Aprenda como resolvedores stub e recursivos usam cache, encaminhamentos, glue e autoridade para responder a uma consulta DNS.

Uma aplicação comum consulta o resolvedor stub do sistema operacional, que examina a política local de serviços de nomes e envia uma consulta recursiva a um resolvedor configurado. O resolvedor recursivo só percorre a hierarquia quando o cache válido ainda não responde à pergunta.

## Começando pela política local e pelo cache

O resolvedor do sistema pode consultar `/etc/hosts`, DNS e outras fontes na ordem configurada. Sufixos de pesquisa podem transformar um nome curto em vários nomes candidatos. Um resolvedor recursivo então verifica entradas positivas e negativas do cache antes de enviar tráfego aos servidores superiores.

## Consultando um servidor raiz

Quando não há uma resposta no cache, um resolvedor recursivo pode consultar um servidor raiz. A raiz do DNS possui 13 identidades de servidores nomeadas de A a M, atendidas por muitas instâncias físicas que usam anycast e outras técnicas resilientes de implantação. Normalmente, a resposta encaminha o resolvedor aos servidores autoritativos do domínio de primeiro nível relevante, em vez de retornar o endereço final do host.

## Seguindo encaminhamentos de TLD e servidores autoritativos

O resolvedor consulta um servidor autoritativo de `com`, que retorna os servidores de nomes autoritativos delegados de `example.com`. O encaminhamento pode incluir registros de endereço glue quando eles são necessários para alcançar um servidor cujo nome está dentro da zona filha delegada. Em seguida, o resolvedor consulta um servidor autoritativo sobre o registro solicitado.

## Seguindo aliases e tipos de registro

Uma resposta pode conter um alias CNAME que exige outra consulta de nome, ou registros específicos de aplicações que levam a outras consultas. Solicitar `A` retorna apenas registros de endereço IPv4 e os dados relacionados da cadeia; uma consulta `AAAA` separada recupera endereços IPv6. A resposta final contém um estado como `NOERROR`, `NXDOMAIN` ou `SERVFAIL`, cada um com um significado diferente.

## Validação, cache e uso pela aplicação

Um resolvedor recursivo com validação pode usar assinaturas DNSSEC e a cadeia de confiança para verificar uma negação autenticada ou a integridade dos registros. O DNSSEC não criptografa as consultas nem comprova que a aplicação no endereço retornado é confiável.

O resolvedor armazena os resultados em cache segundo as regras de TTL e os retorna ao stub. A aplicação então escolhe um endereço e tenta seus próprios protocolos de rede e segurança.

