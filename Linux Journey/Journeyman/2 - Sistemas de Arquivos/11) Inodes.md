# Inodes

Aprenda como os números de inodes conectam os nomes dos diretórios aos metadados e dados dos objetos do sistema de arquivos.

Em sistemas de arquivos Unix baseados em inodes, um diretório mapeia o nome de cada entrada para um número de inode. O inode representa o objeto do sistema de arquivos e registra os metadados necessários para localizar e interpretar seus dados. Portanto, o caminho não é armazenado como a identidade principal do próprio objeto.

## Metadados Armazenados com um Inode

Os metadados normalmente associados a um inode incluem:

- tipo do objeto e modo de permissão
- propriedade de usuário e grupo
- tamanho lógico e contabilização dos blocos alocados
- contagem de links físicos
- timestamps de acesso, modificação e alteração de estado
- referências aos dados do arquivo ou a estruturas de extensões específicas do sistema de arquivos

Normalmente, o inode não armazena o nome da entrada do diretório. Um sistema de arquivos também pode armazenar atributos estendidos, listas de controle de acesso, horário de criação, dados inline ou outras informações por meio de estruturas específicas do formato.

`ctime` é o horário de alteração do estado do inode, não necessariamente o horário de criação do arquivo. Um timestamp separado de nascimento ou criação é opcional e pode não estar disponível.

## Números de Inodes e o Escopo do Sistema de Arquivos

Exiba os números de inodes com:

```bash
$ ls -li
```

O primeiro campo é o número do inode. Inspecione um objeto com mais detalhes usando:

```bash
$ stat path
```

Um número de inode é exclusivo apenas dentro de um sistema de arquivos em determinado momento. O mesmo número pode existir em outro sistema de arquivos e pode ser reutilizado depois que um inode é liberado. Identifique um objeto de forma confiável tanto pela identidade do sistema de arquivos quanto pelo número do inode, não apenas pelo número.

## Links Físicos e Referências Abertas

Várias entradas de diretório podem apontar para o mesmo inode; elas são links físicos. Criar outro link físico incrementa a contagem de links do objeto. Remover um nome reduz a contagem sem excluir os dados enquanto outro link permanecer.

Mesmo depois que a última entrada de diretório é removida, um arquivo aberto continua alocado até que a última referência de processo seja fechada. Sua contagem de links pode ser zero enquanto um descritor ainda o acessa. Isso explica por que excluir um log grande e aberto pode não reduzir imediatamente o uso informado por `df`.

## Capacidade de Inodes

Em sistemas de arquivos com um conjunto finito ou informado de inodes, milhões de arquivos pequenos podem esgotar a capacidade de metadados antes que os blocos de dados sejam preenchidos. Inspecione a contabilidade de inodes dos sistemas de arquivos montados com:

```bash
$ df -i
```

Se não houver inodes livres, a criação de outro arquivo pode falhar mesmo que `df -h` informe blocos disponíveis. As estratégias de alocação variam: alguns sistemas de arquivos pré-alocam estruturas de inodes durante a criação, enquanto outros gerenciam os metadados dinamicamente e podem informar sua capacidade de forma diferente.

## Mapeamento de Dados Específico do Sistema de Arquivos

Não presuma que todo inode possua exatamente 12 ponteiros diretos e três indiretos. Essa é uma descrição útil de alguns layouts clássicos, mas o ext4 moderno pode usar extensões, e XFS, Btrfs e outros sistemas empregam estruturas diferentes. Dados inline e extensões comprimidas ou copy-on-write alteram ainda mais essa relação.

Use ferramentas de diagnóstico específicas do sistema de arquivos apenas em modos somente para leitura ou documentados quando o mapeamento interno for importante. Para a administração comum, `stat`, `find -inum`, `df -i` e ferramentas que consideram links fornecem abstrações mais seguras.

Use o laboratório [Gerenciamento de Arquivos e Diretórios no Linux](https://labex.io/labs/comptia-manage-files-and-directories-in-linux-590835) para comparar números de inodes e contagens de links em arquivos descartáveis.
