# rm (Remover)

Aprenda a remover arquivos e diretórios verificando os destinos e escolhendo opções mais seguras de rm.

O comando `rm` remove entradas do sistema de arquivos. A remoção pela linha de comando normalmente não envia os itens para a lixeira do ambiente gráfico, e `rm` não possui uma função integrada para desfazer a ação; portanto, confirme todos os destinos antes de executá-lo.

A sintaxe básica é:

```bash
rm [OPTIONS] FILE...
```

## Remoção de Arquivos

Forneça um ou mais caminhos de arquivos a `rm`:

```bash
$ rm file1
```

```bash
$ rm notes.txt old-report.txt draft.md
```

Verifique a grafia e a localização antes de pressionar Enter. Um backup ou uma cópia no controle de versão oferece um plano de recuperação mais confiável do que ferramentas de recuperação do sistema de arquivos após a exclusão.

## Visualização dos Destinos de Curingas

O shell pode expandir um curinga para vários operandos. Por exemplo, `*.tmp` seleciona os nomes não ocultos correspondentes no diretório atual:

```bash
$ rm *.tmp
```

Antes de remover qualquer coisa, visualize o mesmo padrão sem aspas usando `ls`:

```bash
$ ls *.tmp
cache.tmp  test.tmp
$ rm *.tmp
```

O shell expande o padrão antes que `rm` seja iniciado. Se a visualização incluir um arquivo inesperado, corrija o padrão em vez de prosseguir.

## Solicitação de Confirmação

A opção `-i` pergunta antes de cada remoção:

```bash
$ rm -i important.txt
rm: remove regular file 'important.txt'? y
```

A opção `-I` é uma salvaguarda menos intrusiva no GNU `rm`: ela pergunta uma vez quando um comando removeria mais de três arquivos ou atuaria recursivamente.

## Como Ignorar Arquivos Ausentes com -f

A opção `-f` ignora operandos ausentes e suprime perguntas:

```bash
$ rm -f old-cache.txt
```

Isso pode tornar uma limpeza por script idempotente quando um arquivo gerado talvez já esteja ausente. Como ela remove a confirmação, não acrescente `-f` apenas para silenciar um erro que você ainda não compreendeu.

## Remoção de Diretórios

`rm` sem opções não remove um diretório:

```bash
$ rm projects
rm: cannot remove 'projects': Is a directory
```

Use `-r` ou `-R` apenas quando pretender remover uma árvore de diretórios e todo o seu conteúdo:

```bash
$ rm -r old-project
```

Para um diretório vazio, `rmdir` é uma alternativa mais restrita:

```bash
$ rmdir empty-directory
```

`rmdir` falha quando o diretório não está vazio, protegendo seu conteúdo contra uma exclusão recursiva.

## Verificação de uma Remoção Recursiva

Uma remoção recursiva pode apagar uma árvore inteira. Combinar `-r` com `-f` também elimina as perguntas; por isso, `rm -rf` exige uma validação especialmente cuidadosa do destino. Antes de qualquer remoção recursiva, verifique:

- Você está no diretório que imagina? Use `pwd`.
- `ls -ld -- TARGET` mostra o caminho superior pretendido?
- Se houver um curinga, uma visualização somente para leitura correspondeu exatamente ao esperado?
- O caminho é absoluto ou relativo? `/tmp/cache` e `tmp/cache` são muito diferentes.
- Há um espaço acidental? `rm -rf old-project` e `rm -rf old project` se referem a caminhos diferentes.

Use `--` antes de um destino que possa começar com hífen para que ele não seja interpretado como uma opção:

```bash
$ rm -- -old-name
```

Não recorra a `sudo` simplesmente porque `rm` informou um erro de permissão. Primeiro, verifique o destino e descubra por que sua conta não consegue modificar o diretório que o contém. Uma remoção recursiva com privilégios elevados pode danificar o sistema operacional ou os dados de outros usuários.

Use `-v` quando quiser que `rm` informe cada remoção bem-sucedida:

```bash
$ rm -rv old-project
removed 'old-project/notes.txt'
removed directory 'old-project'
```

Para praticar a remoção em um ambiente controlado, experimente estes laboratórios:

1. **[Comando rm do Linux: Remoção de Arquivos](https://labex.io/labs/linux-linux-rm-command-file-removing-209741)** — Aprenda a usar `rm` para remover arquivos e diretórios, incluindo opções como `-r` e `-i`, e pratique uma exclusão segura e eficaz.
2. **[Organização de Arquivos e Diretórios](https://labex.io/labs/linux-organizing-files-and-directories-387877)** — Pratique habilidades essenciais de gerenciamento de arquivos, inclusive o uso de `rm` para eliminar diretórios desnecessários, em um desafio prático.