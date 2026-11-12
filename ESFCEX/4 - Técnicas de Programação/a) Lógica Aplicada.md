## PARTE A: LÓGICA APLICADA

### 1. Algoritmo: o conceito central

Um algoritmo é uma sequência finita e ordenada de passos, bem definidos e não ambíguos, que transforma um conjunto de dados de entrada em um conjunto de dados de saída, resolvendo um problema. A receita de bolo é a analogia clássica: ingredientes (entrada), modo de preparo (processamento) e bolo pronto (saída).

Para ser considerado algoritmo, o procedimento precisa ter cinco propriedades:

- **Finitude:** termina após um número finito de passos.
- **Definição (ausência de ambiguidade):** cada passo é preciso.
- **Entrada:** zero ou mais valores fornecidos.
- **Saída:** pelo menos um resultado produzido.
- **Efetividade:** cada passo pode ser executado em tempo finito por uma pessoa ou máquina.

Se um enunciado de prova descrever algo que pode rodar para sempre (um laço sem condição de parada), isso não é algoritmo no sentido estrito.

Um caso real: o algoritmo de Euclides para o máximo divisor comum (MDC), com mais de dois mil anos, continua em uso. Ele repete “troque o par (a, b) por (b, a mod b) até que b seja zero”. Ele termina porque o resto diminui a cada passo, e a saída é o valor de a.

### 2. Técnicas de construção de algoritmos

**Programação estruturada.** O teorema de Böhm-Jacopini (1966) demonstra que qualquer algoritmo computável pode ser escrito com apenas três estruturas de controle: sequência (instruções uma após a outra), seleção (decisão: se/senão, escolha-caso) e repetição (laços). Esse teorema é a base teórica de por que o comando `goto` foi considerado desnecessário e prejudicial, ideia popularizada por Dijkstra.

**Refinamentos sucessivos (top-down).** Parte-se do problema geral e o decompõe em subproblemas cada vez menores, até que cada um seja simples o bastante para ser codificado. Exemplo: “emitir folha de pagamento” se decompõe em ler funcionários, calcular salário bruto, calcular descontos e imprimir contracheque. Cada item se decompõe de novo.

**Abordagem bottom-up.** É o caminho inverso: constroem-se primeiro componentes pequenos e reutilizáveis, que depois são combinados. Na prática, o desenvolvimento real mistura as duas.

**Modularização.** Divide-se o programa em sub-rotinas (funções e procedimentos). Função retorna valor, procedimento apenas executa ações. Isso traz reuso, legibilidade e facilidade de teste. Cabe distinguir parâmetros por valor (a sub-rotina recebe uma cópia, e alterações não afetam o original) de parâmetros por referência (a sub-rotina recebe o endereço, e alterações afetam o original). Distingue-se também variável local (existe só dentro da sub-rotina) de global (visível em todo o programa).

**Paradigmas de projeto de algoritmos**, que também aparecem em questões:

- **Força bruta:** testa todas as possibilidades. É simples e correta, mas costuma ser lenta.
- **Divisão e conquista:** divide o problema em partes menores independentes, resolve cada uma e combina os resultados. Exemplos: merge sort, quick sort, busca binária.
- **Algoritmos gulosos (greedy):** a cada passo escolhem a melhor opção local, na esperança de chegar ao ótimo global. Exemplos: Dijkstra, Kruskal, Prim, troco com moedas em sistemas “bem comportados”.
- **Programação dinâmica:** resolve subproblemas sobrepostos uma única vez e guarda os resultados (memoização). Exemplos: Fibonacci com cache, mochila, menor caminho entre todos os pares (Floyd-Warshall).
- **Backtracking (retrocesso):** constrói a solução passo a passo e desfaz escolhas que levam a becos sem saída. Exemplos: oito rainhas, sudoku, labirinto.

### 3. Resolução de problemas

O método mais citado é o de George Pólya, com quatro etapas: compreender o problema (o que se pede, quais dados existem, quais restrições), elaborar um plano, executar o plano e fazer o retrospecto (verificar se funciona e se poderia ser melhor).

No contexto de algoritmos, isso vira a sequência prática: identificar entradas, saídas e o processamento que liga uma às outras; escolher a estrutura de dados adequada; escrever o algoritmo; fazer o **teste de mesa** (simulação manual, acompanhando o valor das variáveis passo a passo); e testar com casos extremos, como entrada vazia, valores negativos, zero e um único elemento.

Um conceito que as bancas gostam é o de **invariante de laço**: uma propriedade que vale antes e depois de cada iteração e, ao término do laço, garante a correção do resultado.

### 4. Elementos básicos de um algoritmo

**Variáveis e constantes** são posições de memória nomeadas. Os tipos primitivos usuais são inteiro, real, caractere, cadeia (literal) e lógico.

**Operadores:**

- aritméticos (`+`, `-`, `*`, `/`, divisão inteira `div`, resto `mod`);
- relacionais (`=`, `<>`, `<`, `>`, `<=`, `>=`);
- lógicos (`e`, `ou`, `não`).

**Precedência** (da maior para a menor): parênteses, `não`, multiplicação/divisão/mod, soma/subtração, relacionais, `e`, `ou`.

**Tabela-verdade:**

- `e` só é verdadeiro se ambos forem verdadeiros;
- `ou` só é falso se ambos forem falsos;
- `não` inverte o valor.

Vale lembrar a **avaliação em curto-circuito** (em muitas linguagens, `A e B` nem avalia B se A for falso) e as **leis de De Morgan**: `não (A e B) = (não A) ou (não B)` e `não (A ou B) = (não A) e (não B)`.

### 5. Representação por fluxograma

O fluxograma é a representação gráfica do algoritmo, com símbolos padronizados ligados por setas que indicam o fluxo. Os símbolos principais:

- **Terminal (elipse ou retângulo arredondado):** início e fim.
- **Processamento (retângulo):** atribuições e cálculos.
- **Decisão (losango):** teste lógico com duas saídas, verdadeiro e falso.
- **Entrada/saída:** leitura e escrita de dados.
- **Conector (círculo):** liga partes do fluxograma, em especial entre páginas.
- **Sub-rotina (retângulo com barras laterais):** chamada de módulo.

Regras de boa prática: um único ponto de início, setas sempre indicando o sentido, losango com exatamente duas saídas rotuladas e fluxo geral de cima para baixo. O fluxograma deixa muito visível a diferença entre estruturas de repetição: na repetição com teste no início, a seta volta ao losango antes do processamento, e na com teste no final, o processamento vem antes do losango.

### 6. Representação por pseudolinguagem (Portugol)

A pseudolinguagem descreve o algoritmo em texto, num meio-termo entre a língua natural e uma linguagem de programação. Exemplo completo, que calcula a média e decide a situação do aluno:

```
algoritmo "situacao"
var
   n1, n2, media : real
inicio
   leia(n1, n2)
   media <- (n1 + n2) / 2
   se media >= 7 entao
      escreva("Aprovado")
   senao
      se media >= 5 entao
         escreva("Recuperacao")
      senao
         escreva("Reprovado")
      fimse
   fimse
fimalgoritmo
```

As estruturas de repetição:

```
// teste no início: pode executar zero vezes
enquanto condicao faca
   comandos
fimenquanto

// teste no final: executa pelo menos uma vez; repete até a condição ficar VERDADEIRA
repita
   comandos
ate condicao

// número de repetições conhecido
para i de 1 ate 10 passo 1 faca
   comandos
fimpara
```

**Macete clássico:** no `enquanto`, o laço roda _enquanto a condição é verdadeira_. No `repita...ate`, ele roda _até a condição se tornar verdadeira_, ou seja, enquanto for falsa. Muita questão de prova explora essa inversão. Além disso, o `repita` sempre executa ao menos uma vez.

Padrões que você deve saber de cabeça:

- **Contador:** `c <- c + 1`.
- **Acumulador:** `soma <- soma + x`.
- **Maior elemento:** inicializa-se `maior` com o primeiro elemento, não com zero (o zero falha se todos os valores forem negativos).
- **Troca de dois valores:** exige variável auxiliar (`aux <- a; a <- b; b <- aux`).
- **Número par:** `n mod 2 = 0`.

**Erro frequente:** o “off-by-one”, que é errar o limite do laço em uma unidade. Com um vetor de índices 0 a n-1, o laço `para i de 0 ate n` acessa uma posição inexistente.