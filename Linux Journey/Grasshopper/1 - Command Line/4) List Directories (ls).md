# ls (Listar Diretórios)

Aprenda a usar as opções de ls para inspecionar arquivos, entradas ocultas, detalhes, tamanhos e ordem de classificação.

Agora que sabemos navegar pelo sistema de arquivos, como descobrimos o que está disponível? O comando `ls` lista arquivos e diretórios para que você possa inspecionar sua localização atual ou outro caminho.

## Uso Básico do Comando ls

Por padrão, `ls` lista os diretórios e arquivos do diretório atual. Você também pode fornecer um caminho para listar o conteúdo de outro diretório.

```bash
$ ls
$ ls /home/pete
```

Também é possível listar um arquivo específico:

```bash
$ ls /etc/hosts
/etc/hosts
```

## Visualização de Arquivos Ocultos

Nem todos os arquivos de um diretório aparecem por padrão. No Linux, nomes de arquivos que começam com um ponto (`.`) ficam ocultos. Você pode exibi-los com a opção `-a`, de all.

```bash
$ ls -a
.  ..  .bashrc  Documents  Pictures
```

Arquivos com nomes iniciados por ponto ficam ocultos por padrão e frequentemente armazenam configurações, como `.bashrc`.

## Obtenção de Informações Detalhadas

Outra opção essencial de `ls` é `-l`, que seleciona o formato longo. Ela mostra as permissões, o número de links, o proprietário, o grupo, o tamanho, a data de modificação e o nome.

```bash
$ ls -l
```

Veja um exemplo da saída:

```plaintext
pete@icebox:~$ ls -l
total 80
drwxr-x--- 7 pete penguingroup   4096 Nov 20 16:37 Desktop
drwxr-x--- 2 pete penguingroup   4096 Oct 19 10:46  Documents
drwxr-x--- 4 pete penguingroup   4096 Nov 20 09:30 Downloads
drwxr-x--- 2 pete penguingroup   4096 Oct  7 13:13   Music
drwxr-x--- 2 pete penguingroup   4096 Sep 21 14:02 Pictures
drwxr-x--- 2 pete penguingroup   4096 Jul 27 12:41   Public
drwxr-x--- 2 pete penguingroup   4096 Jul 27 12:41   Templates
drwxr-x--- 2 pete penguingroup   4096 Jul 27 12:41   Videos
```

Para facilitar a leitura dos tamanhos, acrescente `-h`, de human-readable:

```bash
$ ls -lh
```

## Classificação em Ordem Inversa

Às vezes, você pode querer mudar a ordem de classificação. A opção `-r` lista os arquivos e diretórios em ordem inversa.

```bash
$ ls -r
```

Você pode classificar pela data de modificação com `-t` e depois inverter a ordem com `-r`:

```bash
$ ls -lt
$ ls -ltr
```

## Combinação de Opções de Comando

Os comandos possuem sinalizadores, também chamados de opções, que acrescentam funcionalidades. Como vimos com `-a` e `-l`, você pode combiná-los em um único comando, como `ls -la`. A ordem dos sinalizadores muitas vezes não importa; portanto, `ls -al` funciona da mesma maneira.

```bash
$ ls -la
```

Algumas combinações úteis são:

```bash
$ ls -lh
$ ls -la
$ ls -ltr
```

## Opções Comuns de ls

- `-a`: mostra todos os arquivos, inclusive os ocultos.
- `-l`: usa o formato longo.
- `-h`: mostra tamanhos legíveis quando usado com `-l`.
- `-r`: inverte a ordem de classificação.
- `-t`: classifica pela data de modificação.
- `-S`: classifica pelo tamanho do arquivo.
- `-d`: lista o próprio diretório, em vez de seu conteúdo.

Alguns sistemas exibem a saída de `ls` em cores diferentes conforme o tipo de arquivo. Esse comportamento costuma vir de um alias ou de uma configuração do ambiente; portanto, as cores podem variar entre sistemas.

Para reforçar sua compreensão do comando `ls`, experimente este laboratório prático:

- **[Comando ls do Linux: Listagem de Conteúdo](https://labex.io/labs/linux-linux-ls-command-content-listing-219205)** — Pratique o uso de `ls` para listar e analisar com eficiência o conteúdo de arquivos e diretórios. Você aprenderá opções para listagens detalhadas, exibição de arquivos ocultos, tamanhos legíveis e técnicas de classificação.

