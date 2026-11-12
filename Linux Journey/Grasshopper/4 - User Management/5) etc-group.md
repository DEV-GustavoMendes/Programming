# /etc/group

Aprenda como os registros de grupos locais mapeiam nomes para GIDs e listam membros suplementares.

`/etc/group` armazena registros de grupos locais. Ele mapeia nomes de grupos para GIDs numéricos e lista membros explícitos, oferecendo suporte ao controle de acesso compartilhado por várias contas.

## Grupos Locais e Grupos Resolvidos

O arquivo é apenas uma das possíveis fontes de grupos. O NSS pode resolver grupos a partir de arquivos locais, serviços de diretório ou outros bancos de dados configurados. Exiba os registros locais com:

```bash
$ cat /etc/group
```

Consulte o banco de dados de grupos resolvido com `getent`:

```bash
$ getent group
$ getent group developers
```

As listas de grupos podem revelar nomes internos de contas e funções, portanto revise a saída antes de compartilhá-la.

## Leitura dos Quatro Campos

Um registro local possui quatro campos separados por dois-pontos:

```text
developers:x:1500:alice,bob
```

1. **Nome do grupo**: `developers`.
2. **Campo de senha**: Normalmente `x`, `*` ou outro marcador; os dados protegidos da senha do grupo podem ser armazenados em `/etc/gshadow`.
3. **GID**: A identidade numérica do grupo, `1500` neste caso.
4. **Lista de membros**: Nomes dos membros explícitos separados por vírgulas, `alice` e `bob` neste caso.

As senhas de grupos são um recurso legado usado por ferramentas como `newgrp` em algumas configurações. Elas não são o mecanismo normal para conceder autorização sudo e não devem ser introduzidas por meio de edições manuais dos campos.

## Consideração da Associação ao Grupo Primário

A lista de membros em `/etc/group` normalmente não repete os usuários cujo registro passwd indica esse GID como grupo primário. Portanto, um usuário pode pertencer ao grupo mesmo que seu nome não apareça no campo 4.

Por exemplo, se o registro passwd de Alice tiver 1500 como GID primário, ela pertence a `developers` mesmo que o registro local do grupo termine com um campo de membros vazio:

```text
developers:x:1500:
```

É por isso que analisar somente o campo 4 produz uma visão incompleta das associações.

## Inspeção dos Grupos de um Usuário

Use `id USER` ou `groups USER` para obter a visão de uma conta resolvida:

```bash
$ id alice
$ groups alice
```

Para o processo atual, `id` sem argumentos informa os grupos efetivamente presentes em suas credenciais. Uma associação suplementar recém-configurada normalmente não aparece em uma sessão de login já em execução; inicie uma nova sessão autenticada ou use um mecanismo configurado deliberadamente, como `newgrp`, quando for apropriado.

## Alteração Segura dos Grupos Locais

Use ferramentas como `groupadd`, `groupmod`, `groupdel`, `gpasswd` e `usermod` em vez de editar registros com um editor de uso geral. Tenha atenção especial a:

- `usermod -aG GROUP USER`, que acrescenta uma associação suplementar.
- `usermod -G ...`, que substitui a lista de grupos suplementares quando `-a` é omitido.

Se o reparo manual do banco de dados local for inevitável, use `vigr` para o bloqueio e `grpck` para a validação. Mantenha um caminho de recuperação antes de realizar alterações remotas de identidade.

Para praticar o gerenciamento de grupos locais em um ambiente controlado, experimente estes laboratórios práticos:

1. **[Gerenciamento de Contas de Usuário Linux com useradd, usermod e userdel](https://labex.io/labs/comptia-manage-linux-user-accounts-with-useradd-usermod-and-userdel-590837)** — Pratique todo o ciclo de administração de usuários, desde a criação e proteção de novas contas até sua modificação e exclusão.
2. **[Gerenciamento de Grupos Linux com groupadd, usermod e groupdel](https://labex.io/labs/comptia-manage-linux-groups-with-groupadd-usermod-and-groupdel-590836)** — Adquira experiência prática com os principais utilitários de linha de comando para administração de grupos, incluindo `groupadd`, `usermod` e `groupdel`.
3. **[Adição de Novo Usuário e Grupo](https://labex.io/labs/linux-add-new-user-and-group-17987)** — Simule a inclusão de novos membros de uma equipe em um servidor, criando contas de usuário, configurando grupos personalizados e gerenciando associações.