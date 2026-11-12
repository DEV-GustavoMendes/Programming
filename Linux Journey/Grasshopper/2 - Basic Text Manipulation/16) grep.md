# grep

Aprenda a selecionar linhas com cadeias fixas ou expressões regulares e interpretar os resultados de grep.

O comando `grep` seleciona linhas da entrada que correspondem a um padrão. Ele pode pesquisar arquivos indicados ou stdin, mostrar o contexto das correspondências, contar linhas selecionadas e comunicar por seu status de saída se encontrou algo.

## Correspondência de Linhas em um Arquivo

Forneça um padrão seguido de um ou mais arquivos de entrada:

```bash
$ grep 'fox' sample.txt
```

Por padrão, o GNU `grep` interpreta o padrão como uma expressão regular básica e mostra cada linha selecionada. Coloque os padrões entre aspas para impedir que espaços e metacaracteres do shell sejam interpretados antes.

Use `-F` quando o padrão deva ser tratado como uma cadeia fixa, não como expressão regular:

```bash
$ grep -F 'price: $5.00' products.txt
```

## Seleção da Sintaxe do Padrão

O GNU `grep` oferece três modos comuns de padrão:

- padrão: expressões regulares básicas;
- `-E`: expressões regulares estendidas, incluindo operadores como `|`, `+` e `?` sem barras invertidas;
- `-F`: cadeias fixas, sem operadores de expressão regular.

Âncoras como `^` e `$` correspondem ao início e ao fim de uma linha. Para localizar nomes terminados no sufixo literal `.txt` em uma lista:

```bash
$ grep -E '\.txt$' filenames.txt
```

A barra invertida torna o ponto literal; um `.` sem escape em uma expressão regular corresponde a qualquer caractere individual.

## Fornecimento Seguro de Padrões

Use `-e PATTERN` para fornecer explicitamente um padrão. Isso é particularmente útil quando ele começa com `-`, pois as aspas sozinhas não impedem a interpretação como opção:

```bash
$ grep -e '-v' settings.conf
```

Você pode repetir `-e` para selecionar linhas correspondentes a qualquer padrão fornecido. Use `-f patterns.txt` para ler um padrão por linha de um arquivo.

## Controle da Saída Selecionada

- `-i`: ignora diferenças entre maiúsculas e minúsculas.
- `-n`: prefixa as linhas selecionadas com seus números.
- `-v`: seleciona as linhas que não correspondem.
- `-c`: mostra a quantidade de linhas selecionadas para cada arquivo de entrada.
- `-o`: mostra apenas cada parte correspondente não vazia, não a linha completa.

Por exemplo, conte as linhas que contêm `fox`, ignorando maiúsculas e minúsculas:

```bash
$ grep -ic 'fox' sample.txt
```

`-c` conta linhas selecionadas, não a quantidade total de correspondências dentro delas. Uma linha com `fox fox` contribui com um para a contagem. Quando você precisar especificamente das ocorrências não sobrepostas com o GNU `grep`, `grep -o PATTERN | wc -l` é um pipeline possível.

## Filtragem de stdin e Pesquisa em Diretórios

Quando nenhum arquivo de entrada é indicado, `grep` lê stdin e se encaixa naturalmente em um pipeline:

```bash
$ env | grep '^USER='
```

Use `-r` para pesquisar recursivamente os arquivos legíveis abaixo de um diretório:

```bash
$ grep -r 'listen_port' config/
```

Diagnósticos como erros de permissão vão para stderr e não fazem parte da entrada pesquisada. Restrinja o caminho e compreenda as permissões em vez de elevar o acesso imediatamente.

## Interpretação do Status de Saída

Em pesquisas comuns, o GNU `grep` retorna o status `0` quando seleciona pelo menos uma linha, `1` quando nenhuma linha é selecionada e `2` em caso de erro. Isso permite que scripts testem uma correspondência sem tratar “nenhuma correspondência” como a mesma condição de um arquivo ilegível ou padrão inválido.

Opções como `-q` suprimem a saída comum e param depois que uma correspondência é encontrada, sendo úteis em testes condicionais. Não deduza sucesso apenas por uma tela vazia: `-q`, redirecionamento, ausência de correspondência e erro podem produzir pouca ou nenhuma stdout, mas seus status são diferentes.

Para praticar pesquisas com cadeias fixas e expressões regulares, experimente estes laboratórios:

1. **[Pesquisa de Texto com grep no Linux](https://labex.io/labs/comptia-search-text-with-grep-in-linux-590841)** — Pratique pesquisas básicas, números de linha, âncoras e expressões regulares básicas e estendidas com `grep`.
2. **[Comando grep do Linux: Pesquisa de Padrões](https://labex.io/labs/linux-linux-grep-command-pattern-searching-219192)** — Aprenda a pesquisar e corresponder padrões em arquivos de texto e a definir padrões complexos com expressões regulares.
3. **[Agulha no Palheiro](https://labex.io/labs/linux-needle-in-the-haystack-388109)** — Aprenda a usar o poder do comando `grep` para pesquisar padrões específicos, contar ocorrências, extrair valores únicos e combinar vários critérios de pesquisa em diversos arquivos de log.
