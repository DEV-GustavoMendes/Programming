
# PARTE D: SISTEMAS OPERACIONAIS LOCAIS E DISTRIBUÍDOS

### **D.1 Fundamentos**

Um sistema operacional (SO) é o software que gerencia o hardware e oferece serviços aos programas. Ele cumpre dois papéis. O primeiro é de **máquina estendida:** esconde a complexidade do hardware e entrega abstrações simples e uniformes (processo, arquivo, socket, memória virtual). O segundo é de **gerente de recursos:** arbitra o uso de CPU, memória, discos e dispositivos entre vários programas concorrentes, garantindo justiça, proteção e eficiência.

A proteção se apoia em dois **modos de execução da CPU.** No **modo usuário** o programa só executa instruções não privilegiadas. No **modo núcleo (kernel/supervisor)** o SO tem acesso a todas as instruções e ao hardware. A passagem de um modo ao outro ocorre por **chamadas de sistema** (**system calls,** a interface programática do kernel, como **read**, **fork** e **open**), por interrupções e por exceções. Instruções de E/S, manipulação de tabelas de página e desabilitação de interrupções são privilegiadas, e tentar executá-las em modo usuário gera uma exceção.

Quanto à estrutura, o **kernel monolítico** (Linux, Unix tradicional) reúne escalonador, memória, sistemas de arquivos e drivers num único espaço de endereçamento privilegiado, o que é rápido, mas um driver defeituoso pode derrubar tudo (o Linux mitiga isso com módulos carregáveis). O **microkernel** (Minix, QNX, Mach) mantém no núcleo só o essencial (escalonamento básico, memória básica, IPC) e leva o restante para servidores em modo usuário, o que aumenta a robustez e a modularidade ao custo de mais trocas de contexto. O **kernel híbrido** (Windows NT, macOS) tenta o meio-termo. Há ainda as estruturas em **camadas** e os **exokernels**.

#### Sistema Operacional Local, Sistema Operacional de Rede Sistema Operacional Distribuído

Um **Sistema Operacional Local** gerencia uma única máquina. Um **Sistema Operacional de Rede** roda em cada máquina, e o usuário sabe que há várias, acessando recursos remotos explicitamente (**ssh**, montagem de um compartilhamento). Um **Sistema Operacional Distribuído** apresenta um conjunto de computadores autônomos como um único sistema coerente. O objetivo central é a transparência, que se divide em: de acesso (esconde diferenças de representação e de acesso), de localização (o usuário não sabe onde está o recurso), de migração (o recurso pode se mover sem que o usuário perceba), de replicação, de concorrência e de falhas. Exemplos acadêmicos são Amoeba, Plan 9 e Sprite. Na prática atual, a “distribuição” é obtida com middleware e sistemas especializados (HDFS, Kubernetes, bancos distribuídos) sobre SOs locais. Os desafios típicos de um sistema distribuído são a ausência de memória e de relógio globais, as falhas parciais, a latência da rede e a consistência de dados replicados.

### D.2 Gerenciamento de processos

Um programa é uma entidade passiva (um arquivo executável), e um **processo** é um **programa em execução**, uma entidade ativa com seu próprio espaço de endereçamento. A imagem de um processo na memória tem o segmento de código (texto), o de dados (variáveis globais), o **heap (alocação dinâmica, que cresce para cima)** e a **pilha** (chamadas de função e variáveis locais, que cresce para baixo).

O SO mantém para cada processo um **PCB (Process Control Block, ou bloco descritor),** que contém o PID, o estado, o contador de programa, o conteúdo dos registradores, informações de gerência de memória (tabelas de página), arquivos abertos, prioridade e dados de contabilização. É o PCB que permite interromper e retomar o processo.

#### Estados e transições

Os estados clássicos são novo, pronto (esperando CPU), executando, bloqueado/em espera (aguardando um evento, como o fim de uma E/S) e terminado. As transições relevantes são: pronto → executando (o escalonador despacha o processo); executando → pronto (preempção, por fim de quantum ou chegada de processo mais prioritário); executando → bloqueado (o processo solicita E/S ou espera um evento); bloqueado → pronto (o evento ocorreu); e executando → terminado. Não existe transição direta de bloqueado para executando, um detalhe muito cobrado. Alguns SOs incluem os estados suspenso (processo levado para o disco por swapping).

#### Troca de contexto

É a operação de salvar o contexto do processo que sai (no PCB) e carregar o do que entra. É puro overhead, pois nenhum trabalho útil é feito durante a troca. Seu custo depende do hardware (número de registradores, TLB e caches que precisam ser invalidadas), e por isso trocas muito frequentes degradam o desempenho.

#### Criação e término

No Unix, a chamada fork() cria um processo filho como cópia do pai (retorna 0 ao filho e o PID do filho ao pai), usando copy-on-write para evitar a cópia imediata das páginas. A família exec() substitui a imagem do processo por um novo programa. O pai pode esperar o filho com wait(). Um processo que terminou, mas cujo pai ainda não coletou seu status, vira zumbi. Um filho cujo pai terminou antes é órfão e é adotado pelo processo init/systemd. No Windows, CreateProcess cria o processo já carregando o programa.

#### Threads

Uma thread (linha de execução) é uma unidade de escalonamento dentro de um processo. As threads de um mesmo processo compartilham o código, os dados, o heap e os arquivos abertos, e cada uma tem seus próprios registradores, contador de programa e pilha. Criar e trocar threads é bem mais barato que criar e trocar processos, e a comunicação é simples, porque a memória é comum, mas isso exige sincronização e um erro em uma thread pode comprometer o processo inteiro. Os modelos de implementação são: threads de usuário (gerenciadas por uma biblioteca, rápidas, mas o bloqueio de uma pode bloquear o processo todo e não aproveitam multiprocessadores), threads de kernel (o SO as conhece e escalona, e o bloqueio de uma não afeta as demais) e os mapeamentos muitos-para-um, um-para-um (o usado em Linux e Windows) e muitos-para-muitos.

### D.3 Escalonamento de processos

O escalonador decide qual processo pronto receberá a CPU. Há três níveis: o de longo prazo (admissão de processos ao sistema, controla o grau de multiprogramação), o de médio prazo (swapping) e o de curto prazo (CPU, executado com muita frequência, também chamado dispatcher na parte de entrega da CPU). Um escalonador não preemptivo só retira a CPU quando o processo termina ou bloqueia. O preemptivo pode retirá-la à força.

Os critérios de avaliação são: utilização da CPU (maximizar), throughput (processos concluídos por unidade de tempo, maximizar), tempo de retorno (turnaround) (do instante de chegada até o término, minimizar), tempo de espera (soma do tempo na fila de prontos, minimizar) e tempo de resposta (da submissão até a primeira resposta, minimizar, crítico em sistemas interativos).

#### Algoritmos

FCFS (First Come, First Served). Atende por ordem de chegada. É simples e não preemptivo, mas sofre do efeito comboio: processos curtos esperam atrás de um longo.

SJF (Shortest Job First). Escolhe o processo de menor tempo de execução (burst). É comprovadamente ótimo para minimizar o tempo médio de espera, mas exige conhecer a duração futura, que na prática é estimada por média exponencial dos bursts anteriores. Pode causar inanição (starvation) dos processos longos. Sua versão preemptiva é o SRTF (Shortest Remaining Time First).

Prioridades. Cada processo tem uma prioridade, e a CPU vai ao de maior prioridade. Pode ser preemptivo ou não. O risco é a inanição dos de baixa prioridade, resolvida pelo envelhecimento (aging), que aumenta gradualmente a prioridade de quem espera.

Round Robin (RR). É o algoritmo clássico dos sistemas de tempo compartilhado. Cada processo recebe uma fatia de tempo (quantum) e, se não terminar, volta ao fim da fila. É preemptivo e justo. Se o quantum for muito grande, degenera em FCFS. Se for muito pequeno, o overhead de troca de contexto domina. Uma regra prática é que cerca de 80% dos bursts devem ser menores que o quantum.

Múltiplas filas e filas multinível com realimentação (MLFQ). Vários níveis de fila, cada um com prioridade e algoritmo próprios, e os processos migram entre níveis conforme seu comportamento (um processo que consome todo o quantum cai de nível, e um interativo que bloqueia cedo é mantido no alto). É a base dos escalonadores de Unix e Windows tradicionais. Outros: loteria (bilhetes sorteados, proporcional), fair-share (justiça por usuário), e o CFS (Completely Fair Scheduler) do Linux, baseado em tempo virtual de execução e uma árvore rubro-negra.

Tempo real. Tempo real rígido (hard) exige cumprir prazos sempre, e o brando (soft) tolera atrasos ocasionais. Dois algoritmos clássicos são o Rate Monotonic (RM), de prioridade estática (quanto menor o período, maior a prioridade) e o EDF (Earliest Deadline First), dinâmico, que escolhe sempre o processo com o prazo mais próximo e é ótimo para um processador, com utilização de até 100%.

#### Exemplo numérico

Sejam três processos que chegam em t=0, com bursts P1=24, P2=3 e P3=3.

- FCFS (ordem P1, P2, P3): esperas 0, 24 e 27, média 17.
    
- SJF (ordem P2, P3, P1): esperas 0, 3 e 6, média 3.
    
- RR com quantum 4: P1 roda de 0 a 4, P2 de 4 a 7, P3 de 7 a 10, e P1 retoma de 10 em diante até 30. Esperas: P1 = 10 − 4 = 6, P2 = 4, P3 = 7, média 17/3 ≈ 5,67.
    

Macete: em questões de escalonamento, desenhe sempre o diagrama de Gantt. Calcule o turnaround como término − chegada e a espera como turnaround − burst. Em empates, siga a regra do enunciado (geralmente a ordem de chegada).

### D.4 Sincronização de processos

Quando processos ou threads compartilham dados, o resultado pode depender da ordem de execução, o que se chama de condição de corrida (race condition). O trecho de código que acessa o recurso compartilhado é a seção crítica. Uma solução correta deve garantir três propriedades: exclusão mútua (no máximo um processo na seção crítica), progresso (a decisão de quem entra não pode ser adiada indefinidamente, e processos fora da seção crítica não bloqueiam os outros) e espera limitada (nenhum processo espera para sempre, ou seja, sem inanição).

#### Mecanismos

Soluções de software: o algoritmo de Peterson resolve a exclusão mútua para dois processos usando um vetor flag[2] (interesse em entrar) e uma variável turn (de quem é a vez). A alternância estrita garante exclusão, mas viola o progresso. O algoritmo da padaria (Lamport) estende a solução para n processos. Em CPUs modernas com reordenação de memória, soluções puramente de software exigem barreiras de memória.

Suporte de hardware: instruções atômicas como Test-and-Set, Swap e Compare-and-Swap (CAS), que leem e modificam uma posição de memória de forma indivisível. Com elas constroem-se os spinlocks, em que o processo fica em espera ocupada (busy waiting), o que é aceitável para seções críticas muito curtas em multiprocessadores. Em monoprocessadores, desabilitar interrupções é uma opção para o kernel, mas inaceitável para programas de usuário.

Semáforos (Dijkstra): variável inteira não negativa acessada somente por duas operações atômicas, P/wait/down (decrementa; se o valor ficaria negativo, bloqueia o processo) e V/signal/up (incrementa e, se há processos bloqueados, acorda um). O semáforo binário (valores 0 e 1) funciona como mutex para exclusão mútua, e o semáforo contador controla o acesso a um conjunto de n recursos idênticos. Também serve para sincronização de ordem (um processo espera o evento de outro) quando inicializado em 0.

Monitores: construção de linguagem de alto nível (Java com synchronized, wait e notify) em que só um processo pode estar ativo dentro do monitor por vez, e variáveis de condição (wait/signal) permitem esperar por condições. É menos propenso a erros que semáforos. Há ainda barreiras, read-write locks e a troca de mensagens como forma de sincronização.

#### Problemas clássicos

O produtor-consumidor (buffer limitado) usa três semáforos: mutex (exclusão no buffer, inicializado em 1), vazio (posições livres, inicializado em N) e cheio (posições ocupadas, inicializado em 0). O produtor faz P(vazio), P(mutex), insere, V(mutex), V(cheio); o consumidor faz o simétrico. Atenção: inverter a ordem dos P (por exemplo, P(mutex) antes de P(vazio)) pode causar deadlock. Leitores-escritores permite vários leitores simultâneos mas exclusividade para escritores (e há variantes com prioridade para leitores ou escritores, que podem gerar inanição). O jantar dos filósofos ilustra o deadlock e a inanição no acesso a recursos compartilhados (garfos).

#### Deadlock (impasse)

É a situação em que um conjunto de processos fica bloqueado para sempre, cada um esperando um recurso retido por outro do conjunto. Ocorre quando as quatro condições de Coffman são satisfeitas simultaneamente: exclusão mútua (o recurso não é compartilhável), posse e espera (o processo retém recursos enquanto pede outros), não preempção (o recurso só é liberado voluntariamente) e espera circular (existe um ciclo de processos, cada um esperando o recurso do próximo). Basta quebrar uma delas para evitar o impasse.

O tratamento tem quatro estratégias. A prevenção ataca uma das condições (por exemplo, exigir que o processo peça todos os recursos de uma vez, quebrando posse e espera, ou numerar os recursos e exigir pedidos em ordem crescente, quebrando a espera circular). A evitação (avoidance) exige informação prévia sobre as necessidades máximas e só concede um recurso se o sistema permanecer em estado seguro (existe uma sequência em que todos terminam). O clássico é o algoritmo do banqueiro (Dijkstra), que usa as matrizes Max, Alocação, Necessidade = Max − Alocação e o vetor Disponível. A detecção e recuperação permite o impasse, detecta-o (ciclo no grafo de alocação de recursos, ou algoritmo semelhante ao do banqueiro) e se recupera, terminando processos ou retirando recursos. A política do avestruz ignora o problema, a escolha de Unix e Windows quando os impasses são raros e a prevenção é cara.

Não confunda: deadlock (ninguém progride, bloqueados), livelock (os processos continuam executando e mudando de estado, mas sem progredir) e starvation (um processo é preterido indefinidamente enquanto o sistema progride). A inversão de prioridade ocorre quando um processo de alta prioridade espera um de baixa que foi preemptado por um de média, e a solução é a herança de prioridade (famosa pelo caso da sonda Mars Pathfinder).

### D.5 Comunicação entre processos (IPC)

Processos independentes têm espaços de endereçamento isolados, e precisam do SO para cooperar. Os dois modelos fundamentais são:

Memória compartilhada. O SO mapeia uma região comum no espaço de ambos. É a forma mais rápida (após configurada, não há intervenção do kernel), mas exige sincronização explícita (semáforos, mutexes).

Troca de mensagens. Usa as primitivas send e receive. É mais simples de usar, não exige memória compartilhada e funciona também entre máquinas distintas, mas tem mais overhead por envolver o kernel. As variações são: comunicação direta (nomeia o processo) ou indireta (via caixas de correio/portas), síncrona (bloqueante) ou assíncrona (não bloqueante), e buffer de capacidade zero (rendezvous), limitada ou ilimitada.

Os mecanismos concretos mais cobrados são: pipes anônimos (canal unidirecional entre processos aparentados, como em ls | grep x), pipes nomeados (FIFOs) (têm nome no sistema de arquivos e unem processos não aparentados), filas de mensagens, sinais (notificações assíncronas simples, como SIGKILL, SIGTERM e SIGINT), sockets (comunicação bidirecional, local ou em rede, TCP ou UDP), arquivos mapeados em memória e semáforos.

#### Comunicação em sistemas distribuídos

Aqui entra a RPC (Remote Procedure Call), que faz a chamada a uma função remota parecer local. O cliente chama um stub que faz o marshalling (serialização dos parâmetros), envia a mensagem, e o stub do servidor a desserializa e executa. Ela oculta a rede, mas não suas falhas (semânticas de at-least-once, at-most-once e exactly-once). Derivados são o RMI do Java, o gRPC e as APIs REST. Outras formas são o middleware orientado a mensagens (filas, publish/subscribe) e a comunicação em grupo (multicast).

Em sistemas distribuídos, como não há relógio global, usam-se relógios lógicos: o de Lamport dá uma ordem parcial consistente com a causalidade (regra: a→b implica C(a) < C(b), mas a recíproca não vale), e o relógio vetorial detecta também a concorrência. A exclusão mútua distribuída pode ser centralizada (um coordenador), por Ricart-Agrawala (multicast de requisição e respostas) ou por token ring. A eleição de líder usa os algoritmos Bully e de anel. Para consenso entre nós sujeitos a falhas usam-se Paxos e Raft. A sincronização de relógios físicos usa Cristian, Berkeley e NTP.

### D.6 Gerenciamento de memória

O gerenciador de memória decide onde cada processo fica, protege-os entre si e permite que a soma das memórias lógicas supere a memória física. O endereço lógico (virtual) é gerado pela CPU, e o endereço físico é o visto pela memória. A tradução é feita em hardware pela MMU (Memory Management Unit). A relocação permite carregar o programa em qualquer lugar, e a proteção impede acessos indevidos (na forma mais simples, por registradores base e limite).

#### Alocação contígua

Na partição fixa, a memória é dividida em partições de tamanhos predefinidos, o que gera fragmentação interna (espaço desperdiçado dentro da partição). Na partição variável, cada processo recebe exatamente o que precisa, mas surge a fragmentação externa (muitos buracos livres pequenos e não contíguos, cuja soma seria suficiente). A solução é a compactação (cara, exige relocação dinâmica). Para escolher o buraco há quatro estratégias: First-Fit (o primeiro que couber, rápido), Next-Fit (como o first-fit, mas continua de onde parou), Best-Fit (o menor que couber, deixa buracos minúsculos e inúteis) e Worst-Fit (o maior, na esperança de deixar sobras utilizáveis). O first-fit e o best-fit costumam ter desempenho melhor que o worst-fit. O sistema Buddy (alocação por potências de 2) é usado no kernel Linux. O swapping move processos inteiros entre memória e disco.

#### Paginação

A memória física é dividida em blocos de tamanho fixo chamados quadros (frames), e o espaço lógico em páginas do mesmo tamanho. Uma tabela de páginas mapeia cada página a um quadro. O endereço lógico se divide em número da página (p) e deslocamento (d). A paginação elimina a fragmentação externa, mas ainda tem fragmentação interna, limitada à última página de cada processo (em média meia página).

Exemplo: endereço lógico de 32 bits e página de 4 KiB (2¹² bytes). O deslocamento usa 12 bits, e o número da página usa 20 bits, o que dá 2²⁰ ≈ 1 milhão de entradas na tabela. Com entradas de 4 bytes, a tabela ocuparia 4 MiB por processo. Por isso se usam tabelas multinível (a tabela de páginas é, ela mesma, paginada), tabelas invertidas (uma entrada por quadro físico, indexada por hash) e a TLB (Translation Lookaside Buffer), uma cache associativa das traduções recentes. O tempo efetivo de acesso com TLB é: EAT = α·(t_TLB + t_mem) + (1 − α)·(t_TLB + 2·t_mem) para tabela de um nível, em que α é a taxa de acerto da TLB. Por exemplo, com t_mem = 100 ns, t_TLB = 10 ns e α = 0,9: EAT = 0,9 × 110 + 0,1 × 210 = 120 ns.

#### Segmentação

O espaço de endereços é dividido em segmentos de tamanho variável com significado lógico (código, dados, pilha). O endereço lógico é o par (segmento, deslocamento), e a tabela de segmentos guarda base e limite de cada um. Combina bem com proteção e compartilhamento, mas sofre de fragmentação externa. Muitos sistemas combinam as duas técnicas (segmentação paginada), como o x86 clássico.

#### Memória virtual

Permite executar processos parcialmente carregados, graças à localidade de referência. Na paginação por demanda, as páginas só são trazidas à memória quando referenciadas. A tabela de páginas tem um bit de validade/presença. Ao acessar uma página ausente ocorre uma falta de página (page fault), tratada em passos: a MMU gera uma exceção; o SO verifica se o acesso é legal; encontra um quadro livre (ou escolhe uma vítima); agenda a leitura do disco; atualiza a tabela; e reinicia a instrução. O tempo efetivo de acesso é EAT = (1 − p)·t_mem + p·t_falta, em que p é a taxa de faltas. Como t_falta é da ordem de milissegundos e t_mem de nanossegundos, p precisa ser pequeníssimo para não destruir o desempenho.

Algoritmos de substituição de páginas:

- Ótimo (OPT/MIN): substitui a página que será usada mais tarde no futuro. É irrealizável, mas serve de referência.
    
- FIFO: substitui a página mais antiga. É simples, porém pode remover páginas muito usadas e sofre da anomalia de Belady (aumentar o número de quadros pode aumentar as faltas). O exemplo clássico é a sequência de referências 1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5, que gera 9 faltas com 3 quadros e 10 faltas com 4 quadros.
    
- LRU (menos recentemente usada): substitui a página sem uso há mais tempo. Aproxima bem o OPT e não sofre da anomalia de Belady (é um “algoritmo de pilha”), mas a implementação exata é cara (contadores ou pilha), então se usam aproximações.
    
- Segunda chance / Relógio (clock): FIFO com um bit de referência, que dá uma segunda chance às páginas recentemente usadas. É a aproximação mais comum do LRU.
    
- NRU (não usada recentemente), baseado nos bits R (referência) e M (modificação), e LFU/MFU (por frequência).
    

O conjunto de trabalho (working set) é o conjunto de páginas que um processo está usando ativamente numa janela recente. Se a soma dos conjuntos de trabalho excede a memória física, o sistema entra em thrashing: gasta mais tempo paginando que executando, e a utilização da CPU despenca. As soluções são reduzir o grau de multiprogramação, e usar alocação baseada em conjunto de trabalho ou por frequência de faltas. A política de substituição pode ser local (só entre páginas do próprio processo) ou global. O copy-on-write e os arquivos mapeados em memória são otimizações importantes.

### D.7 Gerenciamento de arquivos

Um arquivo é uma abstração de armazenamento persistente, uma coleção nomeada de informações relacionadas. Seus atributos típicos são nome, tipo, localização, tamanho, proteção/permissões, datas e proprietário. As operações básicas são criar, abrir, ler, escrever, reposicionar (seek), truncar, fechar e excluir. Os métodos de acesso são o sequencial, o direto (aleatório) e o indexado.

#### Diretórios

Os diretórios organizam os arquivos. As estruturas evoluíram de nível único (conflito de nomes) para dois níveis (um por usuário), árvore (a mais comum, com caminhos absolutos e relativos), grafo acíclico (permite compartilhamento via links) e grafo geral (admite ciclos, e exige tratamento especial na coleta de lixo). Os links são de dois tipos: o hard link (nova entrada de diretório que aponta para o mesmo i-node, só vale dentro do mesmo sistema de arquivos e o dado só some quando o contador de links chega a zero) e o link simbólico (arquivo especial contendo um caminho, que pode apontar para outros sistemas de arquivos e ficar “quebrado” se o alvo sumir).

#### Métodos de alocação de espaço em disco

Alocação contígua. O arquivo ocupa blocos consecutivos. O acesso é muito rápido (sequencial e direto), mas sofre de fragmentação externa e o crescimento do arquivo é difícil.

Alocação encadeada. Cada bloco aponta para o próximo. Não há fragmentação externa, mas o acesso direto é ruim (é preciso percorrer a lista) e um ponteiro perdido corrompe o arquivo. A FAT (File Allocation Table) coloca os ponteiros numa tabela separada, em memória, melhorando o acesso direto.

Alocação indexada. Um bloco de índice reúne os ponteiros para todos os blocos do arquivo. Suporta acesso direto sem fragmentação externa. O i-node do Unix/ext guarda os atributos e, por exemplo, 12 ponteiros diretos, um indireto simples, um indireto duplo e um indireto triplo. Exemplo: bloco de 4 KiB e ponteiros de 4 bytes (1.024 ponteiros por bloco). Os diretos cobrem 12 × 4 KiB = 48 KiB. O indireto simples cobre 1.024 × 4 KiB = 4 MiB. O duplo cobre 1.024² × 4 KiB = 4 GiB. O triplo cobre 1.024³ × 4 KiB = 4 TiB. Isso explica por que arquivos pequenos são acessados com poucos acessos a disco.

#### Gerência de espaço livre

Pode ser feita por mapa de bits (bitmap) (um bit por bloco, simples e eficiente para achar blocos contíguos), lista encadeada de blocos livres, agrupamento e contagem (par endereço/quantidade de blocos livres contíguos).

#### Sistemas de arquivos reais

O FAT16/FAT32 é simples e universal, mas sem permissões nem journaling, com limite de 4 GiB por arquivo no FAT32 (o exFAT o supera, voltado a mídias removíveis). O NTFS (Windows) usa a MFT (Master File Table), tem journaling para metadados, permissões em ACLs, compressão, criptografia (EFS), cotas e links. A família ext2/ext3/ext4 (Linux) usa i-nodes (ext2 sem journaling, ext3 com journaling, ext4 com extents, arquivos grandes e melhor desempenho). Outros: XFS, Btrfs e ZFS (copy-on-write, snapshots, checksums), APFS (Apple).

O journaling registra previamente em um diário as operações de metadados que serão realizadas, de modo que, após uma falha, o sistema reaplica ou descarta as operações incompletas em vez de percorrer todo o disco (fsck), garantindo a consistência do sistema de arquivos. O VFS (Virtual File System) é a camada do kernel que oferece uma interface única sobre diferentes sistemas de arquivos. As permissões Unix (rwx para dono, grupo e outros, por exemplo 755) já foram vistas na parte de Linux.

#### Sistemas de arquivos distribuídos

Fornecem acesso transparente a arquivos remotos. O NFS (Network File System, Unix) e o SMB/CIFS (Windows e Samba) são os clássicos de rede local. O AFS usa cache em disco local e semântica de sessão. O HDFS (Hadoop) divide arquivos em blocos grandes (por exemplo, 128 MB) replicados em vários nós, otimizado para leitura sequencial de grandes volumes, com um NameNode de metadados e vários DataNodes. O Ceph e o GlusterFS são alternativas escaláveis. Os temas centrais são a replicação, a consistência de cache e a tolerância a falhas.

### D.8 Gerenciamento de entrada e saída

O hardware de E/S (controladores, portas, interrupções e DMA) foi visto na Parte C. O foco aqui é o software de E/S, organizado em camadas, de baixo para cima: tratadores de interrupção, drivers de dispositivo (código específico para cada dispositivo, normalmente executado no kernel), software independente de dispositivo (nomeação uniforme, proteção, buffering, alocação, tratamento de erros) e software de nível de usuário (bibliotecas e spooling). Os dispositivos são classificados em de bloco (armazenam informação em blocos endereçáveis, como discos) e de caractere (fluxo de caracteres, como teclado e porta serial).

Conceitos importantes: buffering (área intermediária que acomoda diferenças de velocidade e de tamanho de transferência), caching (cópia dos dados mais usados em memória rápida), spooling (fila em disco para dispositivos dedicados e não compartilháveis, como a impressora) e E/S bloqueante, não bloqueante e assíncrona.

#### Discos

O tempo de acesso a um disco rígido é a soma de tempo de busca (seek) (mover o braço até o cilindro), latência rotacional (esperar o setor passar sob a cabeça, em média meia rotação) e tempo de transferência. Por exemplo, a 7.200 rpm uma rotação leva 60/7200 ≈ 8,33 ms, então a latência média é ≈ 4,17 ms. O seek costuma dominar, e por isso o escalonamento de disco tenta minimizar o movimento do braço:

- FCFS: atende na ordem de chegada. É justo, mas ineficiente.
    
- SSTF (Shortest Seek Time First): atende o pedido mais próximo da posição atual. Reduz o movimento, mas pode causar inanição das requisições distantes.
    
- SCAN (elevador): o braço percorre o disco numa direção atendendo tudo, depois inverte. C-SCAN só atende num sentido e retorna rápido ao início, dando tempo de espera mais uniforme. LOOK e C-LOOK variam o SCAN, invertendo o sentido na última requisição, e não na borda do disco.
    

Para SSDs, o escalonamento de braço não faz sentido (não há partes móveis), e entram os conceitos de wear leveling, TRIM e a camada FTL.

### D.9 Virtualização

Virtualização é a criação de versões virtuais de recursos computacionais. A mais comum é a de máquinas virtuais (VMs): o hipervisor (VMM, Virtual Machine Monitor) permite que vários sistemas operacionais convidados (guests) rodem isolados sobre o mesmo hardware físico (host).

O hipervisor tipo 1 (nativo, bare metal) roda diretamente sobre o hardware (VMware ESXi, Microsoft Hyper-V, Xen, e o KVM, que transforma o próprio Linux em hipervisor), com melhor desempenho e uso em datacenters. O tipo 2 (hospedado) roda como aplicativo sobre um SO hospedeiro (VirtualBox, VMware Workstation, Parallels), mais simples, indicado para desktops e testes, com maior overhead.

Requisitos de Popek e Goldberg (1974) para um VMM: equivalência (o programa se comporta, em essência, como na máquina real), controle de recursos (o VMM controla todos os recursos virtualizados) e eficiência (a maioria das instruções executa diretamente no hardware). Um conceito-chave é que a arquitetura é virtualizável de forma clássica (trap-and-emulate) quando todas as instruções sensíveis são também privilegiadas. O x86 original violava isso, o que levou a três técnicas:

- Virtualização completa com tradução binária (VMware): o hipervisor reescreve dinamicamente o código que contém instruções problemáticas, e o convidado não é modificado.
    
- Paravirtualização (Xen): o SO convidado é modificado para chamar o hipervisor por hypercalls, em vez de executar instruções sensíveis. Tem bom desempenho, mas exige alterar o SO.
    
- Virtualização assistida por hardware (Intel VT-x, AMD AMD-V): o processador ganha um modo adicional para o hipervisor e trata as instruções sensíveis, permitindo executar convidados sem modificação e sem tradução binária.
    

A virtualização de memória usa tabelas de página sombra (shadow page tables) ou, com hardware, tabelas aninhadas (EPT da Intel, NPT da AMD). A de E/S pode ser por emulação de dispositivo, por drivers paravirtualizados (virtio) ou por acesso direto (passthrough, SR-IOV). Recursos operacionais importantes são o snapshot, a migração ao vivo (live migration, mover uma VM em execução entre hosts), o overcommit e a consolidação de servidores.

#### Contêineres

A virtualização em nível de sistema operacional compartilha o kernel do host. Os contêineres (Docker, LXC, Podman) usam namespaces do Linux (isolam a visão de processos, rede, sistema de arquivos, usuários) e cgroups (limitam e contabilizam CPU, memória e E/S). Em comparação com VMs, são muito mais leves e rápidos de iniciar (segundos ou menos), com menos overhead e maior densidade, mas oferecem isolamento mais fraco (todos compartilham o kernel) e só rodam o mesmo tipo de kernel. Os contêineres são orquestrados por Kubernetes, tema da Parte F.

Macete sobre SOs. Estados: não existe bloqueado → executando. SJF é ótimo no tempo médio de espera. Round robin = tempo compartilhado. Semáforo binário ≈ mutex. As 4 condições de Coffman são todas necessárias. FIFO tem anomalia de Belady, LRU e OPT não. Paginação elimina fragmentação externa, segmentação sofre dela. Memória compartilhada é a IPC mais rápida. Hipervisor tipo 1 = bare metal. Contêiner compartilha kernel, VM não.

**