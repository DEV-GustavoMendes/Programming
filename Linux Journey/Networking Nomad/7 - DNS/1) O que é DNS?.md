# O que é DNS?

Aprenda como o DNS organiza e resolve nomes distribuídos e registros de recursos tipados.

O Sistema de Nomes de Domínio é um banco de dados distribuído e hierárquico e um protocolo de consulta. Ele permite que os clientes recuperem informações tipadas associadas a nomes, incluindo endereços, roteamento de mensagens, servidores autoritativos, dados de serviços e registros de verificação.

## Nomes e registros de recursos

O DNS faz mais do que traduzir um nome de host em um endereço IP. Um registro `A` contém um endereço IPv4, `AAAA` um endereço IPv6, `MX` dados de roteamento de mensagens, `NS` nomes de servidores autoritativos, e muitos outros tipos transportam dados diferentes. Um nome pode ter vários registros ou nenhum registro de endereço.

## Nomes hierárquicos

Um nome de domínio totalmente qualificado identifica um caminho na árvore DNS. Em `www.example.com.`, o ponto final representa a raiz, `com` está abaixo dela, `example` está abaixo de `com`, e `www` é um nome dentro desse domínio. O ponto final costuma ser omitido nas interfaces de usuário, mas é importante para diferenciar nomes absolutos de nomes localmente relativos na configuração.

## Autoridade distribuída

A autoridade do DNS é delegada ao longo da hierarquia. Servidores raiz direcionam os resolvedores para servidores de domínios de primeiro nível, que os direcionam aos servidores autoritativos das zonas delegadas. As organizações gerenciam seus próprios dados autoritativos sem armazenar todo o espaço global de nomes em um servidor central.

## Resolução e cache

O resolvedor stub de um host geralmente envia uma consulta a um resolvedor recursivo. Esse resolvedor pode responder a partir de um cache válido ou consultar a hierarquia em nome do cliente. Os TTLs dos registros limitam por quanto tempo as entradas de cache normalmente podem ser reutilizadas, melhorando a escalabilidade, mas adiando a visibilidade das alterações até que os caches sejam atualizados.

O sucesso do DNS não comprova a integridade da rota, do transporte, do TLS nem da aplicação. Uma falha de DNS também pode ocorrer antes de qualquer consulta externa, pois `/etc/hosts`, sufixos de pesquisa, caches locais ou políticas de serviços de nomes afetam o resolvedor do sistema.

