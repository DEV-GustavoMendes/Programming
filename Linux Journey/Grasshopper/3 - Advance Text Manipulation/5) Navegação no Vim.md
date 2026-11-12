# Navegação no Vim

Aprenda a se mover por caracteres, palavras, linhas e posições do arquivo no modo Normal do Vim.

O Vim oferece movimentos pelo teclado que funcionam em um terminal sem exigir um mouse. Algumas configurações também aceitam a entrada do mouse, mas aprender os movimentos torna a navegação combinável com comandos de edição.

Pressione `Esc` antes de praticar para retornar ao modo Normal.

## Movimento por Caracteres e Linhas da Tela

Os movimentos fundamentais do modo Normal são:

- `h`: move um caractere para a esquerda.
- `j`: move uma linha para baixo.
- `k`: move uma linha para cima.
- `l`: move um caractere para a direita.

As teclas de seta normalmente realizam movimentos semelhantes, mas `h`, `j`, `k` e `l` mantêm suas mãos próximas dos outros comandos. Em uma linha visual quebrada, `j` e `k` normalmente se movem pelas linhas do arquivo; `gj` e `gk` se movem pelas linhas exibidas na tela.

## Prefixo Numérico nos Movimentos

Digite uma quantidade positiva antes de muitos movimentos para repeti-los. Por exemplo:

```text
5j
3l
```

`5j` desce cinco linhas, enquanto `3l` avança três posições de caracteres para a direita quando possível. As quantidades também se combinam com palavras e comandos de edição.

`5j` desce cinco linhas, enquanto `3l` avança três posições de caracteres para a direita quando possível. As quantidades também se combinam com palavras e comandos de edição.

## Movimento por Palavras

Alguns movimentos úteis por palavras são:

- `w`: vai ao início da próxima palavra.
- `b`: vai ao início da palavra atual ou anterior.
- `e`: vai ao final da palavra atual ou seguinte.

`W`, `B` e `E` maiúsculos usam WORDS delimitadas por espaços em branco e tratam a pontuação de forma diferente. Use uma quantidade para percorrer várias palavras, como `3w`.

## Movimento Dentro de uma Linha

Estes movimentos indicam posições da linha atual:

- `0`: vai à coluna zero.
- `^`: vai ao primeiro caractere não vazio.
- `$`: vai ao final da linha.

A diferença entre `0` e `^` importa em linhas com recuo.

## Movimento pelo Arquivo

Use estes comandos do modo Normal para saltos maiores:

- `gg`: vai à primeira linha.
- `G`: vai à última linha.
- `42G`: vai à linha 42.
- `Ctrl+F`: avança aproximadamente uma tela.
- `Ctrl+B`: recua aproximadamente uma tela.

O comando `:42`, seguido de Enter, é outra forma de ir à linha 42.

Para praticar a navegação pelo teclado enquanto edita um arquivo descartável, experimente este laboratório:

1. **[Edição de Arquivos de Texto no Linux com Vim e Nano](https://labex.io/labs/comptia-edit-text-files-in-linux-with-vim-and-nano-591076)** — Pratique a criação, edição, gravação e navegação de arquivos com Vim e Nano em um ambiente Linux real.
