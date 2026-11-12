# Detalhes dos Processos

Aprenda quais estados e recursos diferenciam um processo em execução de um programa armazenado no disco.

Um programa é formado por código executável e dados armazenados em um arquivo. Um processo é um contexto ativo de execução: ele inclui código mapeado, memória, credenciais, descritores de arquivos abertos, estado dos sinais, informações de escalonamento e uma ou mais threads. O mesmo programa pode ter várias instâncias de processos independentes.

## Instâncias de Programas e PIDs

Por exemplo, inicie `cat` sem operandos em dois terminais. Cada instância aguarda uma entrada e possui seu próprio ID de processo:

```bash
$ pgrep -a cat
18420 cat
18457 cat
```

Os dois processos executam o mesmo programa, mas podem ter fluxos de entrada, conteúdos de memória, credenciais, diretórios de trabalho e durações diferentes. Um PID identifica um processo ativo por vez e pode ser reutilizado mais tarde, depois que esse processo termina.

## Estado Acompanhado pelo Kernel

O kernel mantém as informações necessárias para escalonar e controlar cada processo, incluindo:

- identificadores do processo e de seu pai
- credenciais de usuário e grupo
- mapeamentos de memória virtual
- descritores de arquivos abertos e diretório atual
- disposições de sinais e sinais pendentes
- política de escalonamento, prioridade e estado de execução
- dados de contabilização, como tempo de CPU

Alguns recursos subjacentes podem ser compartilhados. Processos relacionados podem compartilhar memória mapeada, e as threads de um mesmo processo compartilham um espaço de endereços e muitos recursos do processo. Portanto, um processo oferece limites de isolamento sem implicar que cada byte ou objeto do kernel seja fisicamente privado.

## Escalonamento da CPU e Memória

As threads executáveis disputam tempo de CPU. O escalonador do kernel escolhe qual thread será executada em qual CPU de acordo com a classe de escalonamento, a prioridade, a afinidade de CPU, a carga e a política. Isso não é uma promessa de que todos os processos receberão uma parcela igual.

Cada processo normalmente enxerga um espaço de endereços virtual. O kernel e o hardware mapeiam endereços virtuais para a memória física ou outro armazenamento de apoio, aplicam proteções e podem compartilhar páginas quando apropriado. Portanto, um valor de memória em `ps` ou `top` não é automaticamente a quantidade de RAM física exclusiva atribuída àquele processo.

## Encerramento do Processo e Liberação de Recursos

Quando um processo termina, o kernel libera a maioria de seus recursos privados, fecha os descritores restantes e registra informações de encerramento para o processo pai. Um pequeno registro na tabela de processos pode permanecer como zumbi até que o pai recupere o status de saída. Isso significa que “o processo terminou a execução” e “todos os vestígios desapareceram da tabela de processos” nem sempre ocorrem simultaneamente.

Use o laboratório [Gerenciamento e Monitoramento de Processos Linux](https://labex.io/labs/comptia-manage-and-monitor-linux-processes-590864) para iniciar várias instâncias e comparar seus PIDs e estados. O laboratório [Comando `top` do Linux](https://labex.io/labs/linux-linux-top-command-real-time-system-monitoring-388500) oferece uma visualização dinâmica das métricas de escalonamento e recursos.