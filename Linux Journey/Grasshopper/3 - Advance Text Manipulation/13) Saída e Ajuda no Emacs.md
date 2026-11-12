# Saída e Ajuda no Emacs

Aprenda a sair do Emacs com segurança, cancelar comandos pendentes, consultar tópicos de ajuda e desfazer alterações.

O Emacs oferece ajuda contextual para teclas, funções, variáveis e modos ativos. Ele também protege os buffers modificados associados a arquivos durante a saída, oferecendo a oportunidade de salvar ou recusar cada gravação.

## Saída do Emacs

Use `C-x C-c`, que executa `save-buffers-kill-terminal`, para solicitar o fechamento da sessão ou da conexão de terminal do Emacs:

```text
C-x C-c
```

O Emacs verifica os buffers modificados relevantes associados a arquivos e pergunta se deve salvá-los. Leia o nome de cada buffer e responda conscientemente. Ele também pode perguntar sobre processos ativos. Cancele a saída se precisar inspecionar o trabalho antes de decidir.

Em um fluxo com `emacsclient` ou um servidor do Emacs, o comportamento exato do frame e do servidor pode variar, mas as perguntas sobre buffers modificados continuam merecendo atenção.

## Abertura do Seletor de Ajuda

O prefixo padrão da ajuda é `C-h`. Use `C-h C-h`, que executa a ajuda sobre a própria ajuda, para exibir orientações sobre os comandos disponíveis:

```text
C-h C-h
```

A segunda tecla escolhe o tipo de ajuda necessária.

## Descrição de Teclas e do Estado do Editor

Alguns comandos úteis de ajuda são:

- `C-h k KEY`: descreve o que uma sequência de teclas executa.
- `C-h f FUNCTION`: descreve uma função Emacs Lisp.
- `C-h v VARIABLE`: descreve uma variável Emacs Lisp.
- `C-h m`: descreve os modos principal e secundários atuais.
- `C-h t`: abre o tutorial interativo.

Por exemplo, digite `C-h k C-x C-s` para ver a documentação da associação de `save-buffer`.

## Cancelamento de um Comando Pendente

Use `C-g`, associado a `keyboard-quit`, quando estiver preso em uma pergunta, sequência parcialmente inserida, pesquisa incremental ou outro comando que queira cancelar:

```text
C-g
```

Ele não desfaz alterações do buffer que já ocorreram nem sai do Emacs. Ele interrompe a interação atual e devolve o controle à edição comum quando possível.

## Como Desfazer Alterações do Buffer

Use `C-/`, `C-_` ou `C-x u` para invocar o recurso de desfazer em configurações comuns do Emacs:

```text
C-/
```

Comandos repetidos de desfazer percorrem para trás as alterações recentes do buffer. O movimento do cursor por si só normalmente não é uma alteração. Versões e configurações do Emacs podem oferecer `undo-redo` e ferramentas de histórico mais avançadas; use `C-h k` nas associações reais de desfazer e refazer para verificar o comportamento local.

Pratique abrindo `*scratch*`, fazendo uma alteração descartável, usando desfazer, consultando `C-h k` sobre uma tecla desconhecida e cancelando uma pergunta do minibuffer com `C-g` antes de sair normalmente.
