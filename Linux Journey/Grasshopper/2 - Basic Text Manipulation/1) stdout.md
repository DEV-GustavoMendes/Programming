# stdout (Saída Padrão)

Aprenda como a saída padrão flui para o terminal e como o Bash a redireciona para arquivos.

Os programas se comunicam por meio de fluxos de entrada e saída. A saída padrão, abreviada como **stdout**, é o fluxo que um programa normalmente usa para seus resultados comuns. Em um terminal, o shell conecta inicialmente esse fluxo à tela do terminal.

## Gravação na Saída Padrão

O comando `echo` grava seus argumentos em stdout:

```bash
$ echo Hello World
Hello World
```

Stdout é o descritor de arquivo `1`, um número que se torna útil ao redirecionar mais de um fluxo. Os programas também podem ter entrada padrão, ou stdin, e erro padrão, ou stderr; as próximas lições examinam esses fluxos.

## Substituição de um Arquivo com >

O Bash interpreta `>` como um operador de redirecionamento de saída. Ele abre o arquivo de destino e conecta a stdout do comando a ele:

```bash
$ echo Hello World > peanuts.txt
```

O texto deixa de aparecer no terminal porque stdout é enviada para `peanuts.txt`. Se o arquivo não existir, o shell o criará. Se existir, o shell o truncará antes que o comando grave, e o conteúdo anterior será perdido.

Use `cat` para inspecionar o resultado:

```bash
$ cat peanuts.txt
Hello World
```

Como o shell abre o destino antes de executar o comando, verifique o caminho antes de pressionar Enter. Um nome digitado incorretamente ou um arquivo existente não pretendido pode ser truncado mesmo que o comando falhe depois.

## Acréscimo a um Arquivo com >>

Use `>>` quando a nova stdout deve ser acrescentada depois do conteúdo existente de um arquivo:

```bash
$ echo Another line >> peanuts.txt
$ cat peanuts.txt
Hello World
Another line
```

Assim como `>`, `>>` cria um destino ausente. A diferença está na abertura de um arquivo existente: `>>` acrescenta em vez de truncar.

## O Redirecionamento Pertence ao Shell

O shell reconhece `>` e `>>`, remove esses operadores dos argumentos fornecidos ao programa, abre o arquivo e organiza a conexão do fluxo. O comando simplesmente continua gravando em stdout como de costume.

Isso significa que a mesma sintaxe de redirecionamento funciona com muitos comandos:

```bash
$ pwd > current-directory.txt
$ ls -la >> directory-list.txt
```

Para praticar o redirecionamento dos fluxos padrão, experimente este laboratório:

1. **[Redirecionamento de Entrada e Saída no Linux](https://labex.io/labs/comptia-redirecting-input-and-output-in-linux-590840)** — Pratique o controle do fluxo de dados dos comandos manipulando saída padrão (stdout), erro padrão (stderr) e entrada padrão (stdin) com operadores como `>`, `>>`, `2>` e o comando `tee`.