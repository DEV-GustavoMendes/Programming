# Permissões de Arquivos

Aprenda a interpretar os tipos de arquivos Linux e os bits de permissão do proprietário, do grupo e dos outros usuários.

O Linux representa muitos recursos por meio de interfaces semelhantes a arquivos, e cada objeto do sistema de arquivos possui metadados que controlam o acesso. Saber interpretar esses metadados é fundamental para trabalhar com arquivos e diretórios de forma segura.

## Leitura de uma Listagem Longa

Use `ls -l` para exibir uma listagem longa:

```bash
$ ls -ld Desktop/
drwxr-xr-x 2 pete penguins 4096 Dec 1 11:45 Desktop/
```

O primeiro campo, `drwxr-xr-x`, combina um caractere de tipo de arquivo com nove caracteres de permissão. A listagem também identifica `pete` como proprietário e `penguins` como o grupo associado ao diretório.

O caractere inicial descreve o tipo do objeto. Os valores comuns incluem:

- `-` para um arquivo comum
- `d` para um diretório
- `l` para um link simbólico

Também existem outros tipos de arquivos especiais. Os nove caracteres restantes são as permissões de acesso:

```text
d | rwx | r-x | r-x
```

## Compreensão de `r`, `w` e `x`

Cada trio de permissões usa estes caracteres:

- `r` concede permissão de leitura.
- `w` concede permissão de escrita.
- `x` concede permissão de execução.
- `-` significa que a permissão está ausente.

Para um arquivo comum, a leitura permite acessar seu conteúdo, a escrita permite modificar seu conteúdo e a execução permite que o kernel tente executá-lo como um programa. A execução ainda pode falhar se o formato do arquivo, a linha do interpretador, as opções de montagem ou outro controle de segurança não a permitirem.

Para um diretório, os significados se referem às entradas do diretório:

- A leitura permite listar os nomes no diretório.
- A escrita permite criar ou remover entradas, normalmente em conjunto com a permissão de execução.
- A execução, também chamada de permissão de busca, permite percorrer o diretório e acessar entradas pelo nome.

A exclusão de um arquivo é determinada principalmente pelas permissões do diretório pai, não pelo bit de escrita do próprio arquivo.

## Classes de Proprietário, Grupo e Outros

Os nove caracteres de modo formam três trios em uma ordem fixa:

1. **Proprietário**: permissões usadas quando o ID de usuário efetivo do processo corresponde ao proprietário do arquivo.
2. **Grupo**: permissões usadas quando um ID de grupo aplicável do processo corresponde ao grupo do arquivo.
3. **Outros**: permissões usadas quando nenhuma das classes anteriores corresponde.

O kernel seleciona uma única classe aplicável; ele não combina os três trios para encontrar o resultado mais permissivo. Mecanismos adicionais, como listas de controle de acesso, opções de montagem, capacidades ou controles de acesso obrigatórios, podem afetar ainda mais a decisão final.

No exemplo, o trio do proprietário é `rwx`, enquanto os trios do grupo e dos outros são `r-x`. O proprietário pode ler, escrever e percorrer o diretório. As classes do grupo e dos outros podem ler e percorrê-lo, mas não podem criar nem remover entradas por meio dos bits de modo comuns do diretório.

Para reforçar esses conceitos em um ambiente isolado, experimente o laboratório [Usuários, Grupos e Permissões de Arquivos no Linux](https://labex.io/labs/linux-linux-user-group-and-file-permissions-18002). Ele oferece prática na leitura de modos e na alteração de propriedades e permissões.