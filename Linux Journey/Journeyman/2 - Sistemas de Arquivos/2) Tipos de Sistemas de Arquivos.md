# Tipos de Sistemas de Arquivos

Aprenda como o VFS do Linux apresenta sistemas de arquivos locais, de rede e virtuais por meio de uma única interface.

O Linux oferece suporte a muitas implementações de sistemas de arquivos, com diferentes formatos em disco, protocolos de rede, modelos de consistência, recursos e ferramentas operacionais. A escolha adequada depende do suporte da distribuição, da carga de trabalho, dos requisitos de recuperação, da topologia do armazenamento e da experiência do administrador.

## A Camada de Sistema de Arquivos Virtual

A camada Virtual Filesystem do kernel, ou VFS, fornece operações comuns como abertura, leitura, escrita, renomeação e verificações de permissões. As implementações de sistemas de arquivos conectam essas operações às suas próprias estruturas de dados e meios de armazenamento.

Isso permite que um único processo acesse ext4, XFS, NFS, tmpfs e procfs por meio de um modelo compartilhado de caminhos e descritores de arquivos. Porém, não torna idênticos todos os recursos ou comportamentos: diferenciação entre maiúsculas e minúsculas, bloqueio, permissões, garantias de renomeação, atributos estendidos e tratamento de erros podem variar.

## Journaling e Consistência após Falhas

Um sistema de arquivos com journaling registra atualizações selecionadas em um journal para poder repetir ou descartar transações incompletas após uma falha. O journaling serve principalmente para restaurar a consistência estrutural do sistema de arquivos mais rapidamente do que uma verificação completa.

Ele não garante que os dados mais recentes das aplicações tenham sobrevivido, que transações envolvendo vários arquivos sejam válidas nem que o hardware de armazenamento tenha respeitado todas as gravações concluídas. Os sistemas de arquivos oferecem diferentes modos de dados e garantias de ordenação, enquanto as aplicações devem usar padrões adequados de sincronização e atualização atômica. Um journal não é um backup e não protege contra exclusões, malware ou falhas do dispositivo.

## Sistemas de Arquivos Locais Comuns

- **ext4** é um sistema de arquivos maduro com journaling e amplo suporte nas distribuições Linux e ferramentas de recuperação.
- **XFS** é um sistema de arquivos escalável com journaling, normalmente escolhido para sistemas de arquivos grandes e cargas de trabalho com E/S paralela.
- **Btrfs** é um sistema de arquivos copy-on-write com checksums, subvolumes, snapshots e recursos integrados para vários dispositivos.

Os recursos exigem contexto operacional. Um snapshot do Btrfs inicialmente compartilha o armazenamento com sua origem e não é um backup independente quando permanece no mesmo dispositivo sujeito a falhas. XFS e ext4 possuem capacidades diferentes de expansão, redução, reparo e ajuste. Confirme o suporte do kernel instalado, do ambiente de boot e das ferramentas de recuperação antes de escolher ou alterar um sistema de arquivos raiz.

## Sistemas de Arquivos de Interoperabilidade, Rede e Virtuais

O Linux pode montar formatos de interoperabilidade como variantes FAT, exFAT e NTFS, mas suas semânticas de propriedade Unix, permissões, links e nomes de arquivos são diferentes. As opções de montagem e a implementação do driver determinam como o Linux apresenta os recursos ausentes.

Sistemas de arquivos de rede, como NFS e SMB, dependem de um servidor e de um protocolo de rede, com regras próprias de cache e identidade. Sistemas de arquivos virtuais, como tmpfs, procfs e sysfs, não usam um formato persistente comum em disco: o tmpfs armazena dados voláteis em páginas apoiadas por memória, enquanto procfs e sysfs expõem interfaces do kernel.

## Descoberta dos Tipos Ativos

Mostre os tipos de sistemas de arquivos montados com:

```bash
$ findmnt -o TARGET,SOURCE,FSTYPE,OPTIONS
```

Outras visualizações incluem `df -T` para contabilizar o espaço montado, `lsblk -f` para dispositivos de bloco e assinaturas de sistemas de arquivos detectadas e `/proc/filesystems` para os tipos reconhecidos ou suportados pelo kernel em execução. Elas respondem a perguntas diferentes; um sistema de arquivos desmontado não aparece em uma listagem comum de sistemas montados.

Use o laboratório [Gerenciamento de Partições e Sistemas de Arquivos Linux](https://labex.io/labs/comptia-manage-linux-partitions-and-filesystems-590845) em um armazenamento descartável para comparar tipos, opções de montagem e visualizações de descoberta.