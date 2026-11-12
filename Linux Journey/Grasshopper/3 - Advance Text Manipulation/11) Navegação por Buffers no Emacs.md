# Navegação por Buffers no Emacs

Aprenda a alternar e encerrar buffers do Emacs enquanto divide, seleciona e fecha janelas de exibição.

Um buffer do Emacs mantém texto ou estado do editor, enquanto uma janela exibe um buffer. Um buffer pode existir sem estar visível, e várias janelas podem exibir o mesmo buffer. Gerenciar um desses objetos não gerencia automaticamente o outro.

## Alternância entre Buffers

Use `C-x b`, que executa `switch-to-buffer`, para selecionar pelo nome um buffer na janela atual:

```text
C-x b
```

O minibuffer oferece conclusão para os nomes existentes. Inserir um novo nome pode criar um buffer sem arquivo com esse nome; isso não visita um caminho de arquivo.

Por padrão, `C-x Right` executa `next-buffer`, e `C-x Left` executa `previous-buffer`, percorrendo os buffers na janela selecionada.

## Divisão da Janela Selecionada

Use `C-x 2` para dividir a janela selecionada em uma janela superior e outra inferior:

```text
C-x 2
```

Use `C-x 3` para dividi-la em janelas à esquerda e à direita:

```text
C-x 3
```

A nova janela inicialmente exibe um buffer, muitas vezes o mesmo. Você pode alternar os buffers de cada janela independentemente.

## Seleção e Fechamento de Janelas

Use `C-x o`, que executa `other-window`, para selecionar a próxima janela:

```text
C-x o
```

Use estes comandos para remover exibições:

- `C-x 0`: exclui a janela selecionada.
- `C-x 1`: exclui as outras janelas do frame atual.

Excluir uma janela normalmente mantém vivo o buffer exibido nela. Você pode mostrar esse buffer novamente em outra janela.

## Encerramento de um Buffer

Use `C-x k`, que executa `kill-buffer`, para solicitar um buffer a remover do Emacs:

```text
C-x k
```

O buffer atual é a escolha padrão. Se um buffer associado a um arquivo possuir alterações não salvas, o Emacs avisará antes de encerrá-lo. Leia a pergunta: encerrar um buffer modificado pode descartar edições.

Encerrar um buffer é diferente de excluir uma janela. O Emacs substitui um buffer encerrado em qualquer janela que o exiba, enquanto excluir uma janela pode manter o buffer intacto.

Pratique esses comandos com `*scratch*` e buffers descartáveis. Antes de encerrar qualquer buffer associado a um arquivo, confirme se o indicador de modificação mostra trabalho não salvo.