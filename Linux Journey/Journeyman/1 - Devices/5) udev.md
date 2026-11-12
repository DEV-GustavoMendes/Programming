# udev

Aprenda como o udev processa eventos de dispositivos do kernel para aplicar políticas, permissões e links persistentes.

O kernel Linux informa alterações de dispositivos ao espaço do usuário por meio de uevents. Em muitas distribuições atuais, `systemd-udevd` processa esses eventos usando regras udev e um banco de dados de dispositivos. Junto com o `devtmpfs` preenchido pelo kernel, isso produz as propriedades, permissões, atributos e links simbólicos que as aplicações observam em `/dev`.

## Do Evento do Kernel à Política do Dispositivo

Quando um dispositivo é adicionado, alterado, movido ou removido, o udev pode:

- ler atributos do sysfs e propriedades do evento
- aplicar políticas de proprietário, grupo e modo a um nó de dispositivo
- adicionar links simbólicos estáveis, como `/dev/disk/by-id/...`
- marcar dispositivos para outros serviços
- executar um processamento auxiliar de escopo restrito

O kernel continua responsável pelo dispositivo real e por seu driver. Excluir um nó de `/dev` não remove fisicamente o hardware, e criar manualmente um nó com `mknod` não faz um hardware sem suporte existir nem associa um driver a ele.

## Locais e Precedência das Regras

As regras normalmente ficam em:

- `/usr/lib/udev/rules.d/` para regras fornecidas por fornecedores ou pacotes
- `/run/udev/rules.d/` para regras voláteis em tempo de execução
- `/etc/udev/rules.d/` para a política local do administrador

Os arquivos são processados na ordem lexical dos nomes, e arquivos com o mesmo nome em diretórios de maior prioridade substituem versões de menor prioridade, conforme a implementação do udev instalada. As regras locais devem usar um nome de arquivo deliberado e corresponder a propriedades estáveis, não a nomes de enumeração.

Uma regra pode afetar todos os dispositivos correspondentes, portanto teste seu escopo cuidadosamente. Não edite diretamente regras de pacotes quando uma substituição local ou uma regra suplementar for apropriada.

## Inspeção de um Dispositivo com `udevadm`

Consulte as propriedades do udev para um nó existente:

```bash
$ udevadm info --query=all --name=/dev/sda
```

Use um nó que exista no sistema atual. `udevadm info --attribute-walk --name=...` pode exibir atributos ao longo da cadeia de pais no sysfs, o que ajuda a construir uma regra. `udevadm monitor --kernel --udev --property` observa eventos do kernel e eventos processados; ele pode expor identificadores de dispositivos, portanto trate adequadamente a saída capturada.

## Aplicação Cuidadosa das Alterações de Regras

Recarregar os arquivos de regras afeta o processamento dos eventos futuros; isso não reconstrói automaticamente o estado de todos os dispositivos existentes. Acionar eventos manualmente pode afetar muitos dispositivos e serviços, portanto restrinja o destino e consulte a documentação do `udevadm` instalado. Um comando de teste pode simular a avaliação de regras, mas talvez não reproduza todos os efeitos colaterais de um evento real.

Faça backup das regras locais, valide a sintaxe, observe um único dispositivo de teste conhecido e mantenha um caminho de recuperação antes de alterar permissões ou nomes. Evite trabalhos demorados diretamente no processamento de eventos do udev; delegue-os a um serviço apropriado.

Use o laboratório [Exploração de Dispositivos de Hardware no Linux](https://labex.io/labs/comptia-explore-hardware-devices-in-linux-590861) para relacionar propriedades de `udevadm`, caminhos do sysfs e links de `/dev` em um ambiente controlado.

