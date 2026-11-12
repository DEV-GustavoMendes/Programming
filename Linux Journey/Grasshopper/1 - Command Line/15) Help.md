# help

Aprenda a escolher entre a ajuda integrada, a saída de uso do programa e as páginas de manual de um comando.

Você não precisa memorizar todas as opções dos comandos. O Bash e muitos programas instalados podem explicar sua sintaxe diretamente no terminal, mas a fonte de ajuda adequada depende do tipo de comando usado.

## Obtenção de Ajuda para Comandos Internos do Bash

O Bash fornece o comando interno `help` para comandos implementados pelo próprio shell. Alguns exemplos são `cd`, `history` e `type`.

Forneça o nome do comando interno como argumento:

```bash
$ help echo
```

A saída descreve a sintaxe e o comportamento do comando interno. Executar `help` sem um argumento lista os comandos internos para os quais o Bash possui ajuda.

## Solicitação do Resumo de Uso de um Programa

Muitos programas externos seguem a convenção de aceitar `--help` e mostrar um resumo de uso:

```bash
$ ls --help
```

Essa convenção é comum, mas não universal. Leia a saída e o status de encerramento, em vez de presumir que todos os programas aceitam a mesma opção.

## Descoberta de Como o Bash Resolve um Nome

Use `type` para descobrir se o Bash resolve um nome como comando interno, alias, função, palavra-chave ou arquivo executável:

```bash
$ type cd
cd is a shell builtin
$ type ls
ls is /usr/bin/ls
```

O resultado exato pode variar conforme os aliases, as funções, os programas instalados e o `PATH`. Use `type -a NAME` quando quiser que o Bash mostre todas as resoluções conhecidas, não apenas a primeira que usaria.

## Escolha do Nível de Detalhe

- Use `help COMMAND` para um comando interno do Bash.
- Use `COMMAND --help` para obter um resumo rápido de muitos comandos externos.
- Use `man COMMAND` para uma página de manual instalada com documentação mais detalhada.
- Use `whatis COMMAND` para uma descrição de uma linha.

As próximas lições examinam com mais detalhes as páginas de manual e as descrições de uma linha.