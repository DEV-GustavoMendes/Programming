# mv (Mover)

Aprenda a renomear e mover arquivos ou diretórios evitando substituições indesejadas.

O comando `mv` renomeia um arquivo ou diretório, ou o move para outro local. Ao contrário de `cp`, ele não mantém o caminho original após uma movimentação bem-sucedida.

A sintaxe básica é:

```bash
mv [OPTIONS] SOURCE DESTINATION
```

## Renomeação de Arquivos e Diretórios

Para renomear um item, coloque primeiro o caminho atual e depois o novo caminho.

Para renomear um arquivo:

```bash
$ mv oldfile newfile
```

A mesma ordem dos operandos renomeia um diretório:

```bash
$ mv old_directory_name new_directory_name
```

## Movimentação de Itens para um Diretório

Quando o último operando é um diretório existente, `mv` coloca a origem dentro dele:

```bash
$ mv file2 /home/pete/Documents
```

Para mover várias origens, liste-as primeiro e coloque o diretório de destino por último:

```bash
$ mv file_1 file_2 somedirectory/
```

O GNU `mv` também oferece `-t` para colocar o diretório de destino antes das origens:

```bash
$ mv -t somedirectory/ file_1 file_2
```

Ao contrário de `cp`, `mv` não precisa de uma opção recursiva para um diretório.

## Controle dos Destinos Existentes

Por padrão, `mv` pode substituir um destino existente. Inspecione os caminhos de origem e destino antes de executar uma movimentação e escolha uma política de substituição quando necessário:

- `-i`: solicita confirmação antes de substituir um destino existente.
    
    ```bash
    $ mv -i source_file destination_directory
    ```
    
- `-n`: não sobrescreve um destino existente.
    
    ```bash
    $ mv -n source_file destination_directory
    ```
    
- `-b`: no GNU/Linux, cria um backup do destino que seria substituído. O sufixo padrão do backup geralmente é `~`.
    
    ```bash
    $ mv -b file1 directory_with_file1
    ```
    
- `-v`: mostra cada movimentação conforme ela ocorre.
    

```bash
$ mv -v file1 file2 somedirectory/
```

## Movimentação de Diretórios e Correspondências de Curingas

Um diretório pode ser movido sem `-r`:

```bash
$ mv project /home/pete/Documents/
```

Curingas do shell podem selecionar várias origens:

```bash
$ ls *.txt
$ mv *.txt notes/
```

Visualizar as correspondências com `ls` ajuda a detectar um padrão amplo demais antes de alterar vários caminhos.

Para praticar a movimentação e a renomeação de itens, experimente estes laboratórios:

1. **[Comando mv do Linux: Movimentação e Renomeação de Arquivos](https://labex.io/labs/linux-linux-mv-command-file-moving-and-renaming-209743)** — Pratique o uso de `mv` para mover e renomear arquivos e diretórios, conhecendo suas opções e comportamentos.
2. **[Organização de Arquivos e Diretórios](https://labex.io/labs/linux-organizing-files-and-directories-387877)** — Aplique seu conhecimento de `mv`, junto com `cp` e `rm`, em um desafio prático para organizar um projeto, mover arquivos e eliminar diretórios desnecessários.