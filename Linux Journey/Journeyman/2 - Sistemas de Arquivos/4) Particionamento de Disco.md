# Particionamento de Disco

Aprenda um fluxo baseado em verificação para inspecionar, criar e redimensionar limites de partições com `parted`.

A edição de partições altera o mapa que define os limites do armazenamento. Um dispositivo, início ou fim incorreto pode tornar dados existentes inacessíveis ou sobrescrever metadados essenciais. Pratique somente em um disco virtual descartável e mantenha um backup separado e testado antes de modificar um armazenamento valioso.

## Escolha de uma Ferramenta

Algumas ferramentas comuns são:

- `fdisk`, um editor de partições para terminal do util-linux compatível com MBR e GPT
- `parted`, um editor para terminal e scripts compatível com GPT, MBR e outros formatos de tabela
- `gdisk`, um editor interativo voltado ao GPT
- GParted, uma interface gráfica para partições e sistemas de arquivos

O suporte das ferramentas evolui, portanto use o manual local e a documentação da distribuição. Uma interface gráfica não torna seguras as operações destrutivas; ela ainda altera os mesmos metadados do disco.

## Identificação e Inativação do Destino

Comece com um inventário somente para leitura:

```bash
$ lsblk -o NAME,PATH,TYPE,SIZE,MODEL,SERIAL,TRAN,PTTYPE,FSTYPE,MOUNTPOINTS
$ findmnt --real
$ sudo parted --list
```

Confirme o dispositivo inteiro por sua identidade persistente, modelo, número de série, tamanho, transporte e topologia — não apenas por `/dev/sdX`. Em seguida, identifique todos os consumidores: sistemas de arquivos montados, swap, LVM, RAID, criptografia, contêineres, máquinas virtuais, bancos de dados e descritores de arquivos abertos.

Desmonte ou desative todas as camadas relevantes de acordo com seus procedimentos documentados. Não edite a tabela de partições do disco do sistema em execução apenas porque a ferramenta abre com sucesso. Registre a tabela existente em um formato restaurável e confirme que seu backup esteja em outro domínio de falha.

## Inspeção de um Dispositivo no `parted`

Abra o dispositivo inteiro verificado explicitamente:

```bash
$ sudo parted /dev/VERIFIED-DISK
```

Depois, selecione unidades de exibição consistentes e mostre a tabela:

```text
(parted) unit MiB
(parted) print free
```

`print free` mostra as entradas atuais e as regiões não alocadas. Os comandos do Parted podem atualizar imediatamente os metadados do disco, sem aguardar uma operação final de “salvar”, portanto trate o prompt interativo como um acesso de escrita ativo.

## Criação de uma Entrada de Partição

A sintaxe exata de `mkpart` depende do tipo da tabela. Um exemplo de GPT em unidades MiB se parece com este:

```text
(parted) mkpart data ext4 1MiB 5000MiB
```

Esse comando cria uma entrada de partição com um nome, um tipo de conteúdo sugerido, início e fim. Ele **não** cria um sistema de arquivos ext4. A formatação é uma etapa destrutiva separada, realizada somente depois que o kernel reconhece a nova partição pretendida e sua identidade é verificada.

Use o alinhamento recomendado pela ferramenta e entenda se os pontos finais são inclusivos e como são arredondados. Inspecione o resultado com `print` e `lsblk`; não presuma que um limite decimal solicitado foi registrado exatamente.

## Redimensionamento de Limites e Conteúdo

`resizepart NUMBER END` move somente o limite final de uma partição. Ele não redimensiona o sistema de arquivos nem outra estrutura armazenada nela.

A ordem é essencial:

- Para expandir, aumente primeiro a partição ou o dispositivo lógico que funciona como contêiner e depois expanda o sistema de arquivos com sua ferramenta compatível.
- Para reduzir, confirme que o sistema de arquivos permite redução, diminua-o primeiro respeitando seus requisitos de funcionamento online ou offline e só então reduza o limite do contêiner sem cruzar o novo fim.

Alguns sistemas de arquivos não podem ser reduzidos. Criptografia, LVM, RAID e layouts aninhados acrescentam mais camadas ordenadas. O kernel também pode se recusar a reler uma tabela alterada enquanto os dispositivos estão ocupados, exigindo uma reinicialização controlada antes que o novo layout possa ser usado.

Use o laboratório [Gerenciamento de Partições e Sistemas de Arquivos Linux](https://labex.io/labs/comptia-manage-linux-partitions-and-filesystems-590845) em seu disco virtual secundário designado; não o substitua por um disco do host.

