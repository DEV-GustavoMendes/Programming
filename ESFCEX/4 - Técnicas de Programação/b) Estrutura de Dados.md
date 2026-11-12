## PARTE B: ESTRUTURAS DE DADOS

### 1. Conceitos fundamentais

Uma **estrutura de dados** é uma forma de organizar dados na memória para que operações como inserir, buscar e remover sejam eficientes. Um **tipo abstrato de dados (TAD)** é um modelo definido pelo seu comportamento (o conjunto de valores e as operações permitidas), sem especificar como é implementado. A pilha é um TAD: define-se que existem `push` e `pop`, e a implementação pode ser em vetor ou em lista encadeada. O princípio por trás disso é o **encapsulamento**: o usuário do TAD enxerga a interface e ignora a implementação.

Classificações importantes:

- **Homogênea** (todos os elementos do mesmo tipo, como o vetor) versus **heterogênea** (tipos diferentes, como registros/structs).
- **Linear** (elementos em sequência: vetores, listas, pilhas, filas) versus **não linear** (árvores, grafos).
- **Estática** (tamanho fixo em tempo de compilação) versus **dinâmica** (cresce e encolhe em tempo de execução, com alocação sob demanda no heap).
- **Alocação sequencial** (posições contíguas na memória) versus **encadeada** (nós espalhados, ligados por ponteiros).

### 2. Vetores

O vetor (array) é uma coleção de elementos do mesmo tipo em posições contíguas de memória, acessados por índice. O endereço do elemento `i` é `base + i × tamanho_do_elemento`, e é por isso que o acesso é em tempo constante, O(1).

Operações e custos: acesso por índice O(1); busca sequencial O(n); busca binária O(log n) se estiver ordenado; inserção no meio ou no início O(n), pois exige deslocar elementos; inserção no final O(1) se houver espaço; remoção no meio O(n).

Vantagens: acesso rápido e boa localidade de cache. Desvantagens: tamanho fixo (no caso estático) e custo de inserção/remoção. Os vetores dinâmicos (como `ArrayList` ou `vector`) dobram a capacidade quando enchem, o que dá custo **amortizado** O(1) por inserção.

### 3. Matrizes

A matriz é um vetor de duas ou mais dimensões. Na memória ela é linearizada de duas formas: **ordem por linhas (row-major)**, usada em C, Java e Python, e **ordem por colunas (column-major)**, usada em Fortran. Em row-major, o endereço de `M[i][j]` em uma matriz com `C` colunas é `base + (i × C + j) × tamanho`. Percorrer por linhas é mais eficiente que por colunas por causa do cache.

Casos especiais cobrados em prova: **matriz esparsa** (maioria dos elementos zero, armazenada só com os não nulos, em listas ou tuplas linha-coluna-valor), **matriz triangular** (a metade abaixo ou acima da diagonal é zero, podendo ser armazenada com n(n+1)/2 elementos) e **matriz simétrica**. Uma matriz n × m tem n·m elementos, e o produto de matrizes comuns custa O(n³). Para somar a diagonal principal, percorre-se `M[i][i]`. Na diagonal secundária de uma matriz n × n, vale `i + j = n - 1` (índices a partir de 0).

### 4. Cadeias de caracteres (strings)

A cadeia é uma sequência de caracteres. Em C, é um vetor de `char` terminado pelo caractere nulo `'\0'`, de modo que a string “OLA” ocupa 4 bytes. Em Java, Python e C#, as strings são **imutáveis**: toda “modificação” cria um novo objeto, e por isso concatenar dentro de um laço é ineficiente (usa-se `StringBuilder`).

Operações usuais: comprimento, concatenação, comparação (lexicográfica, baseada na tabela ASCII/Unicode), extração de subcadeia, busca de padrão, inversão e verificação de palíndromo. Na busca de padrão, o algoritmo ingênuo custa O(n·m), e algoritmos como KMP e Boyer-Moore reduzem isso. Lembre que em ASCII as maiúsculas vêm antes das minúsculas (‘A’ = 65, ‘a’ = 97), o que afeta ordenações e comparações.

### 5. Listas lineares

A lista linear é uma sequência de elementos com ordem definida, em que é possível inserir e remover em qualquer posição. Há duas implementações.

**Lista sequencial (em vetor):** acesso direto, mas inserção e remoção exigem deslocamentos.

**Lista encadeada:** cada nó guarda o dado e um ponteiro para o próximo. O acesso a uma posição exige percorrer a lista, custando O(n), mas inserir ou remover, uma vez localizada a posição, custa O(1), pois só se alteram ponteiros. As variações:

- **Simplesmente encadeada:** cada nó aponta para o seguinte.
- **Duplamente encadeada:** cada nó tem ponteiros para o anterior e o próximo, permitindo percorrer nos dois sentidos e remover um nó conhecido em O(1), ao custo de mais memória.
- **Circular:** o último nó aponta para o primeiro.
- **Com nó cabeça (sentinela):** um nó fictício no início simplifica o código ao eliminar casos especiais.

Inserir no início de uma lista simples é O(1); inserir no fim é O(n), a não ser que se mantenha um ponteiro para o último nó. Um erro clássico de implementação é perder a referência ao restante da lista: ao inserir, primeiro faça o novo nó apontar para o sucessor e só depois ligue o antecessor ao novo nó.

### 6. Pilhas

A pilha segue o princípio **LIFO** (Last In, First Out): o último a entrar é o primeiro a sair, como uma pilha de pratos. As operações são `push` (empilhar), `pop` (desempilhar) e `top`/`peek` (consultar o topo), todas O(1). Desempilhar de pilha vazia gera **underflow**, e empilhar em pilha cheia (implementação em vetor) gera **overflow**.

Aplicações reais e muito cobradas:

- **Pilha de chamadas:** é como o processador controla funções e recursão, guardando endereços de retorno e variáveis locais.
- **Desfazer/refazer** em editores e **histórico de navegação**.
- **Verificação de balanceamento** de parênteses, colchetes e chaves.
- **Conversão e avaliação de expressões** (notação infixa, pré-fixa e pós-fixa).
- **Percurso em profundidade** em grafos.

Exemplo de avaliação pós-fixa de `3 4 + 2 *`: empilha 3, empilha 4, ao ver `+` desempilha 4 e 3 e empilha 7, empilha 2, ao ver `*` desempilha 2 e 7 e empilha 14. O resultado é 14, equivalente a `(3 + 4) * 2`. Na notação pós-fixa não há parênteses nem necessidade de regras de precedência.

### 7. Filas

A fila segue o princípio **FIFO** (First In, First Out). As operações são `enqueue` (inserir no final) e `dequeue` (remover do início), ambas O(1) numa implementação adequada. Exemplos reais: fila de impressão, escalonamento de processos, buffers de teclado e rede, e o percurso em largura de grafos.

Variações importantes:

- **Fila circular:** em implementação por vetor, os índices dão a volta (`fim = (fim + 1) mod n`), reaproveitando espaços liberados. Sem isso, uma fila em vetor “anda” para a direita e desperdiça posições. Para distinguir fila cheia de vazia, deixa-se uma posição livre ou mantém-se um contador.
- **Deque (double-ended queue):** inserção e remoção nas duas extremidades.
- **Fila de prioridade:** sai primeiro o elemento de maior prioridade, e não o mais antigo. É tipicamente implementada com **heap** (inserir e remover custam O(log n)).

### 8. Árvores

A árvore é uma estrutura hierárquica não linear. Terminologia indispensável:

- **Raiz:** nó sem pai.
- **Folha:** nó sem filhos.
- **Grau de um nó:** número de filhos.
- **Nível:** a raiz está no nível 0 (ou 1, conforme o autor, e a prova costuma esclarecer).
- **Altura da árvore:** comprimento do maior caminho da raiz até uma folha.
- **Subárvore:** um nó e todos os seus descendentes.

**Árvore binária:** cada nó tem no máximo dois filhos (esquerdo e direito). Uma árvore binária de altura h (contando arestas) tem no máximo 2^(h+1) − 1 nós e no máximo 2^h folhas. Chama-se **cheia** quando todo nó tem 0 ou 2 filhos, **completa** quando todos os níveis estão preenchidos, exceto talvez o último, preenchido da esquerda para a direita, e **perfeita** quando todas as folhas estão no mesmo nível e todos os nós internos têm dois filhos.

**Percursos (a pergunta mais clássica da prova):**

- **Pré-ordem:** raiz, esquerda, direita.
- **Em ordem (in-order):** esquerda, raiz, direita.
- **Pós-ordem:** esquerda, direita, raiz.
- **Em nível (largura):** nível por nível, usando uma fila.

Exemplo: numa árvore com raiz 8, filhos 3 e 10, e 3 com filhos 1 e 6, o percurso em ordem dá 1, 3, 6, 8, 10; o pré-ordem dá 8, 3, 1, 6, 10; e o pós-ordem dá 1, 6, 3, 10, 8.

**Árvore binária de busca (ABB/BST):** para todo nó, os valores da subárvore esquerda são menores e os da direita são maiores. **O percurso em ordem de uma BST devolve os elementos em ordem crescente.** Busca, inserção e remoção custam O(h): O(log n) se a árvore for equilibrada, mas O(n) no pior caso, quando degenera numa lista (por exemplo, ao inserir elementos já ordenados). A remoção tem três casos: nó folha (apenas remove), nó com um filho (o filho toma o lugar) e nó com dois filhos (substitui-se pelo sucessor em ordem, o menor da subárvore direita, ou pelo antecessor, o maior da esquerda, e remove-se este).

**Árvores balanceadas:** a **AVL** exige que, em todo nó, a diferença de altura entre as subárvores seja no máximo 1 (fator de balanceamento em {−1, 0, 1}), e restaura o equilíbrio por rotações: simples à esquerda, simples à direita, dupla esquerda-direita e dupla direita-esquerda. Garante O(log n) no pior caso. A **rubro-negra** é outra árvore balanceada, com regras de cores, usada em bibliotecas padrão (como o `TreeMap` do Java). A **árvore B** tem muitos filhos por nó e é muito usada em bancos de dados e sistemas de arquivos, pois reduz acessos a disco.

**Heap:** árvore binária completa em que cada pai é maior ou igual aos filhos (max-heap) ou menor ou igual (min-heap). Costuma ser armazenada em vetor: para um nó na posição `i` (a partir de 0), os filhos estão em `2i+1` e `2i+2`, e o pai em `(i−1)/2`. É a base do heapsort e da fila de prioridade.

### 9. Grafos

O grafo G = (V, E) é um conjunto de vértices e de arestas que os ligam. Pode ser **não dirigido** (aresta sem sentido) ou **dirigido/dígrafo** (aresta com sentido), **ponderado** (arestas com pesos) ou não, **simples** ou com laços e arestas paralelas (multigrafo). Conceitos-chave:

- **Grau** de um vértice: número de arestas incidentes.
- **Caminho:** sequência de vértices adjacentes.
- **Ciclo:** caminho que volta ao vértice inicial.
- **Grafo conexo:** existe caminho entre todo par de vértices.
- **Árvore:** grafo conexo e acíclico, com exatamente n − 1 arestas.
- **Grafo completo:** todo par de vértices é adjacente.

Duas propriedades que caem muito: em grafo não dirigido, a soma dos graus é igual a 2|E|; e um grafo simples não dirigido com n vértices tem no máximo n(n − 1)/2 arestas.

**Representações:**

- **Matriz de adjacência:** matriz n × n, em que a posição [i][j] indica a existência (ou o peso) da aresta. Testar uma aresta é O(1), mas ocupa O(n²) de memória, o que é desperdício em grafos esparsos.
- **Lista de adjacência:** para cada vértice, uma lista dos seus vizinhos. Ocupa O(V + E) e é ideal para grafos esparsos.

**Algoritmos fundamentais:**

- **Busca em largura (BFS):** explora por camadas usando uma **fila**. Encontra o menor caminho em número de arestas em grafos não ponderados. Custo O(V + E).
- **Busca em profundidade (DFS):** avança o máximo possível antes de retroceder, usando **pilha** ou recursão. Usada para detectar ciclos, achar componentes conexos e fazer ordenação topológica. Custo O(V + E).
- **Dijkstra:** menor caminho a partir de uma origem em grafo com pesos **não negativos** (algoritmo guloso). Com fila de prioridade (heap), custa O((V + E) log V).
- **Bellman-Ford:** aceita pesos negativos e detecta ciclos negativos, custando O(V·E).
- **Floyd-Warshall:** menores caminhos entre todos os pares, custando O(V³), por programação dinâmica.
- **Árvore geradora mínima:** subárvore que conecta todos os vértices com peso total mínimo. **Prim** cresce a partir de um vértice e **Kruskal** ordena as arestas e adiciona as mais baratas que não formem ciclo.
- **Ordenação topológica:** só vale para grafos dirigidos acíclicos (DAG), e é usada, por exemplo, para ordenar tarefas com dependências.

**Macete:** BFS usa fila, DFS usa pilha. Dijkstra falha com pesos negativos. Ordenação topológica exige ausência de ciclos.

### 10. Pesquisa (busca) de dados

**Busca sequencial:** percorre elemento a elemento. É O(n) e funciona em qualquer estrutura, ordenada ou não. Uma otimização é a busca com **sentinela**, que coloca o valor buscado no final para evitar testar o limite a cada passo.

**Busca binária:** exige vetor **ordenado** e acesso direto por índice. Compara o elemento central, descarta metade e repete. Custa O(log n): para um milhão de elementos, são cerca de 20 comparações. Em lista encadeada ela perde a vantagem, por falta de acesso direto.

**Hashing (tabela de dispersão):** uma **função hash** converte a chave em um índice do vetor, dando busca, inserção e remoção em O(1) em média. Quando duas chaves caem no mesmo índice, ocorre **colisão**, tratada por:

- **Encadeamento (separate chaining):** cada posição guarda uma lista dos elementos que colidiram.
- **Endereçamento aberto:** procura outra posição livre por sondagem linear, quadrática ou duplo hash.

O **fator de carga** (n elementos ÷ m posições) controla o desempenho: quando fica alto, a tabela é redimensionada (rehash). No pior caso, com muitas colisões, a busca degenera para O(n). Uma boa função hash distribui uniformemente as chaves. A tabela hash não mantém ordem entre as chaves.

### 11. Classificação (ordenação) de dados

|Algoritmo|Melhor caso|Caso médio|Pior caso|Estável|Espaço extra|
|---|---|---|---|---|---|
|Bubble sort|O(n) (com flag)|O(n²)|O(n²)|Sim|O(1)|
|Selection sort|O(n²)|O(n²)|O(n²)|Não|O(1)|
|Insertion sort|O(n)|O(n²)|O(n²)|Sim|O(1)|
|Merge sort|O(n log n)|O(n log n)|O(n log n)|Sim|O(n)|
|Quick sort|O(n log n)|O(n log n)|O(n²)|Não|O(log n)|
|Heap sort|O(n log n)|O(n log n)|O(n log n)|Não|O(1)|
|Counting sort|O(n + k)|O(n + k)|O(n + k)|Sim|O(k)|

Como funciona cada um, em essência:

- **Bubble sort:** compara pares adjacentes e os troca, “borbulhando” o maior para o fim a cada passada.
- **Selection sort:** seleciona o menor elemento do trecho não ordenado e o coloca na posição certa. Faz poucas trocas (no máximo n − 1).
- **Insertion sort:** insere cada elemento no lugar correto do trecho já ordenado, como ao organizar cartas na mão. É excelente para vetores pequenos ou quase ordenados.
- **Merge sort:** divide ao meio, ordena cada metade recursivamente e intercala. É estável e previsível, mas usa memória adicional.
- **Quick sort:** escolhe um pivô, particiona (menores à esquerda, maiores à direita) e ordena recursivamente. É geralmente o mais rápido na prática, mas o pior caso O(n²) ocorre com pivô mal escolhido (por exemplo, o primeiro elemento num vetor já ordenado).
- **Heap sort:** constrói um max-heap e extrai repetidamente o maior.
- **Counting sort:** não compara elementos; conta ocorrências. Funciona para inteiros em faixa limitada.

Duas definições que aparecem em enunciados: um algoritmo é **estável** se preserva a ordem relativa de elementos com chaves iguais, e é **in-place** (ou “no local”) se usa memória auxiliar constante. Também vale saber que **nenhum algoritmo de ordenação baseado em comparações pode ser melhor que O(n log n) no pior caso**.

### 12. Recursividade

Uma função é recursiva quando chama a si mesma. Toda solução recursiva correta precisa de **caso base** (condição que encerra a recursão) e **caso recursivo** (que reduz o problema em direção ao caso base). Sem caso base, ou com um que nunca é alcançado, a pilha de chamadas se esgota e ocorre **estouro de pilha (stack overflow)**.

Exemplo clássico:

```
funcao fatorial(n : inteiro) : inteiro
inicio
   se n <= 1 entao
      retorne 1
   senao
      retorne n * fatorial(n - 1)
   fimse
fimfuncao
```

O cálculo de fatorial(4) empilha as chamadas até fatorial(1) e depois desempilha multiplicando: 4 × 3 × 2 × 1 = 24.

Outros pontos que caem:

- **Fibonacci recursivo ingênuo** tem custo exponencial, aproximadamente O(2ⁿ), pois recalcula os mesmos subproblemas. Com memoização cai para O(n).
- **Torre de Hanói** com n discos exige 2ⁿ − 1 movimentos.
- **Recursão de cauda (tail recursion):** a chamada recursiva é a última operação, o que permite a alguns compiladores transformá-la em iteração e economizar pilha.
- **Recursão direta** (a função chama a si mesma) versus **indireta** (A chama B, que chama A).
- Todo algoritmo recursivo pode ser convertido em iterativo, usando uma pilha explícita quando necessário. A recursão costuma ser mais legível (árvores, divisão e conquista), enquanto a iteração costuma ser mais eficiente em memória.

### 13. Eficiência e complexidade de algoritmos

Complexidade mede quanto de **tempo** (número de operações) e de **espaço** (memória) um algoritmo consome em função do tamanho da entrada n. A análise é assintótica: interessa o comportamento para n grande, ignorando constantes e termos de menor ordem.

**Notações:**

- **O (big-O):** limite superior (o algoritmo não é pior que isso). É a mais usada, associada ao pior caso.
- **Ω (ômega):** limite inferior.
- **Θ (theta):** limite justo, quando limites superior e inferior coincidem.

Não confunda notação com caso: pode-se falar de O do melhor caso, e Ω do pior caso. Em prova, porém, “complexidade” sem qualificação costuma significar o pior caso em O.

**Ordem de crescimento, do mais lento ao mais rápido:**

O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)

**Como calcular:**

- Sequência de comandos soma os custos, prevalecendo o maior.
- Laço simples de 1 a n custa O(n).
- Laços aninhados multiplicam: dois laços de n custam O(n²).
- Laço em que a variável é dividida ou multiplicada por constante a cada passo custa O(log n).
- Recursões do tipo “divide em duas metades e faz trabalho linear” (merge sort) resolvem-se como T(n) = 2T(n/2) + n = O(n log n). Quando a cada chamada o problema reduz pela metade com trabalho constante (busca binária), T(n) = T(n/2) + 1 = O(log n).

Constantes são ignoradas: 5n² + 100n + 7 é O(n²). A velocidade só importa assintoticamente: para n pequeno, um algoritmo O(n²) simples pode vencer um O(n log n) complexo.

**Complexidade das estruturas, resumo:**

|Estrutura|Acesso|Busca|Inserção|Remoção|
|---|---|---|---|---|
|Vetor|O(1)|O(n)|O(n)|O(n)|
|Lista encadeada|O(n)|O(n)|O(1)*|O(1)*|
|Pilha / Fila|n/a|O(n)|O(1)|O(1)|
|BST equilibrada|n/a|O(log n)|O(log n)|O(log n)|
|Tabela hash|n/a|O(1) médio|O(1) médio|O(1) médio|

* Quando a posição já é conhecida.

Por fim, as classes **P** (problemas resolvíveis em tempo polinomial) e **NP** (soluções verificáveis em tempo polinomial) são vistas em nível conceitual: P ⊆ NP, e se P = NP é problema em aberto. Exemplos de problemas NP-completos: caixeiro viajante (versão de decisão), SAT, mochila.

---

## MACETES E TRUQUES PARA A PROVA

1. **Pilha = LIFO, fila = FIFO.** Parece óbvio, mas a banca costuma trocar nos enunciados e esconder o nome.
2. **Em ordem em BST dá a sequência crescente.** Use isso para montar a árvore a partir do percurso ou para validar uma alternativa.
3. **Para reconstruir uma árvore:** pré-ordem ou pós-ordem sozinhos não bastam; combine com o em ordem. O primeiro elemento do pré-ordem é sempre a raiz, e o último do pós-ordem também.
4. **BFS usa fila, DFS usa pilha.**
5. **Busca binária exige ordenação e acesso direto.**
6. **Quick sort: pior caso O(n²); merge sort: sempre O(n log n), mas gasta memória.** Heap sort é O(n log n) sempre, in-place, mas não estável.
7. **Teste de mesa**: em questão de “o que este trecho imprime”, não tente adivinhar, simule com uma tabela de variáveis.
8. **`repita...ate` executa pelo menos uma vez** e para quando a condição fica verdadeira.
9. **Em questões de complexidade com laços aninhados**, verifique se o limite do laço interno depende do externo (`para j de 1 ate i` soma n(n+1)/2, ainda O(n²)).
10. **Atenção a “sempre”, “nunca” e “somente”** em alternativas: afirmações absolutas costumam estar erradas, por exemplo “a busca binária sempre é melhor que a sequencial” (não vale em vetor de poucos elementos ou não ordenado).
11. **Desconfie de afirmações sobre o pior caso de tabela hash**: em média O(1), mas no pior caso O(n).
12. **Numeração de nível e altura**: leia a convenção do enunciado (raiz no nível 0 ou 1, altura em nós ou em arestas) antes de aplicar as fórmulas.

---

## ANÁLISE DO QUE MAIS CAI (2020 A 2026)

Preciso ser transparente sobre os limites desta parte. Pelas buscas que fiz, consegui confirmar que existem cadernos públicos de 60 questões objetivas na área de Informática do Curso de Formação de Oficiais do Quadro Complementar, na edição de 2022, e que as provas de 2021, 2025 e 2026 aparecem catalogadas como provas da banca VUNESP para a ESFCEx. As provas completas estão no site da ESFCEx e em plataformas como o Qconcursos, mas **não consegui abrir e contar questão por questão**, então não tenho uma estatística verificada por tema e por ano. Inventar percentuais seria desonesto e poderia desviar seu estudo. [ESEX2202 008 CFOQCO Informatica 016 1 +3](https://esfcex.eb.mil.br/images/concurso/2022/ESEX2202_008_CFOQCO-Informatica_016_1.pdf)

O que posso afirmar com segurança é a tendência geral de provas de Informática de concursos militares com esse formato:

- **Estruturas de dados e algoritmos costumam estar entre os temas recorrentes**, ao lado de banco de dados, redes, engenharia de software, segurança da informação e sistemas operacionais (que você já começou a estudar com Linux).
- Dentro do seu recorte, o que historicamente mais aparece em provas de banca VUNESP e similares é: **pilhas e filas** (principalmente identificar o princípio LIFO/FIFO e simular operações), **percursos em árvores binárias**, **complexidade de algoritmos de ordenação e busca**, **recursividade** (simulação do que a função retorna) e **interpretação de trechos de pseudocódigo**.
- Grafos aparecem, mas em geral com menos frequência e em nível conceitual (BFS/DFS, representação por matriz ou lista, Dijkstra).

Para transformar essa tendência em dado real, faça assim: no Qconcursos, filtre por órgão “EsFCEx”, banca “VUNESP” e disciplina de Informática/Programação, e conte as questões por assunto nas provas de 2020 a 2026. Leva cerca de uma hora e dá uma estatística confiável. Se você me enviar as provas em PDF, ou colar as questões, eu faço a classificação e a contagem por tema com você, e também posso montar uma lista de exercícios nos moldes da banca.