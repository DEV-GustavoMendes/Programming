# pwd (Exibir o Diretório de Trabalho)

Aprenda a usar pwd para identificar sua localização atual no sistema de arquivos do Linux.

No Linux, arquivos e diretórios são organizados em uma hierarquia chamada sistema de arquivos. Antes de navegar com confiança, você precisa saber onde está. O comando `pwd` responde a essa pergunta exibindo seu diretório de trabalho atual.

## A Árvore de Diretórios no Linux

Todo o sistema de arquivos começa em um único diretório de nível superior chamado diretório raiz, representado por uma barra (`/`). A partir da raiz, a árvore se ramifica em subdiretórios, que podem conter arquivos e outros subdiretórios.

![[Pasted image 20261111095402.png]]

Veja um exemplo simplificado dessa estrutura:

```plaintext
/
|-- bin
|   |-- file1
|   |-- file2
|-- etc
|   |-- file3
|   `-- directory1
|       |-- file4
|       `-- file5
|-- home
|-- var
```

## Compreensão dos Caminhos de Arquivos

A localização de qualquer arquivo ou diretório é descrita por seu caminho. Um caminho é uma sequência de diretórios que leva de um ponto inicial até um destino específico.

Por exemplo, se houver uma pasta chamada `pete` dentro de `/home` e uma pasta `Movies` dentro de `pete`, o caminho completo será:

```plaintext
/home/pete/Movies
```

Um caminho que começa com `/` é absoluto porque parte do diretório raiz. Um caminho como `Movies` é relativo porque depende da sua localização atual.

> O que torna `/home/pete/Movies` um caminho absoluto?
> 	RESPOSTA: Ele começa na raiz com uma `/` inicial.

## Qual É o Significado de PWD no Linux?

`pwd` significa “print working directory”, ou “exibir o diretório de trabalho”. Seu diretório de trabalho é aquele em que o shell se encontra no momento. Os comandos que usam caminhos relativos partem dessa localização.

## Uso do Comando pwd

Para descobrir seu diretório atual, digite `pwd` e pressione Enter.

```bash
$ pwd
/home/pete
```

A saída é um caminho absoluto. Neste exemplo, o shell está no diretório pessoal do usuário `pete`.

A saída exata pode ser diferente em seu sistema porque o nome do usuário, o diretório pessoal e a localização atual podem variar. O comando `pwd` apenas exibe informações; ele não altera o diretório de trabalho. Já `cd` muda o diretório em que o shell se encontra.

## Por que pwd É Útil

Use `pwd` quando:

- estiver seguindo instruções e precisar confirmar sua localização;
- um comando falhar porque o caminho de um arquivo está errado;
- tiver percorrido vários diretórios e perdido a noção de onde está;
- quiser copiar o caminho do diretório atual para outro comando.

Por exemplo:

```bash
$ pwd
/home/pete/projects
$ ls
app.py  README.md
```

Isso mostra que `app.py` e `README.md` estão localizados em `/home/pete/projects`.

Para reforçar sua compreensão sobre a navegação no sistema de arquivos do Linux e a identificação da localização atual, experimente estes laboratórios práticos:

1. **[Comando pwd do Linux: Exibição de Diretórios](https://labex.io/labs/linux-linux-pwd-command-directory-displaying-209734)** — Este laboratório oferece uma visão geral focada e o uso prático do comando `pwd`, em correspondência direta com a identificação do diretório atual apresentada nesta lição.
2. **[Navegação em Diretórios no Linux](https://labex.io/labs/linux-directory-navigation-387844)** — Teste suas habilidades básicas na linha de comando navegando por vários diretórios e consolide sua compreensão sobre caminhos e a estrutura do sistema de arquivos.
3. **[Comando cd do Linux: Mudança de Diretórios](https://labex.io/labs/linux-linux-cd-command-directory-changing-209733)** — Aprenda a navegar com eficiência pelo sistema de arquivos usando `cd`, conhecendo diferentes técnicas para mudar de diretório e explorar sua estrutura.