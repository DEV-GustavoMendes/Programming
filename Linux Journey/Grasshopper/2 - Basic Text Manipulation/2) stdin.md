# stdin (Entrada Padrão)

Aprenda como os programas leem a entrada padrão e como o Bash conecta esse fluxo a um arquivo.

A entrada padrão, abreviada como **stdin**, é o fluxo que um programa normalmente lê para receber dados. Em um terminal interativo, o shell geralmente conecta stdin à entrada do terminal, permitindo que um programa leia o que você digita.

## Entrada Padrão e o Descritor de Arquivo 0

Por convenção, os três fluxos padrão usam estes números de descritores de arquivo:

- `0`: entrada padrão (`stdin`)
- `1`: saída padrão (`stdout`)
- `2`: erro padrão (`stderr`)

Um programa pode escolher se e como usará esses fluxos. Um comando projetado para ler stdin costuma aguardar a entrada do terminal quando não recebe um operando de arquivo nem outra fonte de entrada.

## Redirecionamento de um Arquivo para stdin

O operador `<` instrui o Bash a abrir um arquivo para leitura e conectá-lo à stdin do comando:

```bash
$ cat < peanuts.txt
Hello World
```

O shell trata `< peanuts.txt`; `cat` simplesmente lê o descritor de arquivo 0. O caminho não é fornecido a `cat` como um operando comum de arquivo.

Se o arquivo de entrada não existir ou não puder ser aberto, o shell informará o erro de redirecionamento e não iniciará o comando com essa entrada.

## Operando de Arquivo versus Redirecionamento de Entrada

Alguns comandos aceitam tanto um nome de arquivo como operando quanto stdin, mas os resultados podem ser ligeiramente diferentes. Por exemplo:

```bash
$ wc -l peanuts.txt
1 peanuts.txt
$ wc -l < peanuts.txt
1
```

As duas formas contam as linhas nos mesmos dados. Na primeira, `wc` conhece o nome do arquivo porque o recebeu como argumento. Na segunda, ele recebe apenas um fluxo em stdin e, portanto, não possui um nome de arquivo para mostrar.

## Combinação dos Redirecionamentos de Entrada e Saída

Uma única linha de comando pode redirecionar mais de um fluxo:

```bash
$ cat < peanuts.txt > banana.txt
```

O shell realiza duas conexões independentes:

1. `< peanuts.txt` abre `peanuts.txt` como stdin de `cat`.
2. `> banana.txt` cria ou trunca `banana.txt` e o conecta à stdout de `cat`.

`cat` lê bytes de stdin e os grava em stdout; assim, `banana.txt` recebe o conteúdo da origem. Para uma cópia comum de arquivo, `cp peanuts.txt banana.txt` comunica a intenção mais diretamente; este exemplo trata das conexões entre fluxos.

Para praticar o redirecionamento de entrada e saída, experimente estes laboratórios:

1. **[Redirecionamento de Entrada e Saída no Linux](https://labex.io/labs/comptia-redirecting-input-and-output-in-linux-590840)** — Pratique o controle do fluxo de dados dos comandos manipulando saída padrão (stdout), erro padrão (stderr) e entrada padrão (stdin) com operadores como `>`, `>>`, `2>` e o comando `tee`.
2. **[Redirecionamento de Fluxos de Dados](https://labex.io/labs/linux-data-stream-redirection-17995)** — Aprenda a manipular os fluxos de entrada, saída e erro, combinar saídas e usar `/dev/null` em operações avançadas com arquivos.