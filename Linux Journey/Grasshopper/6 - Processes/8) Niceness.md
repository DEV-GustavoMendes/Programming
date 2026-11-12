# Niceness

Aprenda como os valores nice influenciam o peso do escalonamento da CPU para processos Linux comuns.

O Linux pode executar threads simultaneamente em diferentes núcleos da CPU e dividir o tempo de um núcleo entre mais threads executáveis do que consegue executar de uma vez. O escalonador faz essas escolhas de acordo com a política de escalonamento, a prioridade, a afinidade e a carga de trabalho. Um valor nice é uma das entradas usadas nas políticas comuns de compartilhamento de tempo.

## Interpretação dos Valores Nice

O intervalo convencional de nice vai de `-20` a `19`:

- Um valor menor atribui a uma tarefa um peso de escalonamento maior em relação a tarefas comparáveis.
- Um valor maior a torna mais “gentil” ao conceder-lhe um peso relativo menor.
- O padrão normalmente é `0`.

Niceness não reserva uma porcentagem da CPU nem garante a execução imediata. Seu efeito é mais visível quando tarefas executáveis comparáveis disputam tempo de CPU. Políticas de tempo real, cgroups, afinidade de CPU, esperas por E/S e outros controles podem predominar no comportamento observado.

## Visualização do Niceness

Em `top`, a coluna `NI` exibe o valor nice. Você também pode solicitá-lo a `ps`:

```bash
$ ps -o pid,ni,pri,stat,cmd -p 3245
```

`NI` é o valor nice visível ao usuário. Uma coluna `PRI` ou semelhante pode representar uma prioridade derivada do escalonador, e sua escala varia conforme a ferramenta e a classe de escalonamento; portanto, não presuma que as duas colunas sejam intercambiáveis.

## Início de um Comando com `nice`

Use `nice` para iniciar um novo comando com um valor ajustado:

```bash
$ nice -n 5 long-computation
```

O ajuste solicitado e a sintaxe aceita podem ser verificados no manual local. Um usuário sem privilégios normalmente pode tornar um comando mais gentil aumentando seu valor. Atribuir um valor nice menor e, portanto, um peso de escalonamento mais favorável exige privilégios adequados ou limites de recursos configurados.

## Alteração de um Processo Existente com `renice`

Use `renice` para um processo que já está em execução:

```bash
$ renice -n 10 -p 3245
```

Esse comando solicita o valor nice `10` para o PID `3245`. Verifique primeiro o destino, pois os PIDs podem ser reutilizados, e então confirme o valor resultante. As permissões dependem da propriedade, dos privilégios, dos limites de recursos e da política do sistema. Aumentar o valor nice normalmente é permitido para um processo que você possui; desfazer essa alteração pode não ser permitido sem privilégios.

O laboratório [Gerenciamento e Monitoramento de Processos Linux](https://labex.io/labs/comptia-manage-and-monitor-linux-processes-590864) oferece um ambiente controlado para visualizar e alterar valores nice. Compare tarefas limitadas pela CPU em disputa, em vez de esperar uma diferença visível em um sistema ocioso.

