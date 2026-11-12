# Uso do Disco

Aprenda como `df` e `du` medem diferentes perspectivas do consumo de blocos e inodes do sistema de arquivos.

A capacidade de um sistema de arquivos possui pelo menos dois limites: blocos de dados e objetos de metadados, como inodes. `df` informa a alocação pela perspectiva do sistema de arquivos, enquanto `du` percorre caminhos acessíveis e soma o uso atribuído a eles. Os valores respondem a perguntas diferentes e não precisam coincidir.

## Capacidade do Sistema de Arquivos com `df`

Mostre o tipo do sistema de arquivos montado e os valores de blocos em formato legível com:

```bash
$ df -hT
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/sda1      ext4  6.2G  2.3G  3.6G  40% /
```

`Size`, `Used` e `Avail` vêm da contabilidade do sistema de arquivos. O espaço disponível pode ser menor que o total menos o usado por causa de blocos reservados, metadados, políticas de alocação, cotas ou arredondamentos. Execute `df` em um caminho para informar o sistema de arquivos que contém esse caminho:

```bash
$ df -hT /var/log
```

## Capacidade de Inodes

Sistemas de arquivos que alocam objetos semelhantes a inodes podem esgotá-los mesmo quando ainda há blocos disponíveis:

```bash
$ df -i /var
```

Grandes quantidades de arquivos pequenos podem consumir os inodes disponíveis. Excluir um arquivo grande libera muitos blocos, mas geralmente apenas um inode; excluir muitos arquivos pequenos desnecessários pode aliviar a pressão sobre os inodes. Alguns sistemas de arquivos alocam metadados dinamicamente e representam esses conceitos de forma diferente.

## Uso de Caminhos com `du`

Resuma o espaço alocado acessível abaixo de um diretório:

```bash
$ du -sh /var/log
```

Compare os filhos imediatos permanecendo em um único sistema de arquivos:

```bash
$ sudo du -xhd1 /var | sort -h
```

As opções do GNU mostradas aqui significam saída legível, profundidade máxima de um nível e um único sistema de arquivos. As permissões podem ocultar subárvores e produzir um total incompleto. Por padrão, `du` também pode contar arquivos com links físicos apenas uma vez, diferenciar o tamanho aparente dos blocos alocados e tratar arquivos esparsos de modo diferente conforme as opções.

## Por que `df` e `du` São Diferentes

Algumas causas comuns são:

- um processo mantém aberto um arquivo excluído, portanto seus blocos continuam alocados, mas não existe um caminho para `du`
- metadados do sistema de arquivos, espaço reservado, journals, reflinks, snapshots ou compressão afetam a contabilidade
- outro sistema de arquivos está montado dentro da árvore percorrida
- as permissões impedem `du` de ler alguns diretórios
- arquivos esparsos possuem tamanhos aparentes e alocados diferentes

Para arquivos excluídos, mas ainda abertos, inspecione os processos autorizados com uma ferramenta como `lsof +L1`; reinicie ou sinalize o serviço responsável por seu procedimento normal, em vez de truncar descritores desconhecidos.

## Investigação sem Agravar o Incidente

Comece pelo sistema de arquivos cheio informado por `df`, identifique seu destino de montagem com `findmnt` e então restrinja as buscas de `du` nesse mesmo sistema de arquivos. Considere snapshots, camadas de contêineres, logs, caches de pacotes e a política de retenção das aplicações. Não exclua arquivos somente porque são grandes; determine primeiro a propriedade, o backup, os requisitos de conformidade e o comportamento do serviço.

