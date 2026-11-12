# Links Simbólicos

Aprenda como links simbólicos e físicos diferem na resolução de caminhos, na identidade dos inodes e no escopo do sistema de arquivos.

Uma entrada de diretório atribui um nome a um inode. Um link físico cria outra entrada de diretório para o mesmo inode, enquanto um link simbólico cria um inode diferente cujo conteúdo é um caminho a ser resolvido. Essa diferença controla a identidade, a duração e o comportamento entre sistemas de arquivos.

## Criação e Inspeção de um Link Simbólico

Crie um link simbólico com `ln -s TARGET LINK_NAME`:

```bash
$ printf '%s\n' 'example' > myfile
$ ln -s -- myfile myfilelink
$ ls -li myfile myfilelink
151   -rw-r--r-- 1 user user 8 ... myfile
93403 lrwxrwxrwx 1 user user 6 ... myfilelink -> myfile
```

O link simbólico possui seu próprio inode e armazena o texto `myfile`. Quando um programa segue `myfilelink`, a resolução do caminho continua até o destino. Exiba o texto armazenado sem segui-lo com:

```bash
$ readlink myfilelink
```

## Destinos Relativos e Absolutos de Links Simbólicos

Um destino absoluto começa em `/`. Um destino relativo é resolvido em relação ao diretório que contém o link simbólico — não em relação ao diretório atual do shell no momento em que alguém o abrir mais tarde.

```bash
$ mkdir -p tree/data tree/current
$ printf '%s\n' 'value' > tree/data/item
$ ln -s ../data/item tree/current/item
```

Mover toda a hierarquia `tree` preserva essa relação relativa. Mover apenas o link ou o destino pode quebrá-la. Um link simbólico pode conter um destino inexistente e, nesse caso, é chamado de pendente ou quebrado.

## Criação de um Link Físico

Crie outro nome para um arquivo comum existente sem `-s`:

```bash
$ ln -- myfile myhardlink
$ ls -li myfile myhardlink
151 -rw-r--r-- 2 user user 8 ... myfile
151 -rw-r--r-- 2 user user 8 ... myhardlink
```

Os dois nomes apontam para o mesmo sistema de arquivos e número de inode. A contagem de links passa a ser 2. Nenhum dos nomes é inerentemente o “original”; alterar o conteúdo por um deles modifica o objeto compartilhado, e remover um nome preserva o outro.

Links físicos não podem atravessar os limites dos sistemas de arquivos, pois um número de inode só possui significado dentro de seu sistema. O Linux também impede usuários comuns de criar links físicos para diretórios e pode restringir links para arquivos que eles não possuem, evitando ciclos e problemas de segurança.

## Duração e Exclusão

Remover um link simbólico exclui esse objeto de link, não seu destino:

```bash
$ rm -- myfilelink
```

Remover o nome de um link físico reduz a contagem de links do inode compartilhado. O sistema de arquivos só pode recuperar o objeto depois que a contagem chega a zero e nenhum descritor de arquivo aberto ou outra referência do sistema de arquivos o mantém existente.

Evite uma barra final ao remover um link simbólico para um diretório, pois a resolução de caminhos com barra final pode seguir a semântica de diretórios, dependendo do comando. Inspecione com `ls -ld -- LINK` e remova deliberadamente o nome do link.

## Seguimento Seguro dos Links

Links simbólicos podem redirecionar um programa privilegiado para fora de um diretório esperado ou mudar entre a validação e o uso. Programas seguros devem evitar condições de corrida entre verificar e abrir caminhos e usar interfaces relativas a diretórios, sem seguimento ou com resolução restrita, conforme a linguagem e o sistema operacional.

Para a inspeção comum:

- `ls -ld LINK` mostra o próprio link.
- `readlink LINK` imprime o texto de destino armazenado.
- `stat LINK` normalmente informa os metadados do link, enquanto `stat -L LINK` o segue no GNU coreutils.
- `find -L` segue links e pode encontrar ciclos; use-o apenas intencionalmente.

As permissões mostradas como `lrwxrwxrwx` não são uma concessão geral de acesso. O acesso é decidido pela travessia dos diretórios, pela política de seguimento dos links e pelas permissões do destino; a propriedade dos links simbólicos também importa para algumas regras de diretórios protegidos.

Use o laboratório [Gerenciamento de Arquivos e Diretórios no Linux](https://labex.io/labs/comptia-manage-files-and-directories-in-linux-590835) para praticar links em arquivos descartáveis e comparar números de inodes.

