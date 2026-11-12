# tail

Aprenda a visualizar o final de uma entrada e acompanhar arquivos conforme novos conteúdos são acrescentados.

O comando `tail` exibe o final de um arquivo ou fluxo de entrada. Ele também pode permanecer ativo e mostrar dados acrescentados a um arquivo, o que é útil ao observar logs.

## Exibição das Dez Últimas Linhas

Sem uma opção de quantidade, `tail` mostra as 10 últimas linhas de cada arquivo indicado:

```bash
$ tail application.log
```

Se o arquivo tiver menos de 10 linhas, todas as linhas disponíveis serão mostradas. O arquivo em si não é alterado.

## Escolha da Quantidade de Linhas ou Bytes

Use `-n NUMBER` para selecionar outra quantidade de linhas finais:

```bash
$ tail -n 20 application.log
```

Use `-c NUMBER` quando precisar dos bytes finais:

```bash
$ tail -c 100 payload.bin
```

O modo de bytes pode começar no meio de uma linha de texto ou de um caractere codificado; por isso, o modo de linhas costuma ser mais claro para textos.

## Início em uma Linha Específica

Uma quantidade com o prefixo `+` muda o significado: `tail -n +N` começa na linha N e mostra até o final.

```bash
$ tail -n +5 report.txt
```

Esse comando ignora as quatro primeiras linhas e começa na linha 5. Ele é útil para remover uma quantidade conhecida de linhas de cabeçalho de um fluxo.

## Acompanhamento de Dados Acrescentados

Com `-f`, `tail` mostra o final inicial e permanece ativo, exibindo os dados conforme são acrescentados:

```bash
$ tail -f application.log
```

Pressione `Ctrl+C` para interromper `tail` e retornar ao shell. Acompanhar um arquivo apenas exibe o conteúdo novo; isso não garante que o aplicativo produtor do log esteja saudável nem que todos os eventos relevantes usem esse arquivo.

## Acompanhamento de um Log Rotacionado pelo Nome

A rotação de logs pode renomear um arquivo antigo e criar um novo no caminho original. No GNU `tail`, `-F` equivale a acompanhar pelo nome e tentar novamente; assim, ele pode reabrir um arquivo substituído ou temporariamente ausente:

```bash
$ tail -F application.log
```

Use `-f` quando quiser acompanhar o arquivo atualmente aberto e `-F` quando for esperado que um log identificado pelo nome seja rotacionado. Esses são comportamentos do GNU; outras implementações podem ser diferentes.

Quando nenhum arquivo é indicado, `tail` lê stdin e pode selecionar o final da saída de um comando. Vários arquivos recebem cabeçalhos de identificação por padrão, como ocorre com `head`.

Para praticar a visualização e o acompanhamento do final dos arquivos, experimente estes laboratórios:

1. **[Comando tail do Linux: Exibição do Final de Arquivos](https://labex.io/labs/linux-linux-tail-command-file-end-display-214303)** — Aprenda a usar `tail` para visualizar e monitorar o final de arquivos de texto, inclusive com `-f` para atualizações em tempo real.
2. **[Visualização de Arquivos de Log e Configuração no Linux](https://labex.io/labs/linux-viewing-log-and-configuration-files-in-linux-387914)** — Pratique o uso de `tail`, junto com `cat` e `more`, para visualizar e percorrer logs e arquivos de configuração.
3. **[Detecção Rápida de Ameaças](https://labex.io/labs/linux-rapid-threat-detection-387930)** — Use `tail` para extrair e analisar rapidamente entradas recentes de logs em um cenário de segurança.