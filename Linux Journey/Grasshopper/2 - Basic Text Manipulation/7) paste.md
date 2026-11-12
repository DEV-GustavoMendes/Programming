# paste

Aprenda a mesclar linhas correspondentes ou serializar linhas com delimitadores configuráveis usando paste.

O comando `paste` combina linhas como colunas. Por padrão, ele obtém uma linha de cada arquivo de entrada, une essas linhas com uma tabulação e repete a operação até que todas as entradas cheguem ao fim.

## Mesclagem de Arquivos Lado a Lado

Crie dois arquivos pequenos:

```bash
$ printf 'alice\nbob\n' > names.txt
$ printf 'admin\nviewer\n' > roles.txt
```

Forneça os dois arquivos a `paste`:

```bash
$ paste names.txt roles.txt
alice	admin
bob	viewer
```

O espaço visível entre as colunas é uma tabulação. Ao contrário de `cat`, que grava um arquivo completo depois do outro, `paste` combina as linhas correspondentes das entradas.

## Escolha de um Delimitador

Use `-d LIST` para substituir o separador padrão de tabulação. Para usar dois-pontos:

```bash
$ paste -d ':' names.txt roles.txt
alice:admin
bob:viewer
```

Coloque entre aspas delimitadores que tenham significado para o shell. `paste` pode alternar entre vários caracteres delimitadores quando a lista contém mais de um, mas um único caractere é a opção mais simples ao criar duas colunas.

## Serialização das Linhas de um Arquivo

A opção `-s` processa cada arquivo de entrada em modo serial, unindo suas linhas em uma única linha de saída. Crie um arquivo com uma palavra por linha:

```bash
$ printf 'The\nquick\nbrown\nfox\n' > words.txt
$ paste -s words.txt
The	quick	brown	fox
```

Combine `-s` com `-d` para escolher o separador:

```bash
$ paste -s -d ' ' words.txt
The quick brown fox
```

Se vários arquivos forem fornecidos com `-s`, cada arquivo se tornará sua própria linha de saída.

## Tratamento de Entradas com Tamanhos Diferentes

Quando arquivos de entrada paralelos possuem números diferentes de linhas, `paste` continua até o final do arquivo mais longo. Os valores ausentes de um arquivo mais curto se tornam campos vazios:

```bash
$ printf 'A\nB\nC\n' > letters.txt
$ printf '1\n2\n' > numbers.txt
$ paste -d ':' letters.txt numbers.txt
A:1
B:2
C:
```

## Leitura de uma Entrada por stdin

Use `-` como operando de arquivo para ler essa posição de stdin:

```bash
$ printf 'admin\nviewer\n' | paste -d ':' names.txt -
alice:admin
bob:viewer
```

Para praticar a mesclagem de dados orientados por linhas, experimente este laboratório:

1. **[Processamento Simples de Texto](https://labex.io/labs/linux-simple-text-processing-18004)** — Aprenda a usar comandos como `tr`, `col`, `join` e `paste` para manipular e analisar dados textuais com eficiência.