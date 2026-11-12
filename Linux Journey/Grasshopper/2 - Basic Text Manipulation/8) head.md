# head

Aprenda a exibir uma quantidade controlada de linhas ou bytes do início de uma entrada.

O comando `head` exibe o início de um arquivo ou fluxo de entrada. Ele é útil para verificar cabeçalhos, visualizar dados estruturados ou obter uma amostra da saída sem mostrar tudo.

## Exibição das Dez Primeiras Linhas

Sem uma opção de quantidade, `head` mostra as 10 primeiras linhas de cada arquivo indicado:

```bash
$ head events.log
```

O arquivo não é modificado. Se tiver menos de 10 linhas, todas as linhas disponíveis serão mostradas.

## Escolha da Quantidade de Linhas

Use `-n NUMBER` para escolher quantas linhas mostrar:

```bash
$ head -n 15 events.log
```

O GNU `head` também aceita a forma compacta `-15`, mas `-n 15` deixa mais claro o significado da opção.

## Escolha da Quantidade de Bytes

Use `-c NUMBER` quando precisar de bytes, não de linhas completas:

```bash
$ head -c 20 archive.bin
```

Esse comando mostra os 20 primeiros bytes. A saída pode terminar no meio de uma linha de texto ou, em um texto multibyte, no meio de um caractere codificado. Use o modo de linhas para visualizações comuns de texto.

## Leitura de stdin e de Vários Arquivos

Quando nenhum arquivo é fornecido, `head` lê stdin:

```bash
$ generate-report | head -n 5
```

Quando vários arquivos são indicados, `head` normalmente acrescenta um cabeçalho que identifica a saída de cada um:

```bash
$ head -n 2 january.txt february.txt
==> january.txt <==
...

==> february.txt <==
...
```

Use `-q` para suprimir esses cabeçalhos ou `-v` para mostrar um cabeçalho até mesmo para um único arquivo.

Para praticar a visualização do início dos arquivos, experimente estes laboratórios:

1. **[Comando head do Linux: Exibição do Início de Arquivos](https://labex.io/labs/linux-linux-head-command-file-beginning-display-214302)** — Aprenda a usar `head` para exibir as linhas iniciais de arquivos de texto e alterar a quantidade de linhas.
2. **[Visualização de Arquivos de Log e Configuração no Linux](https://labex.io/labs/linux-viewing-log-and-configuration-files-in-linux-387914)** — Pratique habilidades para visualizar e percorrer arquivos de texto, inclusive logs e configurações do sistema, que frequentemente exigem comandos como `head`.
3. **[Detecção Rápida de Ameaças](https://labex.io/labs/linux-rapid-threat-detection-387930)** — Aplique seu conhecimento de `head` e `tail` para extrair e analisar rapidamente entradas de logs em um cenário de segurança.