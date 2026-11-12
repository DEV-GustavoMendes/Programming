# Protocolo de Gateway de Borda

Aprenda como o BGP troca informações de acessibilidade IP controladas por políticas entre sistemas autônomos e dentro deles.

O Protocolo de Gateway de Borda é o protocolo de roteamento por vetor de caminhos da Internet. Ele troca informações de acessibilidade de prefixos IP e atributos de caminho para que as redes possam aplicar políticas administrativas, em vez de escolher rotas apenas pela distância física.

## Sistemas autônomos e sessões

Um sistema autônomo é um conjunto de redes sob uma administração comum de roteamento, identificado no BGP por um número de sistema autônomo. O BGP externo troca rotas entre sistemas autônomos; o BGP interno distribui a acessibilidade BGP dentro de um AS.

Os pares BGP estabelecem uma sessão pela porta TCP 179. Uma sessão TCP funcional é apenas a base de transporte; os recursos, as políticas e a troca de rotas do BGP também precisam funcionar.

## Informações de vetor de caminhos

Um anúncio inclui um prefixo e atributos. `AS_PATH` lista os sistemas autônomos atravessados e ajuda a detectar loops. Outros atributos comuns incluem `LOCAL_PREF`, `MED`, origem, próximo salto e comunidades. Seus efeitos dependem da direção, da implementação e da política.

## Seleção baseada em políticas

O “melhor” caminho do BGP é o que vence um processo de decisão configurado. Os operadores podem preferir rotas de clientes, alterar a preferência local, filtrar prefixos, usar comunidades e aplicar políticas de engenharia de tráfego. Um `AS_PATH` mais curto pode importar em uma etapa, mas não substitui universalmente atributos de prioridade mais alta.

Depois que o BGP seleciona candidatos, o encaminhamento IP comum ainda aplica a correspondência de prefixo mais longo. Um `/24` selecionado é usado para seus destinos em vez de um `/16` selecionado que o abrange.

## Anúncio e acessibilidade

Anunciar um prefixo declara acessibilidade conforme uma política; não cria a rota subjacente nem garante o caminho de retorno. Antes de originar um prefixo, assegure o encaminhamento válido, o comportamento da agregação, os filtros, o failover e a autorização de propriedade.

## Segurança de roteamento e controle de mudanças

Vazamentos e sequestros de rotas podem afetar o tráfego muito além de um roteador. Os operadores usam filtros rigorosos de importação e exportação, limites máximos de prefixos, políticas de pares, monitoramento e validação de origem pela Infraestrutura de Chaves Públicas de Recursos, quando apropriado. A validação de origem RPKI verifica se um AS está autorizado a originar um prefixo; ela não valida todo o caminho de ASs.

Alterações no BGP exigem implantação gradual, revisão das diferenças de rotas, acesso fora de banda, reversão e verificação dos planos de controle e dados.

