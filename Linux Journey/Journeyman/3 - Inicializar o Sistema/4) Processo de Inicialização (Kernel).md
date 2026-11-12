# Processo de Inicialização: Kernel

Aprenda como o kernel Linux usa o espaço inicial do usuário para alcançar a raiz real e iniciar o PID 1.

Depois que o controle chega ao kernel Linux, ele inicializa gerenciamento de memória, escalonamento, interrupções, drivers incorporados, estruturas de segurança e outros subsistemas centrais. Em seguida, interpreta a linha de comando e se prepara para iniciar o primeiro processo do espaço do usuário.

## Por que existe o espaço inicial do usuário

Às vezes, um sistema de arquivos raiz simples pode ser montado com drivers incorporados ao kernel. Sistemas mais complexos precisam de módulos e ferramentas antes que a raiz real possa ser alcançada. Exemplos:

- módulos do controlador de armazenamento ou sistema de arquivos
- desbloqueio de uma raiz criptografada
- montagem de LVM ou RAID
- configuração de rede para uma raiz remota
- descoberta de dispositivos e resolução de identificadores persistentes

Um initramfs reúne esses componentes em um ambiente inicial do espaço do usuário fornecido junto com o kernel.

## Initramfs e initrd legado

Um initramfs moderno normalmente consiste em um ou mais arquivos cpio, muitas vezes comprimidos, que o kernel descompacta em seu sistema de arquivos raiz inicial. O kernel executa o programa `/init` desse ambiente.

Um initrd legado é, conceitualmente, uma imagem de sistema de arquivos carregada em um dispositivo de bloco baseado em RAM e montada. Os termos são usados de modo impreciso em nomes de arquivo e comandos; examine as ferramentas reais em vez de deduzir o formato apenas pelo nome.

O initramfs deve corresponder ao kernel e ao projeto de boot. Módulos ausentes, identificadores antigos ou ferramentas de criptografia e LVM omitidas podem tornar um kernel novo incapaz de iniciar, mesmo que sua imagem seja válida.

## Chegada à raiz real

O espaço inicial interpreta parâmetros como `root=`, espera pelos dispositivos, ativa camadas de armazenamento e monta a raiz pretendida. Depois, usa uma operação de troca de raiz para tornar esse sistema de arquivos o novo `/` e liberar o ambiente temporário quando possível.

A solicitação inicial `ro` pode permitir verificações de consistência e uma inicialização controlada, mas a sequência exata depende da distribuição. Verificações de sistema de arquivos são operações do espaço do usuário, e o initramfs ou o sistema init posterior pode remontar a raiz para leitura e escrita quando permitido.

## Inicialização do PID 1

O kernel executa o programa init configurado, normalmente acessado por um caminho como `/sbin/init` ou escolhido com `init=`. Esse processo recebe o PID 1 e assume a responsabilidade pelo principal ambiente de serviços do espaço do usuário.

Se nenhum programa init utilizável puder ser executado, o kernel não consegue chegar a um sistema normal e costuma relatar falha ou panic. Investigue a primeira camada que falhou: kernel e linha de comando, conteúdo do initramfs, descoberta e montagem da raiz ou execução do PID 1.

