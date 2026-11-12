# Editores de Texto

Aprenda a escolher e configurar um editor de texto de terminal para administração e desenvolvimento no Linux.

Configurações, scripts, código-fonte e logs do Linux costumam ser armazenados como texto simples. Um editor de terminal permite trabalhar nesses arquivos em um terminal local, uma sessão SSH remota ou um ambiente sem área de trabalho gráfica.

## Escolha de um Editor para o Ambiente

Nenhum editor é o melhor para todas as pessoas ou tarefas. Editores gráficos, editores de terminal e ambientes integrados de desenvolvimento podem ser adequados. Para o trabalho na linha de comando, escolha um editor instalado, do qual você saiba sair com segurança e cujo modelo básico de edição compreenda.

Não presuma que Vim ou Emacs esteja instalado. Verifique a resolução dos comandos no shell atual:

```bash
$ command -v vim
/usr/bin/vim
$ command -v emacs
/usr/bin/emacs
```

Um resultado vazio com status diferente de zero significa que o nome não foi encontrado pela pesquisa de comandos atual. Sistemas mínimos podem fornecer `vi`, enquanto outros incluem Nano ou nenhum editor interativo.

## Compreensão do Modelo do Vim

O Vim é um editor modal. A mesma tecla pode ter significados diferentes conforme o modo atual:

- O modo Normal interpreta as teclas como comandos de navegação e edição.
- O modo de Inserção insere o texto digitado.
- O modo de Linha de Comando aceita comandos como gravar ou sair.

Esse modelo torna a edição repetitiva pelo teclado eficiente depois de alguma prática, mas novos usuários precisam acompanhar o modo ativo. As próximas lições apresentam uma operação do Vim por vez.

## Compreensão do Modelo do Emacs

O Emacs normalmente usa combinações de teclas e comandos nomeados dentro de um ambiente extensível. Os arquivos são visitados em buffers, e modos principais e secundários personalizam o comportamento para diferentes conteúdos e tarefas. O Emacs pode ser executado em um terminal ou em um frame gráfico.

Tanto Vim quanto Emacs permitem muito mais que a edição básica por meio de configurações e extensões. Comece abrindo, alterando, salvando e fechando um arquivo de texto simples antes de acrescentar personalizações.

## Definição de um Editor Preferido

Muitos programas de linha de comando consultam `VISUAL` ou `EDITOR` quando precisam iniciar um editor. Por exemplo, escolha o Vim para comandos iniciados pela sessão atual do Bash e seus filhos:

```bash
$ export VISUAL=vim
$ export EDITOR="$VISUAL"
```

Essas variáveis expressam uma preferência; elas não instalam o programa. Use um comando que realmente exista e coloque as exportações no arquivo de inicialização adequado somente depois de testá-las.

## Prática sem Arriscar Arquivos Importantes

Aprenda usando um arquivo descartável em um diretório de sua propriedade:

```bash
$ printf 'first line\nsecond line\n' > editor-practice.txt
$ vim editor-practice.txt
```

Evite começar com configurações do sistema ou dados de outro usuário. Faça um backup antes de alterar um arquivo importante, entenda como salvar e sair e examine o resultado com um comando somente para leitura, como `cat` ou `diff`.

Para praticar a abertura, a edição e o salvamento de arquivos de texto no terminal, experimente este laboratório:

1. **[Edição de Arquivos de Texto no Linux com Vim e Nano](https://labex.io/labs/comptia-edit-text-files-in-linux-with-vim-and-nano-591076)** — Pratique a criação, edição e gravação de arquivos e a navegação tanto com vi/vim quanto com nano.