# cp (Copiar)

Aprenda a copiar arquivos e árvores de diretórios controlando substituições e atributos preservados.

O comando `cp` copia arquivos e diretórios, mantendo a origem no lugar. Sua sintaxe básica é:

```bash
cp [OPTIONS] SOURCE DESTINATION
```

Você pode copiar um arquivo para outro caminho, copiar vários arquivos para um diretório ou copiar recursivamente uma árvore de diretórios.

## Cópia de um Arquivo

Coloque primeiro a origem e depois o destino:

```bash
$ cp mycoolfile /home/pete/Documents/cooldocs
```

Se `/home/pete/Documents/cooldocs` for um diretório existente, a cópia será criada dentro dele com o nome `mycoolfile`. Você também pode fornecer um novo nome no destino:

```bash
$ cp mycoolfile /home/pete/Documents/mycoolfile_backup
```

No segundo exemplo, os dados copiados recebem o nome `mycoolfile_backup`.

## Cópia de Vários Arquivos para um Diretório

Liste primeiro todas as origens e coloque o diretório de destino por último:

```bash
$ cp report.txt notes.txt summary.txt /home/pete/Documents/
```

O último argumento deve ser um diretório quando você fornece mais de uma origem.

## Seleção de Arquivos com Curingas

O shell pode expandir padrões curingas para vários caminhos de origem:

- `*`: corresponde a qualquer sequência de caracteres.
- `?`: corresponde a qualquer caractere individual.
- `[]`: corresponde a um dos caracteres entre colchetes.

Por exemplo, copie os nomes terminados em `.jpg` do diretório atual para `Pictures`:

```bash
$ cp *.jpg /home/pete/Pictures
```

Visualize as correspondências antes de uma cópia em massa, especialmente quando o destino contiver dados importantes:

```bash
$ ls *.jpg
beach.jpg  lunch.jpg  profile.jpg
$ cp *.jpg /home/pete/Pictures
```

## Cópia de Árvores de Diretórios

Copiar um diretório e tudo abaixo dele exige uma operação recursiva. Use `-r` ou `-R`:

```bash
$ cp -r Pumpkin/ /home/pete/Documents
```

Esse comando copia o diretório `Pumpkin` e seus descendentes para `Documents`.

`-R` maiúsculo também solicita uma cópia recursiva:

```bash
$ cp -R website /home/pete/backups/
```

O modo de arquivamento, `-a`, é útil para cópias de backup. Ele copia recursivamente, preservando links e muitos atributos:

```bash
$ cp -a project/ project-backup/
```

## Controle das Substituições

Por padrão, `cp` pode substituir um arquivo existente no destino. Use `-i` para solicitar confirmação antes da substituição:

```bash
$ cp -i mycoolfile /home/pete/Pictures
cp: overwrite '/home/pete/Pictures/mycoolfile'? n
```

Use `-n` quando um destino existente não deve ser sobrescrito:

```bash
$ cp -n mycoolfile /home/pete/Pictures
```

A opção `-f` instrui o GNU `cp` a tentar remover um destino existente quando não conseguir abri-lo para gravação e depois repetir a cópia. Ela não substitui a verificação cuidadosa dos destinos. Aliases do shell também podem acrescentar opções como `-i`; portanto, investigue uma solicitação inesperada em vez de presumir uma configuração específica.

## Preservação ou Atualização de Arquivos

Use `-p` para preservar o modo do arquivo de origem, seu proprietário quando permitido e seus carimbos de data e hora:

```bash
$ cp -p mycoolfile /home/pete/backups/
```

Use `-u` para copiar uma origem somente quando o destino estiver ausente ou a origem for mais recente:

```bash
$ cp -u *.txt /home/pete/Documents/
```

Outras opções comuns incluem:

- `-f`: força a substituição removendo primeiro o destino, se necessário.
- `-v`: mostra cada arquivo conforme ele é copiado.

Para praticar a cópia de arquivos e árvores de diretórios, experimente estes laboratórios:

1. **[Comando cp do Linux: Cópia de Arquivos](https://labex.io/labs/linux-linux-cp-command-file-copying-209744)** — Pratique o uso básico, opções avançadas como cópia recursiva e preservação de atributos, além de curingas para copiar arquivos e diretórios com eficiência.
2. **[Organização de Arquivos e Diretórios](https://labex.io/labs/linux-organizing-files-and-directories-387877)** — Pratique habilidades essenciais de gerenciamento de arquivos usando `cp`, `mv` e `rm` para organizar uma estrutura de projeto, mover arquivos e remover diretórios desnecessários.