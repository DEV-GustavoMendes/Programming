# Vim (Vi Improved)

Aprenda o que é o Vim, sua relação com vi e como abrir arquivos, a ajuda e a prática guiada.

O Vim é um editor de texto configurável cujo nome significa **Vi Improved**. Ele preserva o modelo de edição modal associado ao editor `vi` original e acrescenta recursos como desfazer em vários níveis, suporte à sintaxe, scripts e um amplo sistema de ajuda.

## Relação entre Vim e vi

`vi` descreve tanto um editor histórico quanto uma interface de comandos comum. Em um sistema Linux, `vi` pode iniciar o Vim em um modo orientado à compatibilidade; em outro, pode iniciar uma implementação diferente de vi. Não presuma que todo comando `vi` ofereça todos os recursos do Vim.

Verifique o que o shell atual resolve:

```bash
$ command -v vim
/usr/bin/vim
$ command -v vi
/usr/bin/vi
```

O caminho resolvido não identifica por si só se `vi` e `vim` são a mesma implementação. `type -a vi vim` e a saída de versão do editor podem fornecer mais detalhes.

## Abertura do Vim e de Arquivos

Inicie o Vim com um buffer sem nome:

```bash
$ vim
```

Forneça um caminho para editar esse arquivo:

```bash
$ vim filename.txt
```

Se `filename.txt` existir e puder ser lido, o Vim carregará seu conteúdo em um buffer. Se o caminho não existir, o Vim abrirá um novo buffer associado ao nome; nenhum arquivo será criado até que você grave o buffer com sucesso.

O Vim não ignora as permissões do sistema de arquivos. Abrir um arquivo não garante que sua conta consiga salvar alterações nesse caminho.

## Uso dos Recursos Integrados de Aprendizado

Se a instalação do Vim incluir `vimtutor`, execute-o no shell para uma lição prática interativa:

```bash
$ vimtutor
```

Dentro do Vim, entre no modo Normal com `Esc`, digite `:help` e pressione Enter para abrir o sistema de ajuda. Um tópico específico pode vir depois do comando:

```vim
:help user-manual
:help :write
```

As tags da ajuda são precisas; por isso, a pontuação pode importar. Use `Ctrl+]` em um link da ajuda para segui-lo e `Ctrl+T` para retornar.

## Prática com um Arquivo Descartável

Comece com um arquivo em um diretório de sua propriedade:

```bash
$ printf 'alpha\nbeta\n' > vim-practice.txt
$ vim vim-practice.txt
```

As próximas lições apresentam pesquisa, navegação, inserção, edição e salvamento. Até saber sair com segurança, lembre-se de que `Esc` retorna ao modo Normal e `:q!`, seguido de Enter, abandona as alterações não salvas da janela atual. Use esse comando apenas quando realmente quiser descartá-las.

Para praticar a abertura, a edição e o salvamento com Vim, experimente este laboratório:

1. **[Edição de Arquivos de Texto no Linux com Vim e Nano](https://labex.io/labs/comptia-edit-text-files-in-linux-with-vim-and-nano-591076)** — Pratique a criação, edição, gravação e navegação de arquivos com Vim e Nano em um ambiente Linux real.