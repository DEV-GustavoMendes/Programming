# join e split

Aprenda a unir dois arquivos de texto ordenados por uma chave e dividir um arquivo em partes nomeadas.

Os comandos `join` e `split` resolvem problemas diferentes de processamento de arquivos. `join` combina registros relacionados de duas entradas de texto ordenadas, enquanto `split` divide uma entrada em uma sequência de arquivos menores.

## União de Dois Arquivos pelo Primeiro Campo

Por padrão, `join` compara o primeiro campo separado por espaços em exatamente dois arquivos de entrada. Considere estes arquivos já ordenados:

`people.txt`:

```text
1 John
2 Jane
3 Mary
```

`surnames.txt`:

```text
1 Doe
2 Doe
3 Sue
```

Una os registros cujos campos-chave são iguais:

```bash
$ join people.txt surnames.txt
1 John Doe
2 Jane Doe
3 Mary Sue
```

A saída contém uma vez a chave compartilhada, seguida dos campos restantes do primeiro e do segundo arquivo. `join` processa dois arquivos de cada vez; ele não aceita três operandos comuns como uma união relacional de três vias.

## Ordenação das Chaves de União

Cada entrada precisa estar ordenada por seu campo de união com regras de comparação compatíveis. Para o campo 1 padrão, prepare cópias com `sort -k 1,1`:

```bash
$ LC_ALL=C sort -k 1,1 people-raw.txt > people.txt
$ LC_ALL=C sort -k 1,1 surnames-raw.txt > surnames.txt
$ LC_ALL=C join people.txt surnames.txt
```

Usar o mesmo locale na ordenação e na união mantém as regras de comparação consistentes. Não redirecione a saída de `sort` para seu próprio caminho de entrada, pois o shell truncaria esse arquivo primeiro.

## Seleção de Outros Campos de União

Use `-1 FIELD` para a chave do primeiro arquivo e `-2 FIELD` para a chave do segundo. Suponha que a primeira entrada contenha:

```text
John 1
Jane 2
Mary 3
```

A segunda contém:

```text
1 Doe
2 Doe
3 Sue
```

Depois de ordenar o primeiro arquivo pelo campo 2 e o segundo pelo campo 1, execute:

```bash
$ join -1 2 -2 1 people.txt surnames.txt
1 John Doe
2 Jane Doe
3 Mary Sue
```

Use `-t CHARACTER` quando um único caractere não vazio, como `:`, separar os campos. Opções como `-a 1` ou `-a 2` podem incluir linhas sem pares de uma das entradas; por padrão, a saída contém apenas chaves correspondentes.

## Divisão por Quantidade de Linhas

`split` grava partes consecutivas de uma entrada em arquivos separados. Ele não é o inverso de uma operação `join` baseada em chaves.

```bash
$ split large.txt
```

O comportamento padrão do GNU grava até 1.000 linhas por arquivo de saída e usa o prefixo `x`, produzindo nomes como `xaa`, `xab` e `xac`.

Use `-l NUMBER` para escolher a quantidade de linhas e acrescente um último operando para definir o prefixo:

```bash
$ split -l 500 large.txt part-
```

Isso produz `part-aa`, `part-ab` e assim por diante, com no máximo 500 linhas em cada parte.

## Divisão por Tamanho

Use `-b SIZE` para dividir a entrada por tamanho em bytes. Nesse contexto, sufixos do GNU como `K`, `M` e `G` representam potências de 1024:

```bash
$ split -b 10M archive.bin chunk-
```

Esse comando solicita partes de 10 mebibytes, com exceção de uma possível parte final menor. `split` não cria um manifesto nem metadados de remontagem; preserve a ordem dos sufixos e concatene as partes nessa ordem quando a reconstrução for apropriada.

Para praticar uniões por chave e o processamento de dados estruturados, experimente estes laboratórios:

1. **[Comando join do Linux: União de Arquivos](https://labex.io/labs/linux-linux-join-command-file-joining-219193)** — Este laboratório oferece uma introdução prática e direta ao comando `join`, permitindo que você pratique a mesclagem de linhas de dois arquivos de texto ordenados com base em um campo comum, exatamente como apresentado na lição.
2. **[Processamento de Dados de Funcionários](https://labex.io/labs/linux-processing-employees-data-388132)** — Aplique seus conhecimentos de `join` e de outros utilitários avançados da linha de comando do Linux, como `awk`, para combinar e processar dados de várias fontes, simulando um cenário real de análise de dados.
