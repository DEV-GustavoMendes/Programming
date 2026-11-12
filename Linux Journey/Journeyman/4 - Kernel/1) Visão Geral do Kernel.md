# Visão Geral do Kernel

Aprenda como o kernel Linux intermedeia hardware, recursos, isolamento e solicitações do espaço do usuário.

Linux é o kernel do sistema operacional: o software privilegiado que gerencia processadores, memória, dispositivos, processos e abstrações comuns de recursos. Um sistema Linux completo também inclui bibliotecas, utilitários, serviços, shells, software gráfico e políticas da distribuição no espaço do usuário.

## Recursos de hardware

Processadores executam instruções, a memória armazena o estado ativo e controladores conectam armazenamento, redes, telas, dispositivos de entrada e outros periféricos. O hardware expõe mecanismos específicos da arquitetura e dos dispositivos, não uma interface única e segura para todo aplicativo.

O kernel inicializa e controla esses recursos por código de arquitetura e drivers. Ele trata interrupções, coordenação de DMA, temporizadores e eventos de energia, impondo limites de acesso entre cargas de trabalho.

## Responsabilidades do kernel

As principais incluem:

- escalonar threads executáveis nas CPUs
- criar e isolar espaços de endereços virtuais
- impor credenciais, permissões e políticas de segurança dos processos
- fornecer sistemas de arquivos, rede, IPC e interfaces de dispositivos
- tratar sinais, temporizadores e o ciclo de vida dos processos
- alocar, contabilizar e recuperar recursos

O Linux é descrito como kernel monolítico porque serviços centrais e muitos drivers são executados em um único espaço privilegiado de endereços. Também é modular: componentes compatíveis podem ser carregados e removidos como módulos. Uma falha no código privilegiado pode comprometer todo o sistema, tornando atualizações e procedência dos módulos críticas para a segurança.

## Espaço do usuário

O espaço do usuário contém processos comuns: init e serviços, ferramentas de linha de comando, runtimes, bancos de dados, shells e aplicativos gráficos. O privilégio do hardware impede que eles executem diretamente muitas instruções sensíveis ou acessem qualquer memória do kernel.

Processos solicitam trabalho ao kernel por chamadas de sistema e interfaces como descritores de arquivo, sockets, nós de dispositivo, procfs, sysfs, netlink e mapeamentos de memória. Bibliotecas costumam envolver essas interfaces em APIs de alto nível.

O root do espaço do usuário tem ampla autorização por política, mas normalmente continua em modo de usuário do processador. Identidade do usuário e modo de privilégio da CPU são conceitos diferentes.

## Limites e abstrações

O kernel apresenta processos virtuais, arquivos, sockets e espaços de endereços em vez de expor diretamente a maquinaria física. Essas abstrações favorecem isolamento e portabilidade, mas não são limites de segurança perfeitos. Namespaces, cgroups, capabilities, módulos de segurança, seccomp e virtualização acrescentam controles especializados.

Ao diagnosticar, pergunte qual camada é responsável: aplicativo, biblioteca, interface de chamada, sistema de arquivos, driver, subsistema do kernel, firmware ou hardware. Evidências da camada errada podem levar a correções incorretas.

Use [Gerenciar Módulos do Kernel no Linux](https://labex.io/labs/comptia-manage-kernel-modules-in-linux-590865) para observar uma parte modular do kernel em um ambiente controlado.

