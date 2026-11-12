# Modificação de Permissões

Aprenda a alterar os bits de permissão do Linux com modos simbólicos e octais de `chmod`.

O comando `chmod` altera os bits de modo de arquivos e diretórios. Normalmente, apenas o proprietário do arquivo ou um processo com os privilégios necessários pode realizar essa alteração. Inspecione o modo atual com `ls -l` antes e depois de executar `chmod`.

## Uso do Modo Simbólico

Um modo simbólico indica qual classe de permissão será alterada, como alterá-la e quais permissões estão envolvidas.

- `u` seleciona a classe do proprietário.
- `g` seleciona a classe do grupo.
- `o` seleciona a classe dos outros.
- `a` seleciona as três classes.
- `+` adiciona permissões, `-` as remove e `=` define exatamente a classe selecionada.

Por exemplo, adicione a permissão de execução para o proprietário:

```bash
$ chmod u+x myfile
```

Remova a permissão de escrita do grupo:

```bash
$ chmod g-w myfile
```

Adicione a permissão de escrita para o proprietário e para o grupo:

```bash
$ chmod ug+w myfile
```

Várias cláusulas podem ser separadas por vírgulas. Este comando define leitura e escrita para o proprietário, somente leitura para o grupo e nenhuma permissão para os outros:

```bash
$ chmod u=rw,g=r,o= myfile
```

Se a classe for omitida, como em `chmod +x myfile`, o umask do processo afeta quais classes são alteradas. Indicar a classe explicitamente facilita a revisão do resultado pretendido.

## Uso do Modo Octal

Um modo octal define cada trio básico de permissões com um dígito. Some estes valores dentro de cada classe:

- `4` para leitura
- `2` para escrita
- `1` para execução
- `0` para nenhuma permissão

Os três dígitos mais à direita representam o proprietário, o grupo e os outros, nessa ordem. Por exemplo:

```bash
$ chmod 755 myfile
```

O modo `755` se expande da seguinte forma:

- O `7` do proprietário é `4 + 2 + 1`, ou `rwx`.
- O `5` do grupo é `4 + 1`, ou `r-x`.
- O `5` dos outros é `4 + 1`, ou `r-x`.

Ao contrário das operações simbólicas com `+` ou `-`, um modo octal fornece o conjunto completo de permissões comuns. Uma lição posterior aborda o dígito inicial opcional usado para os bits de modo especiais.

## Aplicação Segura das Alterações

Conceda somente o acesso necessário aos usuários e serviços. Evite usar `chmod 777` como atalho de solução de problemas, pois ele concede leitura, escrita e execução a todas as classes, frequentemente criando mais riscos sem resolver questões de propriedade, travessia de diretórios, ACLs ou políticas de serviços.

Alterações recursivas exigem cuidados adicionais. Examine previamente a árvore de destino, considere links simbólicos e sistemas de arquivos montados e teste em um escopo pequeno antes de usar `chmod -R`. Após uma alteração, verifique o modo resultante em vez de presumir que o comando afetou os objetos pretendidos.

Para praticar em um ambiente isolado, use o laboratório [Usuários, Grupos e Permissões de Arquivos no Linux](https://labex.io/labs/linux-linux-user-group-and-file-permissions-18002) e inspecione cada modo antes e depois de alterá-lo.