# sort

Aprenda a ordenar linhas de texto por valores lexicais, numéricos ou de campos selecionados com sort.

O comando `sort` lê linhas completas, ordena-as de acordo com as regras de comparação selecionadas e grava o resultado em stdout. Ele não altera o arquivo de entrada, a menos que você escolha explicitamente uma operação de saída.

## Ordenação de Linhas Completas

Considere `animals.txt`:

```text
dog
cow
cat
elephant
bird
```

Ordene as linhas em ordem crescente:

```bash
$ sort animals.txt
bird
cat
cow
dog
elephant
```

A ordenação do texto segue o locale atual, que pode afetar maiúsculas, acentos e pontuação. Use um locale consistente, como `LC_ALL=C`, quando um script exigir uma comparação reproduzível orientada por bytes:

```bash
$ LC_ALL=C sort animals.txt
```

## Inversão do Resultado

Acrescente `-r` para inverter o resultado da comparação:

```bash
$ sort -r animals.txt
elephant
dog
cow
cat
bird
```

## Comparação de Números

A ordem lexical compara caracteres; por isso, `10` normalmente vem antes de `2`. Use `-n` para uma comparação numérica comum:

```bash
$ printf '10\n2\n30\n' | sort -n
2
10
30
```

Combine opções quando necessário. `sort -nr scores.txt` compara numericamente e coloca os valores maiores primeiro.

## Ordenação por um Campo

Use `-k START[,END]` para escolher uma chave. Por padrão, os campos são separados por sequências de espaços. Para registros separados por dois-pontos, use `-t ':'`:

```bash
$ printf 'alice:30\nbob:8\ncarol:20\n' | sort -t ':' -k 2,2n
bob:8
carol:20
alice:30
```

Aqui, `-t ':'` seleciona o delimitador, `-k 2,2` limita a chave ao campo 2 e o `n` anexado compara essa chave numericamente. Sem o `,2` final, uma chave iniciada no campo 2 normalmente continua até o fim da linha.

## Remoção de Duplicatas e Salvamento da Saída

Use `-u` para mostrar uma linha para cada chave de comparação igual:

```bash
$ sort -u names.txt
```

Isso ordena e remove duplicatas segundo as regras selecionadas. Se quiser apenas remover duplicatas adjacentes de dados já ordenados, o comando `uniq`, abordado mais adiante, pode fazer isso.

Para gravar o resultado em um arquivo, o redirecionamento comum é adequado quando o destino é diferente da entrada:

```bash
$ sort names.txt > names-sorted.txt
```

Não execute `sort names.txt > names.txt`; o shell trunca a entrada antes que `sort` a leia. O GNU `sort -o names.txt names.txt` organiza sua própria saída com segurança quando você pretende usar o mesmo caminho:

```bash
$ sort -o names.txt names.txt
```

Mantenha um backup ou grave e verifique um resultado separado quando os dados originais forem importantes.

Para praticar a ordenação e a análise de dados orientados por linhas, experimente estes laboratórios:

1. **[Comando sort do Linux: Ordenação de Texto](https://labex.io/labs/linux-linux-sort-command-text-sorting-219196)** — Este laboratório oferece uma introdução direta ao comando `sort`, permitindo que você pratique a ordenação de linhas de arquivos de texto de várias formas, inclusive em ordem crescente e decrescente.
2. **[Contagem e Ordenação de Palavras](https://labex.io/labs/linux-word-count-and-sorting-388125)** — Aplique a ordenação junto com a contagem de palavras para analisar dados, encontrar padrões frequentes e organizar resultados.
