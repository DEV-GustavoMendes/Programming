# Dependências de Pacotes

Aprenda como os metadados dos pacotes expressam recursos, versões, conflitos e relações com bibliotecas compartilhadas.

Uma dependência de pacote declara que um pacote precisa de outro pacote, recurso ou versão compatível para ser instalado ou funcionar. Gerenciadores de pacotes que conhecem os repositórios usam esses metadados para calcular um conjunto coerente de alterações, em vez de tratar cada arquivo de pacote isoladamente.

## Relações de Dependência

Os metadados de um pacote podem expressar mais do que um simples nome obrigatório. Dependendo do formato da distribuição, as relações podem incluir:

- dependências obrigatórias
- restrições de versão mínima, máxima ou exata
- alternativas, nas quais qualquer um entre vários provedores satisfaz um requisito
- recomendações ou sugestões com semântica menos rigorosa
- conflitos, incompatibilidades ou substituições
- recursos virtuais fornecidos por mais de um pacote

Essas regras permitem que um resolvedor escolha um conjunto de versões de pacotes compatível com os repositórios configurados, a arquitetura e o estado instalado. Uma solução pode exigir atualizações, remoções ou a escolha entre provedores; por isso, examine a transação proposta antes de aprová-la.

## Bibliotecas Compartilhadas como Dependências

Uma biblioteca compartilhada contém código compilado que vários programas podem mapear durante a execução. O compartilhamento reduz implementações duplicadas e permite que as distribuições atualizem uma biblioteca comum de forma independente, mas os programas dependem de uma interface binária de aplicação, ou ABI, compatível.

Em sistemas Linux baseados em ELF, um executável pode registrar o nome de uma biblioteca necessária, como um SONAME. O vinculador dinâmico localiza uma biblioteca instalada correspondente quando o programa é iniciado. Em geral, os metadados do pacote representam esse requisito como uma dependência do pacote ou recurso que fornece a biblioteca compatível.

## Compatibilidade de Versão e ABI

A existência de um arquivo com nome semelhante ao da biblioteca não é suficiente. A ABI exigida, a arquitetura, os símbolos e, em alguns casos, a versão mínima precisam corresponder. Substituir manualmente uma biblioteca da distribuição pode quebrar todos os programas que dependem dela, mesmo que o nome do arquivo pareça correto.

Os mantenedores de pacotes codificam as relações entre bibliotecas e coordenam as transições quando uma ABI muda. Mantenha as bibliotecas nativas sob o controle do gerenciador de pacotes; para software que precise de uma versão incompatível, use mecanismos compatíveis de instalação paralela, contêiner, ambiente ou compilação.

## Estados de Dependência Quebrada

Um problema de dependência pode surgir de repositórios misturados, operações interrompidas, arquivos de pacote instalados manualmente, versões retidas, arquivos removidos ou software de terceiros incompatível. Não tente resolvê-lo excluindo arquivos do banco de dados de pacotes nem forçando cegamente uma instalação.

Primeiro, leia os diagnósticos do gerenciador de pacotes, atualize apenas os metadados de repositórios confiáveis, inspecione versões retidas ou fixadas e examine o reparo proposto. Um instalador de baixo nível pode desempacotar um arquivo sem obter todas as dependências; uma ferramenta de repositório de nível mais alto costuma ser mais segura para instalações comuns, pois resolve a transação completa.

Use [Gerenciar Bibliotecas Compartilhadas no Linux](https://labex.io/labs/comptia-manage-shared-libraries-in-linux-590867) para inspecionar relações em tempo de execução e depois compare-as com os metadados de pacotes em [Gerenciamento de Pacotes com RPM](https://labex.io/labs/rhel-managing-packages-with-rpm-in-linux-590868).