# cd (Mudar de Diretório)

Aprenda a usar cd com caminhos e atalhos para navegar pelo sistema de arquivos do Linux.

Para navegar pelo sistema de arquivos do Linux, você usa caminhos que especificam seu destino. A principal ferramenta para isso é o comando `cd`, abreviação de change directory, ou mudar de diretório. Ele altera o diretório de trabalho atual do shell.

Para navegar pelo sistema de arquivos do Linux, você usa caminhos que especificam seu destino. A principal ferramenta para isso é o comando `cd`, abreviação de change directory, ou mudar de diretório. Ele altera o diretório de trabalho atual do shell.

O destino deve ser um diretório, não um arquivo comum. Se o diretório não existir, se o nome estiver digitado incorretamente ou se você não tiver permissão para entrar nele, `cd` informará um erro em vez de mudar de localização.

A sintaxe básica é:

```bash
cd [DIRECTORY]
```

## Compreensão dos Caminhos

Há duas maneiras de especificar um caminho: absoluta e relativa.

- **Caminho absoluto**: o caminho completo a partir do diretório raiz (`/`). Por exemplo: `/home/pete/Desktop`.
    
- **Caminho relativo**: um caminho baseado em sua localização atual. Se você estiver em `/home/pete/Documents` e quiser acessar um subdiretório chamado `taxes`, poderá usar `taxes/`.

## Uso do Comando cd

Para mudar para um diretório específico usando um caminho absoluto, digite:

```bash
$ cd /home/pete/Pictures
```

Esse comando leva você diretamente ao diretório `Pictures`.

Confirme sua localização com `pwd`:

```bash
$ pwd
/home/pete/Pictures
```

## Navegação para um Subdiretório

Se você já estiver em um diretório e quiser entrar em um de seus subdiretórios, use um caminho relativo. Por exemplo, se sua localização atual for `/home/pete/Pictures` e ela contiver uma pasta chamada `Hawaii`, você poderá entrar nela com:

```bash
$ cd Hawaii
```

Observe que usamos apenas o nome da pasta. Isso funciona porque já estávamos em seu diretório pai, `/home/pete/Pictures`.


## Atalhos Essenciais de Navegação

Navegar usando caminhos completos pode ser cansativo. Felizmente, o shell oferece vários atalhos que agilizam bastante a movimentação.

- `.` (diretório atual): representa o diretório em que você está.
- `..` (diretório pai): leva você um nível acima, ao diretório que contém o atual.
- `~` (diretório pessoal): é um atalho para seu diretório pessoal, como `/home/pete`.
- `-` (diretório anterior): leva você de volta ao último diretório em que esteve.

Você pode usar esses atalhos com `cd`:

```bash
$ cd .
$ cd ..
$ cd ~
$ cd -
```

Experimente esses atalhos para se tornar mais eficiente na linha de comando.

## Exemplos Práticos de cd

Vá para seu diretório pessoal:

```bash
$ cd
```

Executar `cd` sem um argumento de diretório também leva você ao diretório pessoal.

Suba dois níveis:

```bash
$ cd ../..
```

Entre em um diretório cujo nome contém espaços colocando-o entre aspas:

```bash
$ cd "Vacation Photos"
```

Volte ao diretório anterior:

```bash
$ cd -
/home/pete/Documents
```

Para reforçar sua compreensão sobre a navegação em diretórios no Linux, experimente estes laboratórios práticos:

1. **[Comando cd do Linux: Mudança de Diretórios](https://labex.io/labs/linux-linux-cd-command-directory-changing-209733)** — Aprenda a usar `cd` para navegar com eficiência pelo sistema de arquivos, incluindo diferentes técnicas para mudar de diretório, compreender caminhos e explorar a estrutura de arquivos.
2. **[Navegação em Diretórios no Linux](https://labex.io/labs/linux-directory-navigation-387844)** — Teste suas habilidades básicas na linha de comando navegando por diretórios com comandos essenciais.
3. **[Configuração da Estrutura de um Novo Projeto](https://labex.io/labs/linux-setting-up-a-new-project-structure-387859)** — Pratique o gerenciamento de diretórios criando uma estrutura de projeto específica e navegando por ela com comandos essenciais como `mkdir` e `cd`.