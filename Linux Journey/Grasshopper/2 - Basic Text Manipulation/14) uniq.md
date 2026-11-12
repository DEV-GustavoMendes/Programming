# uniq (Único)

Aprenda a agrupar, contar ou filtrar grupos adjacentes de linhas iguais com uniq.

O comando `uniq` compara cada linha de entrada com a linha anterior. Ele pode agrupar, contar ou selecionar grupos adjacentes de linhas iguais, mas não pesquisa o arquivo inteiro por duplicatas separadas.

## Agrupamento de Linhas Duplicadas Adjacentes

Suponha que `reading.txt` contenha valores agrupados:

```plaintext
book
book
paper
paper
article
article
magazine
```

Execute `uniq` sem opção de filtro para mostrar uma linha representativa de cada grupo adjacente:

```bash
$ uniq reading.txt
book
paper
article
magazine
```

O arquivo de entrada permanece inalterado, pois o resultado vai para stdout.

## Contagem de Grupos Adjacentes

Use `-c` para prefixar cada grupo de saída com sua quantidade de linhas consecutivas:

```bash
$ uniq -c reading.txt
      2 book
      2 paper
      2 article
      1 magazine
```

Essas quantidades representam o tamanho de cada sequência, não os totais globais, a menos que todas as linhas iguais tenham sido agrupadas primeiro.

## Seleção de Grupos Únicos ou Repetidos

Use `-u` para mostrar apenas os grupos que contêm exatamente uma linha:

```bash
$ uniq -u reading.txt
magazine
```

Use `-d` para mostrar uma linha representativa de cada grupo adjacente que contém mais de uma:

```bash
$ uniq -d reading.txt
book
paper
article
```

O GNU `uniq -D` mostra todas as linhas dos grupos repetidos, enquanto `-d` minúsculo mostra o valor de cada grupo uma vez.

## Agrupamento de Duplicatas Separadas

Se linhas iguais estiverem separadas, elas formarão grupos diferentes:

```plaintext
book
paper
book
paper
article
magazine
article
```

Executar `uniq` nesse arquivo produz um resultado que pode surpreender:

```bash
$ uniq reading.txt
book
paper
book
paper
article
magazine
article
```

Nenhuma linha é agrupada porque os valores vizinhos são diferentes. Ordene primeiro quando a mudança de ordem for aceitável e você quiser reunir linhas completas iguais:

```bash
$ sort reading.txt | uniq
article
book
magazine
paper
```

Use um locale e uma política de comparação consistentes nas duas etapas. `sort -u reading.txt` também pode ordenar e preservar uma linha para cada chave igual em um único comando.

`uniq` lê stdin quando nenhum arquivo de entrada é indicado, motivo pelo qual se encaixa naturalmente depois de `sort`. Opções GNU como `-i` podem ignorar maiúsculas e minúsculas, enquanto `-f`, `-s` e `-w` podem ignorar ou limitar regiões de comparação; use-as apenas quando a igualdade deva ser definida por parte de cada linha.

Para praticar o agrupamento, a contagem e a filtragem de duplicatas, experimente estes laboratórios:

1. **[Comando uniq do Linux: Filtragem de Duplicatas](https://labex.io/labs/linux-linux-uniq-command-duplicate-filtering-219199)** — Aprenda a combinar `uniq` com `sort` para identificar, filtrar e analisar linhas duplicadas.
2. **[Comando sort do Linux: Ordenação de Texto](https://labex.io/labs/linux-linux-sort-command-text-sorting-219196)** — Pratique a organização de linhas com `sort`, uma etapa importante antes do uso eficaz de `uniq`.
3. **[Contagem e Ordenação de Palavras](https://labex.io/labs/linux-word-count-and-sorting-388125)** — Aprenda as ferramentas essenciais de processamento de texto do Linux `wc` (contagem de palavras) e `sort` neste desafio prático. Aprenda a contar linhas, palavras e caracteres, encontrar padrões frequentes e ordenar dados com eficiência em várias tarefas de análise de texto.