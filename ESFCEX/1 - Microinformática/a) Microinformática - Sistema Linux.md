### 1. Fundamentos: o que é o Linux

Linux, em sentido estrito, é apenas o **núcleo (kernel)**, criado por Linus Torvalds em 1991 e distribuído sob a licença GPL. O que as pessoas chamam de “sistema Linux” é o kernel somado às ferramentas do projeto GNU (compilador, shell, utilitários), a um gerenciador de pacotes e a um ambiente de usuário. Essa combinação empacotada é a **distribuição**. As famílias principais são a Debian (Ubuntu, Linux Mint), a Red Hat (Fedora, CentOS, Rocky, AlmaLinux), a SUSE, a Slackware e a Arch. A família importa porque define o gerenciador de pacotes (apt/dpkg ou dnf/rpm) e alguns arquivos de configuração, e as bancas gostam de explorar essas diferenças.

O Linux é multiusuário, multitarefa e multiplataforma. Ele segue a filosofia Unix de que programas pequenos fazem uma coisa bem e se combinam, e de que **“tudo é arquivo”**: discos, terminais, impressoras e até processos são representados como arquivos no sistema de arquivos. Esse princípio explica por que o diretório `/dev` existe e por que `/proc` mostra processos como diretórios.

### 2. Núcleo do sistema (kernel)

O kernel é o único software que roda em **modo privilegiado** (espaço de kernel), com acesso direto ao hardware. Todo o resto roda no **espaço de usuário**, com acesso restrito. Quando um programa precisa de um serviço do kernel, como abrir um arquivo, criar um processo ou enviar um pacote, ele faz uma **chamada de sistema (system call)**, que transfere o controle ao kernel de forma controlada. As funções clássicas do kernel são gerenciamento de processos, de memória (incluindo memória virtual e swap), do sistema de arquivos, de dispositivos (drivers) e de rede.

A arquitetura do Linux é **monolítica modular**. É monolítica porque o código principal roda inteiro em espaço de kernel, ao contrário dos microkernels. É modular porque drivers, sistemas de arquivos e protocolos podem ser carregados e descarregados em tempo de execução como **módulos** (arquivos `.ko`, de _kernel object_), sem recompilar nem reiniciar. Os comandos ligados aos módulos são cobrados em prova. O `lsmod` lista os módulos carregados e o `modinfo` mostra informações sobre um módulo. O `insmod` carrega um arquivo de módulo específico, mas **não resolve dependências**. O `rmmod` remove um módulo. O `modprobe` carrega um módulo **resolvendo e carregando suas dependências**, com base em um mapa gerado pelo `depmod`, e é o comando que a prova de 2022 cobrou. A configuração fica em `/etc/modprobe.d`, onde se pode, por exemplo, colocar um módulo em lista negra (_blacklist_).

O kernel expõe informações por **sistemas de arquivos virtuais**, que não ocupam disco. O `/proc` mostra processos (um diretório por PID) e dados do sistema, como `cpuinfo`, `meminfo`, `version`, `interrupts`, `dma`, `ioports`, `mounts` e `filesystems`. O `/sys` (sysfs) organiza dispositivos e drivers de forma hierárquica. O `uname -r` mostra a versão do kernel e o `dmesg` mostra as mensagens do buffer do kernel, úteis para diagnosticar hardware. O `sysctl` lê e ajusta parâmetros do kernel em execução, como `net.ipv4.ip_forward`.

O processo de **inicialização (boot)** segue uma sequência que cai em prova. O firmware (BIOS ou UEFI) executa o POST e localiza o dispositivo de boot. Em seguida o **gerenciador de boot** (GRUB2 nas distribuições modernas, LILO no passado) carrega o kernel e o **initramfs**, um sistema de arquivos temporário em memória com os drivers necessários para montar a partição raiz. O kernel monta a raiz e inicia o primeiro processo de espaço de usuário, o **PID 1**: o `init` clássico (SysV) ou o `systemd`, que hoje domina. Esse processo sobe os serviços conforme o nível de execução. No SysV existem os _runlevels_: 0 desliga, 1 é modo monousuário, 2 a 5 são multiusuário (o 3 é multiusuário em modo texto e o 5 é multiusuário gráfico, nas distribuições Red Hat), e 6 reinicia. No systemd, os equivalentes são os _targets_: `poweroff.target`, `rescue.target`, `multi-user.target`, `graphical.target` e `reboot.target`. Comandos de desligamento como `shutdown -h now`, `halt` e `poweroff` pedem ao sistema que pare de forma ordenada, e `shutdown -r` ou `reboot` reiniciam.

O kernel também tem **suporte nativo à virtualização**, tema que apareceu no bloco de sistemas operacionais da prova de 2026. O **KVM** é um módulo do kernel que transforma o Linux em um hipervisor, normalmente usado com o QEMU para emular o restante do hardware. Há ainda a virtualização leve por **contêineres**, baseada em _namespaces_ (isolamento de processos, rede e sistema de arquivos) e _cgroups_ (limite de CPU e memória), sobre a qual funcionam Docker e LXC. Lembre que contêineres compartilham o kernel do hospedeiro, enquanto máquinas virtuais têm kernel próprio.

### 3. Interface com o usuário

O Linux tem duas famílias de interface: a de linha de comando (CLI) e a gráfica (GUI).

A **linha de comando** é oferecida pelo **shell**, o interpretador que lê o que você digita e o executa. O principal é o **bash** (_Bourne Again Shell_), ao lado do `sh` (Bourne original, hoje frequentemente um link para o `dash` no Debian), `ksh`, `csh`/`tcsh` e `zsh`. O prompt termina em `$` para usuário comum e em `#` para o root. O shell é acessado por um **terminal**, que pode ser um console virtual (tty, acessado com Ctrl+Alt+F1 a F6), um emulador gráfico (pseudo-terminal, pts) ou uma sessão remota por SSH. Ao iniciar, o bash lê arquivos de configuração: `/etc/profile` e `~/.bash_profile` ou `~/.profile` em shells de login, e `~/.bashrc` em shells interativos.

Alguns mecanismos do shell são centrais, inclusive para o shell script. O primeiro é o **redirecionamento**. Todo processo nasce com três fluxos: entrada padrão (0), saída padrão (1) e saída de erro (2). O `>` redireciona a saída padrão sobrescrevendo o arquivo, o `>>` acrescenta ao final, o `<` redireciona a entrada, o `2>` redireciona só os erros e o `2>&1` junta erros e saída. Um detalhe que a prova adora: `echo "teste" 2> arquivo.txt` **ainda mostra “teste” na tela**, porque `echo` escreve na saída padrão e só o fluxo de erro foi redirecionado. O segundo mecanismo é o **pipe** (`|`), que liga a saída de um comando à entrada do seguinte, formando filtros em cadeia. O terceiro é a **expansão**: caracteres curinga (_globbing_) como `*`, `?` e `[abc]` são expandidos pelo shell antes de o comando rodar. Aspas simples preservam tudo literalmente, aspas duplas permitem expansão de variáveis e `$( )` (substituição de comando) executa um comando e insere sua saída. O quarto é o **controle de tarefas**: o `&` ao final executa o comando em segundo plano, Ctrl+C envia SIGINT, Ctrl+Z suspende, e `jobs`, `fg` e `bg` gerenciam as tarefas. O `nohup` mantém um processo vivo após o logout.

A **interface gráfica** não faz parte do kernel; é uma camada de aplicações. Ela se compõe de um **servidor gráfico** (o tradicional X11/X.Org ou o moderno Wayland), um **gerenciador de janelas** (que desenha bordas e controla posicionamento), um **ambiente de desktop** completo (GNOME, KDE Plasma, XFCE, Cinnamon, MATE) e um **gerenciador de login/display manager** (GDM, SDDM, LightDM) que apresenta a tela de entrada. Por ser separada do núcleo, o Linux funciona normalmente sem ambiente gráfico, que é o caso da maioria dos servidores.

### 4. Comandos básicos e avançados

Você disse que vai abordar comandos depois, então aqui eu dou o mapa conceitual e destaco o que a prova realmente cobrou.

**Ajuda e documentação.** O `man` abre o manual de um comando (organizado em seções numeradas), o `info` traz documentação em hipertexto, o `--help` mostra um resumo e o `whatis` e o `apropos` buscam por descrição.

**Navegação e arquivos.** O `pwd` mostra o diretório atual, o `cd` muda de diretório, o `ls` lista (com `-l` para detalhes, `-a` para ocultos, `-h` para tamanhos legíveis e `-R` para recursivo), e `mkdir`, `rmdir`, `cp`, `mv`, `rm` e `touch` criam, copiam, movem, apagam e atualizam datas. O `ln` cria links. O `tree` desenha a estrutura em árvore.

**Visualização.** O `cat` exibe o arquivo inteiro, `more` e `less` paginam, `head` e `tail` mostram o início e o fim (o `tail -f` acompanha um log em tempo real) e o `wc` conta linhas, palavras e bytes.

**Filtros e processamento de texto.** São a base dos shell scripts: `sort`, `uniq`, `cut`, `paste`, `tr`, `tee`, `diff`, `grep`, `sed` e `awk`. O `diff` compara arquivos e produz um arquivo de diferenças, e o **`patch` aplica esse arquivo de diferenças** a um arquivo original, transformando-o na versão nova. Foi a pegadinha de 2023, em que as alternativas erradas descreviam `PATH`, `tree`, `touch` e uma requisição HTTP.

**Busca.** O `find` percorre diretórios em tempo real com critérios como nome, tipo, data de modificação e permissões, e pode executar ações sobre os resultados. O `locate` consulta um banco de dados atualizado pelo `updatedb`, por isso é rápido, mas pode estar desatualizado. O `which` e o `type` indicam qual executável um comando invoca.

**Remoção e destruição segura.** O `rm` apenas remove a entrada de diretório (desfaz o vínculo com o inode); os dados continuam no disco até serem sobrescritos, e por isso podem ser recuperados. O **`wipe`** (e o parente `shred`) **sobrescreve o conteúdo do arquivo** antes de removê-lo, impedindo a recuperação. Há uma nuance de nível universitário: em SSDs, sistemas de arquivos com journaling e sistemas _copy-on-write_, a sobrescrita pode não atingir os blocos físicos originais, então a destruição segura de verdade exige apagar o dispositivo inteiro ou usar criptografia.

**Data, tempo e sistema.** O `date` mostra a data e hora, o `cal` mostra calendários (**`cal 4 2000` exibe abril de 2000**; só o ano gera o calendário anual), e `uptime`, `uname`, `free`, `df` e `du` mostram tempo ligado, informações do sistema, memória e uso de disco.

**Compactação e arquivamento.** O `tar` agrupa arquivos sem comprimir (`-c` cria, `-x` extrai, `-t` lista, `-f` indica o arquivo, `-z` usa gzip, `-j` usa bzip2, `-J` usa xz, `-v` mostra o progresso). Compressores são `gzip`, `bzip2`, `xz` e `zip`.

### 5. Shell script

Um shell script é um arquivo de texto com comandos que o shell executa em sequência, com variáveis e estruturas de controle. A primeira linha, o **shebang** (`#!/bin/bash`), diz qual interpretador deve executá-lo. Para executá-lo diretamente, o arquivo precisa de **permissão de execução** (`chmod +x`).

As **variáveis** são criadas com `nome=valor`, **sem espaços** ao redor do `=`, e lidas com `$nome`. Uma pegadinha clássica: `d=date` guarda o texto “date”, e para guardar a **saída** do comando usa-se a substituição de comando, `d=$(date)`, exatamente como a prova de 2021 cobrou. As variáveis especiais são `$?` (código de saída do último comando; 0 indica sucesso), `$0` (nome do script), `$1`, `$2`... (parâmetros), `$#` (quantidade de parâmetros), `$@` e `$*` (todos os parâmetros) e `$$` (PID do próprio shell).

As **estruturas de controle** são `if/then/elif/else/fi`, `case/esac`, `for/do/done`, `while/do/done` e `until`. Há também funções, o comando `read` para entrada e `exit` para encerrar com um código.

O ponto onde mais se erra é o **teste de condições**. O colchete `[ ]` é, na verdade, o comando `test`, e por isso exige espaços internos. Para **números** usam-se os operadores `-eq`, `-ne`, `-gt`, `-lt`, `-ge` e `-le`. Para **textos** usam-se `=` e `!=`. Para **arquivos** usam-se `-f` (arquivo regular), `-d` (diretório), `-e` (existe), `-r`, `-w`, `-x`. Dentro de `[ ]`, os símbolos `>` e `<` **não são comparadores numéricos**: o shell os interpreta como **redirecionamentos**. Esse foi o ponto central da prova de 2023. O comando `[ $a > $b ]` com `a=1` e `b=2` é lido pelo shell como `[ 1 ]` com a saída redirecionada para um arquivo chamado `2`. Como `[ 1 ]` testa se a string “1” não é vazia, o resultado é verdadeiro, e o script imprime “Sim”, sem erro de sintaxe. Para comparar números corretamente, o certo seria `[ $a -gt $b ]` ou `(( a > b ))`.

Outra questão (2022) mostra como a prova combina redirecionamento e filtro: dois `echo` gravando no mesmo arquivo com `>` fazem o segundo **sobrescrever** o primeiro, e `cat b | grep -i B` filtra ignorando maiúsculas e minúsculas. A resposta, nesse caso, é só a linha que sobrou no arquivo. Quando a questão pedir “a saída do script”, acompanhe o conteúdo do arquivo linha a linha, como se você fosse o interpretador.

### 6. Expressões regulares

Uma expressão regular é um padrão que descreve conjuntos de textos, usado por `grep`, `sed`, `awk` e muitas linguagens. Existem duas variantes POSIX. Na **básica (BRE)**, usada pelo `grep` puro, alguns metacaracteres precisam de barra invertida para ter efeito especial, como `\{n,m\}`, `\+`, `\?` e `\|`. Na **estendida (ERE)**, usada por `grep -E` e `egrep`, eles funcionam sem barra.

Os elementos essenciais são os seguintes. O `.` casa com qualquer caractere único. O `*` indica zero ou mais repetições do item anterior. O `+` indica uma ou mais e o `?` indica zero ou uma (em ERE). As chaves `{n}`, `{n,}` e `{n,m}` indicam quantidades exatas. O `^` ancora no **início** da linha e o `$` ancora no **fim**. Os colchetes `[abc]` definem uma classe de caracteres, `[^abc]` a negação e `[a-z]` uma faixa. Há ainda classes POSIX como `[[:digit:]]`, `[[:alpha:]]` e `[[:space:]]`. O `|` indica alternativa e os parênteses agrupam. A barra invertida “escapa” um metacaractere para tratá-lo como literal.

Para treinar a leitura, pegue o padrão que apareceu em 2024: `egrep '^.{0,9}$' /etc/passwd`. Decomponha: `^` marca o início da linha, `.{0,9}` aceita de zero a nove caracteres quaisquer e `$` marca o fim. Como as âncoras exigem que a linha inteira se encaixe, o comando mostra as linhas do `/etc/passwd` com **no máximo nove caracteres** (incluindo linhas vazias). A técnica é sempre decompor o padrão em peças e traduzir cada uma.

### 7. Arquivos e dispositivos de armazenamento

**A estrutura de diretórios.** O Linux segue o _Filesystem Hierarchy Standard_ (FHS): uma única árvore a partir da raiz `/`, sem letras de unidade. Os diretórios principais são o `/bin` e o `/sbin` (comandos essenciais, os de `/sbin` voltados à administração), `/boot` (kernel e gerenciador de boot), `/dev` (arquivos de dispositivos), `/etc` (configurações), `/home` (diretórios dos usuários), `/root` (home do root), `/lib` (bibliotecas), `/media` e `/mnt` (pontos de montagem), `/opt` (software opcional), `/proc` e `/sys` (virtuais), `/tmp` (temporários), `/usr` (programas e dados de usuário, com `/usr/bin`, `/usr/lib`, `/usr/share` e `/usr/local`) e `/var` (dados variáveis como logs em `/var/log`, filas e cache). Atenção: **`/users` não existe** no padrão e é usado como alternativa errada.

**Tipos de arquivo.** O primeiro caractere da listagem longa (`ls -l`) indica o tipo: `-` arquivo regular, `d` diretório, `l` link simbólico, `b` dispositivo de bloco (como discos), `c` dispositivo de caractere (como terminais), `p` pipe nomeado e `s` socket.

**Inodes e links.** Cada arquivo é descrito por um **inode**, estrutura que guarda metadados (dono, permissões, tamanhos, datas e ponteiros para os blocos de dados), mas **não o nome**, que fica na entrada de diretório. Um **hard link** é outro nome para o mesmo inode: compartilha os dados, não atravessa sistemas de arquivos e não vale para diretórios. Um **link simbólico** é um arquivo com inode próprio que guarda o caminho do alvo; pode cruzar sistemas de arquivos e fica “quebrado” se o alvo sumir.

**Permissões.** Cada arquivo tem dono, grupo e três conjuntos de permissões (dono, grupo, outros), cada um com leitura (r=4), escrita (w=2) e execução (x=1). A representação octal soma os valores. Assim, **`chmod -R 764 /teste`** aplica recursivamente 7 (rwx) ao dono, 6 (rw-) ao grupo e 4 (r--) aos outros, resultando em **`rwxrw-r--`**, como cobrou a prova de 2023. E `chmod 644` dá `rw-r--r--`. Em diretórios, `x` significa poder entrar e acessar o conteúdo, `r` significa listar e `w` significa criar ou apagar entradas. A **umask** define quais permissões são retiradas na criação: com umask 022, arquivos nascem com 644 e diretórios com 755.

As **permissões especiais** são três e a prova de 2025 cobrou uma. O **SUID** (4000) faz o programa executar com os privilégios do **dono do arquivo**, e aparece como `s` no lugar do `x` do dono. Foi o caso de `-rwsr-xr-x` em `/sbin/halt`: qualquer usuário que o executar roda com os poderes do root durante a execução, como o comando `passwd` faz para gravar em `/etc/shadow`. O **SGID** (2000) faz o programa herdar o grupo do arquivo (e, em diretórios, faz novos arquivos herdarem o grupo do diretório). O **sticky bit** (1000) aparece como `t` e, em um diretório como `/tmp`, impede que um usuário apague arquivos de outro. Um `S` ou `T` maiúsculo significa que o bit especial está ligado, mas o `x` correspondente não. O `chown` e o `chgrp` mudam dono e grupo.

**Sistemas de arquivos.** O **ext2** não tem journaling. O **ext3** acrescentou o journal, que registra operações antes de gravá-las para acelerar a recuperação após falhas. O **ext4** é o padrão de muitas distribuições, com extents, volumes e arquivos muito maiores. O **XFS** é o padrão da família Red Hat e é muito eficiente com arquivos grandes. O **Btrfs** é _copy-on-write_ e oferece snapshots e verificação de integridade. Há ainda ReiserFS, JFS, FAT/vfat e NTFS (via `ntfs-3g`) para interoperar com o Windows, além da **swap**, área de troca para memória virtual. O comando `mkfs` cria o sistema de arquivos, o `fsck` o verifica e repara (sempre com o sistema desmontado), `mount` e `umount` montam e desmontam, e o arquivo **`/etc/fstab`** lista as montagens automáticas na inicialização, com seis campos: dispositivo, ponto de montagem, tipo, opções, `dump` e ordem de verificação do `fsck`.

**Dispositivos e partições.** Discos SATA, SCSI e USB aparecem como `/dev/sda`, `/dev/sdb` etc., e IDE antigos como `/dev/hda`. Discos NVMe seguem o padrão `/dev/nvme0n1p1`, e cartões SD, `/dev/mmcblk0p1`. A tabela de partições **MBR** permite no máximo **quatro partições primárias**, ou três primárias mais uma **estendida**, que funciona como contêiner de **partições lógicas**, e limita discos a cerca de 2 TB. A tabela **GPT**, associada ao UEFI, suporta discos gigantes e, por padrão, até 128 partições. Aqui está o raciocínio que a prova cobrou duas vezes: **as partições primárias e a estendida ocupam os números de 1 a 4, e as lógicas sempre começam no 5**, não importando quantas primárias existam. Em 2021, duas primárias e uma lógica dentro de uma estendida resultam na lógica `/dev/sda5`. Em 2025, uma primária, uma estendida e duas lógicas fazem de `/dev/sda6` a **segunda partição lógica**.

**Ferramentas de disco.** O `fdisk` (e o `gdisk`/`parted` para GPT) particiona, o `lsblk` e o `blkid` identificam dispositivos, o `df` mostra espaço por sistema de arquivos e o `du` mostra o consumo por diretório. O **LVM** adiciona uma camada de abstração com volumes físicos (PV), grupos de volumes (VG) e volumes lógicos (LV), permitindo redimensionar volumes com o sistema em uso. O **RAID por software** é feito com o `mdadm` e aparece como `/dev/md0`.

### 8. Usuários e grupos

O Linux identifica usuários por **UID** e grupos por **GID**. O UID 0 é o root, os UIDs baixos (geralmente até 999) são de contas de sistema e os de usuários comuns costumam começar em 1000 (a faixa exata depende da distribuição). Todo usuário tem um grupo primário e pode pertencer a vários secundários.

Três arquivos formam o núcleo. O **`/etc/passwd`** é legível por todos e tem sete campos separados por dois-pontos: login, marcador de senha (`x`), UID, GID, comentário (GECOS), diretório home e shell. As senhas **não ficam mais nele**: ficam no **`/etc/shadow`**, legível só pelo root, com os hashes e a política de envelhecimento (data da última troca, mínimo, máximo, aviso, inatividade e expiração). Um `!` ou `*` no campo da senha indica conta bloqueada, e o prefixo do hash revela o algoritmo (`$6$` é SHA-512, por exemplo). O **`/etc/group`** lista os grupos, seus GIDs e membros, e o `/etc/gshadow` guarda senhas de grupo.

Os comandos de administração são `useradd` (com `-m` para criar o home, `-s` para o shell e `-g`/`-G` para grupos), o script `adduser` (mais amigável, do Debian), `usermod` (`-aG` **acrescenta** a grupos, e sem o `-a` os grupos antigos são substituídos), `userdel -r` (remove o usuário e seu home), `passwd` (altera senha, não confundir com o `pwd`), `chage` (política de validade), `groupadd`, `groupdel` e `gpasswd`. Para consulta existem `id`, `groups`, `who`, `w`, `last` e `lastlog`. O diretório **`/etc/skel`** contém o modelo copiado para o home de novos usuários, e o **`/etc/shells`** lista os shells válidos.

Para elevar privilégios, o `su` troca de usuário (o `su -` carrega também o ambiente do alvo) e o **`sudo`** executa um comando individual com os privilégios definidos no arquivo `/etc/sudoers`, que deve ser editado com o **`visudo`**, pois ele valida a sintaxe antes de salvar.

### 9. Segurança

A segurança do Linux começa pelo **controle de acesso discricionário (DAC)**, o modelo tradicional de dono, grupo e permissões. Sobre ele há o **controle de acesso obrigatório (MAC)**, em que políticas globais do sistema, e não o dono do arquivo, decidem o que cada processo pode fazer. O **SELinux** (desenvolvido pela NSA, padrão na família Red Hat) rotula arquivos e processos com contextos e opera nos modos _enforcing_ (aplica), _permissive_ (só registra) e _disabled_. O **AppArmor** (Ubuntu, SUSE) usa perfis baseados em caminhos de arquivo. Ambos são implementados **no kernel**, via o framework **LSM (Linux Security Modules)**. Isso derruba afirmações da banca de que o SELinux seria “um mecanismo em nível de usuário” ou de que o AppArmor não usaria o LSM.

O **firewall** do Linux é o **Netfilter**, parte do kernel, controlado por ferramentas em espaço de usuário. O **iptables** é o clássico e organiza regras em tabelas (`filter`, `nat`, `mangle`, `raw`) e cadeias (`INPUT`, `OUTPUT`, `FORWARD`, e `PREROUTING`/`POSTROUTING` na tabela `nat`), com uma política padrão (`ACCEPT` ou `DROP`) e regras avaliadas em ordem. O **nftables** é o sucessor, e `ufw` e `firewalld` são interfaces simplificadas.

As práticas de **hardening** incluem manter o sistema atualizado, instalar só o necessário e desativar serviços não usados, aplicar o princípio do menor privilégio, proibir login direto do root por SSH, preferir autenticação por **chave** a senha, usar o **PAM** (módulos de autenticação plugáveis) para políticas de senha e bloqueio por tentativas, e usar opções de montagem como `noexec` e `nosuid` em partições sensíveis. Também entram a criptografia de disco com **LUKS/dm-crypt**, o monitoramento de logs, a auditoria com o `auditd`, o `fail2ban` contra força bruta e o atributo imutável (`chattr +i`).

Os **logs** ficam em `/var/log`: `messages` ou `syslog` (geral), `auth.log` ou `secure` (autenticação), `kern.log`, `dmesg`, além de `wtmp` (consultado pelo `last`) e `btmp` (logins falhos, consultado pelo `lastb`). O `rsyslog` e o `journald` (systemd) coletam, e o `logrotate` faz a rotação.

### 10. Processos e serviços

Um **processo** é um programa em execução, com PID, PPID (pai), dono, estado e recursos próprios. Novos processos nascem por **`fork`** (clone do pai) seguido de **`exec`** (troca do programa em execução). O PID 1 é o ancestral de todos e adota processos órfãos. Um **zumbi** é um processo que já terminou mas continua na tabela de processos porque o pai ainda não recolheu seu código de saída. Os estados comuns no `ps` são R (executando), S (dormindo, interrompível), D (dormindo, em E/S não interrompível), T (parado) e Z (zumbi).

Para observar, usam-se o `ps` (`ps aux` e `ps -ef` listam todos), `top` e `htop` (visão dinâmica), `pstree` (hierarquia) e `pgrep`. A **prioridade** é ajustada pelo valor _nice_, de **-20 (mais prioritário) a 19 (menos prioritário)**, e só o root pode reduzir o valor abaixo do atual. Os comandos são `nice` (ao iniciar) e `renice` (em execução).

Os **sinais** são o mecanismo de comunicação com processos. Os principais são SIGHUP (1, tradicionalmente “recarregar configuração”), SIGINT (2, o Ctrl+C), SIGKILL (9, encerra de forma forçada e **não pode ser capturado nem ignorado**), SIGTERM (15, o **padrão** do `kill`, pedido de término educado), SIGSTOP (19, pausa e também não pode ser capturado), SIGTSTP (20, o Ctrl+Z) e SIGCONT (18, retoma). Os comandos `kill`, `killall` e `pkill` **enviam sinais**, não “excluem” nada. A diferença cobrada em 2021 é que o `kill` identifica o processo por **PID** e o `killall` por **nome**.

**Serviços (daemons)** são processos de segundo plano, normalmente iniciados no boot. No **systemd**, a unidade de trabalho é a _unit_ (de tipos `service`, `socket`, `timer`, `mount`, `target`), controlada com o `systemctl`: `start`, `stop`, `restart`, `reload`, `status`, `enable` (inicia no boot), `disable` e `is-active`. Os arquivos de unidade ficam em `/usr/lib/systemd/system` e `/etc/systemd/system`, e os logs são consultados com o `journalctl` (por exemplo, `-u nome`). No **SysV**, os scripts ficam em `/etc/init.d`, são controlados por `service nome start` e ligados aos runlevels por links em `/etc/rc?.d` com prefixo `S` (start) e `K` (kill); o `chkconfig` (Red Hat) e o `update-rc.d` (Debian) habilitam serviços.

Para **agendar tarefas** há o **cron** (tarefas recorrentes, definidas em `crontab` com cinco campos: minuto, hora, dia do mês, mês e dia da semana), o **at** (execução única no futuro) e o **anacron** (para máquinas que não ficam ligadas o tempo todo).

### 11. Comunicação de dados e redes

A configuração de rede passou das ferramentas antigas do pacote `net-tools` (`ifconfig`, `route`, `netstat`, hoje obsoletas) para o pacote **`iproute2`**: `ip addr` (endereços), `ip link` (interfaces), `ip route` (rotas) e `ss` (conexões e portas). As interfaces tinham nomes como `eth0` e hoje seguem nomes previsíveis (`enp0s3`, `wlp2s0`).

A configuração **persistente** varia por distribuição, e a banca explora isso. No **Debian** clássico, usa-se `/etc/network/interfaces` (endereço estático, máscara, gateway). No **Ubuntu** moderno, usa-se o **netplan** (`/etc/netplan/*.yaml`). Na família **Red Hat**, o **NetworkManager** (`nmcli`) é o padrão, com os antigos `ifcfg-*` em `/etc/sysconfig/network-scripts`. Alguns arquivos valem para todas: `/etc/hostname` (nome da máquina), `/etc/hosts` (resolução local de nomes), `/etc/resolv.conf` (servidores DNS), `/etc/nsswitch.conf` (ordem das fontes de resolução) e `/etc/services` (mapeamento nome-porta).

As ferramentas de diagnóstico seguem a lógica de camadas. O `ping` testa conectividade IP (ICMP), o `traceroute` e o `mtr` mostram a rota, o `dig` e o `nslookup` consultam DNS, o `ss` mostra portas em escuta, o `tcpdump` captura pacotes, o `nmap` varre portas e o `curl` e o `wget` testam serviços web. Para transferência segura, `scp`, `sftp` e `rsync` rodam sobre SSH.

Os principais **serviços de rede** no Linux, com suas portas típicas: SSH/OpenSSH (22), servidores web Apache e Nginx (80/443), DNS com BIND (53), DHCP (UDP 67/68), e-mail com Postfix ou Sendmail (SMTP 25) e Dovecot (POP3 110, IMAP 143), FTP (21), compartilhamento de arquivos com **Samba** (protocolo SMB, portas 139/445, interoperando com Windows) e **NFS** (2049, comum entre sistemas Unix), proxy com Squid (3128), impressão com CUPS (631) e diretório com OpenLDAP (389). Para encaminhar pacotes entre redes, o Linux precisa ter o _IP forwarding_ habilitado, e o NAT é feito pelo Netfilter. Em contêineres Docker, a rede padrão tem três redes: `bridge`, `host` e `none`.

### 12. Periféricos

O kernel reconhece hardware pelos **drivers**, normalmente módulos, e o firmware proprietário fica em `/lib/firmware`. O **udev** gerencia o diretório `/dev` **dinamicamente**: quando um dispositivo é conectado (_hotplug_), cria o arquivo correspondente e aplica as regras de `/etc/udev/rules.d`. Isso explica por que um pen drive que você pluga surge como `/dev/sdX` sem configuração manual, e as áreas de trabalho o montam automaticamente via serviços como o `udisks`.

Para inspecionar hardware, existem o `lspci` (barramento PCI), `lsusb` (USB), `lscpu`, `lshw`, `dmidecode` e `lsblk`. Na **impressão**, o padrão é o **CUPS** (_Common Unix Printing System_), que usa o protocolo IPP, tem interface web na porta 631 e arquivos em `/etc/cups`; os comandos de linha são `lpr`/`lp` (imprimir), `lpq`/`lpstat` (consultar fila) e `lprm`/`cancel` (cancelar). O áudio passou de ALSA para servidores como PulseAudio e PipeWire. O vídeo e os dispositivos de entrada dependem do servidor gráfico e do subsistema de entrada do kernel. Há também o `bluez` para Bluetooth e o SANE para scanners.

### 13. Instalação, configuração e administração

Uma **instalação** típica envolve escolher a mídia (ISO gravada em USB, ou uma versão _live_ que permite testar antes), definir o idioma, o teclado e o fuso horário, **particionar** o disco e configurar a rede, criar o usuário e instalar o gerenciador de boot. Um esquema comum de partições separa `/boot`, `/` (raiz), `/home` e a **swap**, o que facilita reinstalações sem perder dados do usuário. Em _dual boot_ com o Windows, instala-se o Windows primeiro e depois o Linux, para que o GRUB reconheça ambos.

O **gerenciamento de pacotes** muda por família, e os comandos aparecem com frequência. Na família **Debian**, o **`dpkg`** instala e consulta pacotes `.deb` locais, sem resolver dependências, e o **`apt`** resolve dependências a partir de repositórios (configurados em `/etc/apt/sources.list`): `apt update` **atualiza o índice** de pacotes, `apt upgrade` **atualiza os pacotes instalados**, `apt install`, `remove` (mantém a configuração), `purge` (remove com a configuração) e `autoremove` (remove dependências órfãs). Na família **Red Hat**, o **`rpm`** trata pacotes `.rpm` locais e o **`dnf`** (antigo `yum`) resolve dependências a partir de repositórios em `/etc/yum.repos.d`. Há ainda `pacman` (Arch), `zypper` (SUSE) e pacotes universais como Snap, Flatpak e AppImage. Também é possível **compilar do código-fonte**, no fluxo clássico `./configure`, `make`, `make install`.

A **administração** do dia a dia inclui criar usuários, monitorar processos e recursos, ler logs, agendar tarefas com o cron, manter **backups** (`tar`, `rsync`, `dd`), sincronizar o relógio (NTP/chrony) e atualizar o sistema e o kernel. Os arquivos de configuração do sistema ficam em **`/etc`**, em texto puro, o que facilita automação e versionamento.

### 14. Análise do que cai na prova (2020 a 2026)

A banca é a **VUNESP** e a prova de Informática tem 4 horas. Vale começar por uma limitação: não consegui abrir a prova de 2020, nem os cadernos completos de 2024 e 2026, e o que digo desses anos vem de listagens de questões. Para 2021, 2022, 2023 e 2025 li as provas inteiras.

Nos anos que li, **o padrão é muito estável**: as provas de 2021, 2022 e 2023 tinham 60 questões e a de 2025 tinha 50 questões, e em todas Linux ocupou as **questões 21 a 23**, abrindo os conhecimentos específicos. Isso equivale a algo entre 5% e 6% da prova, mas são pontos muito previsíveis. A prova de 2026 manteve o bloco, com três questões de sistemas operacionais cobrindo Linux, comandos e virtualização. [Info A +2](https://esfcex.eb.mil.br/images/concurso/2023/Info_A.pdf)

O histórico, ano a ano:

- **2021**: atribuição de data a uma variável de script (`d=$(date)`), diferença entre `kill` e `killall`, e identificação de uma partição lógica (`/dev/sda5`).
- **2022**: diferença entre `rm` e `wipe`, o comando que carrega um módulo resolvendo dependências (`modprobe`), e a saída de um script que combina redirecionamento e `grep -i`.
- **2023**: permissões com `chmod -R 764`, a pegadinha do `>` dentro de `[ ]` em um script, e a finalidade do `patch`.
- **2024**: pelo que as listagens mostram, houve uma questão de `egrep` com expressão regular sobre o `/etc/passwd`; não vi as outras duas. [qconcursos](https://www.qconcursos.com/questoes-militares/questoes?discipline_ids%5B%5D=94&institute_ids%5B%5D=6159)
- **2025**: o comando `cal 4 2000`, identificação da partição `/dev/sda6` e a leitura de permissão especial (SUID) em `/sbin/halt`.
- **2026**: houve uma questão sobre o comando `wipe`, ou seja, o mesmo tema de 2022 voltou. Há também uma questão de virtualização, e não consegui ver a terceira. [qconcursos](https://www.qconcursos.com/questoes-militares/disciplinas/tecnologia-da-informacao-sistemas-operacionais/linux/questoes)

Dá para tirar quatro conclusões. A primeira é que **a prova cobra prática, não teoria**: quase toda questão é “o que faz este comando”, “qual a saída deste script” ou “qual a permissão resultante”. A segunda é que **temas se repetem**: partições (2021 e 2025), shell script (2021, 2022 e 2023), permissões (2023 e 2025), `wipe` (2022 e 2026). Quem estudou as provas anteriores acertaria várias dessas questões. A terceira é que a banca **gosta de comandos pouco usados** (`patch`, `wipe`, `cal`, `modprobe`) cuja função dá para deduzir. A quarta é que o **conteúdo “grande” do edital** (segurança, redes, periféricos e serviços) quase não aparece como questão de Linux; esses temas costumam ser cobrados nos blocos de segurança e redes, em versão genérica.

Isso justifica que você deixe os comandos para depois, como planejou, mas com um aviso: eles são o grosso dos pontos de Linux. Quando for estudá-los, priorize permissões, partições e dispositivos, redirecionamento, `grep` e expressões regulares, o ciclo de vida de processos e sinais, e a diferença entre comandos parecidos.

### 15. Macetes e truques para a prova

**Partições.** Memorize “primárias e estendida de 1 a 4, lógicas a partir do 5”. Se a questão pergunta qual é a terceira lógica, some: 5, 6, 7.

**Permissões em octal.** Converta dígito por dígito (r=4, w=2, x=1) e monte a string em três blocos. Se a questão trouxer um `s` ou `t` na listagem, é permissão especial; `s` no dono é SUID.

**Shell script.** Em testes com `[ ]`, **desconfie de `>` e `<`**, que são redirecionamentos. Lembre que `[ texto ]` sem operador é verdadeiro se o texto não for vazio. Em questões de saída, simule linha a linha e lembre que `>` sobrescreve e `>>` acrescenta.

**Substituição de comando.** Atribuir a **saída** de um comando a uma variável exige `$( )`. Sem isso, a variável só guarda o texto do comando.

**Comando desconhecido.** Em questões sobre comandos pouco usados, procure a alternativa mais coerente com o nome (`wipe` apaga de forma segura, `cal` mostra calendário, `patch` aplica correções). As alternativas erradas costumam ser absurdas, como “chamar uma interrupção” ou “enviar uma requisição HTTP”.

**Regex.** Quebre em peças: âncoras (`^` e `$`), classe ou ponto, quantificador. Se há âncoras nas duas pontas, o padrão descreve a linha inteira.

**kill não “mata” sozinho.** Ele **envia sinais**; o padrão é o 15 (TERM), e o 9 (KILL) é o forçado. `kill` usa PID e `killall` usa nome.

**Armadilhas de generalização.** Desconfie de alternativas com “sempre”, “somente”, “nunca” ou “exclusivamente” sobre Linux. Quase tudo no sistema tem uma exceção ou uma segunda forma de fazer.

**Segurança.** O SELinux e o AppArmor são mecanismos **de kernel** (via LSM), e as senhas ficam no `/etc/shadow`, não no `/etc/passwd`.