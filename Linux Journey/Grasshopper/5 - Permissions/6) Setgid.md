# Setgid

Aprenda como o set-group-ID afeta as credenciais de executáveis e a herança de grupos em diretórios compartilhados.

O bit set-group-ID, normalmente chamado de setgid ou SGID, tem duas utilidades importantes. Em um arquivo comum executável, ele pode alterar o ID de grupo efetivo do novo processo. Em um diretório, ele faz com que as entradas recém-criadas herdem o grupo do diretório, algo especialmente útil em árvores colaborativas.

## Setgid em Arquivos Executáveis

Uma listagem longa pode mostrar o setgid na posição de execução do grupo:

```bash
$ ls -l /path/to/program
-rwxr-sr-x 1 root operators 24576 Jan 10 09:30 /path/to/program
```

O `s` minúsculo significa que tanto o setgid quanto a execução do grupo estão definidos. O `S` maiúsculo significa que o setgid está definido, mas a execução do grupo está ausente.

Quando o kernel respeita esse bit durante a execução, o processo recebe um ID de grupo efetivo baseado no grupo proprietário do executável. O comportamento pode ser suprimido por controles como uma montagem `nosuid` e não deve ser tratado como uma garantia universal para todos os tipos de arquivo ou ambientes.

## Setgid em Diretórios

O setgid em um diretório tem uma finalidade diferente. Novos arquivos e subdiretórios normalmente herdam o grupo do diretório, em vez do grupo padrão de quem os criou. No Linux, novos subdiretórios também herdam o bit setgid, ajudando uma árvore de projeto compartilhada a manter um grupo consistente.

O setgid não concede por si só acesso de escrita ao grupo. O modo do diretório, o umask do processo, o modo de criação solicitado, as ACLs padrão e outros controles ainda determinam o acesso.

```bash
$ sudo chgrp developers /srv/project
$ sudo chmod g+s /srv/project
$ ls -ld /srv/project
drwxr-sr-x 2 root developers 4096 Jan 10 09:30 /srv/project
```

## Definição e Remoção do Setgid

Defina o bit simbolicamente com:

```bash
$ sudo chmod g+s myfile
```

Defina-o junto com os bits de modo comuns usando um `2` octal inicial:

```bash
$ sudo chmod 2755 myfile
```

Remova somente o bit especial com `chmod g-s myfile`.

## Uso Seguro de Diretórios Compartilhados

Em um diretório colaborativo, combine o grupo proprietário pretendido, o setgid e bits de acesso escolhidos com cuidado. Teste a criação como usuários representativos e inspecione os resultados com `ls -ld`. Evite tornar uma árvore gravável por todos apenas para resolver problemas de compartilhamento por grupo; um grupo dedicado, um umask ou uma ACL padrão apropriados e um diretório setgid normalmente oferecem um controle mais claro.