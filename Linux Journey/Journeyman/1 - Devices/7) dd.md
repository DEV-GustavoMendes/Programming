# dd

Aprenda como `dd` copia fluxos de blocos e como evitar erros destrutivos de entrada, saída e tamanho.

`dd` copia dados de um fluxo de entrada para um fluxo de saída enquanto aplica os tamanhos de blocos e as conversões solicitados. Ele não compreende sistemas de arquivos, limites de partições nem se um destino de saída contém dados valiosos. Isso o torna útil para imagens e dispositivos brutos — e imediatamente destrutivo quando o destino está errado.

## Entrada, Saída e Tamanho de Bloco

Um comando possui este formato geral:

```bash
$ dd if=input.img of=output.img bs=4M status=progress
```

- `if=` seleciona a entrada; sem ele, `dd` lê da entrada padrão.
- `of=` seleciona a saída; sem ele, `dd` grava na saída padrão.
- `bs=` define o tamanho dos blocos de entrada e saída para uma cópia comum.
- `status=progress` solicita que o `dd` do GNU informe periodicamente o progresso da transferência.

`dd` copia blocos, não necessariamente um byte por vez. Um `bs` maior pode reduzir a sobrecarga das chamadas de sistema, mas o valor ideal depende dos dispositivos, do alinhamento, do cache e da carga de trabalho. Ele não altera os dados lógicos copiados.

## Limitação da Cópia

`count=` limita o número de blocos de entrada processados. Para um arquivo de entrada comum:

```bash
$ dd if=source.img of=prefix.img bs=1M count=2 status=progress
```

Isso solicita dois blocos de entrada de até 1 MiB cada, portanto copia no máximo 2 MiB. Leituras curtas podem complicar a multiplicação simples em fluxos como pipes; o `dd` do GNU oferece `iflag=fullblock` quando são necessários blocos de entrada completos. Diferencie unidades binárias e a sintaxe dos sufixos conforme a implementação local.

## Gravação de uma Imagem em um Dispositivo de Bloco

Uma restauração bruta pode se parecer com esta:

```bash
$ sudo dd if=backup.img of=/dev/sdX bs=4M status=progress conv=fsync
```

`/dev/sdX` é deliberadamente um marcador, não um comando para copiar. Antes de substituí-lo:

1. Mantenha um backup testado de todos os dados valiosos.
2. Identifique o destino pelo modelo, número de série, tamanho, transporte e link persistente usando `lsblk`, `udevadm` ou ferramentas equivalentes.
3. Confirme que nenhuma partição do destino esteja montada, usada como swap, integrada a RAID ou LVM ou aberta por outro serviço.
4. Verifique novamente o dispositivo após qualquer desconexão, reinicialização ou alteração da topologia.
5. Garanta que a imagem caiba e que a gravação de todo o dispositivo seja realmente pretendida.

O dispositivo de saída é sobrescrito desde o início. Inverter `if` e `of`, selecionar o disco do sistema ou usar um disco inteiro quando a intenção era uma partição pode destruir dados sem uma solicitação de confirmação.

## Criação de uma Imagem Consistente

Ler um dispositivo de bloco ativo enquanto seu sistema de arquivos está sendo alterado pode produzir uma imagem internamente inconsistente. Prefira um sistema de arquivos desmontado, um snapshot consistente com a aplicação ou um fluxo documentado de congelamento e snapshot. Bancos de dados e máquinas virtuais podem exigir seus próprios procedimentos de suspensão das alterações.

Uma imagem bruta do dispositivo copia blocos, incluindo os metadados do sistema de arquivos e regiões não utilizadas. Por isso, ela pode ser muito maior que um backup no nível dos arquivos e pode reproduzir identificadores que precisam ser alterados antes de montar um clone junto do original.

## Conclusão e Verificação

O comando terminar sem um erro de E/S não comprova que a origem e o destino pretendidos foram selecionados nem que a imagem possa ser usada. Registre as identidades e os tamanhos exatos, garanta que a saída em buffer tenha chegado ao armazenamento, compare uma leitura posterior com limites apropriados ou hashes criptográficos e teste a recuperação conforme o plano de backup.

Não divulgue passagens de sobrescrita com `dd` como apagamento seguro garantido para SSDs, camadas de tradução de flash, armazenamento com provisionamento dinâmico, snapshots ou setores remapeados. Use a sanitização compatível com o dispositivo e a plataforma junto com uma política explícita de destruição de dados.

Pratique somente com arquivos comuns ou discos virtuais descartáveis antes de trabalhar com hardware bruto. Os conceitos de partições e sistemas de arquivos em [Gerenciamento de Partições e Sistemas de Arquivos Linux](https://labex.io/labs/comptia-manage-linux-partitions-and-filesystems-590845) fornecem um contexto essencial.

