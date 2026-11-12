# Hierarquia do Sistema de Arquivos

Aprenda as funções pretendidas dos principais diretórios Linux e como os layouts modernos unificados podem ser diferentes.

O Linux apresenta os sistemas de arquivos montados como uma única árvore de diretórios enraizada em `/`. O Filesystem Hierarchy Standard, ou FHS, atribui funções convencionais a muitos diretórios, mas distribuições, contêineres, sistemas imutáveis e políticas locais podem ser diferentes. Inspecione o host real antes de depender de um caminho.

```bash
$ ls -ld /*
```

## Raiz e Caminhos Essenciais do Sistema

- `/` é a raiz da árvore visível do sistema de arquivos.
- `/etc` contém a configuração do sistema específica do host. Ele pode conter scripts auxiliares ou de inicialização executáveis, portanto é incorreto afirmar que nunca possui conteúdo executável.
- `/boot` contém arquivos relacionados à inicialização, como dados do carregador de boot e, em muitos sistemas, kernels e imagens iniciais do sistema de arquivos em RAM.
- `/bin` e `/sbin` tradicionalmente contêm comandos essenciais para usuários e para a administração do sistema.
- `/lib` e suas variantes específicas de arquitetura tradicionalmente contêm bibliotecas compartilhadas essenciais e componentes do carregador.

Muitas distribuições atuais usam um layout `/usr` unificado, no qual `/bin`, `/sbin` e `/lib` são links simbólicos para os diretórios correspondentes em `/usr`. Use a descoberta de comandos e os registros de pacotes em vez de presumir se um caminho é um diretório físico ou um link.

## Software da Distribuição e Software Local

- `/usr` contém a principal hierarquia compartilhável e predominantemente somente para leitura do sistema operacional e das aplicações, incluindo comandos, bibliotecas e dados independentes da arquitetura.
- `/usr/local` é reservado para software e dados instalados pelo administrador local fora do gerenciamento normal de `/usr` pela distribuição.
- `/opt` pode conter pacotes de aplicações adicionais em subárvores autocontidas.

Apesar do nome, `/usr` não é o local onde normalmente ficam os arquivos pessoais de cada usuário. Os gerenciadores de pacotes da distribuição geralmente controlam grande parte desse diretório, portanto copiar arquivos compilados localmente para `/usr/bin` pode entrar em conflito com os pacotes gerenciados.

## Dados de Usuários e Serviços

- `/home` contém convencionalmente os diretórios pessoais dos usuários que não são root, embora serviços de diretório e políticas locais possam colocá-los em outros locais.
- `/root` é o diretório pessoal convencional da conta root.
- `/srv` é destinado aos dados específicos do local servidos por este sistema.

O caminho do diretório pessoal vem das informações da conta, não apenas da combinação de `/home` com um nome de usuário. Use `getent passwd USER` ou o diretório pessoal resolvido pelo shell em vez de codificar suposições.

## Dados Variáveis, de Execução e Temporários

- `/var` contém dados variáveis, como logs, caches, filas e estados de aplicações. Os logs do sistema normalmente aparecem em `/var/log`, embora alguns sistemas dependam principalmente de uma interface de journal.
- `/run` contém o estado volátil em tempo de execução da inicialização atual, como sockets, estados de serviços e arquivos PID. Ele normalmente é recriado durante o boot.
- `/tmp` é destinado a arquivos temporários e geralmente permite escrita por todos com a proteção do sticky bit.
- `/var/tmp` é destinado a arquivos temporários que devem sobreviver por mais tempo que os arquivos de `/tmp`.

A política de limpeza de `/tmp` varia; não presuma que os arquivos persistam até a reinicialização nem que sempre sejam excluídos durante ela. As aplicações devem criar arquivos temporários com segurança, em vez de usar nomes previsíveis.

## Dispositivos, Interfaces do Kernel e Pontos de Montagem

- `/dev` contém nós de dispositivos e links relacionados em tempo de execução.
- `/proc` expõe interfaces de processos e do kernel por meio do procfs.
- `/sys` expõe objetos, dispositivos, drivers e atributos do kernel por meio do sysfs.
- `/media` costuma ser usado para mídias removíveis montadas automaticamente.
- `/mnt` é um local convencional para montagens temporárias realizadas pelo administrador.

Essas são convenções, não concessões de permissões. Montar outro sistema de arquivos sobre um diretório não vazio oculta temporariamente o conteúdo anterior do diretório até que ele seja desmontado.

Use o laboratório [Navegação pelo Sistema de Arquivos no Linux](https://labex.io/labs/comptia-navigate-the-filesystem-in-linux-590971) para inspecionar esses caminhos e [Localização de Arquivos e Comandos no Linux](https://labex.io/labs/comptia-find-files-and-commands-in-linux-590834) para evitar depender de locais presumidos.

