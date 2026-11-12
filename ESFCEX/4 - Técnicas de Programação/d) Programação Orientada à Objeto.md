## PARTE D: PROGRAMAÇÃO ORIENTADA A OBJETOS (C++, PYTHON E JAVA)

### 1. O paradigma orientado a objetos

A programação orientada a objetos (POO) organiza o software como um conjunto de **objetos** que reúnem dados (estado) e comportamentos (operações) e que colaboram entre si trocando **mensagens**, isto é, chamando métodos uns dos outros. Na programação estruturada, que você viu na parte anterior, o foco está nos procedimentos que manipulam dados separados. Na POO, dados e operações que agem sobre eles ficam juntos, e o programa modela entidades do domínio do problema: Cliente, Conta, Veículo, Pedido.

Historicamente, o paradigma nasceu com a linguagem **Simula 67**, criada na Noruega por Dahl e Nygaard para simulações. Foi consolidado pelo **Smalltalk** (Alan Kay, Xerox PARC, anos 1970) e popularizado por **C++** (Bjarne Stroustrup, início dos anos 1980, como extensão de C), **Java** (James Gosling, Sun, 1995) e **Python** (Guido van Rossum, 1991). Hoje, a POO sustenta a maior parte dos sistemas corporativos, de frameworks web a software militar e bancário.

Os **quatro pilares** da POO, que formam a base de praticamente qualquer questão de prova, são:

- **Abstração:** representar apenas as características essenciais de uma entidade para o problema em questão, ignorando detalhes irrelevantes.
- **Encapsulamento:** esconder o estado interno do objeto e expor apenas uma interface controlada.
- **Herança:** criar novas classes a partir de classes existentes, reaproveitando e especializando seu comportamento.
- **Polimorfismo:** permitir que uma mesma mensagem (chamada de método) produza comportamentos diferentes conforme o tipo real do objeto que a recebe.

### 2. Classes e objetos

**Classe** é o molde, a especificação: define os atributos (variáveis que guardam o estado) e os métodos (funções que definem o comportamento) comuns a um conjunto de objetos. **Objeto** é uma **instância** da classe, uma entidade concreta, alocada na memória, com valores próprios para os atributos. A classe `Conta` é o conceito. O objeto `contaDaAna`, com saldo 500, é uma instância. Cada objeto possui três características: **identidade** (é distinto dos demais, mesmo com atributos iguais), **estado** (valores atuais dos atributos) e **comportamento** (métodos que pode executar).

Um mesmo exemplo nas três linguagens, com atributos privados, construtor e métodos:

**Java**
```java
public class Conta {
    private String titular;
    private double saldo;
    private static int totalContas = 0;      // atributo de classe

    public Conta(String titular, double saldoInicial) {   // construtor
        this.titular = titular;
        this.saldo = saldoInicial;
        totalContas++;
    }

    public void depositar(double valor) {
        if (valor > 0) saldo += valor;
    }

    public boolean sacar(double valor) {
        if (valor > 0 && valor <= saldo) { saldo -= valor; return true; }
        return false;
    }

    public double getSaldo() { return saldo; }
    public static int getTotalContas() { return totalContas; }
}

// uso
Conta c = new Conta("Ana", 500);
c.depositar(100);
```

**C++**
```cpp
class Conta {
private:
    std::string titular;
    double saldo;
    static int totalContas;                 // declaração do membro estático

public:
    Conta(const std::string& titular, double saldoInicial)
        : titular(titular), saldo(saldoInicial) { ++totalContas; }   // lista de inicialização

    ~Conta() { --totalContas; }             // destrutor

    void depositar(double valor) { if (valor > 0) saldo += valor; }
    bool sacar(double valor) {
        if (valor > 0 && valor <= saldo) { saldo -= valor; return true; }
        return false;
    }
    double getSaldo() const { return saldo; }
    static int getTotalContas() { return totalContas; }
};

int Conta::totalContas = 0;                 // definição fora da classe

// uso
Conta c("Ana", 500);          // objeto na pilha (automático)
Conta* p = new Conta("Rui", 50);   // objeto no heap
delete p;                     // liberação manual obrigatória
```

**Python**
```python
class Conta:
    total_contas = 0                        # atributo de classe

    def __init__(self, titular, saldo_inicial):
        self._titular = titular             # convenção: _ = "uso interno"
        self._saldo = saldo_inicial
        Conta.total_contas += 1

    def depositar(self, valor):
        if valor > 0:
            self._saldo += valor

    def sacar(self, valor):
        if 0 < valor <= self._saldo:
            self._saldo -= valor
            return True
        return False

    @property
    def saldo(self):                        # acesso como atributo: c.saldo
        return self._saldo

# uso
c = Conta("Ana", 500)
c.depositar(100)
print(c.saldo)
```

#### 2.1 Elementos importantes

**Construtor** é o método chamado na criação do objeto, para inicializar seu estado. Em Java e C++ tem o mesmo nome da classe e não declara tipo de retorno. Em Python, o inicializador é `__init__` (tecnicamente, quem cria o objeto é `__new__`, e `__init__` o inicializa). Se você não define nenhum construtor em Java ou C++, o compilador fornece um **construtor padrão** sem parâmetros. Em Java, ao definir qualquer construtor, o padrão deixa de ser gerado automaticamente.

**Destrutor** libera recursos quando o objeto deixa de existir. Em C++, é `~Classe()` e é chamado automaticamente quando um objeto da pilha sai de escopo ou quando se faz `delete`. Em Java não há destrutor propriamente dito: o **coletor de lixo (garbage collector)** libera a memória de objetos inalcançáveis, em momento não determinístico (o antigo `finalize()` foi descontinuado). Em Python, há contagem de referências somada a um coletor de ciclos, e existe `__del__`, raramente recomendado.

**Referência à própria instância:** `this` em Java (referência) e em C++ (**ponteiro**, usa-se `this->atributo`), e `self` em Python, que é parâmetro explícito e obrigatório como primeiro parâmetro de todo método de instância.

**Membros estáticos (de classe):** pertencem à classe e não a cada objeto, sendo compartilhados por todas as instâncias. Declaram-se com `static` em Java e C++. Em Python, atributos declarados no corpo da classe são de classe, e há `@classmethod` (recebe a classe, `cls`) e `@staticmethod` (não recebe nem instância nem classe). Um método estático não pode usar `this` nem acessar diretamente membros de instância.

**Atributos de instância versus de classe:** cada objeto tem sua cópia dos de instância; os de classe existem em uma única cópia.

**Tipos por valor versus por referência (Java):** tipos primitivos (`int`, `double`, `boolean`, `char`...) guardam o valor; objetos são acessados por **referência**. Atribuir `Conta b = a;` copia a referência, e `a` e `b` passam a apontar para o **mesmo** objeto. Java passa parâmetros sempre **por valor**, mas, para objetos, o valor copiado é a referência. Em C++ é possível escolher: por valor (cópia do objeto), por ponteiro ou por referência (`&`).

**Operadores relacionados:** `new` cria objeto em Java e C++ (em Python basta chamar a classe: `Conta(...)`). Em Java, `==` compara referências, e o método `equals` compara conteúdo (se sobrescrito). A classe raiz de todas as classes Java é `Object`, que fornece `toString()`, `equals()` e `hashCode()`. Ao sobrescrever `equals`, deve-se sobrescrever `hashCode` de forma consistente, ou coleções baseadas em hash se comportam mal.

**Construtor de cópia e regra dos três/cinco (C++):** quando uma classe gerencia um recurso (como memória alocada com `new`), é preciso definir destrutor, construtor de cópia e operador de atribuição de cópia (e, desde o C++11, os de movimentação). Sem isso, a cópia padrão é **rasa** (copia o ponteiro, não o dado apontado), gerando duplo `delete` e corrupção. A prática moderna é usar **RAII** (o recurso é adquirido no construtor e liberado no destrutor) e **ponteiros inteligentes** (`std::unique_ptr`, `std::shared_ptr`), evitando `new` e `delete` explícitos.

### 3. Encapsulamento e modificadores de acesso

Encapsular é proteger o estado interno do objeto, obrigando que o acesso aconteça por métodos que garantem as regras de negócio. Em `Conta`, o saldo é privado, de modo que ninguém pode atribuir um valor negativo diretamente: só `depositar` e `sacar` o alteram, e ambos validam. Isso reduz acoplamento e permite mudar a implementação interna sem afetar quem usa a classe. Os métodos de acesso são chamados de **getters** e **setters** (ou acessores e mutadores).

|Modificador|Java|C++|Python|
|---|---|---|---|
|Público|`public` (qualquer classe)|`public`|padrão (sem prefixo)|
|Protegido|`protected` (subclasses **e** mesmo pacote)|`protected` (classe e derivadas)|convenção `_nome`|
|Privado|`private` (só a própria classe)|`private` (só a classe e `friend`)|convenção `__nome` (name mangling)|
|Padrão (sem modificador)|**package-private** (mesmo pacote)|`private` em `class`, `public` em `struct`|público|

Pontos de atenção:

- Em **Java**, existe o nível **package-private** (ausência de modificador), restrito ao pacote. O `protected` de Java é mais permissivo que o de C++, porque também libera para o mesmo pacote.
- Em **C++**, a diferença entre `class` e `struct` é só o acesso padrão (privado na `class`, público na `struct`). Uma função ou classe declarada `friend` pode acessar os membros privados, quebrando o encapsulamento de forma intencional e controlada.
- Em **Python**, **não há privacidade real**. O prefixo `_` é uma convenção, e o prefixo duplo `__x` faz o interpretador renomear o atributo para `_Classe__x` (_name mangling_), dificultando, mas não impedindo, o acesso. A filosofia da linguagem é “somos todos adultos consentidos”. Para acesso controlado usa-se `@property`.

### 4. Herança

Herança permite que uma **subclasse** (classe derivada, filha) herde atributos e métodos de uma **superclasse** (classe base, pai), podendo acrescentar novos membros e **sobrescrever** comportamentos. Expressa uma relação do tipo **“é um”**: Cachorro _é um_ Animal; ContaPoupança _é uma_ Conta. Os benefícios são reaproveitamento de código, organização hierárquica e a base para o polimorfismo.

**Java**
```java
class Animal {
    protected String nome;
    Animal(String nome) { this.nome = nome; }
    void emitirSom() { System.out.println("..."); }
}

class Cachorro extends Animal {
    Cachorro(String nome) { super(nome); }       // chama o construtor da superclasse
    @Override
    void emitirSom() { System.out.println(nome + " faz Au!"); }
}
```

**C++**
```cpp
class Animal {
protected:
    std::string nome;
public:
    Animal(const std::string& n) : nome(n) {}
    virtual void emitirSom() const { std::cout << "...\n"; }
    virtual ~Animal() = default;
};

class Cachorro : public Animal {
public:
    Cachorro(const std::string& n) : Animal(n) {}
    void emitirSom() const override { std::cout << nome << " faz Au!\n"; }
};
```

**Python**

```python
class Animal:
    def __init__(self, nome):
        self.nome = nome
    def emitir_som(self):
        print("...")

class Cachorro(Animal):
    def __init__(self, nome):
        super().__init__(nome)
    def emitir_som(self):
        print(f"{self.nome} faz Au!")
```

#### 4.1 Pontos importantes sobre herança

- **Ordem de construção e destruição:** o construtor da superclasse executa **antes** do da subclasse, e os destruidores executam na ordem inversa (C++). Em Java, a primeira linha do construtor da subclasse é uma chamada a `super(...)`, e, se omitida, o compilador insere `super()` implicitamente. Se a superclasse não tiver construtor sem parâmetros, isso gera erro de compilação.
- **Sobrescrita (override) versus sobrecarga (overload):** sobrescrever é redefinir, **na subclasse**, um método herdado com a **mesma assinatura**. Sobrecarregar é definir, na mesma classe, vários métodos com o **mesmo nome e parâmetros diferentes** (veja a seção de polimorfismo).
- **`super`:** acessa membros da superclasse (`super.metodo()` em Java; `super().metodo()` em Python; em C++ chama-se com o nome da base, `Animal::emitirSom()`).
- **Impedir herança ou sobrescrita:** em Java, `final class` não pode ser estendida, e `final` em método impede sobrescrita (`String` é uma classe final). Em C++11, usa-se `final` em classe ou método virtual.
- **Herança simples versus múltipla:**
    - **Java** permite herança simples de classes (uma única superclasse direta), mas uma classe pode **implementar várias interfaces**.
    - **C++** permite herança múltipla de classes. Isso causa o **problema do diamante**: se B e C herdam de A, e D herda de B e C, D contém duas cópias de A, gerando ambiguidade. A solução é a **herança virtual** (`class B : virtual public A`), que garante uma única instância compartilhada de A.
    - **Python** permite herança múltipla e resolve a ordem de busca de métodos com o **MRO (Method Resolution Order)**, baseado no algoritmo **C3**, consultável por `Classe.__mro__`.
- **Modos de herança em C++:** `public` (mantém os níveis de acesso, e representa “é um”), `protected` e `private` (reduzem a visibilidade dos membros herdados). Em provas, o padrão é a herança pública. Sem especificar, `class` herda de forma privada e `struct` de forma pública.
- **Herança versus composição:** composição expressa a relação **“tem um”**: um Carro _tem um_ Motor. A recomendação clássica (“prefira composição à herança”, do livro de padrões da _Gang of Four_) existe porque a herança cria acoplamento forte entre subclasse e superclasse. Use herança só quando houver genuinamente uma relação “é um” e a subclasse puder substituir a superclasse.
- **Princípio da Substituição de Liskov:** onde se espera um objeto da superclasse, deve ser possível usar um da subclasse sem quebrar o programa. O clássico contra-exemplo é fazer `Quadrado` herdar de `Retângulo`: alterar apenas a largura de um quadrado viola o contrato do retângulo.

### 5. Polimorfismo

Polimorfismo significa “muitas formas”: o mesmo nome de método ou operação se comporta de maneiras distintas conforme o contexto ou o tipo do objeto. Costuma-se classificar em quatro tipos:

**1) Polimorfismo de inclusão (subtipo ou por sobrescrita)**, o mais cobrado. Uma referência do tipo da superclasse pode apontar para objetos de qualquer subclasse, e o método executado é decidido em **tempo de execução**, segundo o tipo **real** do objeto. Esse mecanismo chama-se **ligação dinâmica (dynamic binding, ligação tardia ou _late binding_)**.

**2) Polimorfismo de sobrecarga (ad hoc)**: vários métodos com o mesmo nome e assinaturas diferentes (número ou tipos de parâmetros) na mesma classe, resolvidos em **tempo de compilação** (**ligação estática**). O **tipo de retorno sozinho não distingue** sobrecargas.

**3) Polimorfismo paramétrico**: o mesmo código opera sobre tipos diferentes, parametrizados. Em Java, são os **generics** (`List<String>`). Em C++, os **templates** (`template <typename T> T maximo(T a, T b)`). Em Python o mesmo efeito vem naturalmente da tipagem dinâmica.

**4) Coerção (conversão implícita)**: um valor é convertido automaticamente, como em `3 + 2.5`, em que o inteiro vira real.

#### 5.1 Exemplo de polimorfismo de inclusão

**Java**
```java
abstract class Forma {
    abstract double area();
}
class Circulo extends Forma {
    private double r;
    Circulo(double r) { this.r = r; }
    double area() { return Math.PI * r * r; }
}
class Retangulo extends Forma {
    private double b, h;
    Retangulo(double b, double h) { this.b = b; this.h = h; }
    double area() { return b * h; }
}

Forma[] formas = { new Circulo(1), new Retangulo(2, 3) };
for (Forma f : formas)
    System.out.println(f.area());     // cada objeto executa SEU método area()
```

**C++** (aqui o `virtual` é indispensável)
```cpp
class Forma {
public:
    virtual double area() const = 0;      // virtual puro: classe abstrata
    virtual ~Forma() = default;
};
class Circulo : public Forma {
    double r;
public:
    explicit Circulo(double r) : r(r) {}
    double area() const override { return 3.14159265 * r * r; }
};
class Retangulo : public Forma {
    double b, h;
public:
    Retangulo(double b, double h) : b(b), h(h) {}
    double area() const override { return b * h; }
};

std::vector<std::unique_ptr<Forma>> formas;
formas.push_back(std::make_unique<Circulo>(1));
formas.push_back(std::make_unique<Retangulo>(2, 3));
for (const auto& f : formas)
    std::cout << f->area() << "\n";
```

**Python**
```python
import math
from abc import ABC, abstractmethod

class Forma(ABC):
    @abstractmethod
    def area(self): ...

class Circulo(Forma):
    def __init__(self, r): self.r = r
    def area(self): return math.pi * self.r ** 2

class Retangulo(Forma):
    def __init__(self, b, h): self.b, self.h = b, h
    def area(self): return self.b * self.h

for f in [Circulo(1), Retangulo(2, 3)]:
    print(f.area())
```

#### 5.2 Diferenças entre linguagens que caem em prova

- **Java:** todos os métodos de instância são “virtuais” por padrão (ligação dinâmica automática), exceto os `private`, `static` e `final`. Métodos `static` não são sobrescritos, e sim **ocultados** (_method hiding_).
- **C++:** só há ligação dinâmica se o método da base for declarado **`virtual`** e a chamada ocorrer **por ponteiro ou referência**. Sem `virtual`, a chamada é resolvida estaticamente pelo tipo declarado do ponteiro, e roda o método da base. Se você passar um objeto derivado **por valor** a uma função que espera a base, ocorre o **fatiamento (_slicing_)**: a parte derivada é descartada. **Destrutor da classe base deve ser virtual** quando se usa herança polimórfica. Caso contrário, `delete` por ponteiro da base não chama o destrutor da derivada (comportamento indefinido, com vazamento de recursos). A palavra `override` (C++11) faz o compilador verificar se de fato há sobrescrita.
- **Python:** a ligação é sempre dinâmica. Python **não tem sobrecarga por assinatura**: se você define dois métodos com o mesmo nome, o último substitui o anterior. Simula-se com parâmetros padrão, `*args` ou `functools.singledispatch`. Vale o **duck typing**: “se anda como pato e grasna como pato, é um pato”. Qualquer objeto que tenha o método necessário serve, **sem exigir relação de herança**. Python permite **sobrecarga de operadores** com métodos especiais (`__add__`, `__eq__`, `__str__`, `__len__`, `__lt__`...), assim como C++ (`operator+`), enquanto Java **não** permite sobrecarga de operadores definida pelo usuário (apenas `+` já é sobrecarregado para `String`).

#### 5.3 Conversão entre tipos de objetos

- **Upcasting** (da subclasse para a superclasse): é implícito e sempre seguro.
- **Downcasting** (da superclasse para a subclasse): exige conversão explícita e pode falhar. Em Java, `(Cachorro) animal` lança `ClassCastException` se o objeto real não for um Cachorro, e verifica-se antes com `instanceof`. Em C++, usa-se `dynamic_cast<Cachorro*>(ptr)`, que retorna `nullptr` se a conversão for inválida (exige classe polimórfica). Em Python, usa-se `isinstance(obj, Cachorro)`.

### 6. Classes abstratas e interfaces

#### 6.1 Classe abstrata

Classe que **não pode ser instanciada** e serve como base conceitual para outras. Pode conter métodos concretos (com implementação), atributos e construtores, e métodos **abstratos** (sem implementação), que as subclasses concretas são obrigadas a implementar.

- **Java:** palavra-chave `abstract` na classe e no método (`abstract double area();`).
- **C++:** uma classe é abstrata se tiver ao menos um **método virtual puro**: `virtual double area() const = 0;`.
- **Python:** herda de `abc.ABC` e usa o decorador `@abstractmethod`. Tentar instanciar uma classe com métodos abstratos pendentes gera `TypeError`.

#### 6.2 Interface

Interface é um **contrato**: define _o quê_ uma classe deve fazer, sem dizer _como_. Qualquer classe que a implemente se compromete a fornecer os métodos declarados. Interfaces permitem polimorfismo entre classes sem relação de herança e são o instrumento do **baixo acoplamento** (programa-se para a interface, não para a implementação).

**Java** (suporte nativo)

java

```java
interface Voador {
    void voar();                                  // implicitamente public abstract
    default void pousar() { System.out.println("Pousando"); }   // Java 8+
}
interface Nadador { void nadar(); }

class Pato extends Animal implements Voador, Nadador {
    public void voar()  { System.out.println("Voando"); }
    public void nadar() { System.out.println("Nadando"); }
}
```

Regras em Java: uma classe **estende** uma classe (`extends`) e **implementa** interfaces (`implements`). Uma interface **estende** outras interfaces, inclusive várias. Os métodos de interface são **públicos** por padrão, e os campos são implicitamente `public static final` (constantes). Desde o Java 8, interfaces podem ter métodos `default` e `static`, e desde o Java 9, métodos `private`.

**C++** não tem a palavra `interface`: o equivalente é uma classe com **apenas métodos virtuais puros** (e destrutor virtual).

cpp

```cpp
class Voador {
public:
    virtual void voar() = 0;
    virtual ~Voador() = default;
};
```

**Python:** usa classes abstratas (ABC) ou, mais modernamente, `typing.Protocol` (tipagem estrutural, na linha do duck typing).

#### 6.3 Classe abstrata versus interface (pergunta frequente)

|Aspecto|Classe abstrata|Interface|
|---|---|---|
|Estado (atributos de instância)|Pode ter|Não (em Java, apenas constantes)|
|Construtor|Pode ter|Não|
|Métodos concretos|Sim|Em Java, só `default`, `static` ou `private`|
|Herança múltipla|Não em Java (uma só)|Sim, uma classe implementa várias|
|Relação|“é um” (especialização, base comum)|“sabe fazer” (capacidade, contrato)|
|Uso típico|Compartilhar código e estado entre classes parecidas|Garantir um comportamento entre classes diversas|

### 7. Relacionamentos entre classes e UML

Num **diagrama de classes UML**, cada classe é um retângulo dividido em três compartimentos (nome, atributos e métodos). A visibilidade é indicada por `+` (público), `-` (privado), `#` (protegido) e `~` (pacote). Nome de classe abstrata e de método abstrato aparece em _itálico_. Os relacionamentos principais:

- **Associação:** relação genérica entre classes (Cliente _faz_ Pedido), representada por uma linha, com **multiplicidade** (`1`, `0..1`, `*`, `1..*`).
- **Agregação:** relação “todo-parte” **fraca**. A parte pode existir sem o todo (Time _tem_ Jogadores). Losango **vazio** do lado do todo.
- **Composição:** relação “todo-parte” **forte**. A parte não existe sem o todo, e morre junto com ele (Pedido _compõe-se de_ Itens de pedido). Losango **preenchido** do lado do todo.
- **Generalização (herança):** seta com **triângulo vazio** apontando para a superclasse.
- **Realização (implementação de interface):** seta **tracejada** com triângulo vazio apontando para a interface.
- **Dependência:** seta tracejada com ponta aberta, indicando que uma classe usa outra de forma temporária (como parâmetro de método).

### 8. Outros conceitos relevantes

**Tratamento de exceções.** É o mecanismo para lidar com erros em tempo de execução. Java: `try { ... } catch (Excecao e) { ... } finally { ... }`, com exceções **verificadas** (_checked_, que o compilador obriga a tratar ou declarar com `throws`, como `IOException`) e **não verificadas** (_unchecked_, herdeiras de `RuntimeException`, como `NullPointerException`). O bloco `finally` executa sempre, com ou sem exceção. C++ usa `try/catch/throw`. Python usa `try/except/else/finally` e `raise`.

**Coleções e generics** em Java (`ArrayList<T>`, `HashMap<K,V>`, `HashSet<T>`) e a biblioteca padrão (STL) de C++ (`vector`, `map`, `set`). Em Python, `list`, `dict`, `set` e `tuple` (esta imutável).

**Princípios SOLID**, que sintetizam o bom projeto OO:

- **S**ingle Responsibility: uma classe, uma razão para mudar.
- **O**pen/Closed: aberta para extensão, fechada para modificação.
- **L**iskov Substitution: subclasses substituem superclasses sem quebrar o programa.
- **I**nterface Segregation: várias interfaces pequenas e específicas são melhores que uma grande e genérica.
- **D**ependency Inversion: dependa de abstrações, não de implementações concretas.

**Padrões de projeto (design patterns)** são soluções reutilizáveis para problemas recorrentes. Os 23 clássicos da _Gang of Four_ dividem-se em criacionais (Singleton, Factory Method, Abstract Factory, Builder, Prototype), estruturais (Adapter, Decorator, Facade, Composite, Proxy...) e comportamentais (Observer, Strategy, Template Method, Command, Iterator...). Nas provas, os mais comuns são **Singleton** (garante uma única instância), **Factory**, **Observer** e **Strategy**, geralmente cobrados pela definição.

**Coesão e acoplamento:** uma boa classe tem **alta coesão** (responsabilidades bem focadas) e **baixo acoplamento** (pouca dependência de outras classes).

**Genéricos e variância.** Em Java, `List<Animal>` **não** é supertipo de `List<Cachorro>`, mesmo que Cachorro herde de Animal. É preciso usar curingas (`List<? extends Animal>`).

---

## COMPARAÇÃO RESUMIDA: C++, PYTHON E JAVA

| **Característica**              | **C++**                                                               | **Python**                                               | **Java**                                          |
| ------------------------------- | --------------------------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------- |
| Execução                        | Compilado para código nativo                                          | Interpretado (bytecode na VM CPython)                    | Compilado para bytecode, executado na JVM         |
| Tipagem                         | Estática, fraca em alguns aspectos (conversões implícitas, ponteiros) | Dinâmica e forte                                         | Estática e forte                                  |
| Gerenciamento de memória        | Manual (`new`/`delete`) ou RAII e ponteiros inteligentes              | Automático (contagem de referências + coletor de ciclos) | Automático (garbage collector)                    |
| Herança múltipla de classes     | Sim (com herança virtual para o diamante)                             | Sim (MRO C3)                                             | Não (apenas de interfaces)                        |
| Interfaces                      | Classes com virtuais puros                                            | ABC e Protocol                                           | Palavra-chave `interface`                         |
| Sobrecarga de métodos           | Sim                                                                   | Não (último método vence)                                | Sim                                               |
| Sobrecarga de operadores        | Sim                                                                   | Sim (métodos `__x__`)                                    | Não                                               |
| Ponteiros                       | Sim, aritmética de ponteiros                                          | Não (referências)                                        | Não (referências, sem aritmética)                 |
| Métodos polimórficos por padrão | Não, exige `virtual`                                                  | Sim                                                      | Sim (exceto `private`, `static`, `final`)         |
| Privacidade                     | Real (`private`)                                                      | Por convenção                                            | Real (`private`)                                  |
| Classe raiz comum               | Não existe                                                            | `object`                                                 | `Object`                                          |
| Genéricos                       | Templates (resolvidos em compilação)                                  | Duck typing                                              | Generics (com apagamento de tipo, _type erasure_) |

---

## MACETES E ARMADILHAS PARA A PROVA

1. **Classe é o molde, objeto é a instância.** Atributos de classe (`static`) são compartilhados, e os de instância são individuais.
2. **Encapsulamento = esconder os atributos e expor métodos.** Na dúvida sobre qual pilar uma frase descreve: “esconder detalhes internos” é encapsulamento, “mostrar só o essencial” é abstração, “reaproveitar de uma classe pai” é herança e “mesma chamada, comportamentos distintos” é polimorfismo.
3. **Sobrecarga ≠ sobrescrita.** Sobrecarga: mesma classe, assinaturas diferentes, ligação em tempo de compilação. Sobrescrita: subclasse, mesma assinatura, ligação em tempo de execução. O tipo de retorno não diferencia sobrecargas.
4. **C++ sem `virtual` não há polimorfismo dinâmico.** Lembre também do destrutor virtual na base e do fatiamento ao passar por valor.
5. **Java não tem herança múltipla de classes**, mas implementa várias interfaces. C++ e Python têm herança múltipla.
6. **Classe abstrata não pode ser instanciada** (`new Forma()` é erro em Java; em C++, erro de compilação; em Python, `TypeError`).
7. **Em Java, `==` compara referências, `equals` compara conteúdo.** Dois `new String("a")` são `==` falso e `equals` verdadeiro.
8. **Em Java, uma interface tem métodos públicos e abstratos por padrão**, e campos constantes. Desde o Java 8, aceita `default`.
9. **Construtor não tem tipo de retorno** (nem `void`) e tem o nome da classe em Java e C++. Em Python, é `__init__`, e o `self` é obrigatório.
10. **Ordem de construção: base primeiro, derivada depois. Destruição: o inverso.**
11. **Python: `self` explícito, sem sobrecarga real, sem privacidade real, e duck typing.** `__init__` não é, tecnicamente, o construtor.
12. **Agregação (losango vazio) versus composição (losango cheio)**: pergunte “se o todo morre, a parte morre junto?”. Se sim, composição.
13. **“É um” → herança. “Tem um” → composição ou agregação.**
14. **Polimorfismo de inclusão exige referência da superclasse apontando para objeto da subclasse**, e o método executado depende do tipo **real** do objeto, não do tipo declarado da variável (exceto em C++ sem `virtual`, e em métodos `static` ou campos, que não são polimórficos).
15. **Campos não são polimórficos em Java:** se a subclasse declara um campo com o mesmo nome do da superclasse, ele apenas oculta o da superclasse, e o acesso depende do tipo declarado da referência.
16. **`final` em Java**: variável = constante, método = não pode ser sobrescrito, classe = não pode ser herdada.
17. **`protected` em Java** também dá acesso a classes do mesmo pacote, ao contrário de C++.

---

**Sobre incidência na prova:** vale o mesmo cuidado das partes anteriores. Não consegui contar por tema as questões de 2020 a 2026, então não posso afirmar percentuais. Por experiência com provas de informática desse perfil, é plausível que conceitos de **herança, polimorfismo (sobrecarga versus sobrescrita), classes abstratas versus interfaces** e **encapsulamento** apareçam de forma recorrente, geralmente em afirmações para julgar certo ou errado, ou com um trecho de código para prever a saída. Se você me enviar as provas em PDF, faço a contagem real por assunto.