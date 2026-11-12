# Emacs

Aprenda a iniciar o Emacs, interpretar sua notação de teclas e diferenciar buffers, janelas e frames.

O GNU Emacs é um editor de texto extensível cujo comportamento pode ser personalizado com Emacs Lisp. Ele oferece edição de texto simples, modos de programação, gerenciamento de arquivos e buffers e muitos pacotes opcionais. Você pode aprender seus comandos essenciais de edição sem adotar todas as extensões.

## Verificação e Inicialização do Emacs

Não presuma que o Emacs está instalado. Verifique como o shell o resolve:

```bash
$ command -v emacs
/usr/bin/emacs
```

Inicie o Emacs com sua seleção normal de exibição:

```bash
$ emacs
```

Em uma sessão gráfica, isso pode criar um frame gráfico. Use `-nw`, abreviação de no window system, quando o Emacs deva permanecer dentro do terminal atual:

```bash
$ emacs -nw
```

## Abertura de um Arquivo

Forneça um caminho para visitar um arquivo quando o Emacs for iniciado:

```bash
$ emacs notes.txt
```

Se o arquivo existir, o Emacs o lerá em um buffer. Se estiver ausente, criará um novo buffer associado ao caminho; o arquivo só será criado depois de um salvamento bem-sucedido. As permissões do sistema de arquivos continuam determinando se a gravação pode ter sucesso.

## Compreensão de Buffers, Janelas e Frames

O Emacs usa objetos relacionados, mas distintos:

- Um **buffer** mantém texto ou outro estado do editor. O conteúdo de um arquivo visitado vive em um buffer.
- Uma **janela** é uma área dentro de um frame do Emacs que exibe um buffer.
- Um **frame** é uma exibição de nível superior do Emacs, como um frame gráfico ou de terminal.

Vários buffers podem existir sem estar visíveis, e duas janelas podem exibir o mesmo buffer. Fechar uma janela não necessariamente encerra seu buffer nem exclui um arquivo.

## Compreensão de Buffers, Janelas e Frames

O Emacs usa objetos relacionados, mas distintos:

- Um **buffer** mantém texto ou outro estado do editor. O conteúdo de um arquivo visitado vive em um buffer.
- Uma **janela** é uma área dentro de um frame do Emacs que exibe um buffer.
- Um **frame** é uma exibição de nível superior do Emacs, como um frame gráfico ou de terminal.

Vários buffers podem existir sem estar visíveis, e duas janelas podem exibir o mesmo buffer. Fechar uma janela não necessariamente encerra seu buffer nem exclui um arquivo.

## Inicialização do Tutorial Integrado

Dentro do Emacs, digite `C-h t` para abrir o tutorial interativo. Ele ensina movimentação, inserção, salvamento e saída em um buffer seguro de prática. `C-h` é o prefixo da ajuda; `C-h C-h` mostra ajuda sobre como usar a ajuda.

Se o Emacs exibir um menu ou buffer de boas-vindas, o tutorial ainda será um ponto de partida mais estruturado do que experimentar em um arquivo importante.

