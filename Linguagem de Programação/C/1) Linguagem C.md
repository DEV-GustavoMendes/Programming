
---

### 1. Introdução

C é uma linguagem **compilada**, **procedural** e de **baixo/médio nível**. Foi criada por Dennis Ritchie nos anos 1970 e é usada em sistemas operacionais, bancos de dados, drivers e sistemas embarcados. Ela é a base de C++, Java, C# e outras. Você tem acesso direto à memória e quase nada é feito por você (sem coletor de lixo, sem verificação de limites).

Para compilar e executar:

bash

```bash
gcc main.c -o main     # compila
./main                 # executa
```

### 2. Sintaxe e instruções

```c
#include <stdio.h>          // diretiva de pré-processador: inclui a biblioteca de entrada/saída

int main() {                // ponto de entrada do programa
  printf("Hello World!");   // instrução; termina com ;
  return 0;                 // 0 = terminou com sucesso
}
```

- `#include <stdio.h>`: traz declarações como `printf` e `scanf`.
- `main()`: o programa começa aqui.
- `{ }`: delimitam blocos.
- `;`: termina cada instrução (esquecer é erro de compilação).
- Espaços e quebras de linha são ignorados pelo compilador.

### 3. Saída (`printf`)

```c
printf("Olá\n");        // \n = nova linha
printf("A\tB\n");       // \t = tabulação
printf("Aspas: \"x\" e barra: \\ e %%\n");
```

Sequências de escape: `\n`, `\t`, `\\`, `\"`, `\'`, `\0` (caractere nulo).

### 4. Comentários

```c
// comentário de uma linha
/* comentário
   de várias linhas */
```

### 5. Variáveis

Variável é um **espaço nomeado na memória** com um tipo.

```c
int idade = 25;
float pi = 3.14f;
char letra = 'A';        // char usa aspas simples
int x = 5, y = 10, z;    // várias de uma vez
int a, b, c;
a = b = c = 50;          // mesmo valor para todas
```

**Especificadores de formato** (dizem ao `printf`/`scanf` o tipo):

|Tipo|Especificador|
|---|---|
|`int`|`%d` ou `%i`|
|`float`|`%f`|
|`double`|`%lf`|
|`char`|`%c`|
|string (`char[]`)|`%s`|
|`unsigned int`|`%u`|
|`long` / `long long`|`%ld` / `%lld`|
|hexadecimal|`%x`|
|ponteiro|`%p`|
|`size_t` (sizeof)|`%zu`|

```c
printf("Idade: %d, letra: %c\n", idade, letra);
```

**Nomes válidos:** letras, dígitos e `_`; não começam com dígito; diferenciam maiúsculas de minúsculas; não podem ser palavras reservadas.

### 6. Tipos de dados

|Tipo|Tamanho típico|Uso|
|---|---|---|
|`char`|1 byte|um caractere / inteiro pequeno|
|`short`|2 bytes|inteiro curto|
|`int`|4 bytes|inteiro|
|`long`|4 ou 8 bytes|inteiro grande|
|`long long`|8 bytes|inteiro muito grande|
|`float`|4 bytes|~6-7 dígitos de precisão|
|`double`|8 bytes|~15 dígitos de precisão|

- Modificadores: `signed`, `unsigned` (só não negativos, dobra o limite positivo).
- Precisão na impressão: `printf("%.2f", 3.14159);` imprime `3.14`.
- Notação científica: `float f = 35e3;`
- `sizeof(int)` retorna o tamanho em bytes. Os tamanhos dependem da plataforma.
- `char` é, na verdade, um número: `'A'` vale 65 (tabela ASCII).

### 7. Conversão de tipos

```c
// Implícita: o compilador converte sozinho
float f = 5;            // int -> float

// Explícita (cast)
int a = 5, b = 2;
float r1 = a / b;           // 2.0  (divisão inteira antes!)
float r2 = (float) a / b;   // 2.5
```

Pegadinha clássica: divisão entre dois inteiros descarta a parte decimal.

### 8. Constantes

```c
const int MAX = 100;      // não pode ser alterada (precisa ser inicializada)
#define PI 3.14159        // macro do pré-processador (sem ; e sem tipo)
```

### 9. Operadores

- **Aritméticos:** `+ - * / % ++ --` (`%` = resto, só para inteiros).  
    `x++` usa e depois incrementa, `++x` incrementa e depois usa.
- **Atribuição:** `= += -= *= /= %= &= |= ^= >>= <<=`
- **Comparação** (resultam em `1` verdadeiro ou `0` falso): `== != > < >= <=`
- **Lógicos:** `&&` (E), `||` (OU), `!` (NÃO). Fazem avaliação em curto-circuito.
- **Bitwise:** `& | ^ ~ << >>` (veja a seção 24).
- **Outros:** `sizeof`, ternário `? :`, `&` (endereço), `*` (desreferência).
- **Precedência:** `()` > `++ --` / `!` > `* / %` > `+ -` > comparações > `&&` > `||` > atribuição. Na dúvida, use parênteses.

⚠️ Confundir `=` (atribuição) com `==` (comparação) é um erro muito comum.

### 10. Booleanos

C não tinha tipo booleano originalmente: **0 é falso, qualquer outro valor é verdadeiro**.

```c
#include <stdbool.h>
bool ligado = true;
printf("%d", ligado);   // 1
```

### 11. Condicionais: `if`, `else`, `else if`

```c
if (nota >= 7) {
  printf("Aprovado");
} else if (nota >= 5) {
  printf("Recuperação");
} else {
  printf("Reprovado");
}

// Abreviado (operador ternário)
char *msg = (idade >= 18) ? "Adulto" : "Menor";
```

Também há `if` aninhado e combinações com `&&`, `||`, `!`.

### 12. `switch`

```c
switch (dia) {
  case 1: printf("Domingo"); break;
  case 2: printf("Segunda"); break;
  default: printf("Outro");
}
```

Sem `break`, a execução “cai” para o próximo `case` (_fall-through_). O `switch` só aceita inteiros e `char`, não strings.

### 13. Laços de repetição

```c
// while: testa antes
while (i < 5) { printf("%d ", i); i++; }

// do/while: executa pelo menos 1 vez, testa depois
do { printf("%d ", i); i++; } while (i < 5);

// for: inicialização; condição; atualização
for (int i = 0; i < 5; i++) { printf("%d ", i); }

// aninhado
for (int i = 0; i < 3; i++)
  for (int j = 0; j < 3; j++)
    printf("(%d,%d) ", i, j);
```

- `break` sai do laço.
- `continue` pula para a próxima iteração.

### 14. Arrays (vetores)

Coleção de elementos do **mesmo tipo** em memória contígua. O índice começa em **0**.

```c
int nums[3] = {10, 20, 30};
nums[0] = 99;
int tamanho = sizeof(nums) / sizeof(nums[0]);   // 3

for (int i = 0; i < tamanho; i++) printf("%d ", nums[i]);

int matriz[2][3] = {{1,2,3}, {4,5,6}};          // multidimensional
printf("%d", matriz[1][2]);                      // 6
```

⚠️ C **não verifica limites**: acessar `nums[10]` compila, mas gera comportamento indefinido.

### 15. Strings

C não tem tipo string: é um **array de `char` terminado por `'\0'`** (caractere nulo).

```c
char s1[] = "Hello";              // 6 bytes: H e l l o \0
char s2[] = {'H','i','\0'};
printf("%s\n", s1);

s1[0] = 'J';                      // altera caractere
```

- `sizeof(s1)` = bytes do array (6), `strlen(s1)` = caracteres até o `\0` (5).
- Não dá para fazer `s1 = "outro"` depois de declarar; use `strcpy`.

**`<string.h>`:**

```c
strlen(s);            // tamanho
strcpy(dest, src);    // copia
strcat(dest, src);    // concatena (dest precisa ter espaço)
strcmp(a, b);         // 0 se iguais
strncpy, strncat, strchr, strstr, strtok ...
```

**`<ctype.h>`:** `isalpha`, `isdigit`, `isspace`, `isupper`, `islower`, `toupper`, `tolower`.

**Array de strings:** `char nomes[3][10] = {"Ana", "Bia", "Caio"};`

### 16. Entrada do usuário

```c
int n;
scanf("%d", &n);                 // & = endereço da variável

char nome[50];
scanf("%s", nome);               // lê só até o primeiro espaço (sem &, array já é endereço)
fgets(nome, sizeof(nome), stdin);// lê a linha inteira (inclui o \n)
```

`scanf` retorna quantos itens leu com sucesso, o que é útil para validar a entrada. Prefira `fgets` para strings, porque `%s` pode estourar o buffer.

### 17. Endereço de memória

Toda variável vive em um endereço. O operador `&` o revela:

```c
int x = 10;
printf("%p", &x);   // ex.: 0x7ffee4b3a9ac
```

### 18. Ponteiros

Ponteiro é uma variável que **guarda um endereço**.

```c
int x = 10;
int *p = &x;        // p aponta para x
printf("%d", *p);   // 10  (* desreferencia: acessa o valor apontado)
*p = 20;            // altera x indiretamente
```

**Ponteiros e arrays:** o nome do array é (quase sempre) o endereço do 1º elemento.


```c
int v[3] = {1,2,3};
int *p = v;
printf("%d", *(p + 1));   // 2  → v[1]
```

**Aritmética de ponteiros:** `p + 1` avança `sizeof(tipo)` bytes, não 1 byte. Também existem `p++`, `p--` e `p2 - p1`.

**Ponteiro para ponteiro:**

```c
int **pp = &p;
printf("%d", **pp);
```

### 19. Funções

```c
#include <stdio.h>

int soma(int a, int b);          // declaração (protótipo)

int main() {
  printf("%d", soma(2, 3));      // chamada
  return 0;
}

int soma(int a, int b) {         // definição
  return a + b;
}
```

- `void` = não retorna nada.
- **Passagem por valor:** a função recebe uma cópia, então alterar o parâmetro não afeta o original.
- **Passagem por referência (via ponteiro):**

```c
void troca(int *a, int *b) { int t = *a; *a = *b; *b = t; }
troca(&x, &y);
```

- **Escopo:** variável **local** (dentro da função) só existe ali. **Global** (fora de tudo) é visível em todo o arquivo.
- **Arrays como parâmetro** decaem para ponteiro, então passe o tamanho junto: `void f(int v[], int n)`.
- **`<math.h>`:** `sqrt`, `pow`, `ceil`, `floor`, `round`, `fabs`, `sin`, `cos`, `log`... (no gcc, compile com `-lm`).
- **`inline`:** sugere ao compilador copiar o corpo da função no ponto de chamada.
- **Recursão:** a função chama a si mesma e precisa de caso-base.

```c
int fat(int n) { return (n <= 1) ? 1 : n * fat(n - 1); }
```

- **Ponteiros para função e callbacks:**

```c
int dobro(int x) { return x * 2; }
int (*fp)(int) = dobro;          // ponteiro para função
printf("%d", fp(4));             // 8

void aplicar(int v[], int n, int (*f)(int)) {   // callback
  for (int i = 0; i < n; i++) v[i] = f(v[i]);
}
```

### 20. Arquivos

```c
#include <stdio.h>

FILE *f = fopen("dados.txt", "w");   // modos: "r" ler, "w" escrever (apaga), "a" anexar, "r+", "w+"
if (f == NULL) { printf("Erro ao abrir\n"); return 1; }

fprintf(f, "Olá %d\n", 42);
fputs("linha\n", f);
fclose(f);                           // sempre fechar

// leitura
f = fopen("dados.txt", "r");
char linha[100];
while (fgets(linha, sizeof(linha), f)) printf("%s", linha);
fclose(f);
```

Outras funções: `fscanf`, `fgetc`, `fputc`, `fread`, `fwrite` (binário), `fseek`, `ftell`, `rewind`, `remove`, `rename`.

### 21. Estruturas (`struct`), `union`, `typedef`

```c
struct Pessoa {
  char nome[30];
  int idade;
};

struct Pessoa p1 = {"Ana", 20};
printf("%s %d", p1.nome, p1.idade);       // acesso com .

struct Pessoa *pp = &p1;
printf("%d", pp->idade);                  // acesso via ponteiro com ->

struct Pessoa turma[3];                   // array de structs

struct Aluno { struct Pessoa dados; float nota; };   // struct aninhada
```

- **`typedef`:** cria apelido de tipo.

```c
typedef struct { int x, y; } Ponto;
Ponto a = {1, 2};
```

- **`union`:** todos os membros **compartilham o mesmo espaço**, e só um valor é válido por vez. O tamanho é o do maior membro.

```c
union Dado { int i; float f; char c; };
```

- **Padding (preenchimento):** o compilador insere bytes vazios entre membros para alinhar à memória, então `sizeof(struct)` pode ser maior que a soma dos campos. Ordenar os membros do maior para o menor reduz o desperdício.

### 22. Enums


```c
enum Dia { DOM, SEG, TER, QUA };      // DOM=0, SEG=1, ...
enum Nivel { BAIXO = 1, MEDIO = 5, ALTO = 10 };

enum Dia hoje = TER;
```

São constantes inteiras com nomes legíveis, úteis com `switch`.

### 23. Gerenciamento de memória (heap)

- **Stack:** variáveis locais, automática e rápida.
- **Heap:** memória dinâmica que você controla (`<stdlib.h>`).


```c
int *v = malloc(5 * sizeof(int));     // aloca (não inicializa)
if (v == NULL) { /* falhou */ }

int *z = calloc(5, sizeof(int));      // aloca zerado

v = realloc(v, 10 * sizeof(int));     // redimensiona

free(v);                              // libera
v = NULL;                             // evita ponteiro "pendurado"
```

**Erros comuns:**

- _Memory leak_: alocar e nunca dar `free`.
- _Dangling pointer_: usar o ponteiro depois do `free`.
- _Double free_: liberar duas vezes.
- Acessar fora do bloco alocado.
- Não checar `NULL` após `malloc`.

### 24. Erros, depuração e `NULL`

- **Tipos de erro:** de sintaxe (compilação), de execução (_runtime_, como divisão por zero e _segfault_) e lógicos (compila e roda, mas resultado errado).
- **Depuração:** `printf` estratégico, `gdb`, `valgrind` (detecta vazamentos) e `-Wall -Wextra -g` ao compilar.
- **`NULL`:** ponteiro que não aponta para nada. Sempre verifique antes de desreferenciar.
- **Tratamento de erros:**

c

```c
#include <errno.h>
#include <string.h>
FILE *f = fopen("x.txt", "r");
if (!f) { perror("fopen"); printf("%s\n", strerror(errno)); exit(1); }
```

`exit(0)` ou `EXIT_SUCCESS` significa sucesso, `EXIT_FAILURE` significa erro.

- **Validação de entrada:**

c

```c
int n;
if (scanf("%d", &n) != 1) { printf("Entrada inválida\n"); }
```

### 25. Resolução de problemas

Metodologia sugerida: entender o problema, montar a lógica passo a passo, tratar **casos de borda** (zero, negativo, vazio, limites) e escrever **casos de teste** antes de dar o código como pronto.

### 26. Tópicos extras

**Data e hora (`<time.h>`)**

c

```c
time_t agora = time(NULL);
printf("%s", ctime(&agora));
```

**Números aleatórios**

c

```c
#include <stdlib.h>
srand(time(NULL));        // semente (uma vez)
int r = rand() % 10;      // 0 a 9
```

**Macros**

c

```c
#define QUADRADO(x) ((x) * (x))
#define DEBUG
#ifdef DEBUG
  printf("modo debug\n");
#endif
```

Macros são substituições de texto feitas antes da compilação, então use parênteses em tudo.

**Organização do código:** declarações em `.h` e implementações em `.c`. Use _include guards_ para evitar inclusão dupla:

c

```c
// util.h
#ifndef UTIL_H
#define UTIL_H
int soma(int a, int b);
#endif
```

Inclua com `#include "util.h"` (aspas para arquivos seus, `< >` para a biblioteca padrão).

**Classes de armazenamento**

|Palavra|Efeito|
|---|---|
|`auto`|padrão das locais|
|`register`|sugere guardar no registrador|
|`static`|local que **mantém o valor** entre chamadas; ou global visível só no arquivo|
|`extern`|usa variável definida em outro arquivo|

**Operadores bitwise**

c

```c
a & b    // E bit a bit
a | b    // OU
a ^ b    // XOR
~a       // NOT
a << 2   // desloca à esquerda (multiplica por 4)
a >> 1   // desloca à direita (divide por 2)
```

**Inteiros de largura fixa (`<stdint.h>`):** `int8_t`, `uint8_t`, `int16_t`, `int32_t`, `uint64_t`... o tamanho é garantido em qualquer plataforma.

**Argumentos de linha de comando**

c

```c
int main(int argc, char *argv[]) {   // argc = quantidade, argv = vetor de strings
  for (int i = 0; i < argc; i++) printf("%s\n", argv[i]);
  return 0;
}
```

### 27. Estruturas de dados e algoritmos em C (visão geral)

C não traz coleções prontas, então você as constrói com `struct` + ponteiros + `malloc`:

- **Busca** (linear e binária) e **ordenação** (bubble, selection, insertion, quick; `qsort` na stdlib).
- **Pilha** (LIFO), **fila** (FIFO), **lista encadeada** (nós com ponteiro `next`), **árvores**, **tabelas hash**.
- **Big O:** mede como o custo cresce com o tamanho da entrada (O(1), O(log n), O(n), O(n²)).

c

```c
struct No { int valor; struct No *prox; };   // nó de lista encadeada
```

### 28. Palavras-chave (32 do C clássico)

`auto break case char const continue default do double else enum extern float for goto if int long register return short signed sizeof static struct switch typedef union unsigned void volatile while`

Padrões mais novos adicionaram: `inline restrict _Bool _Complex _Imaginary _Alignas _Alignof _Atomic _Generic _Noreturn _Static_assert _Thread_local`.

---

#### Bibliotecas padrão mais usadas

`stdio.h` (E/S), `stdlib.h` (malloc, rand, exit, atoi), `string.h` (strings), `math.h` (matemática), `ctype.h` (caracteres), `time.h` (tempo), `stdbool.h` (bool), `stdint.h` (inteiros fixos), `errno.h`, `limits.h`, `assert.h`.