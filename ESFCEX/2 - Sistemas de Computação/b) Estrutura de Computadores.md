
# PARTE B: ESTRUTURA DE COMPUTADORES

### **B.1 Álgebra booleana e lógica binária**

A álgebra booleana, formulada por George Boole e aplicada a circuitos por Claude Shannon, trabalha com variáveis que assumem apenas dois valores (0 e 1) e três operações básicas: **AND** (produto lógico, A·B), OR (soma lógica, A+B) e **NOT** (complemento, A’).

Postulados e teoremas fundamentais:

- Identidade: A + 0 = A; A · 1 = A.
    
- Elemento nulo (dominação): A + 1 = 1; A · 0 = 0.
    
- Idempotência: A + A = A; A · A = A.
    
- Complemento: A + A’ = 1; A · A’ = 0.
    
- Involução (dupla negação): (A’)’ = A.
    
- Comutativa e associativa, para AND e OR.
    
- Distributiva: A·(B+C) = A·B + A·C, e também A + B·C = (A+B)·(A+C). Esta segunda forma não existe na álgebra comum, e a prova gosta dela.
    
- Absorção: A + A·B = A; A·(A+B) = A.
    
- Consenso: A·B + A’·C + B·C = A·B + A’·C.
    
- **Leis de De Morgan:** (A·B)’ = A’ + B’ e (A+B)’ = A’·B’. Em palavras: negar uma conjunção equivale a disjuntar as negações, e vice-versa.
    
- **Princípio da dualidade:** trocando AND por OR e 0 por 1 em um teorema, obtém-se outro teorema válido.
    

Exemplo de simplificação. Simplificar S = A·B + A·B’ + A’·B. Agrupando os dois primeiros termos: A·(B + B’) = A·1 = A. Então S = A + A’·B. Pela segunda distributiva, A + A’·B = (A + A’)·(A + B) = A + B. O resultado é S = A + B.

#### Formas canônicas

Todo circuito combinacional pode ser descrito por uma tabela-verdade, e dela extraem-se as formas canônicas. A soma de produtos (SOP, ou forma de mintermos) é a soma dos produtos que correspondem às linhas em que a saída é 1, escrevendo cada variável direta quando vale 1 e complementada quando vale 0. O produto de somas (POS, forma de maxtermos) é obtido pelas linhas em que a saída é 0, com a lógica invertida. Para a notação, S = Σm(1, 3, 5) indica as linhas de mintermos que valem 1, e S = ΠM(0, 2, 4, 6, 7) indica as de maxtermos que valem 0. Para n variáveis há 2ⁿ mintermos e 2ⁿ maxtermos, e os dois conjuntos são complementares.

#### Mapa de Karnaugh

É uma representação gráfica da tabela-verdade em que as células adjacentes diferem de uma só variável (por isso usa a ordem do código Gray: 00, 01, 11, 10). O método é agrupar uns em blocos retangulares de tamanho potência de 2 (1, 2, 4, 8, 16), os maiores possíveis, e cada grupo gera um termo em que ficam apenas as variáveis que não mudam dentro do grupo. As bordas do mapa são adjacentes entre si (o mapa é “cilíndrico”). As condições de indiferença (don’t care, marcadas com X) podem ser usadas como 0 ou 1 conforme convenha para ampliar grupos. Quanto maior o grupo, menos variáveis restam no termo: um grupo de 2 elimina 1 variável, um de 4 elimina 2, um de 8 elimina 3.

Macete: em um K-map de 4 variáveis, as quatro pontas são adjacentes e formam um grupo de 4. Em provas, quando o enunciado pede “expressão mínima”, procure sempre os maiores grupos e verifique se não deixou nenhum 1 sem cobertura.

### **B.2 Portas lógicas**

Portas lógicas são os blocos elementares dos circuitos digitais, implementadas em hardware com transistores (tecnologias TTL e CMOS).

![[Pasted image 20261110103632.png]]

> - **AND:** saída 1 só se todas as entradas forem 1.

> - **OR**: saída 1 se pelo menos uma entrada for 1.

> - **NOT** (inversor): inverte a entrada.

> - **NAND**: AND seguida de NOT. Saída 0 só se todas as entradas forem 1.

> - **NOR**: OR seguida de NOT. Saída 1 só se todas as entradas forem 0.

> - **XOR** (OU exclusivo): saída 1 se o número de entradas em 1 for ímpar. Para duas entradas, vale 1 quando elas são diferentes. É o “detector de diferença” e o gerador da soma binária.

> - **XNOR** (coincidência): saída 1 quando as entradas são iguais. É o complemento do XOR.


**NAND** e **NOR** são portas universais: qualquer função booleana pode ser construída usando exclusivamente uma delas. Com NAND, um inversor é obtido ligando as duas entradas juntas; um AND é um NAND seguido de um inversor; um OR é obtido invertendo as entradas antes do NAND, por De Morgan (A’ · B’)’ = A + B. A universalidade é importante na prática, porque permite fabricar circuitos inteiros com um único tipo de porta, o que simplifica a produção em CMOS.

Também vale conhecer o atraso de propagação (tempo entre a mudança na entrada e a estabilização da saída), o fan-in (número máximo de entradas de uma porta) e o fan-out (número de portas que uma saída consegue acionar).

### **B.3 Circuitos combinacionais**

Um circuito é **combinacional** quando suas saídas dependem exclusivamente das entradas atuais, sem memória nem realimentação. Os principais, que a prova costuma cobrar, são os seguintes.

> **Meio somador (half adder):** Soma dois bits A e B. A soma é S = A +  B e o transporte de saída é C = A · B.

> **Somador completo (full adder):** Soma três bits (A, B e o transporte de entrada Cin). A soma é S = A ⊕ B ⊕ Cin e o transporte de saída é Cout = A·B + Cin·(A + B). Pode ser construído com dois meios somadores e uma porta OR.

> **Somador de n bits:** O mais simples é o ripple carry, em que o transporte “ondula” de um somador completo para o seguinte. É barato, mas lento, porque o atraso cresce linearmente com n. O carry lookahead (antecipação de transporte) calcula os transportes em paralelo usando os sinais de geração (G = A·B) e propagação (P = A + B) de cada posição: Cᵢ₊₁ = Gᵢ + Pᵢ·Cᵢ. Troca mais portas por menos atraso. A subtração A − B é feita por um somador somando A ao complemento de 2 de B (inverte-se B e faz-se Cin = 1), o que permite um único circuito somador/subtrator.

> **Decodificador:** Tem n entradas e 2ⁿ saídas, e exatamente uma saída fica ativa para cada combinação de entrada. É usado para selecionar endereços de memória e dispositivos. Um decodificador 3×8 é um exemplo clássico. Codificador faz o inverso (2ⁿ entradas e n saídas), e o codificador de prioridade resolve o caso de várias entradas ativas.

> **Multiplexador (MUX):** Seleciona uma de 2ⁿ entradas de dados para a saída, usando n linhas de seleção. Um MUX 4×1 tem 4 entradas de dados e 2 seletoras. Qualquer função de n variáveis pode ser implementada com um MUX de 2ⁿ entradas ou, usando uma variável como dado, com um de 2ⁿ⁻¹. O demultiplexador (DEMUX) faz a operação inversa, distribuindo uma entrada para uma de 2ⁿ saídas.

> **Comparador:** Compara dois números e indica se A > B, A = B ou A < B. A igualdade é um XNOR bit a bit seguido de AND.

> **ULA (Unidade Lógica e Aritmética):** Combina somador, portas lógicas e multiplexadores para realizar operações aritméticas (soma, subtração) e lógicas (AND, OR, XOR, NOT, deslocamentos) conforme sinais de controle, e produz flags de estado (zero, sinal/negativo, transporte, estouro).

> **Circuitos programáveis:** a ROM funciona como um decodificador seguido de uma matriz OR, e por isso implementa qualquer tabela-verdade. PLA e PAL têm planos AND e OR programáveis. Os FPGAs usam tabelas de consulta (look-up tables).

> **Hazards (azares):** São glitches transitórios na saída, causados por diferenças de atraso entre caminhos. Um hazard estático pode ser eliminado acrescentando termos redundantes de consenso no K-map.

### **B.4 Circuitos sequenciais**

Um circuito sequencial tem saídas que dependem das entradas atuais e do estado anterior. Para isso possui memória, obtida por realimentação. Os circuitos sequenciais podem ser assíncronos, em que o estado muda a qualquer mudança de entrada, ou síncronos, em que as mudanças são coordenadas por um sinal de clock, o que torna o projeto muito mais previsível.

#### Latches e flip-flops

O elemento básico é o latch SR (Set-Reset), feito com duas portas NOR (ou NAND) realimentadas. Ele guarda 1 bit. Com S=1 e R=0 a saída vai a 1; com S=0 e R=1 vai a 0; com S=0 e R=0 mantém o estado; e S=1 com R=1 é uma entrada proibida (estado inválido, ou indeterminado na liberação). O latch D elimina o estado proibido, usando um único dado D com uma entrada de habilitação (enable), e é transparente enquanto o enable estiver ativo.

> **O flip-flop** difere do latch porque é sensível à borda do clock (transição de subida ou de descida), e não ao nível. Isso evita que o circuito “veja” mudanças múltiplas durante o período em que o clock está ativo. Os tipos clássicos são:

>  **Flip-flop D:** na borda do clock, Q recebe D. Equação: Q⁺ = D. É o mais usado em registradores e na memória SRAM.

> **Flip-flop T (toggle):** se T=1 inverte o estado, se T=0 mantém. Q⁺ = T + Q. É a base dos contadores.

> **Flip-flop JK:** é o mais versátil. J=0 e K=0 mantém; J=0 e K=1 zera; J=1 e K=0 seta; J=1 e K=1 inverte (resolve o estado proibido do SR). Equação característica: Q⁺ = J·Q’ + K’·Q.

> **Flip-flop SR:** como o latch, mas com clock.


**Macete:** ligando J e K juntos temos um T. Ligando J=D e K=D’ temos um D. Para a prova, memorize as equações características e as tabelas de excitação. Os parâmetros temporais importantes são o tempo de setup (o dado deve estar estável antes da borda do clock), o tempo de hold (o dado deve permanecer estável após a borda) e a metaestabilidade (quando esses tempos são violados e a saída oscila por tempo indeterminado).

#### Registradores e contadores

Um **registrador** é um conjunto de flip-flops D que armazenam uma palavra, e pode ser de carga paralela. O registrador de deslocamento (shift register) transfere bits de um flip-flop para o próximo a cada clock, e serve para conversão serial-paralelo, delays e para multiplicar ou dividir por 2 (deslocamento à esquerda multiplica por 2, e à direita divide por 2).

Os c**ontadores** podem ser **assíncronos (ripple),** em que a saída de cada flip-flop serve de clock do seguinte (simples, mas com atraso acumulado), ou síncronos, em que todos recebem o mesmo clock (mais rápidos e livres de glitches). Um contador módulo-N conta de 0 a N−1 e precisa de ⌈log₂ N⌉ flip-flops. Por exemplo, um contador de década (módulo 10) precisa de 4 flip-flops. Um contador com n flip-flops em binário divide a frequência do clock por 2ⁿ na saída mais significativa.

#### Máquinas de estados finitos

A teoria dos circuitos sequenciais é formalizada nas máquinas de estados finitos (FSM). Na máquina de Moore, a saída depende somente do estado atual. Na máquina de Mealy, a saída depende do estado atual e das entradas, o que costuma exigir menos estados, mas produz saídas que podem mudar assincronamente com a entrada. O projeto passa por: diagrama de estados, tabela de estados, minimização, codificação dos estados, escolha dos flip-flops, e derivação das equações de próximo estado e de saída. Exemplos clássicos são o detector de sequência de bits, o semáforo e a máquina de venda.

**Diferenciação-chave para a prova:** combinacional = sem memória, saída função só das entradas; sequencial = com memória, saída função das entradas e do estado.

