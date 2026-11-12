
# PARTE C: ARQUITETURA DE COMPUTADORES

### **C.1 Aritmética binária para números inteiros**

#### Soma e subtração

A soma binária segue as regras 0+0=0, 0+1=1, 1+0=1 e 1+1=10 (soma 0 com vai-um 1), e 1+1+1=11. Para o complemento de 2, o hardware soma os números e descarta o transporte final, sem nenhuma correção. A subtração é feita somando o complemento de 2 do subtraendo. Exemplo em 8 bits, 25 − 10: +25 = 00011001, −10 = 11110110, e a soma é 1 00001111. Descartando o transporte final, o resultado é 00001111 = 15.

#### Estouro (overflow)

Ocorre quando o resultado não cabe na faixa representável. Em complemento de 2, o estouro só pode ocorrer quando se somam dois números de mesmo sinal e o resultado tem sinal diferente. Em hardware, detecta-se comparando o transporte que entra no bit de sinal com o transporte que sai dele: se forem diferentes, houve estouro. Por exemplo, em 8 bits, 100 + 50 = 150 > 127, e 01100100 + 00110010 = 10010110, que é negativo (−106), indicando estouro. Não confunda: o transporte final (carry) indica estouro em números sem sinal, enquanto o overflow indica estouro em números com sinal. Eles são flags distintas (CF e OF no x86).

#### Deslocamentos

O deslocamento lógico insere zeros. O deslocamento aritmético à direita preserva o bit de sinal (replica-o), mantendo a divisão por 2 correta para negativos. Deslocar k posições à esquerda equivale a multiplicar por 2ᵏ (se não houver estouro), e deslocar à direita equivale a dividir por 2ᵏ. Existem também as rotações, em que os bits que saem de um lado voltam do outro.

#### Multiplicação e divisão

A multiplicação binária segue o método escolar de somar e deslocar (shift-and-add): para cada bit 1 do multiplicador, soma-se o multiplicando deslocado ao produto parcial. Um produto de dois números de n bits precisa de até 2n bits. O algoritmo de Booth trata multiplicadores em complemento de 2 e reduz o número de somas ao agrupar sequências de uns (recodifica 0111 como +1000 −0001). A divisão binária é um processo de subtrações e deslocamentos sucessivos, com algoritmos de restauração e sem restauração.

### **C.2 Aritmética para números reais**

#### Ponto fixo

Na representação em ponto fixo, a posição da vírgula é fixa por convenção: reservam-se i bits para a parte inteira e f bits para a fracionária. O valor é o inteiro armazenado dividido por 2ᶠ. É simples, rápido, e usado em DSPs, microcontroladores e aplicações monetárias (em que se armazena centavos como inteiros). A desvantagem é a faixa e a precisão limitadas: com f bits fracionários, a resolução é 2⁻ᶠ.

#### Ponto flutuante (IEEE 754)

O ponto flutuante representa números na forma de notação científica binária: (−1)ˢ × 1,M × 2ᴱ, com um bit de sinal (S), um expoente (E) em excesso e uma mantissa (fração, M). O padrão IEEE 754 define:

> - **Precisão simples (32 bits, float):** 1 bit de sinal, 8 bits de expoente (viés 127), 23 bits de mantissa. Cerca de 7 dígitos decimais de precisão.

> - **Precisão dupla (64 bits, double):** 1 bit de sinal, 11 bits de expoente (viés 1023), 52 bits de mantissa. Cerca de 15 a 16 dígitos decimais.


Em números normalizados, o primeiro bit da mantissa é sempre 1 e, por isso, é implícito (não armazenado, o chamado “bit escondido”), o que dá uma precisão efetiva de 24 bits em simples e 53 em dupla. O valor é: (−1)ˢ × (1 + M/2²³) × 2^(E−127).

Exemplo: representar −13,25 em precisão simples.

1. Sinal: negativo, então S = 1.
    
2. Converter para binário: 13 = 1101 e 0,25 = 0,01, logo 13,25 = 1101,01₂.
    
3. Normalizar: 1101,01 = 1,10101 × 2³.
    
4. Expoente armazenado: 3 + 127 = 130 = 10000010₂.
    
5. Mantissa (sem o 1 implícito): 10101 completada com zeros até 23 bits.
    
6. Resultado: 1 | 10000010 | 10101000000000000000000, que em hexadecimal é C1540000.


Na direção inversa (de hexadecimal para decimal), separa-se o sinal, o expoente e a mantissa, subtrai-se o viés do expoente, restaura-se o “1,” e desloca-se a vírgula.

Valores especiais:

- Expoente todo 0 e mantissa 0: zero (existem +0 e −0).
    
- Expoente todo 0 e mantissa ≠ 0: número desnormalizado (subnormal), que preenche a lacuna perto do zero com perda gradual de precisão.
    
- Expoente todo 1 e mantissa 0: infinito (±∞).
    
- Expoente todo 1 e mantissa ≠ 0: NaN (Not a Number), resultado de operações inválidas como 0/0 ou √(−1).
    

Pontos conceituais importantes:

- Os números em ponto flutuante não são uniformemente espaçados: a distância entre dois números consecutivos cresce com a magnitude.
    
- A representação é aproximada: 0,1 não tem forma exata, então comparar floats com == é perigoso. O correto é comparar a diferença com uma tolerância (épsilon).
    
- A soma não é associativa: (a + b) + c pode diferir de a + (b + c). Somar um número muito pequeno a um muito grande pode simplesmente não alterar o grande (absorção).
    
- Modos de arredondamento: para o mais próximo (par em caso de empate, o padrão), em direção a zero, em direção a +∞ e em direção a −∞.
    
- Em contextos fiscais e financeiros, o ponto flutuante binário é desaconselhado para valores monetários, e se usa decimal (BigDecimal, inteiros em centavos), pelos motivos acima.
    

**Aritmética em ponto flutuante:** para somar, alinham-se os expoentes (deslocando a mantissa do menor), somam-se as mantissas, normaliza-se e arredonda-se. Para multiplicar, somam-se os expoentes (subtraindo o viés uma vez), multiplicam-se as mantissas e somam-se os sinais por XOR. O cancelamento catastrófico ocorre ao subtrair números quase iguais, quando se perdem dígitos significativos.

**Macete:** decore 8/23/127 e 11/52/1023. Para a faixa de expoente em simples, o expoente real vai de −126 a +127 (os valores armazenados 0 e 255 são reservados). Em questões de “qual o maior número de bits necessário”, lembre que simples tem 24 bits significativos e dupla tem 53.

### **C.3 Elementos de um processador**

O processador (CPU) executa instruções armazenadas em memória. Na arquitetura de **Von Neumann**, instruções e dados compartilham a mesma memória e o mesmo barramento, o que cria o chamado “gargalo de von Neumann”. Na arquitetura Harvard, instruções e dados têm memórias e barramentos separados, o que permite acesso simultâneo. As CPUs modernas são um híbrido: memória principal unificada, mas caches L1 de instruções e de dados separadas (Harvard modificada).

Os componentes principais são:

> **Unidade de Controle (UC):** Busca, decodifica e coordena a execução das instruções, emitindo os sinais de controle para os demais componentes. Pode ser cabeada (hardwired), implementada com lógica combinacional e sequencial, rápida e típica de RISC; ou microprogramada, em que cada instrução é interpretada por uma sequência de microinstruções armazenadas numa memória de controle, mais flexível e típica de CISC.

> **Unidade Lógica e Arimética (ULA):** Executa as operações aritméticas e lógicas, como já visto.

> **Registradores:** São a memória mais rápida e menor do sistema, dentro da CPU. Os principais são:
>	**PC (Program Counter, ou IP, Instruction Pointer):** guarda o endereço da próxima instrução.
>	**R (Instruction Register):** guarda a instrução em execução.
>	**MAR (Memory Address Register, ou REM):** guarda o endereço a ser acessado na memória.
>	**MBR (Memory Buffer Register, ou MDR/RDM):** guarda o dado lido ou a ser escrito.
>	**Acumulador (ACC):** guarda operandos e resultados da ULA em arquiteturas mais simples.
>	**SP (Stack Pointer):** aponta para o topo da pilha.
>	**Registrador de estado/flags (PSW):** guarda indicadores como Zero, Sinal (Negativo), Carry, Overflow, Paridade e habilitação de interrupção.
>	**Registradores de uso geral:** guardam operandos e resultados.


**Ciclo de instrução:** Cada instrução passa pelas fases de busca (fetch), em que o endereço do PC vai para o MAR, a instrução é lida para o MBR e depois para o IR, e o PC é incrementado; decodificação, em que a UC interpreta o opcode; busca de operandos, se necessário; execução; e escrita do resultado (write-back). Se houver interrupção pendente, o ciclo inclui um tratamento de interrupção. Esse ciclo é o núcleo conceitual que fundamenta o pipeline.

**Formato de instrução:** Uma instrução é composta de opcode (código da operação), especificadores de modo de endereçamento e operandos. Pode ter 0, 1, 2 ou 3 endereços. Máquinas de pilha usam instruções sem endereço explícito (operam no topo da pilha); máquinas de acumulador usam um endereço; e RISC modernos usam três (destino e dois fontes). Um opcode de k bits permite 2ᵏ operações distintas.

**Desempenho:** o tempo de CPU de um programa é T = (nº de instruções × CPI) / frequência, em que CPI é o número médio de ciclos por instrução. MIPS = frequência / (CPI × 10⁶). A Lei de Amdahl limita o ganho de uma melhoria que atinge apenas uma fração f da execução: speedup = 1 / ((1 − f) + f/s), em que s é o ganho da parte melhorada. Mesmo com s → ∞, o speedup máximo é 1/(1 − f).

**Interrupções:** São desvios forçados do fluxo normal, que podem ser de hardware (E/S, timer), de software (traps, chamadas ao sistema) ou exceções (divisão por zero, falta de página). Ao receber uma interrupção, a CPU conclui a instrução corrente, salva o contexto (PC e flags) na pilha, consulta a tabela de vetores de interrupção e salta para a rotina de tratamento, retornando depois. Interrupções mascaráveis podem ser bloqueadas por software, enquanto as não mascaráveis (NMI) não podem.

**Multicore e paralelismo:** Processadores modernos têm vários núcleos, hyper-threading (SMT), arquiteturas superescalares (várias instruções por ciclo) e execução fora de ordem. A taxonomia de Flynn classifica máquinas em SISD (monoprocessador clássico), SIMD (instruções vetoriais, GPUs), MISD (raro) e MIMD (multiprocessadores).

### **C.4 Modos de endereçamento**

O modo de endereçamento define como a CPU obtém o operando de uma instrução. A escolha envolve um equilíbrio entre flexibilidade, tamanho da instrução e número de acessos à memória.

> **-** **Imediato:** o operando está na própria instrução (ADD R1, #5). Não requer acesso à memória para o operando, mas o valor é limitado ao tamanho do campo.

> **- Direto (ou absoluto):** a instrução contém o endereço do operando (LOAD 100). Requer 1 acesso à memória, e o espaço de endereçamento é limitado pelo tamanho do campo.

> **- Indireto:** a instrução contém o endereço de uma posição que, por sua vez, contém o endereço do operando (LOAD @100). Requer 2 acessos (ou mais), mas amplia o espaço de endereçamento e permite ponteiros.

> **- Por registrador:** o operando está num registrador (ADD R1, R2). Muito rápido, sem acesso à memória.

> **- Indireto por registrador:** o registrador contém o endereço do operando (LOAD (R1)). Ideal para ponteiros e percursos de vetores.

> **- Com deslocamento (displacement):** o endereço efetivo é a soma de um registrador com uma constante. Há três variantes: relativo ao PC (usado em desvios e para código relocável), por base (registrador-base mais deslocamento, usado em segmentação e acesso a campos de estruturas) e indexado (endereço-base mais um registrador de índice, usado em vetores).

> **- Pilha:** o operando está no topo da pilha, referenciado implicitamente pelo SP (PUSH, POP).

> **- Auto-incremento/auto-decremento:** o registrador é incrementado ou decrementado automaticamente após ou antes do uso.


**Macete:** conte os acessos à memória para obter o operando (excluindo a busca da instrução): imediato 0, registrador 0, direto 1, indireto por registrador 1, indireto de memória 2. Distinga também “endereço efetivo” (onde o dado está) do “operando” (o dado em si). Em desvios condicionais, o modo mais comum é o relativo ao PC.

### **C.5 Paradigmas RISC e CISC**

**CISC (Complex Instruction Set Computer)** surgiu em uma era em que memória era cara e lenta e os compiladores eram simples, então se buscava reduzir o gap semântico com instruções poderosas, que fazem muito em uma só instrução. Suas características são: muitas instruções (centenas), com formatos e tamanhos variáveis, muitos modos de endereçamento, operações que acessam diretamente a memória, poucos registradores, controle tipicamente microprogramado, e CPI variável e alto. Exemplos: x86/x86-64 (Intel, AMD), VAX, Motorola 68000, IBM System/360.

**RISC (Reduced Instruction Set Computer)** surgiu nos anos 1980 (projetos de Berkeley, Stanford e IBM 801) a partir da observação de que programas usam principalmente um pequeno conjunto de instruções simples. Suas características são: conjunto reduzido de instruções simples, formato fixo (por exemplo, 32 bits), poucos modos de endereçamento, arquitetura load/store (apenas as instruções de carga e armazenamento acessam a memória, e as operações da ULA ocorrem entre registradores), muitos registradores, controle cabeado (hardwired), e a meta de CPI ≈ 1, o que favorece o pipeline. A complexidade é transferida para o compilador. Exemplos: ARM, MIPS, RISC-V, SPARC, PowerPC, Alpha.

**Comparação resumida:** o CISC tem programas com menos instruções, mas cada uma é mais lenta e complexa de implementar; o RISC tem programas com mais instruções, porém simples, rápidas e fáceis de passar por pipeline. Pela equação do desempenho, o RISC diminui CPI e tempo de ciclo ao custo de aumentar o número de instruções, e o CISC faz o oposto.

**Cuidado com a visão atual:** a fronteira se tornou tênue. Processadores x86 modernos traduzem internamente as instruções CISC em micro-operações (µops) semelhantes às de RISC, e processadores RISC adotaram extensões complexas (como as vetoriais). Em prova, no entanto, prevalece a distinção clássica. Vale lembrar ainda o conceito de janelas de registradores (SPARC) e as arquiteturas VLIW/EPIC (Itanium), em que o compilador define o paralelismo.

### **C.6 Barramentos**

Um barramento é o conjunto de linhas elétricas que interliga os componentes e transporta informações entre eles. Funcionalmente, divide-se em três grupos de linhas:

> **- Barramento de dados:** transporta os dados, bidirecional. Sua largura (8, 16, 32, 64 bits) define quantos bits trafegam por transferência.

> **- Barramento de endereços:** transporta o endereço da posição de memória ou do dispositivo, unidirecional (da CPU para fora). Sua largura determina o espaço endereçável: 2ⁿ posições. Com 16 linhas, 64 K posições; com 32 linhas, 4 GiB; com 64 linhas, 16 EiB (teórico).

> **- Barramento de controle:** transporta os sinais de controle e temporização (leitura/escrita, clock, interrupção, reconhecimento, bus request/grant, reset).


A **taxa de transferência (largura de banda)** é: largura do barramento × frequência × transferências por ciclo. Um barramento de 64 bits a 100 MHz com uma transferência por ciclo oferece 8 bytes × 100 × 10⁶ = 800 MB/s. Em memórias DDR são feitas duas transferências por ciclo (nas bordas de subida e de descida), dobrando a taxa efetiva.

Quanto à temporização, o barramento **síncrono** é controlado por um clock comum (simples e rápido, porém limitado pelo dispositivo mais lento); o assíncrono usa sinais de handshake (mais flexível entre dispositivos de velocidades diferentes). Quando há mais de um mestre (CPU, DMA), é necessária arbitragem, que pode ser centralizada (daisy chain, polling, requisição independente) ou distribuída.

Em computadores reais há uma hierarquia: um barramento local/frontal de alta velocidade (FSB, hoje substituído por interconexões ponto a ponto como o QPI/UPI da Intel e o Infinity Fabric da AMD), o barramento de memória e os barramentos de expansão (PCI, PCI Express, USB, SATA). O PCIe merece destaque por ser serial e ponto a ponto, composto de lanes (x1, x4, x8, x16) com a taxa crescendo a cada geração, em contraste com o PCI paralelo e compartilhado, que ele substituiu. A tendência geral é a substituição de barramentos paralelos compartilhados por enlaces seriais ponto a ponto, porque o paralelismo sofre com o skew (dessincronização entre linhas) em altas frequências.

### **C.7 Sistema de memória**

#### Organização e características

A memória é organizada como uma matriz de células endereçáveis, cada uma com um endereço único. Uma memória com barramento de endereços de n bits e palavras de m bits tem capacidade de 2ⁿ × m bits. Por exemplo, 16 linhas de endereço e palavra de 8 bits dão 64 KiB. Os atributos que caracterizam uma memória são a capacidade, o tempo de acesso, o ciclo de memória, a taxa de transferência, o método de acesso (aleatório, sequencial, direto, associativo), a volatilidade e o custo por bit.

#### Tipos

Memórias semicondutoras **voláteis**:

> **- SRAM (estática):** usa flip-flops (cerca de 6 transistores por bit). É rápida e não precisa de refresh, mas é cara e ocupa muita área. Usada em caches e registradores.

> **- DRAM (dinâmica):** usa 1 transistor e 1 capacitor por bit. É densa e barata, mas o capacitor descarrega e exige refresh periódico. Usada como memória principal. As variantes síncronas são SDRAM, DDR, DDR2, DDR3, DDR4 e DDR5, e as memórias gráficas são GDDR e HBM.
   

Memórias **não voláteis:**

> **- Read Only Memory (ROM):** gravada na fabricação;
> **- Programable ROM (PROM):** programável uma vez;
> **- Erasable PROM (EPROM):** apagável por luz ultravioleta;
> **- Eletrically EPROM (EEPROM):** apagável eletricamente por byte;
> **- Flash:** apagável eletricamente por blocos. A flash tem as variantes:
> 	**- NOR:** acesso aleatório, usada em firmware/BIOS);
> 	**- NAND:** (maior densidade, usada em SSDs, pendrives e cartões). A flash tem número limitado de ciclos de escrita, o que exige wear leveling.


**Memórias secundárias:** discos magnéticos (HDD), SSDs, discos ópticos (CD, DVD, Blu-ray) e fitas magnéticas (usadas em backup de longo prazo).

#### Hierarquia de memória

Existe um compromisso fundamental: memórias rápidas são caras e pequenas, e memórias grandes são lentas e baratas. A solução é uma hierarquia, em que, ao descer de nível, aumentam a capacidade e o tempo de acesso, e diminui o custo por bit: 
**registradores → cache L1 → L2 → L3 → memória principal (RAM) → armazenamento secundário (SSD/HD) → armazenamento terciário (fita, nuvem).**

![[Pasted image 20261110111605.png]]

A hierarquia funciona por causa do **princípio da localidade**. A **localidade temporal** indica que um item acessado recentemente tende a ser acessado de novo logo. A **localidade espacial** indica que itens próximos ao acessado tendem a ser acessados também (por isso as caches carregam blocos inteiros). O desempenho médio é medido pelo tempo médio de acesso: AMAT = tempo de acerto + taxa de falha × penalidade de falha. Por exemplo, com 1 ns de acerto, 5% de falhas e 100 ns de penalidade, AMAT = 1 + 0,05 × 100 = 6 ns.

#### Memória cache

A cache guarda cópias de blocos da memória principal. Quando a CPU acessa um endereço, ocorre um **acerto (hit)** se o bloco está na cache, ou uma **falha (miss)** se não está. As falhas se classificam em compulsórias (primeiro acesso), de capacidade (a cache é pequena demais para o conjunto de trabalho) e de conflito (blocos disputam a mesma linha ou o mesmo conjunto).

Funções de mapeamento. O endereço é dividido em campos: tag, índice e deslocamento (offset).

> **- Mapeamento direto:** cada bloco da memória só pode ir para uma linha específica (linha = nº do bloco mod nº de linhas). É simples e rápido, mas sujeito a muitos conflitos.

> **- Totalmente associativo:** qualquer bloco pode ir para qualquer linha. Elimina conflitos, mas exige comparar a tag com todas as linhas em paralelo, o que é caro.

> **- Associativo por conjuntos (n-way set associative):** compromisso entre os dois. O bloco é mapeado para um conjunto e pode ocupar qualquer das n vias dele. É o mais usado.


**Exemplo de cálculo de campos:** cache direta de 16 KiB, blocos de 64 bytes, endereços de 32 bits. O offset precisa de log₂64 = 6 bits. O número de linhas é 16 KiB / 64 B = 256, então o índice tem 8 bits. A tag tem 32 − 8 − 6 = 18 bits. Numa cache associativa por conjuntos com 4 vias, o número de conjuntos seria 64 e o índice teria 6 bits, ficando a tag com 20 bits.

**Política de substituição (quando não há linha livre):** LRU (menos recentemente usado), FIFO, aleatória e LFU. Política de escrita: write-through atualiza cache e memória simultaneamente (simples e consistente, porém mais lenta); write-back atualiza só a cache e marca o bloco como “sujo” (dirty bit), gravando na memória apenas quando o bloco é substituído (mais eficiente). Para falha em escrita há write-allocate e no-write-allocate. Em multiprocessadores é necessário manter a coerência de cache, com protocolos como o MESI (Modified, Exclusive, Shared, Invalid).

#### Memória virtual

Permite que programas usem um espaço de endereçamento maior que a memória física, com a memória principal funcionando como cache do disco. O endereço virtual é traduzido em físico por uma **tabela de páginas**, com tradução acelerada pela **TLB (Translation Lookaside Buffer, uma cache de traduções).** Se a página não está na memória, ocorre uma **falta de página (page fault),** tratada pelo sistema operacional. O excesso de faltas de página leva ao **thrashing**. A paginação divide o espaço em blocos de tamanho fixo, e a segmentação em blocos de tamanho variável com significado lógico.

### C.8 Sistema de entrada e saída

O subsistema de E/S conecta a CPU e a memória aos periféricos, que variam enormemente em velocidade (teclado a alguns bytes por segundo, SSD NVMe a vários GB/s). Entre o periférico e o barramento fica o **controlador (ou interface) de E/S**, que traduz sinais, converte formatos, faz buffering e fornece registradores de **status, controle e dados,** as chamadas **portas**.

**Endereçamento de E/S:** na **E/S mapeada em memória (memory-mapped I/O)**, os registradores do dispositivo ocupam endereços do espaço de memória e são acessados com as mesmas instruções (LOAD/STORE), o que é comum em RISC e ARM. **Na E/S isolada (port-mapped)**, há um espaço de endereços próprio e instruções específicas (IN/OUT no x86).

**Três técnicas de controle de E/S**, que a prova cobra muito:

> **E/S programada (polling, ou espera ocupada):** a CPU consulta repetidamente o registrador de status do dispositivo até que ele esteja pronto. É simples, mas desperdiça tempo de processamento.

> **E/S por interrupção:** a CPU inicia a operação e segue executando outras tarefas, e o dispositivo sinaliza por interrupção quando está pronto. Elimina a espera ocupada, mas cada transferência de palavra ainda passa pela CPU. Para identificar o dispositivo, usa-se polling de software, daisy chain ou interrupções vetoradas, e um controlador de interrupções (PIC/APIC) trata as prioridades.

> **DMA** **(Direct Memory Access):** um controlador dedicado transfere blocos de dados entre o dispositivo e a memória sem passar pela CPU. A CPU apenas programa o DMA (endereço de memória, quantidade de bytes, sentido) e é avisada por uma interrupção ao final. O DMA precisa do barramento e o disputa com a CPU: no modo roubo de ciclo (cycle stealing) ele toma o barramento ciclo a ciclo, e no modo rajada (burst) o toma por um bloco inteiro. É a técnica adequada para grandes volumes (discos, placas de rede e vídeo).


**Outros conceitos:** canais de E/S e processadores de E/S (mainframes), spooling (uso de disco como fila para dispositivos lentos, como impressoras), buffering e cache de disco, interfaces serial e paralela, e a evolução dos padrões (USB, SATA, NVMe, Thunderbolt). Destaque ainda a diferença entre transmissão síncrona e assíncrona (esta última delimita cada caractere com bits de início e parada).

### **C.9 Arquiteturas avançadas: pipeline e cache**

#### Pipeline

O **pipeline (escalonamento em linha de montagem)** divide a execução de uma instrução em estágios independentes, de modo que v**árias instruções sejam executadas em paralelo, cada uma em um estágio diferente.** O exemplo clássico de pipeline RISC de 5 estágios é: IF (busca), ID (decodificação e leitura de registradores), EX (execução), MEM (acesso à memória) e WB (escrita do resultado).

O pipeline **não reduz a latência** de uma instrução individual (pode até aumentá-la um pouco), **mas aumenta a vazão (throughput):** idealmente, após encher o pipeline, uma instrução termina por ciclo. Com k estágios e n instruções, o tempo é (k + n − 1) ciclos, contra k × n sem pipeline. Para 5 estágios e 100 instruções são 104 ciclos contra 500, um speedup de 500/104 ≈ 4,8, que tende a k quando n cresce. O ciclo de clock é determinado pelo estágio mais lento, e por isso procura-se balancear os estágios.

**Conflitos (hazards)** são situações que impedem a próxima instrução de executar no ciclo previsto:

> **- Estruturais:** duas instruções precisam do mesmo recurso ao mesmo tempo (por exemplo, uma memória única para instruções e dados). Soluciona-se duplicando recursos (caches L1 separadas) ou introduzindo bolhas.

> **- De dados:** uma instrução depende do resultado de outra ainda não concluída. Há três tipos: RAW (read after write, a dependência verdadeira e a mais comum), WAR e WAW (dependências de nome, relevantes na execução fora de ordem). Soluciona-se com adiantamento (forwarding ou bypassing), que encaminha o resultado diretamente da saída da ULA para a entrada, e com bolhas (stalls). O caso de load-use ainda exige uma bolha mesmo com forwarding. Escalonamento de instruções pelo compilador também ajuda.

> **- De controle:** causados por desvios, porque a instrução seguinte só é conhecida após resolver o desvio. Soluciona-se com predição de desvios (estática, ou dinâmica com tabelas de histórico e preditores de 1 e 2 bits), delayed branch (slot de atraso) e execução especulativa. Uma predição errada obriga a descartar instruções (flush), com penalidade de vários ciclos.


**Evoluções do pipeline:** o **superpipeline** tem estágios mais curtos e clock mais alto. A arquitetura superescalar tem múltiplos pipelines e emite várias instruções por ciclo. A execução fora de ordem (OoO) reordena dinamicamente as instruções para aproveitar unidades ociosas, e a renomeação de registradores elimina dependências de nome. A execução especulativa ajudou o desempenho, mas esteve na raiz das vulnerabilidades Spectre e Meltdown. Os CPIs típicos ficam em torno de 1 para um pipeline ideal escalar e abaixo de 1 para os superescalares.

#### Cache como arquitetura avançada

A cache já foi detalhada na seção de memória, mas, sob a ótica de arquitetura avançada, valem alguns pontos complementares. Os processadores modernos têm **cache multinível:**

> **L1** (dividida em instruções e dados, ~32 a 64 KiB por núcleo, acesso em poucos ciclos);
> **L2** (centenas de KiB a poucos MiB, por núcleo);
> **L3** (compartilhada entre os núcleos, vários MiB). 

Há duas políticas de relação entre níveis: inclusiva (o que está em L1 também está em L2) e exclusiva. Técnicas de otimização incluem **prefetching (busca antecipada),** caches não bloqueantes, victim cache e write buffers. Em sistemas multiprocessados, a coerência (MESI) e o problema do false sharing (núcleos disputando a mesma linha de cache por dados diferentes) são relevantes.

---

## **MACETES E TRUQUES PARA A PROVA**

> **Sobre números e conversões:** Treine as conversões até que sejam automáticas, e use sempre a ponte binária entre octal e hexadecimal. Em questões de faixa, memorize: sem sinal 0 a 2ⁿ − 1; complemento de 2 de −2ⁿ⁻¹ a 2ⁿ⁻¹ − 1; sinal-magnitude e complemento de 1 de −(2ⁿ⁻¹ − 1) a +(2ⁿ⁻¹ − 1). Lembre que só sinal-magnitude e complemento de 1 têm dois zeros. Em questões de quantos bits são necessários para representar N valores, use ⌈log₂ N⌉.

> **Sobre caracteres:** ASCII = 7 bits (128 símbolos); UTF-8 é compatível com ASCII e tem tamanho variável de 1 a 4 bytes; Unicode é o repertório e UTF-8/16/32 são as codificações. “A” = 65, “a” = 97, “0” = 48.

> **Sobre álgebra booleana:** Pratique De Morgan até o automatismo, porque ele aparece em questões de equivalência de expressões. Em simplificações, procure primeiro a absorção e o consenso. Em mapas de Karnaugh, maximize os grupos e use os don’t care.

> **Sobre portas:** NAND e NOR são universais; XOR é a “soma sem transporte” e o detector de diferença; XNOR é a igualdade. Para decorar: NAND só dá 0 quando todas as entradas são 1, e NOR só dá 1 quando todas são 0.

> **Sobre circuitos:** Combinacional não tem memória, e sequencial tem. O decodificador n→2ⁿ e o multiplexador 2ⁿ→1 são sempre cobrados. O flip-flop JK com J=K=1 inverte (toggle), o que o torna a base dos contadores. Um contador módulo N precisa de ⌈log₂ N⌉ flip-flops. Latch é sensível a nível, e flip-flop é sensível a borda.

> **Sobre ponto flutuante:** Decore 1/8/23 (viés 127) e 1/11/52 (viés 1023). Lembre o bit implícito, os valores especiais (expoente todo 0 e todo 1) e que 0,1 não tem representação exata em binário. Se a questão pergunta “por que 0,1 + 0,2 ≠ 0,3”, a resposta é a dízima periódica em binário.

> **Sobre processador e endereçamento:** Conte os acessos à memória por modo de endereçamento. Decore os registradores (PC, IR, MAR, MBR, SP) e a sequência do ciclo de busca. Em RISC × CISC, as palavras-chave são: RISC = formato fixo, load/store, muitos registradores, hardwired, pipeline; CISC = formato variável, muitos modos, microprogramado, poucos registradores.

> **Sobre memória:** SRAM = rápida, cara, cache; DRAM = lenta, barata, precisa de refresh, memória principal. A hierarquia é movida pela localidade (temporal e espacial). Treine o cálculo de tag/índice/offset e o AMAT. Em cache, write-through = sempre escreve na memória; write-back = só ao substituir, com dirty bit.

> **Sobre E/S:** Polling gasta CPU; interrupção libera a CPU até o dispositivo avisar; DMA tira a CPU do caminho dos dados. A resposta correta para grandes blocos de dados é quase sempre DMA.

> **Sobre pipeline:** Pipeline melhora o throughput, e não a latência de uma instrução. Speedup ideal = número de estágios. As três famílias de hazards são estrutural, de dados e de controle, e o forwarding trata os de dados.

> **Estratégia geral:** Em questões de interpretação de enunciado, desconfie de afirmações com “sempre”, “nunca” e “apenas”, porque costumam estar erradas em arquitetura, em que quase tudo é um compromisso (trade-off). Em questões de cálculo, escreva as unidades e converta tudo para a mesma base antes de operar.

