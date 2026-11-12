# tar e gzip

Aprenda a arquivar arquivos com `tar`, compactar fluxos com `gzip` e inspecionar arquivos antes de extraí-los com segurança.

Arquivamento e compactação resolvem problemas diferentes. Um arquivo de arquivamento combina uma árvore de diretórios e seus metadados em um único fluxo. A compactação codifica um fluxo para reduzir seu tamanho. Por convenção, um arquivo `.tar.gz` é um arquivo tar cujo fluxo foi compactado com gzip.

## Compactação de um Fluxo com `gzip`

Por padrão, `gzip` compacta um arquivo e substitui o nome original por um arquivo com a extensão `.gz`:

```bash
$ gzip report.txt
```

Normalmente, isso remove `report.txt` após criar `report.txt.gz` com sucesso. Descompacte-o com:

```bash
$ gunzip report.txt.gz
```

Use `gzip -k report.txt`, quando houver suporte, para manter o arquivo de entrada, ou use os fluxos padrão quando precisar de controle explícito. Uma extensão de nome de arquivo é apenas uma convenção, não uma prova do formato real; ferramentas como `file` podem inspecionar o conteúdo.

## Criação de um Arquivo Tar

Crie um arquivo não compactado com:

```bash
$ tar -cvf project.tar file1 file2 directory1
```

- `-c` cria um novo arquivo.
- `-v` lista os membros durante o processamento e é opcional.
- `-f project.tar` define o nome do arquivo; como `-f` consome um argumento, mantenha o nome ao lado dessa opção.

Os caminhos são armazenados como nomes de membros do arquivo. Crie arquivos a partir de um diretório de trabalho escolhido conscientemente e evite incluir sem querer segredos, caches, sockets ou caminhos absolutos muito abrangentes.

## Criação de um Arquivo Tar Compactado com Gzip

O GNU tar e muitas outras implementações podem chamar o gzip por meio de `-z`:

```bash
$ tar -czvf project.tar.gz file1 file2 directory1
```

O resultado é um único fluxo tar compactado com gzip. A compactação não criptografa o arquivo nem oculta seu conteúdo de quem puder lê-lo e descompactá-lo. Se houver necessidade de confidencialidade, use um fluxo de trabalho adequado com criptografia autenticada e gerencie as chaves separadamente.

## Listagem Antes da Extração

Trate um arquivo recebido de terceiros como uma entrada não confiável. Primeiro, liste os nomes de seus membros:

```bash
$ tar -tzf download.tar.gz
```

Procure caminhos absolutos inesperados, componentes de travessia `..`, links simbólicos ou físicos suspeitos, arquivos de dispositivo e nomes que sobrescreveriam arquivos importantes. Implementações modernas de tar aplicam proteções, mas os comportamentos e as opções variam, e a extração ainda cria nomes e conteúdos escolhidos pelo remetente.

Extraia o conteúdo em um diretório de preparação recém-criado e sem privilégios:

```bash
$ mkdir extraction-stage
$ tar -xzf download.tar.gz -C extraction-stage
```

Não extraia como root um arquivo que ainda não foi examinado. Verifique o que foi criado antes de mover os arquivos selecionados para seus locais definitivos.

## Outros Formatos de Compactação

As implementações de tar podem trabalhar com compactadores como bzip2 e xz, normalmente selecionados no GNU tar com `-j` e `-J`, respectivamente. O suporte a formatos e a detecção automática variam; portanto, consulte `tar --help` ou o manual local. ZIP é um formato de arquivamento separado, operado por ferramentas como `zip` e `unzip`.

Pratique com arquivos descartáveis em [Empacotamento e Compactação de Arquivos](https://labex.io/labs/linux-file-packaging-and-compression-385413) e depois aplique a inspeção e a preparação em [Criar e Restaurar um Backup com tar](https://labex.io/labs/comptia-create-and-restore-a-backup-with-tar-in-linux-590843).