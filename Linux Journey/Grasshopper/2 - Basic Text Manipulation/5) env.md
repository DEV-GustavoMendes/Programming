# env (Ambiente)

Aprenda como o Bash expande, exporta, inspeciona e substitui temporariamente variáveis de ambiente.

Todo processo possui um ambiente: uma coleção de cadeias de nome e valor herdadas de seu processo pai. Os shells usam variáveis de ambiente para transmitir aos programas iniciados configurações como idioma e caminhos de pesquisa de executáveis.

## Expansão dos Valores das Variáveis no Bash

O Bash expande `$NAME` ou `${NAME}` para o valor de uma variável antes de executar um comando. Coloque a expansão entre aspas para preservar o valor como um único argumento:

```bash
$ printf '%s\n' "$HOME"
/home/pete
```

Algumas variáveis de ambiente comuns são:

- `HOME`: caminho do diretório pessoal do usuário atual.
- `USER`: nome de usuário fornecido pelo ambiente de login em muitos sistemas.
- `PWD`: diretório de trabalho atual do shell.
- `PATH`: diretórios pesquisados em busca de nomes de comandos.

Os valores dependem do ambiente do processo atual; eles não são constantes universais. Uma variável não definida se expande para uma cadeia vazia, a menos que um comportamento mais rigoroso do shell esteja ativado.

## Inspeção do Ambiente Atual

Execute `env` sem operandos para mostrar o ambiente herdado pelo processo `env`:

```bash
$ env
```

A saída contém registros `NAME=value`, por exemplo:

```plaintext
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/bin
PWD=/home/user
USER=pete
```

Variáveis de ambiente podem conter credenciais, tokens, caminhos internos ou outros dados confidenciais. Não cole a saída completa de `env` em relatórios públicos ou logs sem revisá-la e ocultar os segredos.

## Localização de Comandos por meio de PATH

`PATH` é uma lista de diretórios separados por dois-pontos que o Bash pesquisa quando um nome de comando não contém uma barra:

```bash
$ printf '%s\n' "$PATH"
```

A ordem importa: o Bash usa o primeiro comando adequado que encontra conforme suas regras de resolução. Use `type -a NAME` para inspecionar como o shell atual resolve um nome.

Para acrescentar `/opt/coolapp/bin` ao início no shell atual e em seus futuros filhos, preservando o caminho de pesquisa existente:

```bash
$ export PATH="/opt/coolapp/bin:$PATH"
```

Não substitua acidentalmente `PATH` apenas pelo novo diretório nem acrescente diretórios graváveis não confiáveis. Qualquer um desses erros pode impedir a localização de comandos normais ou fazer um executável inesperado ser executado.

## Exportação de uma Variável para Processos Filhos

Variáveis do Bash não fazem parte automaticamente do ambiente fornecido aos processos filhos. Marque um nome para exportação com `export`:

```bash
$ export TEST=test
```

O processo Bash atual agora possui uma variável chamada `TEST`, e os comandos que ele inicia herdam `TEST=test`. Um processo filho não pode usar esse mecanismo para alterar o ambiente de seu pai.

```bash
$ printenv TEST
test
```

A atribuição normalmente dura até que você a remova ou o shell seja encerrado. Ela não modifica um ambiente de todo o sistema.

## Definição de um Valor para um Único Comando

Coloque atribuições antes de um comando para fornecer valores apenas ao ambiente desse comando:

```bash
$ LANG=C sort names.txt
```

O valor de `LANG` no shell atual não é alterado permanentemente. O utilitário `env` oferece outra forma explícita:

```bash
$ env LANG=C sort names.txt
```

Use `env -i COMMAND` para iniciar um comando com um ambiente inicialmente vazio e depois acrescente as atribuições necessárias. Muitos programas dependem de valores do ambiente; portanto, use essa opção conscientemente.

## Carregamento de Valores Pessoais em Sessões Futuras

Para recriar uma variável exportada em futuras sessões interativas do Bash, coloque uma linha `export` adequada no arquivo de inicialização realmente lido por essas sessões, normalmente `~/.bashrc` para o Bash interativo não iniciado como login:

```bash
export TEST=test
```

O Zsh normalmente usa `~/.zshrc`, enquanto o Fish usa uma sintaxe e configuração diferentes. Shells de login e não interativos podem ler outros arquivos; por isso, identifique o shell e o tipo de sessão em vez de presumir que um único arquivo configura todos os processos.

Para praticar a herança do ambiente e a configuração do shell, experimente estes laboratórios:

1. **[Gerenciamento do Ambiente e da Configuração do Shell no Linux](https://labex.io/labs/comptia-manage-shell-environment-and-configuration-in-linux-590838)** — Pratique a criação e o gerenciamento de variáveis locais e de ambiente, entenda a herança e torne configurações persistentes modificando `.bashrc`.
2. **[Variáveis de Ambiente no Linux](https://labex.io/labs/linux-environment-variables-in-linux-385274)** — Aprenda o conceito e o uso das variáveis de ambiente, como criá-las, modificá-las e gerenciá-las e seu papel na configuração do sistema.