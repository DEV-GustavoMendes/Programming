# rsync

Aprenda a prever, executar e verificar com segurança sincronizações locais ou por SSH usando rsync.

`rsync` reconcilia arquivos e árvores evitando transferir dados inalterados. Essa eficiência não torna toda invocação segura: sintaxe da origem, barras finais, metadados, exclusões e política de remoção determinam o resultado.

## Leitura da origem e do destino

Sincronize o conteúdo de `source/` em `destination/`:

```bash
$ rsync -a -- source/ destination/
```

A barra em `source/` significa “copie o conteúdo deste diretório”. Sem ela, `rsync -a source destination/` cria ou atualiza `destination/source`. Sempre preveja os caminhos ao mudar a barra.

## Entendendo o modo archive

O modo archive, `-a`, equivale a um conjunto de opções recursivas e de preservação de metadados normalmente resumido como `-rlptgoD`. Ele preserva links simbólicos, permissões, horários de modificação, grupos, proprietários e arquivos de dispositivos ou especiais quando as permissões e o suporte da plataforma permitem.

O modo archive não inclui a preservação de hard links, ACLs nem atributos estendidos; eles normalmente exigem `-H`, `-A` e `-X`. Ele também não cria versões históricas por si só.

## Previsão da transferência

Faça um dry run com mudanças detalhadas antes de uma sincronização importante:

```bash
$ rsync -a --dry-run --itemize-changes -- source/ destination/
```

Uma execução de teste prevê ações usando a varredura atual; ela não pode garantir que os arquivos não mudarão antes do comando real. Salve e revise o comando exato e só o execute sem `--dry-run` depois de confirmar os dois pontos de extremidade.

## Sincronização por SSH

Envie ou receba usando o operando remoto:

```bash
$ rsync -a -- source/ alice@example.net:/srv/data/
$ rsync -a -- alice@example.net:/srv/data/ destination/
```

Rsync moderno costuma usar SSH nessa forma, mas confirme shell remoto, chave do host, privilégios e disponibilidade do rsync remoto. `-z` pode ajudar dados compressíveis em link limitado, mas desperdiçar CPU em dados já comprimidos.

## Tratando a exclusão como destrutiva

`--delete` remove do destino entradas ausentes na origem dentro do escopo. Pontas invertidas, barra errada ou exclusão ruim podem apagar dados válidos. Faça dry run num destino de teste, garanta backups recuperáveis, revise montagens e considere limites de exclusão.

Depois da execução real, examine o status de saída e os logs, compare as quantidades de arquivos e os metadados esperados e teste um conteúdo ou uma restauração representativa. A sincronização com rsync, por si só, replica exclusões ou corrupções indesejadas e não constitui uma estratégia completa de backup.

