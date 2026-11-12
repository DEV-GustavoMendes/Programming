# expand e unexpand

Aprenda como as paradas de tabulação controlam a conversão entre tabulações e espaços com expand e unexpand.

As tabulações armazenam um movimento até uma parada de tabulação, não uma quantidade fixa de espaços visíveis. Sua largura exibida depende da coluna atual e da configuração das paradas. Os comandos `expand` e `unexpand` convertem entre caracteres de tabulação e espaços levando essas posições em conta.

## Conversão de Tabulações em Espaços

`expand` lê a entrada, substitui as tabulações pelos espaços necessários para alcançar as paradas adequadas e grava o resultado em stdout:

```bash
$ expand sample.txt
```

Por padrão, as paradas ocorrem a cada 8 colunas. Assim, uma tabulação na coluna 1 se expande de forma diferente de uma na coluna 6; ela nem sempre é substituída por oito espaços.

## Escolha das Paradas de Tabulação

Use `-t NUMBER` para colocar paradas a cada quantidade especificada de colunas. Para paradas a cada quatro colunas:

```bash
$ expand -t 4 sample.txt
```

O GNU `expand` também aceita uma lista de posições explícitas separadas por vírgulas. Use `-i` quando apenas as tabulações anteriores ao primeiro caractere não vazio de cada linha devam ser convertidas.

## Salvamento Seguro da Saída Convertida

`expand` não edita seu arquivo de entrada. Redirecione stdout para outro caminho quando quiser salvar o texto convertido:

```bash
$ expand sample.txt > result.txt
```

Não use `expand sample.txt > sample.txt`. O shell trunca o destino antes que `expand` possa lê-lo; assim, os dados de origem podem ser perdidos. Depois de verificar um resultado gravado separadamente, você pode substituir o original conscientemente com uma etapa apropriada de gerenciamento de arquivos.

## Conversão de Espaços em Tabulações

`unexpand` substitui espaços elegíveis por tabulações, preservando o alinhamento nas paradas escolhidas. Por padrão, o GNU `unexpand` converte apenas os espaços iniciais anteriores ao primeiro caractere não vazio de uma linha:

```bash
$ unexpand result.txt
```

Use `-a` para considerar os espaços adequados em toda a linha:

```bash
$ unexpand -a result.txt
```

Isso não substitui simplesmente todo grupo de oito espaços. A conversão depende das posições das colunas e das paradas, assim como em `expand`. Use `-t 4` ou outra especificação quando o arquivo seguir uma convenção diferente.

Os dois comandos leem stdin quando nenhum arquivo é indicado e, portanto, podem ser usados em pipelines. Lembre-se de que converter para espaços e de volta pode não reconstruir a escolha original entre tabulações e espaços, mesmo que o alinhamento exibido permaneça igual.