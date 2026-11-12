# file

Aprenda a identificar o provável tipo de conteúdo de um arquivo sem depender do nome ou da extensão.

Na lição anterior, você usou `touch` para criar um arquivo sem acrescentar uma extensão. No Linux, os nomes dos arquivos não precisam descrever seu conteúdo: um arquivo chamado `funny.gif` não é necessariamente uma imagem GIF.

Use o comando `file` para inspecionar um arquivo e informar seu provável tipo:

```bash
$ file banana.jpg
banana.jpg: JPEG image data
```

## Por que as Extensões Não São Suficientes

As ferramentas do Linux normalmente não exigem uma extensão para determinar o tipo de um arquivo. Um script de shell pode se chamar `backup`, um arquivo de texto pode se chamar `README` e uma imagem pode ter uma extensão enganosa. O comando `file` examina propriedades como metadados do sistema de arquivos e padrões reconhecíveis no conteúdo.

```bash
$ file README
README: ASCII text
$ file /bin/ls
/bin/ls: ELF 64-bit LSB executable
```

O resultado é uma classificação, não uma garantia. Um arquivo incomum, incompleto ou danificado pode receber uma descrição abrangente, como `data`, em vez de um tipo preciso.

## Verificação de Vários Arquivos

Você pode verificar vários arquivos de uma só vez:

```bash
$ file notes.txt image.png archive.tar.gz
notes.txt: ASCII text
image.png: PNG image data
archive.tar.gz: gzip compressed data
```

Também é possível fornecer um curinga do shell. O shell expande `*` para os nomes correspondentes antes que `file` os examine:

```bash
$ file *
```

## Exibição de Informações MIME

A opção `-i` mostra informações no estilo MIME, incluindo um tipo de mídia e, quando disponível, um conjunto de caracteres. Essa forma é útil quando outro programa espera valores como `text/html`.

```bash
$ file -i index.html
index.html: text/html; charset=us-ascii
```

## Opções Úteis de file

- `-i`: mostra informações no estilo MIME.
- `-b`: usa o modo breve e omite o nome do arquivo na saída.
- `-L`: segue links simbólicos e classifica seus destinos.
- `-z`: tenta examinar o conteúdo de arquivos compactados.

Por exemplo:

```bash
$ file -b notes.txt
ASCII text
```