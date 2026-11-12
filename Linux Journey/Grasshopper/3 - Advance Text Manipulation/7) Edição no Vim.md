# Edição no Vim

Aprenda como o Vim combina operadores, movimentos, registros, inserções e comandos de desfazer na edição de texto.

Os comandos de edição do Vim frequentemente combinam um operador com um movimento ou objeto de texto. Essa gramática permite aplicar as mesmas ações a caracteres, palavras, linhas e escopos maiores. Pressione `Esc` antes de praticar para retornar ao modo Normal.

## Combinação de um Operador com um Movimento

A forma geral é:

```text
[count] operator [count] motion
```

Alguns operadores comuns são:

- `d`: exclui texto.
- `c`: altera o texto e entra no modo de Inserção.
- `y`: copia, ou “yank”, o texto.

Por exemplo, `dw` exclui pelo movimento `w`, enquanto `d$` exclui do cursor até o final da linha. `2dw` aplica a exclusão ao longo de dois movimentos por palavras.

## Edição de Caracteres e Linhas

Alguns comandos são atalhos convenientes:

- `x`: exclui o caractere sob o cursor.
- `dd`: exclui a linha atual como linha inteira.
- `3dd`: exclui três linhas a partir da atual.
- `cc`: altera a linha atual e entra no modo de Inserção.
- `r{char}`: substitui o caractere sob o cursor por `{char}`.
- `R`: entra no modo de Substituição até que `Esc` seja pressionado.

Repetir um operador, como em `dd`, torna a ação orientada por linha. Uma quantidade amplia o número de linhas.

## Alteração de Texto e Entrada no Modo de Inserção

O operador `c` remove o texto selecionado e entra no modo de Inserção para que você digite a substituição:

- `ce`: altera até o final da palavra.
- `c$`: altera até o final da linha.
- `cc`: altera toda a linha atual.
- `ciw`: altera a palavra interna sob o cursor.
- `caw`: altera um objeto de palavra, incluindo os espaços ao redor conforme a definição do Vim.

O comportamento de `cw` possui um caso especial histórico e frequentemente age como `ce`. Objetos de texto como `iw` podem deixar o limite pretendido mais claro.

## Cópia e Inserção de Texto

O Vim chama a cópia de **yank** e a colagem de **put**:

- `yw`: copia ao longo de um movimento por palavra.
- `yy`: copia a linha atual.
- `p`: insere depois do cursor para texto por caracteres ou abaixo da linha atual para texto por linhas.
- `P`: insere antes do cursor ou acima da linha atual.

Exclusões e alterações também armazenam texto em registros; por isso, um `p` posterior pode inserir o texto excluído mais recentemente, não uma cópia anterior. Registros nomeados permitem preservar textos específicos, mas comece observando o que a última operação armazenou.

## Desfazer, Refazer e Repetir

No modo Normal:

- `u`: desfaz a alteração mais recente.
- `Ctrl+R`: refaz uma alteração desfeita.
- `.`: repete a alteração mais recente no local atual quando aplicável.
- `J`: une a linha atual à seguinte.

O histórico de desfazer se aplica a alterações no buffer, não apenas a movimentos do cursor. Salve pontos de controle e examine as edições em vez de depender de um histórico ilimitado ou permanente.

Para praticar operadores, movimentos e recuperação em textos descartáveis, experimente este laboratório:

1. **[Edição de Arquivos de Texto no Linux com Vim e Nano](https://labex.io/labs/comptia-edit-text-files-in-linux-with-vim-and-nano-591076)** — Pratique a criação de arquivos, a edição de texto, o salvamento de arquivos e a navegação tanto com vi/vim quanto com nano. Este laboratório ajudará você a aplicar em situações reais conceitos como excluir, alterar, copiar e inserir texto.