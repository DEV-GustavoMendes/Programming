# history

Aprenda a inspecionar, pesquisar, reutilizar e gerenciar o histórico de comandos no Bash.

Shells interativos podem manter um registro dos comandos digitados. Esta lição se concentra no Bash, em que o comando interno `history` exibe e gerencia esse registro. Outros shells podem usar atalhos, arquivos ou configurações diferentes.

## Visualização do Histórico do Bash

Execute `history` para exibir a lista atual:

```bash
$ history
  101  pwd
  102  ls -la
  103  cat notes.txt
```

Cada linha possui um número de histórico seguido do comando.

## Reutilização de Comandos Anteriores

O Bash oferece vários atalhos para recuperar ou executar comandos imediatamente:

- **Seta para cima**: recupera comandos anteriores para revisão ou edição.
- **`!!`**: expande e executa o comando mais recente.
- **Executar pelo número**: use `!102` para executar o comando número 102 do histórico.
- **Executar pelo prefixo**: use `!cat` para executar o comando mais recente que começou com `cat`.

As formas de expansão do histórico iniciadas por `!` podem executar um comando assim que você pressiona Enter. Inspecione a correspondência primeiro sempre que houver dúvida, especialmente antes de acrescentar privilégios elevados ou operar em arquivos importantes.

## Pesquisa Interativa no Histórico

Pressione `Ctrl+R` para iniciar uma pesquisa incremental reversa e digite parte do comando desejado. Pressione `Ctrl+R` novamente para ir a uma correspondência mais antiga.

Pressione Enter para executar a correspondência exibida. Se quiser revisá-la ou editá-la primeiro, use uma tecla de seta para colocar o comando na linha de edição.

## Gerenciamento da Lista de Histórico

O comando interno `history` pode modificar ou salvar a lista atual:

- `history -c`: limpa a lista atual do histórico na memória.
- `history -w`: grava a lista atual no arquivo de histórico configurado, geralmente `~/.bash_history`.
- `history -d <offset>`: exclui a entrada na posição indicada.

Exemplos:

```bash
$ history -d 101
$ history -w
```

Limpar a lista na memória não garante, por si só, que os comandos antigos tenham desaparecido de todos os arquivos, backups ou outros shells ativos. O comportamento do histórico também depende das configurações do Bash e do momento em que as sessões leem ou gravam seus arquivos.

## Limpeza da Tela e Conclusão de Nomes

Use `clear` quando quiser uma área visível limpa no terminal:

```bash
$ clear
```

Esse comando não apaga a lista do histórico do Bash. Dependendo do terminal, o conteúdo antigo da tela também pode continuar disponível no histórico de rolagem.

A conclusão com Tab é outra forma de evitar redigitação. Comece a digitar um comando, nome de arquivo ou diretório e pressione Tab. O Bash pode completar uma correspondência inequívoca ou mostrar as possibilidades quando houver mais de uma.

As linhas de comando podem ser armazenadas no histórico; portanto, não coloque senhas, tokens ou outros segredos diretamente nos comandos quando houver um método de entrada mais seguro.