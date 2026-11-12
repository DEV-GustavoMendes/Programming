# cut

Aprenda a selecionar posições de caracteres ou campos delimitados de cada linha com cut.

O comando `cut` seleciona posições de caracteres ou campos especificados em cada linha da entrada. Ele funciona melhor com textos de estrutura consistente cujos delimitadores e posições de campos são conhecidos.

Crie um pequeno arquivo separado por tabulações para os exemplos. `printf` interpreta `\t` como uma tabulação literal e `\n` como uma nova linha:

```bash
$ printf 'name\trole\nalice\tadmin\nbob\tviewer\n' > team.tsv
```

## Seleção de Posições de Caracteres

Use `-c LIST` para selecionar posições de cada linha. As posições começam em 1:

```bash
$ cut -c 1 team.tsv
n
a
b
```

A lista pode conter posições individuais e intervalos:

```bash
$ cut -c 1-4 team.tsv
name
alic
bob
$ cut -c 1,3 team.tsv
nm
ai
bb
```

Espaços, tabulações e sinais de pontuação também ocupam posições. `cut` processa cada linha independentemente.

## Seleção de Campos Delimitados por Tabulação

Use `-f LIST` para selecionar campos. O delimitador padrão é uma tabulação:

```bash
$ cut -f 2 team.tsv
role
admin
viewer
```

Assim como na seleção de caracteres, uma lista pode incluir valores como `1`, `1,3`, `2-4`, `-3` ou `2-`.

## Escolha de um Delimitador Personalizado

Use `-d CHARACTER` com `-f` quando os campos utilizarem outro delimitador. Este exemplo cria dados separados por ponto e vírgula:

```bash
$ printf 'alice;admin\nbob;viewer\n' > team.txt
$ cut -d ';' -f 1 team.txt
alice
bob
```

O delimitador dessa forma é um único caractere. Coloque `;` entre aspas, pois um ponto e vírgula sem aspas possui significado de controle no shell.

## Tratamento de Linhas sem o Delimitador

No modo de campos, `cut` normalmente mostra uma linha inalterada quando ela não contém o delimitador. Acrescente `-s` para suprimir essas linhas:

```bash
$ printf 'alice;admin\nheader\nbob;viewer\n' | cut -s -d ';' -f 2
admin
viewer
```

Isso não valida um arquivo CSV genérico. CSV pode conter delimitadores entre aspas, novas linhas incorporadas e regras de escape que uma divisão por um único caractere não compreende; use uma ferramenta compatível com CSV para esses dados.

## Leitura de stdin

Quando nenhum arquivo é indicado, ou quando `-` é usado como operando de entrada, `cut` lê stdin. Isso o torna uma etapa natural de um pipeline:

```bash
$ printf 'red:1\nblue:2\n' | cut -d ':' -f 1
red
blue
```

Para praticar a seleção por posição e campo, experimente estes laboratórios:

1. **[Comando cut do Linux: Recorte de Texto](https://labex.io/labs/linux-linux-cut-command-text-cutting-219187)** — Este laboratório oferece uma introdução prática e direta ao comando `cut`, permitindo que você pratique a extração de colunas ou campos específicos de arquivos de texto, exatamente como apresentado na lição.
2. **[Controle de Sequência e Pipeline](https://labex.io/labs/linux-sequence-control-and-pipeline-17994)** — Aumente sua eficiência na linha de comando aprendendo a controlar sequências de execução de comandos, usar pipelines e aproveitar ferramentas avançadas de processamento de texto como `cut`, `grep`, `wc`, `sort` e `uniq`.