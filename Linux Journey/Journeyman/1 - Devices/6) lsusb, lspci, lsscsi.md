# lsusb, lspci, lsscsi

Aprenda a inspecionar a topologia USB, funções PCI, dispositivos da camada SCSI e seus drivers ativos.

O Linux oferece ferramentas de inventário específicas para barramentos e subsistemas. Cada comando apresenta uma visão diferente, portanto combine seus identificadores, topologia, drivers, caminhos do sysfs e logs, em vez de esperar uma única lista completa do hardware.

## Inspeção de Dispositivos USB

`lsusb` lista os dispositivos USB visíveis pelo subsistema USB:

```bash
$ lsusb
```

A saída normalmente inclui números de barramento e dispositivo, um par de IDs de fornecedor e produto e uma descrição do banco de dados USB local. O endereço numérico de barramento/dispositivo pode mudar após uma reconexão ou reinicialização e não deve ser tratado como uma identidade persistente.

Exiba as relações entre controladores, hubs, portas, interfaces, drivers e velocidades com:

```bash
$ lsusb -t
```

Também há uma saída detalhada dos descritores, mas alguns dados exigem acesso elevado de leitura. Não conceda permissões amplas aos dispositivos USB apenas para evitar mensagens de uma ferramenta de inspeção.

## Inspeção de Funções PCI

`lspci` lista as funções encontradas nos barramentos PCI e PCI Express:

```bash
$ lspci
```

Dispositivos PCIe internos e conectados externamente podem incluir controladores gráficos, de rede, armazenamento, USB, áudio e pontes. Mostre o driver do kernel em uso e os módulos candidatos com:

```bash
$ lspci -k
```

O aparecimento de um controlador PCI nessa lista não comprova que todos os dispositivos atrás dele estejam inicializados ou funcionando. Verifique a associação do driver e os logs do kernel durante a solução de problemas.

## Inspeção de Dispositivos da Camada SCSI

`lsscsi` lista os dispositivos representados pela camada intermediária SCSI do Linux:

```bash
$ lsscsi
```

Isso pode incluir dispositivos SCSI nativos e discos SATA, de armazenamento USB ou virtuais apresentados por camadas compatíveis com SCSI. Os namespaces NVMe normalmente pertencem a outro subsistema e não são inventariados de forma abrangente por `lsscsi`.

Para uma hierarquia orientada ao armazenamento que inclua muitos tipos de dispositivos de bloco, use também `lsblk`:

```bash
$ lsblk -o NAME,TYPE,SIZE,MODEL,SERIAL,TRAN,FSTYPE,MOUNTPOINTS
```

## Interpretação dos Resultados do Inventário

As descrições muitas vezes vêm de bancos de dados locais de IDs e podem ser genéricas ou desatualizadas. Um dispositivo listado pode não ter um driver funcional, e um ambiente virtualizado pode apresentar hardware emulado ou paravirtual. Relacione os resultados a `udevadm info`, sysfs, `lsblk`, ferramentas de rede e `journalctl -k` ou `dmesg`, de acordo com as permissões e com o problema investigado.

Os utilitários podem ser distribuídos separadamente, normalmente em pacotes como `usbutils`, `pciutils` e `lsscsi`. Quando um comando estiver ausente, use o gerenciador de pacotes da distribuição em vez de baixar substitutos desconhecidos.

Use o laboratório [Exploração de Dispositivos de Hardware no Linux](https://labex.io/labs/comptia-explore-hardware-devices-in-linux-590861) para comparar essas visualizações de subsistemas em um único host controlado.

