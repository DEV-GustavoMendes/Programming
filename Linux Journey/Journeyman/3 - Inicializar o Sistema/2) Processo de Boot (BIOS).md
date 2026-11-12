# Processo de Boot: BIOS

Aprenda como o BIOS legado e o firmware UEFI moderno localizam e autorizam a próxima etapa do boot.

O firmware é executado antes do kernel Linux. Em hardware da classe PC, as duas principais interfaces são BIOS legado e UEFI. Elas usam modelos diferentes para descobrir o boot; portanto, “o BIOS lê o carregador” descreve apenas um dos caminhos.

## Boot com BIOS legado

Depois da inicialização inicial da plataforma e da escolha do dispositivo, o BIOS legado normalmente lê o primeiro setor de 512 bytes do disco escolhido e transfere o controle ao código ali presente se encontrar a assinatura esperada.

Em um layout MBR, esse setor contém uma pequena região de código, quatro entradas de partição e uma assinatura. O espaço é insuficiente para um carregador completo, por isso o código costuma localizar outra etapa no disco ou em um sistema de arquivos.

É possível iniciar por BIOS em um disco GPT, mas o MBR protetor sozinho não contém as etapas posteriores. O GRUB costuma usar uma pequena BIOS Boot Partition no GPT para seu código central incorporado. A organização exata depende do carregador instalado.

## Boot com UEFI

O firmware UEFI compreende um sistema de arquivos definido em uma Partição de Sistema EFI (ESP) e pode carregar executáveis EFI. Entradas de boot guardadas em variáveis não voláteis normalmente identificam disco, partição e caminho do executável. Um caminho alternativo padronizado pode ser usado em mídias removíveis ou recuperação.

A ESP contém aplicativos de boot e arquivos de apoio, não “todas as informações de inicialização”. Kernel, initramfs e configuração do carregador podem ficar nela ou em outro lugar. GPT é convencional em UEFI, mas a interface do firmware e a tabela de partições continuam sendo camadas distintas.

## Secure Boot e confiança

Com o Secure Boot ativado, o UEFI verifica as assinaturas da cadeia de boot conforme as chaves e a política cadastradas. Uma distribuição Linux pode usar shim, carregador e kernel assinados, além de uma política para módulos, para estender essa cadeia.

Secure Boot não criptografa o disco nem prova que todo programa é seguro. Ele ajuda a impedir que código de pré-boot não autorizado seja aceito pela política de confiança configurada.

## Entrada na configuração do firmware

As teclas variam por fabricante e modelo, incluindo Delete, Escape ou teclas de função durante o início. Consulte a documentação do dispositivo. Alguns sistemas UEFI também permitem que o sistema operacional solicite uma reinicialização para a configuração.

Registre os valores existentes e as chaves de recuperação antes de mudar Secure Boot, modo do controlador de armazenamento, TPM, virtualização ou ordem de boot. Uma alteração pode tornar volumes criptografados ou o sistema temporariamente inacessíveis.

