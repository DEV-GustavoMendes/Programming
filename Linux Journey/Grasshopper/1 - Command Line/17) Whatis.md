# whatis

Aprenda a obter descrições concisas das páginas de manual e interpretar seus números de seção.

Quando você reconhece o nome de um comando, mas esquece sua finalidade, `whatis` pode fornecer um breve lembrete com base no banco de dados das páginas de manual.

## Consulta de um Nome Exato

Forneça um ou mais nomes exatos de tópicos a `whatis`. Cada resultado deriva da seção `NAME` registrada em uma página de manual instalada:

```bash
$ whatis cat
cat (1)              - concatenate files and print on the standard output
```

A saída é uma descrição, não uma lista de opções ou exemplos do comando. Use `man cat` ou `cat --help` quando precisar de mais detalhes.

## Leitura dos Números de Seção

Se o mesmo tópico tiver páginas de manual em várias seções, `whatis` poderá mostrar mais de um resultado:

```bash
$ whatis passwd
passwd (1)           - change user password
passwd (5)           - the password file
```

O número entre parênteses é a seção do manual. Aqui, `passwd(1)` descreve o comando de usuário, e `passwd(5)` descreve um formato de arquivo. Você pode abrir uma delas explicitamente com `man 1 passwd` ou `man 5 passwd`.

## Escolha entre whatis, man e apropos

- `whatis NAME`: mostra descrições concisas de um nome exato de tópico do manual.
- `man NAME`: abre uma página de manual completa.
- `apropos KEYWORD`: pesquisa uma palavra-chave nos nomes e nas descrições das páginas.

Por exemplo:

```bash
$ apropos password
```

Use `apropos` quando conhecer a tarefa, mas não o nome do comando. Use `whatis` quando já souber o nome.

## Quando Nenhuma Descrição Aparece

Se `whatis` informar que nada é apropriado, talvez o tópico não possua uma página de manual instalada ou o banco de dados esteja desatualizado. Esse resultado não comprova a inexistência de um executável, alias, função ou comando interno com esse nome. Use `type NAME` para descobrir como o Bash resolve o nome e então escolha a fonte de ajuda apropriada.