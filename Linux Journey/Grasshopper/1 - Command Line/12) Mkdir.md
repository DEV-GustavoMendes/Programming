# mkdir (Criar Diretório)

Aprenda a criar diretórios únicos, múltiplos e aninhados com as opções de mkdir.

O comando `mkdir`, abreviação de make directory, cria diretórios para organizar arquivos e outros diretórios.

A sintaxe básica é:

```bash
mkdir [OPTIONS] DIRECTORY...
```

## Criação de um Diretório

Forneça um caminho para criar um diretório. Este exemplo cria `documents` no diretório de trabalho atual:

```bash
$ mkdir documents
```

Se já existir uma entrada chamada `documents`, `mkdir` informará um erro em vez de substituí-la. Use `ls -ld documents` para inspecionar a entrada existente.

## Criação de Vários Diretórios

Liste vários caminhos para criar vários diretórios em um único comando:

```bash
$ mkdir books paintings
```

## Criação de Diretórios Pais Ausentes

Sem uma opção, `mkdir books/hemingway/favorites` falha quando um diretório intermediário está ausente. Acrescente `-p` para criar os diretórios pais que faltam ao longo do caminho:

```bash
$ mkdir -p books/hemingway/favorites
```

Esse comando cria as partes ausentes do caminho. Ele também não informa um erro apenas porque o diretório final já existe, embora outros erros, como permissões insuficientes, ainda possam ocorrer.

## Definição do Modo Inicial

Use `-m MODE` para especificar as permissões de um novo diretório:

```bash
$ mkdir -m 755 public
```

Você estudará os modos de permissão mais adiante. Neste exemplo, o modo `755` concede ao proprietário permissões de leitura, gravação e pesquisa, enquanto o grupo e os demais recebem permissões de leitura e pesquisa.

Acrescente `-v` para mostrar uma mensagem para cada diretório criado:

```bash
$ mkdir -pv projects/app/src
mkdir: created directory 'projects'
mkdir: created directory 'projects/app'
mkdir: created directory 'projects/app/src'
```

Para praticar a criação e a organização de diretórios, experimente estes laboratórios:

1. **[Comando mkdir do Linux: Criação de Diretórios](https://labex.io/labs/linux-linux-mkdir-command-directory-creating-209739)** — Aprenda a usar `mkdir` para criar diretórios, definir permissões e organizar o sistema de arquivos. O laboratório aborda o uso básico e avançado, incluindo diretórios aninhados.
2. **[Configuração da Estrutura de um Novo Projeto](https://labex.io/labs/linux-setting-up-a-new-project-structure-387859)** — Pratique o gerenciamento de diretórios criando uma estrutura de projeto específica e navegando por ela com comandos essenciais como `mkdir` e `cd`.