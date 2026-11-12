# sysfs

Aprenda como o sysfs expõe o modelo ativo de dispositivos, drivers, barramentos e classes do kernel Linux em `/sys`.

`sysfs` é um sistema de arquivos virtual normalmente montado em `/sys`. Ele representa objetos do kernel e suas relações por meio de diretórios, links simbólicos e pequenos arquivos de atributos. Ferramentas de descoberta e gerenciadores de dispositivos o utilizam para compreender o modelo de dispositivos atual do kernel.

## Navegação pelo Modelo de Dispositivos

Algumas visualizações importantes no nível superior são:

- `/sys/devices/`: a hierarquia física e lógica dos dispositivos
- `/sys/class/`: dispositivos agrupados por classe funcional, como bloco ou rede
- `/sys/bus/`: barramentos, seus dispositivos e drivers
- `/sys/block/`: uma visualização conveniente dos dispositivos de bloco
- `/sys/dev/`: links indexados pelos números maiores e menores de dispositivos de caractere ou bloco

Muitas entradas fora de `/sys/devices` são links simbólicos para a hierarquia canônica. Resolva um link com `readlink -f` quando precisar do caminho pai real:

```bash
$ readlink -f /sys/class/block/sda
```

O nome do exemplo pode não existir em sistemas que usam outras interfaces de armazenamento.

## Leitura de Atributos

Os arquivos de atributos expõem valores ou controles individuais. Para um dispositivo de bloco, alguns exemplos podem ser:

```bash
$ cat /sys/class/block/sda/dev
8:0
$ cat /sys/class/block/sda/ro
0
$ cat /sys/class/block/sda/size
1953525168
```

`dev` informa os números maior e menor do dispositivo. `ro` informa o indicador de somente leitura do dispositivo de bloco. Para dispositivos de bloco do Linux, `size` é convencionalmente expresso em setores de 512 bytes, independentemente do tamanho de setor físico do dispositivo. Consulte sempre a documentação da ABI do kernel para conhecer as unidades e o significado de um atributo específico.

## Relação entre `/sys` e `/dev`

`/dev` contém os nós que as aplicações abrem para a E/S dos dispositivos. `/sys` expõe relações entre objetos, propriedades, estados e controles selecionados. Um nó de bloco como `/dev/sda` pode ser associado a `/sys/dev/block/8:0`, que resolve para o objeto relevante do sysfs.

As duas interfaces se complementam. Nenhuma contém, sozinha, um inventário completo de todos os dados do hardware, e um dispositivo pode desaparecer enquanto é inspecionado.

## Gravação Segura em Atributos

Alguns atributos do sysfs permitem escrita e podem alterar o estado de energia, a associação de drivers, o comportamento das filas, a autorização de dispositivos, LEDs ou outros controles ativos. Uma gravação de texto bem-sucedida pode causar efeitos imediatos no hardware ou nos serviços; ela não equivale a editar um arquivo de configuração persistente.

Leia a ABI documentada e o valor atual, identifique como tornar a configuração persistente e teste somente em um sistema autorizado. Nunca edite permissões recursivamente nem grave valores deduzidos em toda a árvore `/sys`.

Use o laboratório [Exploração de Dispositivos de Hardware no Linux](https://labex.io/labs/comptia-explore-hardware-devices-in-linux-590861) para navegar pelo sysfs somente para leitura e relacioná-lo aos nós de dispositivos.