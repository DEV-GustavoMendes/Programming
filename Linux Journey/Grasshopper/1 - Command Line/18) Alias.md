# alias

Aprenda a criar, inspecionar, persistir, ignorar e remover aliases de comandos no Bash.

Um alias instrui um shell interativo a substituir uma palavra de comando por outra sequência antes de executar a linha. Isso pode abreviar um comando frequente ou fornecer um conjunto preferido de opções.

## Criação de um Alias no Shell Atual

No Bash, defina um alias com `alias NAME='REPLACEMENT'`. Não coloque espaços ao redor do sinal de igualdade:

```bash
$ alias ll='ls -la'
```

Depois dessa definição, inserir `ll` como comando o expande para `ls -la`. As aspas mantêm a substituição agrupada durante a definição do alias.

Aliases são mais adequados para substituições simples do prefixo de um comando. Use uma função do shell quando precisar processar argumentos de maneira mais estruturada.

## Carregamento de um Alias em Sessões Futuras do Bash

Um alias definido no prompt pertence ao shell atual e desaparece quando ele é encerrado. Sessões interativas não iniciadas como login do Bash normalmente leem `~/.bashrc`; por isso, esse arquivo é o local habitual para aliases pessoais do Bash:

```bash
alias ll='ls -la'
```

Depois de editar o arquivo, inicie uma nova sessão interativa do Bash ou recarregue-o no shell atual:

```bash
$ source ~/.bashrc
```

O comportamento de inicialização pode variar conforme o shell, o modo de login e a configuração da distribuição. Um usuário do Zsh, por exemplo, normalmente usaria a configuração do Zsh, não o `.bashrc` do Bash.

## Inspeção de Aliases e da Resolução de Nomes

Execute `alias` sem argumentos para listar os aliases do shell atual:

```bash
$ alias
alias ll='ls -la'
alias grep='grep --color=auto'
```

Use `type NAME` para inspecionar como o Bash resolve um nome específico:

```bash
$ type ll
ll is aliased to 'ls -la'
```
## Como Ignorar e Remover um Alias

Para ignorar um alias em uma única linha, coloque uma barra invertida antes do nome do comando ou use-o depois do comando interno `command` do Bash:

```bash
$ \ls
$ command ls
```

Isso é útil quando você precisa do comportamento normal do comando subjacente. Mantenha os aliases curtos e previsíveis e evite ocultar comportamentos surpreendentes ou destrutivos por trás de nomes conhecidos.

Remova um alias do shell atual com `unalias`:

```bash
$ unalias ll
```

Se a definição continuar em `~/.bashrc`, um shell futuro poderá recriá-la. Remova ou altere também essa linha de configuração quando quiser excluir o alias permanentemente.