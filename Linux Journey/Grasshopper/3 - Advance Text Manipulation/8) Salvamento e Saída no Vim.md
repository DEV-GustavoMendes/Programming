# Salvamento e Saída no Vim

Aprenda a gravar, sair, salvar com outro nome ou descartar conscientemente as alterações de um buffer do Vim.

Gravar e sair são operações separadas no Vim. Antes de inserir um comando Ex, pressione `Esc` para retornar ao modo Normal, digite `:`, insira o comando e pressione Enter. Leia a mensagem de status ou erro do Vim antes de presumir que uma gravação teve sucesso.

## Gravação do Buffer Atual

Use `:w` para gravar o buffer atual em seu arquivo associado sem fechar a janela:

```vim
:w
```

Uma gravação pode falhar porque o buffer não possui nome, o diretório não permite gravação, o sistema de arquivos está cheio ou outra condição impede a operação. Verifique a mensagem informada pelo Vim.

Use `:w copy.txt` para gravar o buffer atual em outro caminho, mantendo o nome atual do buffer. Use `:saveas copy.txt` quando o buffer deva adotar o novo caminho.

## Saída de um Buffer sem Alterações

Use `:q` para fechar a janela atual quando isso não abandonar alterações não salvas do buffer:

```vim
:q
```

Se o buffer atual estiver modificado e suas alterações forem perdidas, o Vim normalmente se recusará e mostrará um aviso. Essa salvaguarda oferece a oportunidade de gravar ou reconsiderar.

## Descarte de Alterações Não Salvas

Use `:q!` apenas quando quiser intencionalmente fechar a janela atual e abandonar as alterações que impediriam a saída:

```vim
:q!
```

O ponto de exclamação ignora o aviso de alterações não salvas. Essas mudanças do buffer não serão gravadas; portanto, confirme que são realmente descartáveis antes de pressionar Enter.

## Gravação e Saída em Conjunto

Use `:wq` quando o buffer deva ser gravado e a janela atual fechada depois de uma gravação bem-sucedida:

```vim
:wq
```

Se a gravação falhar, o Vim não concluirá a saída solicitada. Resolva o erro em vez de presumir que os dados chegaram ao disco.

## Uso de :x e ZZ

`:x` grava o buffer somente se ele estiver modificado e depois sai. No modo Normal, `ZZ` maiúsculo realiza o mesmo comportamento:

```vim
:x
```

```text
ZZ
```

Isso é ligeiramente diferente de `:wq`, que solicita uma gravação mesmo quando o buffer não foi alterado. `ZQ` maiúsculo é a contraparte do modo Normal para sair sem gravar, semelhante a `:q!`.

Quando várias janelas ou buffers estão envolvidos, um comando pode fechar apenas a janela atual. Comandos como `:qa`, `:wqa` e `:qa!` atuam em todas as janelas, mas examine todos os buffers modificados antes de usar um comando forçado global.

Para praticar a gravação e a saída em um arquivo descartável, experimente este laboratório:

1. **[Edição de Arquivos de Texto no Linux com Vim e Nano](https://labex.io/labs/comptia-edit-text-files-in-linux-with-vim-and-nano-591076)** — Pratique a criação de arquivos, a edição de texto, o salvamento de arquivos e a navegação tanto com Vim quanto com Nano. Este laboratório consolidará sua compreensão das operações básicas do Vim, inclusive como salvar e sair.