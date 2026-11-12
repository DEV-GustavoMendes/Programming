# /etc/passwd

Aprenda a ler registros passwd locais e diferenciá-los da visão completa de contas fornecida pelo NSS.

`/etc/passwd` armazena registros de contas locais em um formato de texto separado por dois-pontos. Ele mapeia nomes de login para UIDs numéricos e registra um GID primário, um campo descritivo, o caminho do diretório pessoal e o programa de login.

## Registros Locais e Contas Resolvidas

Exiba o arquivo local com um comando somente para leitura:

```bash
$ cat /etc/passwd
```

Isso não representa necessariamente todas as contas conhecidas pelo sistema. O Name Service Switch (NSS) pode resolver contas a partir de arquivos, serviços de diretório, bancos de dados do sistema ou outras fontes configuradas. Use `getent` para consultar o banco de dados passwd resolvido:

```bash
$ getent passwd
$ getent passwd root
```

O primeiro comando pode revelar nomes de contas e metadados, portanto revise a saída antes de compartilhá-la publicamente.

## Leitura dos Sete Campos

Um registro local geralmente se parece com isto:

```text
root:x:0:0:root:/root:/bin/bash
```

Os sete campos separados por dois-pontos são:

1. **Nome de login**: O nome legível da conta, como `root`.
2. **Campo de senha**: Normalmente `x` em um sistema com senhas shadow, indicando que os dados protegidos da senha são armazenados separadamente.
3. **UID**: A identidade numérica do usuário. O UID 0 recebe o tratamento tradicional de superusuário.
4. **GID primário**: O ID numérico do grupo primário da conta.
5. **GECOS/comentário**: Informações descritivas da conta, frequentemente separadas internamente por vírgulas.
6. **Diretório pessoal**: O caminho usado como configuração do diretório pessoal da conta; ele pode não existir no disco.
7. **Shell/programa de login**: O programa solicitado para as sessões de login aplicáveis, como `/bin/bash` ou um programa que não permite login.

O kernel não exige que os valores de UID sejam exclusivos em registros malformados ou deliberadamente duplicados, mas contas que compartilham um UID tornam-se indistinguíveis em muitas decisões de propriedade e permissões. Em geral, os administradores devem manter os UIDs das contas exclusivos.

## Interpretação do Marcador de Senha

Em sistemas comuns com senhas shadow, `x` no campo 2 direciona as ferramentas de senha para os dados protegidos em `/etc/shadow`. Valores como `*` ou `!` não são hashes de senha válidos e geralmente impedem a autenticação com uma senha Unix por meio desse registro.

Isso não prova que a conta não possa se autenticar por qualquer método. Chaves SSH, certificados, tokens ou mecanismos específicos de serviços podem ser independentes. Da mesma forma, um campo de senha vazio possui um comportamento sensível à segurança que depende da pilha de autenticação; não o crie nem tente “corrigi-lo” manualmente.

## Reconhecimento de Contas de Serviço

Muitos registros representam serviços, não pessoas. Identidades de serviço separadas ajudam a limitar arquivos e processos à autoridade necessária para um daemon. Seus caminhos de diretórios pessoais podem ser incomuns ou inexistentes, e seu programa de login pode ser `/usr/sbin/nologin`, `/bin/false` ou outro programa restrito.

Não deduza a finalidade de uma conta apenas pelo intervalo de UID sem verificar a política da distribuição. Os intervalos de alocação variam, e contas gerenciadas centralmente podem seguir convenções diferentes.

## Modificação Segura dos Registros de Contas

Prefira ferramentas de gerenciamento de contas, como `useradd`, `usermod` e `userdel`, pois elas coordenam os registros relacionados e aplicam os padrões do sistema. O comportamento exato dessas ferramentas pode ser configurado pela distribuição, portanto revise as opções antes de alterar uma conta.

Se um banco de dados passwd local realmente precisar de reparo manual, use `vipw` em vez de um editor comum. Ele aplica um bloqueio destinado a evitar edições simultâneas. Valide os bancos de dados com ferramentas como `pwck` e mantenha uma sessão de recuperação antes de alterar arquivos de autenticação remotamente.

Para praticar com registros de usuários e grupos em um ambiente controlado, experimente estes laboratórios práticos:

1. **[Gerenciamento de Contas de Usuário Linux com useradd, usermod e userdel](https://labex.io/labs/comptia-manage-linux-user-accounts-with-useradd-usermod-and-userdel-590837)** — Pratique todo o ciclo de administração de usuários, desde a criação e proteção de novas contas até sua modificação e exclusão.
2. **[Gerenciamento de Grupos Linux com groupadd, usermod e groupdel](https://labex.io/labs/comptia-manage-linux-groups-with-groupadd-usermod-and-groupdel-590836)** — Adquira experiência prática com os principais utilitários de linha de comando para administração de grupos, incluindo a criação de novos grupos e a alteração das associações dos usuários.