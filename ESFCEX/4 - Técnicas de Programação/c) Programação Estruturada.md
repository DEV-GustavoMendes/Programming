## PARTE C: PROGRAMAÇÃO ESTRUTURADA (LINGUAGEM C)

### 1. O que é programação estruturada

Programação estruturada é um paradigma que organiza o código usando apenas três formas de controle (sequência, seleção e repetição), subdividindo o programa em blocos e módulos com entrada e saída bem definidas. Ela surgiu nos anos 1960 como reação ao chamado “código espaguete”, cheio de desvios `goto` que tornavam impossível acompanhar o fluxo de execução. Dois marcos teóricos sustentam o paradigma: o teorema de Böhm-Jacopini (1966), que prova que qualquer algoritmo computável pode ser escrito com sequência, seleção e repetição, e a carta de Dijkstra, “Go To Statement Considered Harmful” (1968), que popularizou a crítica ao `goto`.

Os princípios que decorrem disso são: cada estrutura tem **um único ponto de entrada e um único ponto de saída**, o programa é decomposto em partes menores e testáveis, e a leitura do código de cima para baixo reflete a ordem de execução. C é uma linguagem procedural e estruturada por excelência, criada por Dennis Ritchie por volta de 1972 nos laboratórios Bell, e é a base de sistemas como Unix e Linux.

### 2. Refinamentos sucessivos

Refinamento sucessivo (ou desenvolvimento top-down, proposto por Niklaus Wirth em 1971) consiste em partir de uma descrição geral do problema e detalhá-la em etapas, cada uma mais concreta que a anterior, até chegar a comandos da linguagem. Em cada passo, uma tarefa abstrata é substituída por uma sequência de tarefas menores, e o processo para quando cada tarefa pode ser escrita diretamente.

Exemplo: “calcular a média de uma turma e listar os aprovados”. No primeiro nível ficam três tarefas: ler as notas, calcular a média e mostrar os aprovados. No segundo nível, “ler as notas” se refina em “repetir n vezes: ler uma nota e guardá-la no vetor”. “Calcular a média” vira “somar todos os elementos e dividir por n”. Cada uma dessas tarefas, no fim, se torna uma função em C, como `lerNotas()`, `calcularMedia()` e `listarAprovados()`. Por isso o refinamento sucessivo conduz naturalmente à programação modular. As vantagens são o controle de complexidade (resolve-se um problema pequeno por vez), a possibilidade de dividir o trabalho em equipe, a facilidade de teste e a documentação natural da lógica.

### 3. Estruturas em blocos

Bloco é um conjunto de comandos tratado como uma unidade. Em C, o bloco é delimitado por chaves `{ }`. O corpo de uma função, de um `if`, de um `while` e de um `for` são blocos. Pontos importantes:

- Variáveis declaradas dentro de um bloco têm **escopo de bloco**: só existem ali, e são destruídas quando o bloco termina (no caso de variáveis automáticas).
- Blocos podem ser aninhados, e uma variável interna com o mesmo nome de uma externa a **oculta** (shadowing) dentro do bloco.
- Um `if` ou laço sem chaves aplica-se apenas ao **próximo comando**, e isso é fonte clássica de erro:

c

```c
if (x > 0)
    printf("positivo\n");
    printf("fim\n");   // esta linha executa SEMPRE, a indentação engana
```

Boa prática: usar sempre chaves, mesmo para um único comando. Há ainda a distinção entre **comando simples** (termina em `;`, como `x = 5;`) e **comando composto** (o bloco entre chaves, que não termina com `;`).

### 4. Estruturas de controle de fluxo em C

#### 4.1 Sequência

Os comandos executam na ordem em que aparecem. Um programa mínimo:

c

```c
#include <stdio.h>

int main(void) {
    int a, b, soma;
    printf("Digite dois numeros: ");
    scanf("%d %d", &a, &b);
    soma = a + b;
    printf("Soma = %d\n", soma);
    return 0;
}
```

Observe o `&` no `scanf`: ele passa o **endereço** da variável, para que a função possa escrever nela. É um erro frequente esquecê-lo.

#### 4.2 Seleção

**`if` e `if-else`:**

c

```c
if (media >= 7.0) {
    printf("Aprovado\n");
} else if (media >= 5.0) {
    printf("Recuperacao\n");
} else {
    printf("Reprovado\n");
}
```

Em C, **qualquer valor diferente de zero é verdadeiro e zero é falso** (o tipo `bool` só existe com `<stdbool.h>`, a partir do C99). Daí o erro clássico:

c

```c
if (x = 5)   // ATRIBUI 5 a x e o resultado (5) é verdadeiro: sempre entra!
if (x == 5)  // comparação, o que se queria
```

**Operador condicional (ternário):** `maior = (a > b) ? a : b;`

**`switch-case`:** seleção por múltiplos valores **inteiros ou caracteres** (não aceita `float` nem strings):

c

```c
switch (opcao) {
    case 1:
        printf("Cadastrar\n");
        break;
    case 2:
        printf("Listar\n");
        break;
    default:
        printf("Invalida\n");
}
```

**Pegadinha clássica:** sem `break`, a execução “cai” para o `case` seguinte (fall-through). Questões de prova adoram um `switch` sem `break` e perguntam o que será impresso.

#### 4.3 Repetição

**`while` (teste no início, executa zero ou mais vezes):**

c

```c
int i = 1;
while (i <= 5) {
    printf("%d ", i);
    i++;
}
```

**`do-while` (teste no final, executa ao menos uma vez; repete enquanto a condição for VERDADEIRA):**

c

```c
int n;
do {
    printf("Digite um valor positivo: ");
    scanf("%d", &n);
} while (n <= 0);
```

Atenção: diferente do `repita...até` do pseudocódigo, o `do-while` de C repete enquanto a condição é verdadeira. Terminar o `do-while` com `;` é obrigatório.

**`for` (número de repetições conhecido):**

c

```c
for (int i = 0; i < 5; i++) {
    printf("%d ", i);
}
```

O `for` tem três partes: inicialização, condição e incremento, e todas são opcionais (`for(;;)` é um laço infinito). A variável declarada no cabeçalho só é permitida a partir do C99.

#### 4.4 Desvios controlados

- **`break`:** encerra imediatamente o laço ou `switch` mais interno.
- **`continue`:** pula o restante da iteração atual e vai para a próxima.
- **`return`:** encerra a função e devolve o controle (e o valor) ao chamador.
- **`goto`:** desvio incondicional para um rótulo. Existe em C, mas é desaconselhado. Seu uso residual aceitável é sair de laços muito aninhados ou tratar erros com limpeza centralizada.

#### 4.5 Operadores que geram questões

- **Incremento:** `i++` (pós-fixo) usa o valor e depois incrementa; `++i` (pré-fixo) incrementa e depois usa. Com `i = 5`, `x = i++;` resulta em `x = 5` e `i = 6`, enquanto `x = ++i;` resulta em `x = 6` e `i = 6`.
- **Divisão inteira:** `7 / 2` vale `3` em C se os dois operandos são inteiros; para obter 3.5 é preciso `7 / 2.0`. O operador `%` devolve o resto e só funciona com inteiros.
- **Lógicos:** `&&` (e), `||` (ou) e `!` (não), com **curto-circuito**: em `a && b`, se `a` é falso, `b` nem é avaliado.
- **Bit a bit:** `&`, `|`, `^`, `~`, `<<`, `>>` são operadores distintos dos lógicos.

### 5. Programação modular

Programação modular é a prática de dividir o programa em módulos independentes, cada um responsável por uma tarefa coesa. As vantagens são: **reuso** (escreve-se uma vez, usa-se em muitos pontos), **legibilidade**, **facilidade de manutenção e teste**, **trabalho em equipe** e **ocultação de complexidade** (abstração). A qualidade de uma modularização é avaliada por dois conceitos clássicos: **coesão** (um módulo deve fazer uma coisa só e fazê-la bem, o que se quer alto) e **acoplamento** (grau de dependência entre módulos, que se quer baixo).

Em C, a modularização acontece em dois níveis: funções dentro de um arquivo e **múltiplos arquivos**. Um módulo costuma ter um arquivo de cabeçalho (`.h`) com as declarações (a interface pública) e um arquivo `.c` com as definições (a implementação). Exemplo:

c

```c
/* matematica.h */
#ifndef MATEMATICA_H     // proteção contra inclusão múltipla
#define MATEMATICA_H

int fatorial(int n);
double media(double a, double b);

#endif
```

c

```c
/* matematica.c */
#include "matematica.h"

int fatorial(int n) {
    int r = 1;
    for (int i = 2; i <= n; i++) r *= i;
    return r;
}

double media(double a, double b) {
    return (a + b) / 2.0;
}
```

c

```c
/* main.c */
#include <stdio.h>
#include "matematica.h"

int main(void) {
    printf("%d\n", fatorial(5));
    return 0;
}
```

Compilação: `gcc main.c matematica.c -o programa`. Aqui entra o processo de compilação em etapas: **pré-processamento** (trata `#include` e `#define`), **compilação** (gera código-objeto de cada `.c`), **ligação (linking)** (une os objetos e bibliotecas no executável). As aspas em `#include "arq.h"` indicam arquivo do projeto, e os sinais `< >` indicam cabeçalhos do sistema. A palavra-chave `static`, aplicada a uma função ou variável global, restringe sua visibilidade ao arquivo (ligação interna), funcionando como um “privado” do módulo.

### 6. Rotinas, sub-rotinas, procedimentos e funções

**Sub-rotina** (ou rotina) é um trecho de código nomeado, que pode ser chamado de vários pontos do programa. Tradicionalmente, distinguem-se duas categorias:

- **Função:** sub-rotina que **retorna um valor** ao chamador.
- **Procedimento:** sub-rotina que executa ações e **não retorna valor**.

Em C **não existe a palavra “procedimento”**: tudo é função, e um procedimento é uma função do tipo `void`. Essa é uma pegadinha comum de concurso.

#### 6.1 Anatomia de uma função

c

```c
tipo_de_retorno nome(tipo param1, tipo param2) {
    // corpo
    return valor;
}
```

Exemplo de função e de procedimento:

c

```c
int quadrado(int x) {          // função: retorna int
    return x * x;
}

void saudacao(const char *nome) {   // procedimento: retorna void
    printf("Ola, %s!\n", nome);
}
```

#### 6.2 Declaração (protótipo), definição e chamada

O **protótipo** informa ao compilador o nome, o tipo de retorno e os parâmetros da função antes de ela ser usada, permitindo que `main` venha antes das definições:

c

```c
int quadrado(int x);          // protótipo (termina com ;)

int main(void) {
    printf("%d\n", quadrado(4));   // chamada
    return 0;
}

int quadrado(int x) {         // definição
    return x * x;
}
```

Sem protótipo, o compilador de C moderno acusa erro ou aviso, pois não conhece a função no ponto da chamada. Os termos **parâmetro formal** (a variável na definição) e **argumento ou parâmetro real** (o valor passado na chamada) são distintos e podem aparecer em questões teóricas.

#### 6.3 Passagem de parâmetros

**Em C, a passagem é sempre por valor:** a função recebe uma **cópia** do argumento, e alterá-la não afeta a variável original.

c

```c
void troca_errada(int a, int b) {
    int aux = a; a = b; b = aux;      // troca só as cópias locais
}
```

Para simular a **passagem por referência**, passa-se o **endereço** da variável (um ponteiro), e a função altera o conteúdo apontado:

c

```c
void troca(int *a, int *b) {
    int aux = *a;
    *a = *b;
    *b = aux;
}

int main(void) {
    int x = 3, y = 7;
    troca(&x, &y);                    // agora x = 7 e y = 3
    return 0;
}
```

**Vetores são exceção aparente:** o nome de um vetor, ao ser passado para uma função, “decai” para um ponteiro para o primeiro elemento. Assim, a função altera o vetor original, e o tamanho precisa ser passado como parâmetro separado, pois `sizeof` dentro da função mede o ponteiro e não o vetor:

c

```c
void dobra(int v[], int n) {
    for (int i = 0; i < n; i++) v[i] *= 2;   // altera o vetor original
}
```

Quando a função não deve modificar o dado, usa-se `const`: `void imprime(const int *v, int n)`.

#### 6.4 Retorno de valores

`return` devolve um único valor e encerra a função. Para devolver **múltiplos resultados**, usam-se parâmetros por ponteiro ou uma `struct`. Cuidado clássico: **nunca retorne o endereço de uma variável local**, pois ela deixa de existir quando a função termina:

c

```c
int *errado(void) {
    int x = 10;
    return &x;      // ponteiro para memória já liberada: comportamento indefinido
}
```

`main` retorna `int`: `0` indica sucesso e valores diferentes de zero indicam erro para o sistema operacional.

#### 6.5 Escopo e tempo de vida das variáveis

- **Local (automática):** declarada dentro de função ou bloco; existe só durante a execução, vive na pilha e **não é inicializada automaticamente** (contém lixo).
- **Global:** declarada fora de funções; visível a todo o arquivo, vive durante toda a execução e é inicializada com zero. O uso excessivo prejudica a modularidade, pois cria acoplamento oculto entre funções.
- **`static` local:** mantém o valor entre chamadas da função, embora só seja visível dentro dela.

c

```c
int contador(void) {
    static int c = 0;     // inicializada uma única vez
    c++;
    return c;             // 1ª chamada devolve 1, 2ª devolve 2...
}
```

- **`extern`:** declara que uma variável global é definida em outro arquivo.
- **`register`:** sugestão ao compilador para manter a variável em registrador (hoje, raramente relevante).

#### 6.6 Recursividade em C

Uma função pode chamar a si mesma, desde que tenha caso base:

c

```c
int fatorial(int n) {
    if (n <= 1) return 1;          // caso base
    return n * fatorial(n - 1);    // caso recursivo
}
```

Cada chamada cria um novo **registro de ativação** na pilha de chamadas, com seus parâmetros e variáveis locais. Recursão profunda demais ou sem caso base provoca estouro de pilha (stack overflow).

### 7. Funções de biblioteca e `main` com argumentos

A biblioteca padrão fornece funções prontas, organizadas por cabeçalho: `<stdio.h>` (entrada e saída: `printf`, `scanf`, `fgets`, `fopen`), `<stdlib.h>` (`malloc`, `free`, `atoi`, `exit`, `rand`), `<string.h>` (`strlen`, `strcpy`, `strcmp`, `strcat`) e `<math.h>` (`sqrt`, `pow`, `sin`).

A função `main` pode receber argumentos da linha de comando: `int main(int argc, char *argv[])`, onde `argc` é a quantidade de argumentos (incluindo o nome do programa) e `argv` é o vetor de strings com seus valores.

Um tópico correlato e muito cobrado é a **alocação dinâmica**: `malloc(n * sizeof(int))` reserva memória no heap e devolve um ponteiro (ou `NULL` se falhar), `free` a libera. Esquecer o `free` causa **vazamento de memória (memory leak)**, e usar a memória após liberada causa comportamento indefinido.

### 8. Strings em C e registros (`struct`)

Strings são vetores de `char` terminados por `'\0'`. Não se compara strings com `==` (isso compara endereços); usa-se `strcmp`, que devolve 0 quando são iguais. Também não se copia com `=`, mas com `strcpy`. `scanf("%s")` para no primeiro espaço, e por isso se usa `fgets` para ler linhas inteiras.

Para dados heterogêneos, usa-se `struct`:

c

```c
struct Aluno {
    char nome[50];
    int matricula;
    float nota;
};

struct Aluno a = {"Ana", 101, 8.5f};
printf("%s %.1f\n", a.nome, a.nota);

struct Aluno *p = &a;
printf("%d\n", p->matricula);    // operador seta para acesso via ponteiro
```

Com `typedef`, cria-se um apelido para o tipo, o que torna o código mais limpo.

### 9. Exemplo integrado: refinamento, modularização e funções

Problema: ler `n` notas, calcular a média e informar quantas ficaram acima dela.

c

```c
#include <stdio.h>
#define MAX 100

void lerNotas(float v[], int n);
float calcularMedia(const float v[], int n);
int contarAcima(const float v[], int n, float limite);

int main(void) {
    float notas[MAX];
    int n;

    printf("Quantas notas? ");
    scanf("%d", &n);

    lerNotas(notas, n);
    float media = calcularMedia(notas, n);
    printf("Media: %.2f\n", media);
    printf("Acima da media: %d\n", contarAcima(notas, n, media));
    return 0;
}

void lerNotas(float v[], int n) {
    for (int i = 0; i < n; i++) {
        printf("Nota %d: ", i + 1);
        scanf("%f", &v[i]);
    }
}

float calcularMedia(const float v[], int n) {
    float soma = 0;
    for (int i = 0; i < n; i++) soma += v[i];
    return soma / n;
}

int contarAcima(const float v[], int n, float limite) {
    int c = 0;
    for (int i = 0; i < n; i++)
        if (v[i] > limite) c++;
    return c;
}
```

Repare em como a estrutura espelha o refinamento: `main` descreve o algoritmo em alto nível, e cada função resolve um subproblema com baixa dependência das demais.

---

## MACETES E ARMADILHAS PARA A PROVA (C)

1. **Em C não existe “procedimento”**: é função `void`. Funções sem `return` de valor devem ser `void`.
2. **Passagem de parâmetros é sempre por valor.** “Por referência” é simulada com ponteiros. Vetores passam como endereço do primeiro elemento.
3. **`=` é atribuição e `==` é comparação.** `if (x = 0)` nunca entra, e `if (x = 1)` sempre entra.
4. **`switch` sem `break` executa os `case` seguintes.** Simule linha a linha.
5. **`do-while` sempre executa pelo menos uma vez** e repete enquanto a condição for verdadeira.
6. **Divisão de inteiros trunca**: `5 / 2 = 2`, e `5 / 2.0 = 2.5`. Cuidado também com `float x = 5 / 2;`, que resulta em `2.0`.
7. **`i++` versus `++i`** em expressões: o pós-fixo usa o valor antigo.
8. **Variáveis locais não inicializadas contêm lixo**, e globais e `static` começam em zero.
9. **`static` local preserva valor entre chamadas**: questões pedem o que imprime na terceira chamada.
10. **Strings: `strcmp` e `strcpy`, nunca `==` e `=`**, e lembre do espaço extra para o `'\0'` (a string “ABC” precisa de `char s[4]`).
11. **Índices começam em 0**: um vetor `v[5]` vai de `v[0]` a `v[4]`, e `v[5]` é acesso fora do limite (o C não verifica, é comportamento indefinido).
12. **Protótipo termina com `;`, definição tem corpo `{ }`.**
13. **Recursão**: procure o caso base e acompanhe a pilha. Na ausência dele, haverá estouro de pilha.
14. **`scanf` exige `&`** para tipos básicos, mas não para vetores de `char` (o nome já é endereço).
15. **Escopo**: uma variável declarada dentro de um bloco `{ }` não existe fora dele, e uma local com mesmo nome de uma global a oculta.

---

**Sobre incidência na prova:** vale o mesmo aviso da parte anterior. Não consegui contabilizar por tema as questões de 2020 a 2026, então não posso dar percentuais confiáveis. Pelo perfil usual dessas provas, é razoável esperar que questões de interpretação de trechos em C ou em pseudocódigo (“o que será impresso”) e conceitos de funções, passagem de parâmetros e escopo apareçam com regularidade. Se você me enviar as provas em PDF, faço a contagem real por assunto.