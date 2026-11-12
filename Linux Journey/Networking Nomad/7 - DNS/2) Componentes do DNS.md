# Componentes do DNS

Aprenda como resolvedores recursivos, servidores autoritativos, zonas e registros de recursos dividem as responsabilidades do DNS.

O DNS separa a função de recursão voltada para o cliente da publicação autoritativa. Entender esse limite evita confundir uma resposta em cache com o proprietário de uma zona.

## Resolvedores stub e recursivos

Um resolvedor stub em uma aplicação ou sistema operacional envia consultas a um resolvedor recursivo configurado. O resolvedor recursivo retorna uma resposta final, um erro ou o resultado de um encaminhamento depois de usar o cache e, quando necessário, realizar consultas iterativas. Sua resposta só pode conter o sinalizador de resposta autoritativa quando o servidor que responde possui autoridade sobre os dados; a recursão por si só não o torna autoritativo.

## Servidores de nomes autoritativos

Um servidor autoritativo responde a partir dos dados de zona sobre os quais possui autoridade. Uma zona deve ter vários servidores autoritativos com dados sincronizados e considerações independentes de falha. Um servidor apenas autoritativo não precisa realizar recursão para clientes arbitrários.

## Zonas e armazenamento de zonas

Uma zona é uma parte do espaço de nomes DNS servida administrativamente. Ela começa no ápice da zona e pode delegar zonas filhas. Os dados da zona podem ser armazenados em um arquivo de zona textual, gerados a partir de um banco de dados, carregados por uma API ou sintetizados por software; um “arquivo de zona” não é uma implementação física obrigatória.

O ápice da zona normalmente possui um registro SOA e um conjunto NS. Os dados de delegação no pai identificam os servidores autoritativos filhos, às vezes acompanhados por registros de endereço glue necessários para alcançar nomes de servidores pertencentes à própria zona delegada.

## Campos dos registros de recursos

Um registro de recurso possui nome do proprietário, TTL, classe, tipo e RDATA específico do tipo. Por exemplo:

```text
www.example.com.  300  IN  A  192.0.2.25
```

O proprietário é `www.example.com.`, o TTL é 300 segundos, a classe é Internet, o tipo é endereço IPv4 e o RDATA é o endereço. As regras de omissão de campos e nomes relativos na sintaxe dos arquivos de zona exigem cuidado com a origem.

## TTL e cache negativo

Registros positivos usam TTLs para limitar a reutilização do cache. Respostas negativas, como um nome comprovadamente inexistente, também podem ser armazenadas em cache de acordo com regras derivadas do SOA. Reduzir um TTL pouco antes de uma mudança planejada afeta apenas registros obtidos depois que os caches observam o valor menor; valores maiores armazenados anteriormente permanecem até expirar.

