# Encerramento de Processos

Aprenda como status de saída, espera, zumbis e reparentalização completam o ciclo de vida dos processos Linux.

Um processo pode terminar retornando de sua função principal, chamando uma interface de saída ou sendo encerrado por um sinal. O kernel libera a maioria de seus recursos, mas a contabilização entre pai e filho continua até que o pai colete as informações de encerramento.

## Status de Saída

Um programa que termina normalmente fornece um status inteiro. Por convenção, o status `0` significa sucesso, enquanto um valor diferente de zero informa algum tipo de falha ou resultado alternativo. Os significados exatos dos valores diferentes de zero pertencem à interface do programa.

Em um shell, inspecione o status do pipeline em primeiro plano mais recente com:

```bash
$ command
$ printf '%s\n' "$?"
```

Os shells expõem um intervalo limitado de status codificados e também representam o encerramento por sinais, portanto esse valor não é um registro de diagnóstico completo. Os programas devem documentar seus próprios códigos de saída.

## Espera e Coleta

O kernel registra como um filho terminou e notifica seu pai. O pai usa uma função da família de chamadas de sistema `wait()` para recuperar essas informações. A coleta do registro é chamada de reaping.

A espera também pode coordenar a execução: um shell aguarda um comando em primeiro plano antes de exibir outro prompt, mas pode adiar a espera por uma tarefa em segundo plano. Um pai de longa duração bem projetado deve se organizar para coletar seus filhos sem bloquear trabalhos não relacionados.

## Processos Zumbis

Depois que um filho termina, mas antes que seu registro de encerramento seja coletado, ele aparece como zumbi, normalmente com o estado `Z` em `ps`. Ele não executa mais nem mantém um espaço de endereços comum, mas uma entrada mínima na tabela de processos e informações de contabilização permanecem.

Enviar um sinal a um zumbi não pode fazê-lo terminar novamente. Corrija o acúmulo persistente de zumbis diagnosticando o pai que não está aguardando, reiniciando ou corrigindo esse pai por um procedimento operacional adequado ou permitindo a reparentalização para um processo que fará a coleta. Grandes quantidades podem esgotar a capacidade de PIDs ou da tabela de processos.

## Órfãos e Reparentalização

Se um pai termina enquanto seu filho continua, o kernel reparentaliza esse filho para um subreaper elegível ou para o processo init do namespace de PIDs relevante. O filho pode estar em execução, dormindo, interrompido ou tornar-se um zumbi mais tarde; “órfão” descreve a perda da relação com o pai original, não um estado de execução.

O processo adotante passa a ser responsável por coletar o status de encerramento. Gerenciadores de serviços modernos e ambientes de contêineres tornam importante não presumir que o novo pai sempre seja o PID 1 do host.

Use o laboratório [Gerenciamento e Monitoramento de Processos Linux](https://labex.io/labs/comptia-manage-and-monitor-linux-processes-590864) para observar códigos de saída e estados de processos sem afetar uma carga de trabalho de produção.