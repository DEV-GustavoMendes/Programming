# Padrões de Pesquisa no Vim

Aprenda a pesquisar para a frente ou para trás no Vim e repetir, refinar ou limpar correspondências.

O Vim pesquisa padrões a partir da posição atual do cursor. Comece no modo Normal, inicie uma pesquisa para a frente ou para trás e depois repita as correspondências sem redigitar o padrão.

## Pesquisa para a Frente

No modo Normal, digite `/`, insira um padrão e pressione Enter. O Vim avança para a próxima correspondência depois do cursor:

```vim
/pretty
```

As pesquisas usam a sintaxe de expressões regulares do Vim; portanto, caracteres como `.`, `*`, `[` e `\` podem ter significados especiais. Use `\V` no início quando o restante do padrão deva ser tratado como “very nomagic”, ou escape os caracteres especiais conscientemente.

## Pesquisa para Trás

Digite `?`, insira um padrão e pressione Enter para ir à correspondência anterior ao cursor:

```vim
?pretty
```

Isso não significa necessariamente “a última correspondência do arquivo”. O resultado depende da posição atual do cursor. Com a configuração padrão `wrapscan` do Vim, uma pesquisa pode recomeçar no início ou no final; `:set nowrapscan` desativa essa continuidade.

## Repetição de uma Pesquisa

Depois de qualquer tipo de pesquisa:

- Pressione `n` para repetir na direção original.
- Pressione `N` para repetir na direção oposta.

Portanto, depois de `/pretty`, `n` avança e `N` recua. Depois de `?pretty`, `n` recua e `N` avança.

## Pesquisa da Palavra sob o Cursor

No modo Normal, coloque o cursor sobre uma palavra e use:

- `*` para pesquisá-la inteira para a frente;
- `#` para pesquisá-la inteira para trás.

Esses comandos definem o padrão de pesquisa mais recente; assim, `n` e `N` podem continuar a partir dele.

## Controle de Maiúsculas e do Destaque

Opções do Vim podem mudar o comportamento de maiúsculas e minúsculas:

- `:set ignorecase` faz as pesquisas ignorarem a diferença;
- `:set smartcase` restaura a sensibilidade quando o padrão contém uma letra maiúscula e `ignorecase` também está ativo;
- `\c` dentro de um padrão força essa pesquisa a ignorar a diferença;
- `\C` força essa pesquisa a respeitá-la.

Por exemplo, `/\cerror` corresponde a `error`, `Error` e `ERROR`, independentemente das opções atuais.

Quando o destaque das pesquisas está ativo, `:nohlsearch` limpa os destaques visuais atuais sem excluir o padrão. A próxima pesquisa ou repetição poderá destacá-los novamente.

Para praticar a navegação e a pesquisa do Vim em um arquivo controlado, experimente este laboratório:

1. **[Edição de Arquivos de Texto no Linux com Vim e Nano](https://labex.io/labs/comptia-edit-text-files-in-linux-with-vim-and-nano-591076)** — Pratique a criação, edição, gravação, navegação e pesquisa de arquivos com Vim e Nano.