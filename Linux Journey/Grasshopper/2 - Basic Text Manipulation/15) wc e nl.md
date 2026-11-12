# wc e nl

Aprenda a contar linhas, palavras, bytes ou caracteres com wc e numerar linhas com nl.

O comando `wc` conta propriedades de fluxos de texto, enquanto `nl` grava a entrada com números de linha gerados. Os dois leem arquivos ou stdin e enviam seus resultados a stdout.

## Leitura da Saída Padrão de wc

Sem uma opção de contagem, `wc` mostra a quantidade de caracteres de nova linha, palavras e bytes, seguida do nome do arquivo quando um foi fornecido:

```bash
$ printf 'red blue\ngreen\n' > colors.txt
$ wc colors.txt
 2  3 15 colors.txt
```

Da esquerda para a direita:

1. `2` caracteres de nova linha, informados como linhas.
2. `3` palavras delimitadas por espaços em branco.
3. `15` bytes neste exemplo ASCII.

Uma linha final de texto sem uma nova linha de terminação não é contada por `wc -l`, pois essa opção conta caracteres de nova linha, não linhas percebidas visualmente.

## Solicitação de uma Única Contagem

Selecione apenas a medida necessária:

- `-l`: conta caracteres de nova linha.
- `-w`: conta palavras.
- `-c`: conta bytes.
- `-m`: conta caracteres de acordo com o locale atual.

Por exemplo:

```bash
$ wc -w colors.txt
3 colors.txt
```

As quantidades de bytes e caracteres são iguais em textos ASCII, mas podem ser diferentes em codificações multibyte como UTF-8. Quando stdin é usada sem um operando de nome de arquivo, `wc` normalmente omite o rótulo do nome:

```bash
$ printf 'one two\n' | wc -w
2
```

Quando vários arquivos são indicados, `wc` mostra um resultado para cada um e uma linha `total`. O GNU `wc -L` informa a largura máxima de exibição de uma linha da entrada.

## Numeração de Linhas Não Vazias com nl

Por padrão, `nl` numera as linhas não vazias do corpo lógico da entrada. Suponha que `notes.txt` contenha uma segunda linha vazia:

```text
alpha

beta
```

A linha vazia é preservada, mas não recebe um número:

```bash
$ nl notes.txt
     1	alpha

     2	beta
```

`nl` grava uma saída numerada; ele não modifica `notes.txt`.

## Numeração de Todas as Linhas

Use `-ba` para selecionar o estilo de corpo `a`, que numera todas as linhas:

```bash
$ nl -ba notes.txt
     1	alpha
     2
     3	beta
```

Outras opções controlam a formatação. Por exemplo, `-w 3` define a largura do campo numérico, e `-s ': '` muda o separador depois do número.

Para praticar a contagem e a numeração de textos, experimente estes laboratórios:

1. **[Comando wc do Linux: Contagem de Texto](https://labex.io/labs/linux-linux-wc-command-text-counting-219200)** — Pratique a contagem de palavras, linhas e caracteres em arquivos de texto com `wc`.
2. **[Comando nl do Linux: Numeração de Linhas](https://labex.io/labs/linux-linux-nl-command-line-numbering-210988)** — Aprenda a numerar as linhas dos arquivos com `nl`.
3. **[Contagem e Ordenação de Palavras](https://labex.io/labs/linux-word-count-and-sorting-388125)** — Aplique `wc` para contar linhas, palavras e caracteres e combine-o com ordenação em análises práticas.