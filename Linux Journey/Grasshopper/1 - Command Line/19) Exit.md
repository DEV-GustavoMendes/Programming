# exit

Aprenda a sair do shell atual e escolher o status que ele devolve ao processo chamador.

Os shells podem ser aninhados: um terminal gráfico inicia um shell, uma conexão SSH inicia um shell remoto e um shell pode iniciar outro shell. Sair de um deles normalmente devolve o controle ao processo que iniciou o shell atual.

## Saída do Shell Atual

O comando `exit` solicita que o shell atual seja encerrado:

```bash
$ exit
```

Se esse shell for o processo principal de uma aba do terminal gráfico, a aba poderá ser fechada de acordo com as configurações do terminal. Em uma sessão SSH, sair do shell remoto normalmente leva você de volta ao shell local. Se você iniciou um shell aninhado, `exit` retorna ao shell pai.

## Retorno de um Status de Saída

Um argumento numérico opcional define o status devolvido ao processo chamador do shell:

```bash
$ exit 0
```

Por convenção, `0` significa sucesso, e um valor diferente de zero representa falha ou outra condição definida pelo programa. Se o Bash não receber um argumento numérico, ele sairá com o status do último comando executado antes de `exit`.

## Uso de logout em um Shell de Login

O comando interno `logout` do Bash encerra um shell de login:

```bash
$ logout
```

Em um shell do Bash que não seja de login, `logout` informa que ele não é um shell de login; nesse caso, use `exit`.

## Uso de Ctrl+D ou Fechamento de um Terminal

Em um prompt interativo vazio, pressionar `Ctrl+D` normalmente fornece o caractere de fim de arquivo do terminal. O Bash costuma interpretar essa condição como uma solicitação para sair. Isso não é um sinal, e configurações do shell como `ignoreeof` do Bash podem alterar o comportamento.

Fechar uma janela de terminal gráfico solicita que o aplicativo encerre seus processos e pode afetar tarefas em execução. Quando for possível, prefira uma saída organizada com `exit` e verifique se há trabalhos ativos antes de fechar a sessão.