Aprenda a inspecionar e alterar a propriedade de usuário e grupo dos objetos do sistema de arquivos Linux.

Todo objeto do sistema de arquivos Linux registra um usuário proprietário e um grupo proprietário. Essas identidades determinam qual trio de permissões do proprietário ou do grupo se aplica, mas não concedem por si só uma permissão específica. Inspecione tanto a propriedade quanto o modo com `ls -l`.

## Alteração do Usuário Proprietário

Use `chown`, abreviação de change owner, para atribuir outro usuário proprietário:

```bash
$ sudo chown patty myfile
```

Esse comando altera o usuário proprietário de `myfile` para `patty` e mantém seu grupo inalterado. Alterar o usuário proprietário de um arquivo normalmente exige privilégios adequados, mesmo que você seja seu proprietário atual. Essa restrição impede que usuários transfiram arquivos para evitar cotas ou outros controles baseados em propriedade.

## Alteração do Grupo Proprietário

Use `chgrp` para atribuir outro grupo proprietário:

```bash
$ chgrp whales myfile
```

Em sistemas comuns, um proprietário sem privilégios pode alterar o grupo de um arquivo somente para um grupo do qual esse usuário seja membro. Processos privilegiados podem realizar alterações mais amplas. A forma equivalente com `chown` começa com dois-pontos:

```bash
$ chown :whales myfile
```

Depois disso, os bits de modo do grupo se aplicam quando o kernel seleciona a classe do grupo; alterar o grupo não adiciona automaticamente bits de leitura, escrita ou execução.

## Alteração Conjunta do Usuário e do Grupo

Forneça `USER:GROUP` a `chown` para atualizar os dois campos em uma única operação:

```bash
$ sudo chown patty:whales myfile
```

O comando atribui `patty` como usuário proprietário e `whales` como grupo proprietário. Verifique o resultado em vez de presumir que a operação foi bem-sucedida:

```bash
$ ls -l myfile
```

## Cuidados com Alterações Recursivas

A opção `-R` altera a propriedade recursivamente, mas um comando recursivo amplo pode atravessar árvores de diretórios inesperadas ou afetar dados de serviços. Confirme o destino exato, entenda o comportamento de sua implementação para links simbólicos, examine previamente a árvore e verifique uma pequena amostra antes de alterar uma hierarquia extensa. Não copie comandos privilegiados de propriedade de exemplos para sistemas reais sem revisar seu escopo.

Para praticar em um ambiente isolado, o laboratório [Usuários, Grupos e Permissões de Arquivos no Linux](https://labex.io/labs/linux-linux-user-group-and-file-permissions-18002) aborda a inspeção e a modificação da propriedade junto com os modos dos arquivos.