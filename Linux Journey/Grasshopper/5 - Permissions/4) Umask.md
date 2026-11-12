# Umask

Aprenda como o umask de um processo limita os bits de permissão solicitados para novos arquivos e diretórios.

A máscara de criação de arquivos de um processo, ou umask, impede que determinados bits de permissão sejam definidos quando esse processo cria um objeto do sistema de arquivos. Ela é uma máscara, não um modo padrão completo: primeiro a aplicação solicita um modo, e então o kernel remove os bits proibidos pelo umask.

Conceitualmente:

```text
resulting mode = requested mode AND NOT umask
```

As listas de controle de acesso e o comportamento das aplicações podem acrescentar outros detalhes, portanto inspecione o resultado quando as permissões exatas forem importantes.

## Visualização e Definição do Umask

Execute `umask` sem um operando para exibir a máscara do shell atual, normalmente em formato octal:

```bash
$ umask
0022
```

Defina-a para o shell atual e para os processos iniciados posteriormente por esse shell:

```bash
$ umask 027
```

Cada posição octal corresponde ao proprietário, ao grupo e aos outros. Um bit da máscara remove a permissão solicitada correspondente: `2` mascara a escrita, `4` mascara a leitura e `1` mascara a execução.

## Cálculo dos Modos de Novos Arquivos e Diretórios

Muitos programas comuns solicitam `0666` para novos arquivos regulares, pois criar arquivos executáveis por padrão não seria seguro. Eles normalmente solicitam `0777` para novos diretórios, nos quais a permissão de execução é necessária para a travessia.

Com o umask `0022`:

```text
regular file: 0666 masked by 0022 -> 0644 (rw-r--r--)
directory:    0777 masked by 0022 -> 0755 (rwxr-xr-x)
```

O umask apenas remove bits solicitados. Ele não pode adicionar a permissão de execução quando uma aplicação não a solicitou. Uma aplicação também pode solicitar um modo inicial mais restritivo, produzindo um resultado mais restritivo.

## Escopo e Persistência

Alterar o umask em um shell não modifica seu processo pai nem sessões não relacionadas. O valor se aplica às criações futuras desse shell e de seus descendentes; os arquivos existentes mantêm seus modos.

Para tornar persistente um valor de sua preferência, configure-o no login, shell, PAM, gerenciador de serviços ou aplicação apropriado para seu ambiente. O local correto varia, e os serviços podem definir seu próprio umask. Não presuma que editar um único arquivo de shell interativo controlará todos os processos do sistema.

Para praticar, crie arquivos e diretórios sob diferentes máscaras em um ambiente isolado e compare seus modos com `ls -ld`. O laboratório [Usuários, Grupos e Permissões de Arquivos no Linux](https://labex.io/labs/linux-linux-user-group-and-file-permissions-18002) oferece um ambiente apropriado para permissões.