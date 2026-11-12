Aprenda a exibir, concatenar e redirecionar conteúdo de arquivos com segurança usando o comando cat.

Depois de aprender a identificar arquivos, o próximo passo é ler seu conteúdo. O comando `cat` exibe arquivos e une seus conteúdos; seu nome é uma abreviação de “concatenate”.

## Visualização do Conteúdo de um Arquivo

O uso mais simples de `cat` exibe um arquivo diretamente no terminal:

```bash
$ cat myfile.txt
```

O comando grava o arquivo inteiro na saída padrão. Isso funciona bem para textos curtos, mas um arquivo longo pode passar rápido demais pela tela.

## Concatenação de Arquivos

Quando você fornece vários arquivos a `cat`, ele os lê na ordem dos operandos e grava seus conteúdos um após o outro:

```bash
$ cat dogfile birdfile
```

Esse comando exibe primeiro `dogfile` e depois `birdfile`. Para salvar a saída combinada em um novo arquivo, redirecione a saída padrão com `>`:

```bash
$ cat dogfile birdfile > animals
```

O shell cria `animals` ou o trunca antes de executar `cat` e então envia a saída combinada para ele. Não use um dos arquivos de entrada como destino, pois ele pode ser esvaziado antes que `cat` consiga lê-lo.

## Leitura da Entrada do Terminal para um Arquivo

Quando nenhum arquivo de entrada é fornecido, `cat` lê a entrada padrão. Você pode combinar esse comportamento com `>` para digitar um texto no terminal e gravá-lo em um arquivo:

```bash
$ cat > newfile.txt
```

Depois de executar o comando, digite o texto desejado. Pressione `Ctrl+D` para enviar um sinal de fim de arquivo e retornar ao shell. Tenha cuidado: se `newfile.txt` já existir, `>` truncará seu conteúdo anterior.

Use `>>` para acrescentar a nova entrada em vez de substituir o conteúdo existente:

```bash
$ cat >> notes.txt
```

## Formatação da Saída

Várias opções facilitam a inspeção da saída:

- `-n`: numera todas as linhas de saída a partir de 1.
- `-b`: numera apenas as linhas de saída não vazias.
- `-s`: reduz várias linhas vazias consecutivas a uma só.
- `-A`: mostra caracteres não imprimíveis, tabulações e finais de linha.

Exemplos:

```bash
$ cat -n script.sh
$ cat -b notes.txt
$ cat -s messy.txt
```

## Escolha de um Visualizador para Arquivos Longos

Use `cat` quando quiser toda a saída de uma só vez. Para um arquivo longo, `less` costuma ser mais conveniente, pois permite rolar, pesquisar e sair sem inundar o terminal:

```bash
$ less /var/log/syslog
```

Para praticar a exibição e a combinação do conteúdo de arquivos, experimente estes laboratórios:

1. **[Comando cat do Linux: Concatenação de Arquivos](https://labex.io/labs/linux-linux-cat-command-file-concatenating-210986)** — Aprenda a usar `cat` para visualizar, concatenar e manipular arquivos de texto, aprimorando suas habilidades de linha de comando.
2. **[Visualização de Arquivos de Log e Configuração no Linux](https://labex.io/labs/linux-viewing-log-and-configuration-files-in-linux-387914)** — Pratique comandos como `cat` para visualizar e percorrer arquivos de texto, inclusive logs e configurações do sistema, e extrair informações importantes.