# root

Aprenda como as políticas de su, sudo e sudoers fornecem acesso controlado a identidades privilegiadas.

A conta tradicionalmente chamada `root` possui UID 0 e ampla autoridade em seu contexto de segurança. Use uma conta sem privilégios para o trabalho cotidiano e eleve seus privilégios apenas para uma finalidade administrativa específica que você compreenda.

## Início de um Shell como Outro Usuário com su

`su`, que significa substitute user, inicia um shell ou comando com a identidade de outra conta. Sem um nome de usuário, o destino padrão é root:

```bash
$ su
```

A autenticação é controlada pelo PAM e pela política local. Um sistema pode solicitar a senha da conta de destino, restringir quem pode usar `su` ou manter a senha do root bloqueada. Não presuma que conhecer uma senha seja a única condição.

O `su` simples altera a identidade, mas preserva uma parte maior do ambiente atual. `su - USER`, também escrito como `su --login USER`, inicia um shell no estilo de login e configura um ambiente mais próximo de um novo login para a conta de destino:

```bash
$ su - operator
```

Saia do subshell quando o trabalho específico da conta de destino estiver concluído.

## Execução de um Comando Específico com sudo

`sudo COMMAND` solicita autorização da política para executar um comando como um usuário de destino, normalmente root por padrão. Use `-u USER` para solicitar outro destino:

```bash
$ sudo -u postgres id
```

Isso não significa que a solicitação será permitida. A política do sudo controla o usuário solicitante, o host, a identidade de destino, o comando e outras condições. Dependendo da configuração, a autenticação pode usar a senha do usuário solicitante, outro mecanismo ou não apresentar nenhuma solicitação.

Quando for viável, prefira um único comando administrativo de escopo restrito a um shell privilegiado de longa duração. O escopo menor reduz a probabilidade de comandos acidentais serem executados com autoridade elevada.

## Como Evitar Shells Privilegiados Persistentes

Comandos como `su -`, `sudo -s` ou `sudo -i` podem criar um shell privilegiado quando a política permite. Todos os comandos posteriores nesse shell podem ter impacto elevado até que você saia dele. Erros em caminhos, scripts não revisados e expansões do shell tornam-se mais perigosos.

O comportamento de auditoria depende da configuração. `sudo` normalmente registra as invocações, mas o registro da inicialização de um único shell não fornece automaticamente um histórico completo de todos os comandos digitados dentro dele. O histórico do shell, a auditoria do sistema e o registro de E/S do sudo são mecanismos distintos, cada um com suas próprias políticas.

## Revisão da Autorização do sudo

Execute `sudo -l` para listar o que a conta atual pode solicitar segundo a política ativa:

```bash
$ sudo -l
```

Revise os caminhos dos comandos, os usuários de destino permitidos e as restrições de argumentos. Uma regra aparentemente ampla não deve ser tratada como permissão para realizar trabalhos não relacionados.

## Edição Segura da Política do sudoers

A política padrão do sudo normalmente lê `/etc/sudoers` e pode incluir arquivos em `/etc/sudoers.d/`. Outras fontes de política são possíveis. A sintaxe controla muito mais do que uma simples lista de usuários e grupos.

Use `visudo` para alterar a política, pois ele bloqueia o arquivo e valida a sintaxe antes da instalação:

```bash
$ sudo visudo
```

Para um arquivo complementar, especifique seu caminho exato:

```bash
$ sudo visudo -f /etc/sudoers.d/application-admins
```

Não edite sudoers com um redirecionamento comum nem por meio de um fluxo de edição sem validação. Um erro de sintaxe ou permissão pode remover o acesso administrativo. Mantenha outro caminho de recuperação verificado disponível ao alterar a autorização remotamente.

Para praticar a administração delegada em um ambiente controlado, experimente este laboratório prático:

1. **[Configuração de Contas de Usuário e Privilégios sudo no Linux](https://labex.io/labs/comptia-configure-user-accounts-and-sudo-privileges-in-linux-590856)** — Pratique a aplicação de políticas de senha, o bloqueio e desbloqueio de contas de usuário, a proteção da conta root e a concessão de permissões administrativas, assuntos diretamente relacionados ao gerenciamento do acesso de superusuário.