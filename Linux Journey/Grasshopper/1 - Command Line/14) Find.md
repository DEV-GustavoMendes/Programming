# find

Aprenda a pesquisar árvores de diretórios por nome, tipo, tamanho e data e a agir sobre correspondências verificadas.

O comando `find` percorre uma árvore de diretórios e testa cada entrada segundo critérios como nome, tipo, tamanho ou data de modificação.

## Escolha de Onde Pesquisar

A sintaxe básica é:

```bash
find [PATH] [EXPRESSION]
```

O caminho escolhe o ponto inicial, e a expressão seleciona ou atua nas entradas abaixo dele.

Este comando pesquisa `/home` e seus descendentes em busca de entradas chamadas `puppies.jpg`:

```bash
$ find /home -name puppies.jpg
```

A recursão é o comportamento padrão. Use `.` como caminho inicial para pesquisar a árvore do diretório atual.

## Correspondência de Nomes e Tipos

O teste `-name` aceita um nome-base exato ou um padrão no estilo do shell. Coloque padrões curingas entre aspas para que o shell atual os forneça inalterados a `find`:

```bash
$ find . -name "*.txt"
```

Sem as aspas, o shell pode expandir `*.txt` no diretório atual antes que `find` seja iniciado. Use `-iname` no lugar de `-name` quando a correspondência não deva diferenciar maiúsculas de minúsculas.

Acrescente `-type d` para selecionar diretórios ou `-type f` para selecionar arquivos comuns:

```bash
$ find /home -type d -name MyFolder
```

Os dois testes precisam ser verdadeiros: a entrada deve ser um diretório e seu nome-base deve ser `MyFolder`.

## Correspondência por Tamanho e Data de Modificação

Use `-size` com `+` para tamanhos maiores que a unidade indicada ou `-` para menores:

```bash
$ find . -type f -size +10M
$ find . -type f -size -1k
```

Aqui, `M` maiúsculo representa unidades de 1.048.576 bytes, enquanto `k` minúsculo representa unidades de 1.024 bytes. `find` arredonda os tamanhos para cima na unidade escolhida antes de aplicar a comparação numérica; portanto, o comportamento nos limites se baseia nessas unidades.

Use `-mtime` para testar a quantidade de períodos completos de 24 horas desde a modificação do arquivo:

```bash
$ find . -type f -mtime -7
$ find . -type f -mtime +30
```

`-mtime -7` corresponde a um valor menor que 7, enquanto `-mtime +30` corresponde a um valor maior que 30. Como são usados períodos completos de 24 horas, esses testes não se baseiam na virada do dia do calendário.

## Exibição e Ações sobre as Correspondências

Se nenhuma ação for fornecida, o GNU `find` exibe os caminhos correspondentes. Você pode escrever `-print` explicitamente para deixar clara a ação da expressão:

Exiba as correspondências explicitamente:

```bash
$ find . -name "*.log" -print
```

Use `-exec` para executar outro comando nas correspondências:

```bash
$ find . -name "*.log" -exec ls -l {} \;
```

Na forma com `\;`, `{}` é substituído por um caminho correspondente em cada invocação do comando. O ponto e vírgula encerra a ação `-exec` e recebe uma barra invertida para que o shell o forneça a `find`.

Antes de usar uma ação destrutiva como `-delete` ou um comando `-exec` que altere arquivos, execute os mesmos testes com `-print` e inspecione todos os resultados. Um caminho inicial mais restrito e `-maxdepth N` também podem limitar a pesquisa.

Mensagens de permissão negada normalmente indicam que a conta atual não consegue pesquisar parte da árvore. Prefira um caminho inicial mais restrito e relevante; não acrescente privilégios elevados sem compreender e pretender o acesso ampliado.

Para praticar a criação de expressões de pesquisa, experimente estes laboratórios:

1. **[Comando find do Linux: Pesquisa de Arquivos](https://labex.io/labs/linux-linux-find-command-file-searching-219191)** — Este laboratório apresenta o comando `find`, uma ferramenta versátil para pesquisar e localizar arquivos e diretórios com base em vários critérios. Você praticará o uso de `find` para localizar arquivos específicos.
2. **[Descoberta de Recursos Críticos do Sistema](https://labex.io/labs/linux-discover-critical-system-resources-388032)** — Aprenda comandos essenciais para localizar arquivos e executáveis, inclusive `find`, e pratique a navegação eficiente pelo sistema de arquivos.