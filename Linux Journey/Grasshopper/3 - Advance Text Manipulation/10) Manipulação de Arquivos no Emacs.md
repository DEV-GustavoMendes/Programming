# Manipulação de Arquivos no Emacs

Aprenda a visitar, salvar, renomear, recarregar e revisar buffers associados a arquivos no Emacs.

O Emacs visita arquivos em buffers. A edição altera primeiro o buffer; o salvamento grava seu conteúdo atual no caminho associado. Leia as mensagens do minibuffer, pois permissões, alterações conflitantes no disco ou outros erros podem impedir uma gravação.

## Visita a um Arquivo

Use `C-x C-f`, que executa `find-file`, insira um caminho no minibuffer e pressione Enter:

```text
C-x C-f
```

O Emacs abre um arquivo existente e legível em um buffer ou prepara um novo buffer de visita quando o caminho está ausente. No segundo caso, nenhum arquivo existe no disco até que um salvamento tenha sucesso.

Você pode usar a conclusão com Tab enquanto insere o caminho. Visitar um diretório normalmente abre o Dired, o editor de diretórios do Emacs, em vez de tratá-lo como arquivo de texto.

## Salvamento do Buffer Atual

Use `C-x C-s`, que executa `save-buffer`, para salvar o buffer atual associado a um arquivo:

```text
C-x C-s
```

Se o buffer não possuir um nome associado, o Emacs solicitará um. Uma gravação bem-sucedida limpa o indicador de modificação; uma falha mantém os dados não salvos no buffer e informa um erro.

## Gravação com Outro Nome

Use `C-x C-w`, que executa `write-file`, para solicitar um caminho, gravar o buffer nele e fazer o buffer visitar esse novo arquivo:

```text
C-x C-w
```

Esse é o comportamento “Salvar como” do Emacs. Ele difere de simplesmente gravar uma cópia separada e continuar visitando o caminho original.

## Revisão de Vários Buffers Modificados

Use `C-x s`, que executa `save-some-buffers`, para examinar os buffers modificados associados a arquivos:

```text
C-x s
```

O Emacs normalmente pergunta se deve salvar cada buffer modificado elegível. Leia o nome do buffer e responda conscientemente; esse não é um atalho incondicional para salvar tudo.

## Reversão a Partir do Disco

Se um arquivo mudou no disco e você quiser conscientemente descartar o conteúdo atual do buffer, execute `M-x revert-buffer` e examine a confirmação. A reversão pode destruir edições não salvas; por isso, use-a somente depois de confirmar qual fonte deve prevalecer.

Para comparar antes de decidir, salve uma cópia separada ou use controle de versão e ferramentas de diff. Não trate operações de recarga como inofensivas quando o buffer estiver modificado.

