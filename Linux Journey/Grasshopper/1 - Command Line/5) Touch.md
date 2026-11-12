# touch

Aprenda a criar arquivos vazios e gerenciar os carimbos de data e hora com o comando touch.

O comando `touch` altera os carimbos de data e hora dos arquivos. Ele também é usado com frequência para criar um ou mais arquivos vazios.

A sintaxe básica é:

```bash
touch [OPTIONS] FILE...
```

## Criação de Arquivos Vazios

Se o arquivo indicado não existir, `touch` o criará como um arquivo vazio:

```bash
$ touch mysuperduperfile
```

Você pode criar vários arquivos em um único comando listando cada nome:

```bash
$ touch file1.txt file2.txt file3.log
```

Isso é útil para criar marcadores de posição, mas `touch` não adiciona texto a um arquivo. Use um editor de texto ou outro comando destinado à gravação de conteúdo quando precisar de um arquivo não vazio.

## Atualização dos Carimbos de Data e Hora

Os arquivos registram vários carimbos de data e hora. Por padrão, executar `touch` em um arquivo existente altera tanto a hora de acesso quanto a de modificação para o momento atual. O conteúdo do arquivo permanece inalterado.

Você pode comparar a hora de modificação exibida antes e depois da execução:

```bash
$ ls -l mysuperduperfile
$ touch mysuperduperfile
$ ls -l mysuperduperfile
```

A saída de `ls -l` normalmente mostra a hora de modificação, não a hora de acesso.

> O que acontece ao executar `touch report.txt` quando `report.txt` já existe?
> 	RESPOSTA: Seus carimbos de data e hora são atualizados sem substituir o conteúdo. Por padrão, `touch` atualiza as horas de acesso e modificação de um arquivo existente. Ele não sobrescreve os dados.

## Controle do Carimbo Alterado

Use `-a` para alterar apenas a hora de acesso ou `-m` para alterar apenas a hora de modificação:

```bash
$ touch -a notes.txt
$ touch -m notes.txt
```

## Definição ou Cópia de um Horário

A opção `-d` aceita uma expressão de data em vez de usar o horário atual:

```bash
$ touch -d "2026-06-23 12:30:00" mysuperduperfile
```

Para atribuir a um arquivo as mesmas horas de acesso e modificação de um arquivo de referência, use `-r`:

```bash
$ touch -r file1.txt file2.txt
```

Aqui, `file1.txt` fornece os horários e `file2.txt` é o arquivo alterado. A opção `-t` é outra maneira de fornecer um horário, usando um formato numérico compacto.

## Como Evitar a Criação de Arquivos

Normalmente, `touch` cria um arquivo quando o caminho indicado não existe. Acrescente `-c` quando quiser atualizar um arquivo somente se ele já existir:

```bash
$ touch -c existing-file.txt
```

Se `existing-file.txt` estiver ausente, esse comando não o criará. Esse comportamento pode ser útil em scripts que devem atualizar um horário sem introduzir um novo arquivo.