# regex (Expressões Regulares)

Aprenda como âncoras, conjuntos de caracteres, repetições e variantes de regex controlam a correspondência de textos.

Expressões regulares, frequentemente abreviadas como **regex**, descrevem padrões de texto. Ferramentas como `grep`, `sed` e `awk` usam regex, mas a sintaxe aceita pode variar; por isso, sempre identifique a ferramenta e a variante de expressão regular.

O GNU `grep` usa expressões regulares básicas (BRE) por padrão e expressões regulares estendidas (ERE) com `-E`. Esta lição apresenta construções compartilhadas pelas duas e depois destaca algumas extensões comuns de ERE.

Use esta entrada nos exemplos:

```text
sally sells seashells
by the seashore
```

## Correspondência de Texto Literal

A maioria dos caracteres comuns corresponde a si mesma. O padrão `seashells` seleciona uma linha que contenha essa sequência exata em qualquer posição:

```bash
$ grep 'seashells' sample.txt
sally sells seashells
```

Coloque padrões regex entre aspas para que o shell não os expanda nem os divida antes que a ferramenta os receba. Regex também é diferente da expansão de caminhos do shell: em uma regex, `*` repete o átomo anterior; em um glob do shell, `*` é por si só um curinga para uma sequência de caracteres de um caminho.

## Ancoragem de uma Correspondência

Fora de uma expressão entre colchetes, `^` no início de um padrão ancora a correspondência no início da linha:

```plaintext
^by
```

A âncora `$` corresponde ao final de uma linha:

```plaintext
seashore$
```

Combine as duas âncoras quando a linha inteira precisar se ajustar ao padrão:

```text
^by the seashore$
```

## Correspondência de um Caractere

O ponto corresponde a um caractere no modo comum de regex orientada por linhas:

```plaintext
b.
```

Isso corresponde a `by`, mas também pode corresponder a `ba` ou `b7`. Não corresponde a um `b` isolado, pois exige um caractere depois dele. Para corresponder a um ponto literal, use o escape `\.` ou coloque-o em uma expressão adequada entre colchetes.

## Uso de Expressões entre Colchetes

Uma expressão entre colchetes corresponde a um caractere de um conjunto especificado:

```plaintext
s[ae]lls
```

Isso corresponde a `sells` ou `salls` nessa posição.

Quando `^` é o primeiro caractere depois de `[`, ele nega o conjunto:

```plaintext
s[^e]lls
```

Isso corresponde a `salls`, mas não a `sells`, pois o caractere depois do primeiro `s` não pode ser `e`.

Intervalos podem descrever caracteres entre dois extremos:

```plaintext
d[a-c]g
```

Isso pode corresponder a `dag`, `dbg` ou `dcg`. O comportamento dos intervalos pode depender da ordenação do locale. Classes como `[[:lower:]]`, `[[:upper:]]` e `[[:digit:]]` muitas vezes expressam a intenção com mais clareza.

## Repetição e Combinação de Padrões

Tanto em BRE quanto em ERE, `*` significa zero ou mais repetições do átomo anterior:

```text
seashells*
```

Isso corresponde a `seashell` seguido de zero ou mais caracteres `s` adicionais. No modo ERE com `grep -E`, alguns operadores comuns são:

- `+`: uma ou mais repetições;
- `?`: zero ou uma repetição;
- `|`: a expressão à esquerda ou à direita;
- `(...)`: agrupa expressões.

Por exemplo:

```bash
$ grep -E '^(cat|dog)s?$' animals.txt
```

Esse comando seleciona linhas completas iguais a `cat`, `cats`, `dog` ou `dogs`. No modo BRE, esses operadores possuem regras de escape diferentes; portanto, não copie um padrão entre variantes sem verificá-lo.

Para praticar a seleção com regex nas ferramentas de texto do Linux, experimente estes laboratórios:

1. **[Pesquisa de Texto com grep no Linux](https://labex.io/labs/comptia-search-text-with-grep-in-linux-590841)** — Neste laboratório, você aprenderá a pesquisar textos em arquivos de um sistema Linux com o comando `grep`. Você realizará pesquisas básicas, exibirá números de linha, usará âncoras como `^` e `$` para corresponder a posições nas linhas e aplicará expressões regulares básicas e estendidas para correspondências complexas de padrões.
2. **[Processamento de Texto e Expressões Regulares](https://labex.io/labs/linux-text-processing-and-regular-expressions-18003)** — Aprenda ferramentas como `grep`, `sed` e `awk` e use expressões regulares para manipular e corresponder textos.
3. **[Extração de E-mails e Números](https://labex.io/labs/linux-extracting-mails-and-numbers-17991)** — Neste desafio, você aprenderá a usar `grep` e expressões regulares para extrair endereços de e-mail e números de um arquivo, demonstrando habilidades essenciais de processamento de texto no Linux.