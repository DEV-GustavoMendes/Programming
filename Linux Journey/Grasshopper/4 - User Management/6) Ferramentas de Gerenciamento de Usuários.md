# Ferramentas de Gerenciamento de Usuários

Aprenda a criar, modificar, proteger, verificar e remover contas locais com opções explícitas.

As distribuições Linux normalmente oferecem ferramentas de contas do conjunto de utilitários shadow, mas os padrões e as interfaces de nível superior variam. Antes de alterar uma conta local, confirme que ela não é gerenciada centralmente, consulte o manual local do comando e mantenha um caminho de recuperação.

Os comandos desta lição alteram o estado de autenticação e propriedade. Pratique apenas em um ambiente descartável autorizado, nunca em um host de produção.

## Revisão dos Padrões de Criação de Contas

`useradd` cria uma conta local usando as opções do comando e os padrões do sistema. Inspecione os padrões compilados e configurados com:

```bash
$ useradd -D
```

Arquivos como `/etc/default/useradd`, `/etc/login.defs` e o conteúdo do diretório de modelos podem influenciar o comportamento, mas suas funções variam conforme a distribuição. Pode existir um comando `adduser` de nível superior, porém sua interface não é padronizada em todos os sistemas Linux.

## Criação Explícita de uma Conta Local

Em um ambiente controlado, especifique as propriedades importantes em vez de depender de padrões desconhecidos:

```bash
$ sudo useradd -m -s /bin/bash -c "Bob Example" bob
```

- `-m` solicita a criação do diretório pessoal.
- `-s /bin/bash` escolhe o shell de login após confirmar que esse caminho é permitido e está instalado.
- `-c` fornece o campo GECOS/comentário.

A nova conta normalmente não consegue se autenticar com uma senha local válida até que uma seja definida, mas o estado inicial exato da senha e do bloqueio depende das ferramentas e políticas locais. Verifique os registros em vez de fazer suposições:

```bash
$ getent passwd bob
$ sudo passwd -S bob
$ id bob
```

## Definição ou Alteração de uma Senha

Um usuário comum altera sua própria senha local interativamente com:

```bash
$ passwd
```

Um administrador autorizado pode definir a senha de outra conta local com:

```bash
$ sudo passwd bob
```

Digite senhas somente no prompt protegido, não em argumentos de comandos, no histórico do shell, nas anotações da lição nem em conversas. A política do PAM pode rejeitar senhas fracas ou reutilizadas. Contas gerenciadas por serviços de diretório podem exigir outra ferramenta.

## Modificação das Propriedades e dos Grupos da Conta

`usermod` altera campos de contas locais. Alguns exemplos são:

```bash
$ sudo usermod -s /bin/zsh bob
$ sudo usermod -d /srv/home/bob -m bob
$ sudo usermod -aG developers bob
```

Antes de mover o diretório pessoal, verifique o destino, a propriedade, o espaço disponível, os processos em execução, as montagens e os serviços. Para grupos suplementares, `-aG` significa acrescentar à lista atual. Usar `-G` sem `-a` substitui toda a lista de grupos suplementares e pode remover acessos inesperadamente.

As alterações de grupos normalmente afetam novas sessões de login, não os processos que já estão em execução com o conjunto antigo de credenciais.

## Bloqueio de uma Senha Local

Um administrador pode bloquear o hash da senha local com `passwd -l USER` e inspecionar seu estado com `passwd -S USER`. O desbloqueio é realizado com `passwd -u USER` somente após verificar por que o bloqueio existe e se ainda há um hash válido.

O bloqueio de uma senha não necessariamente impede chaves SSH, tokens, tarefas agendadas, processos já em execução nem a autenticação específica de serviços. Para desabilitar uma conta de forma abrangente, defina a ameaça e os caminhos de acesso e aplique uma política coordenada, que pode incluir a expiração da conta, o shell de login, o acesso a serviços, chaves e o encerramento de sessões.

## Remoção Deliberada de uma Conta Local

`userdel bob` sem opções remove os registros da conta local, mas normalmente mantém o diretório pessoal. `userdel -r bob` também tenta remover o diretório pessoal e a caixa de correio, tornando-se uma operação destrutiva.

Antes de qualquer remoção:

1. Confirme a conta exata com `getent passwd bob` e `id bob`.
2. Identifique processos em execução, tarefas agendadas, serviços, chaves e acessos delegados.
3. Faça um inventário dos arquivos pertencentes ao UID nos sistemas de arquivos pretendidos.
4. Decida se os dados devem ser transferidos, arquivados, mantidos ou excluídos com segurança.
5. Confirme que o UID não será reatribuído enquanto ainda houver arquivos órfãos.

`userdel -r` não garante a remoção de arquivos fora dos locais configurados para o diretório pessoal e a caixa de correio. A exclusão da conta também pode deixar propriedades numéricas em arquivos, permissões de bancos de dados, identidades de aplicações e registros em diretórios remotos.

Para praticar o ciclo de vida das contas em um ambiente isolado, experimente estes laboratórios práticos:

1. **[Gerenciamento de Contas de Usuário Linux com useradd, usermod e userdel](https://labex.io/labs/comptia-manage-linux-user-accounts-with-useradd-usermod-and-userdel-590837)** — Pratique todo o ciclo de administração de usuários, desde a criação e proteção de novas contas até sua modificação e exclusão.
2. **[Gerenciamento de Grupos Linux com groupadd, usermod e groupdel](https://labex.io/labs/comptia-manage-linux-groups-with-groupadd-usermod-and-groupdel-590836)** — Adquira experiência prática com os principais utilitários de linha de comando para administração de grupos, incluindo a adição, a modificação e a exclusão de grupos.
3. **[Configuração de Contas de Usuário e Privilégios sudo no Linux](https://labex.io/labs/comptia-configure-user-accounts-and-sudo-privileges-in-linux-590856)** — Aprenda técnicas essenciais de gerenciamento de contas de usuário e privilégios sudo para aumentar a segurança de um sistema Linux.