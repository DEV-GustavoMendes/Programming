# stderr (Erro Padrão)

Aprenda a redirecionar o erro padrão separadamente ou combiná-lo com a saída padrão no Bash.

Os programas normalmente gravam resultados comuns na saída padrão e diagnósticos em um fluxo separado chamado erro padrão, ou **stderr**. Manter os fluxos separados permite salvar dados úteis sem misturar mensagens de erro.

## Separação dos Resultados Comuns e dos Erros

Considere um comando cujo caminho não existe:

```bash
$ ls /fake/directory > peanuts.txt
ls: cannot access '/fake/directory': No such file or directory
```

O operador `>` redireciona apenas stdout. O diagnóstico é gravado em stderr, que continua conectada ao terminal. Enquanto isso, o shell cria ou trunca `peanuts.txt` para stdout, mesmo que `ls` não produza um resultado comum.

Os fluxos padrão usam convencionalmente estes descritores de arquivo:

- `0`: stdin (entrada padrão)
- `1`: stdout (saída padrão)
- `2`: stderr (erro padrão)

## Redirecionamento de stderr com 2>

Coloque o descritor de arquivo `2` antes de `>` para redirecionar stderr:

```bash
$ ls /fake/directory 2> errors.txt
```

O shell cria ou trunca `errors.txt` e o conecta ao descritor 2. Stdout mantém seu destino anterior. Use `2>> errors.txt` quando a saída de erro deva ser acrescentada.

## Combinação de stdout e stderr

Para colocar os dois fluxos de saída em um arquivo, primeiro redirecione stdout e depois duplique stderr para o destino atual de stdout:

```bash
$ ls /fake/directory /etc/passwd > combined.txt 2>&1
```

Os redirecionamentos são processados da esquerda para a direita:

1. `> combined.txt` conecta stdout ao arquivo.
2. `2>&1` conecta stderr ao destino para o qual stdout aponta naquele momento.

Inverter a ordem muda o resultado:

```bash
$ ls /fake/directory /etc/passwd 2>&1 > regular.txt
```

Aqui, stderr primeiro duplica o destino original de stdout no terminal. Depois, stdout muda para `regular.txt`; assim, os dois fluxos terminam em lugares diferentes.

O Bash também oferece `&>` como uma sintaxe mais curta para substituir um arquivo com os dois fluxos:

```bash
$ ls /fake/directory /etc/passwd &> combined.txt
```

Use `&>>` para acrescentar os dois fluxos no Bash. É útil reconhecer a forma explícita `> file 2>&1`, pois ela também aparece em scripts e documentações do shell.

## Descarte Consciente de um Fluxo

`/dev/null` é um dispositivo especial que descarta os dados gravados nele. Redirecione stderr para ele apenas quando tiver determinado que os diagnósticos são esperados e desnecessários:

```bash
$ ls /fake/directory 2> /dev/null
```

Isso não faz o comando ter sucesso nem altera seu status de saída; apenas oculta o fluxo de diagnóstico. Durante uma investigação, preserve ou exiba stderr em vez de descartar as informações necessárias.

Para praticar o gerenciamento dos três fluxos padrão, experimente este laboratório:

1. **[Redirecionamento de Entrada e Saída no Linux](https://labex.io/labs/comptia-redirecting-input-and-output-in-linux-590840)** — Neste laboratório, você aprenderá a redirecionar entrada e saída no shell do Linux. Você praticará o controle do fluxo de dados dos comandos manipulando a saída padrão (stdout), o erro padrão (stderr) e a entrada padrão (stdin) com operadores como `>`, `>>`, `2>` e o comando `tee`.