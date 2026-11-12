## PARTE E: ANÁLISE E PROJETO ORIENTADOS A OBJETO E UML

### 1. Fundamentos da orientação a objetos

Antes de chegar à UML, é preciso dominar o vocabulário da orientação a objetos (OO), porque os diagramas são apenas a forma gráfica de expressar esses conceitos.

Um **objeto** é uma entidade que combina **estado** (os dados que ele guarda, chamados de **atributos**) e **comportamento** (as operações que ele realiza, chamadas de **métodos**), com uma **identidade** própria que o distingue de outros objetos, mesmo que tenham o mesmo estado. Um cliente específico, a conta nº 123 ou o pedido 4587 são objetos. Uma **classe** é o **molde (tipo)** a partir do qual os objetos são criados: define os atributos e métodos que todos os seus objetos terão. O objeto é a **instância** da classe, e o ato de criá-lo é a **instanciação**. Os objetos se comunicam por meio de **mensagens**, ou seja, um objeto solicita a outro que execute um de seus métodos.

Os quatro pilares clássicos da OO, extremamente cobrados, são:

A **abstração** consiste em representar apenas as características **essenciais** de uma entidade para o problema em questão, ignorando os detalhes irrelevantes. O **encapsulamento** esconde o estado interno do objeto e expõe apenas uma interface pública controlada, protegendo a integridade dos dados (em linguagens como Java e C#, usa-se _private_, _protected_ e _public_). A **herança** permite que uma classe (subclasse ou classe derivada) **reaproveite e estenda** atributos e métodos de outra (superclasse ou classe base), modelando uma relação do tipo “**é um**” (um Gerente **é um** Funcionário). O **polimorfismo** (do grego, “muitas formas”) permite que **a mesma mensagem produza comportamentos diferentes** conforme o objeto que a recebe: por exemplo, o método `calcularArea()` executa uma fórmula diferente para um círculo e para um retângulo, embora seja chamado do mesmo jeito. Ele aparece como **sobrecarga** (_overload_, mesmo nome com parâmetros diferentes, resolvido em tempo de compilação) e **sobrescrita** (_override_, a subclasse redefine um método da superclasse, resolvido em tempo de execução).

Conceitos complementares: uma **classe abstrata** não pode ser instanciada e serve de base para outras, podendo ter métodos abstratos (sem implementação). Uma **interface** define um **contrato** (conjunto de operações) sem implementação, que as classes se comprometem a cumprir. A **associação** é um relacionamento estrutural entre classes. A **agregação** é uma associação do tipo “**tem um**” em que as partes **podem existir independentemente** do todo (um departamento tem professores, mas se o departamento acabar, os professores continuam existindo). A **composição** é uma agregação **forte**, em que as partes **não existem sem o todo** e seu ciclo de vida depende dele (um pedido e seus itens: se o pedido é excluído, os itens vão junto).

#### Análise OO versus projeto OO

A **Análise Orientada a Objetos (AOO)** foca no **domínio do problema**: identifica os **conceitos do mundo real** (classes conceituais), seus atributos e relacionamentos, e os requisitos (casos de uso), respondendo “**o quê**”. O **Projeto Orientado a Objetos (POO)** foca na **solução**: define as classes de software, suas responsabilidades, colaborações, interfaces e a arquitetura, considerando a tecnologia real, respondendo “**como**”. A transição entre eles é contínua, e a UML é usada nas duas fases, com **níveis de detalhe diferentes** (modelo conceitual na análise, modelo de classes de projeto no projeto).

#### Princípios e padrões de projeto OO

Relacionados ao projeto OO, e frequentemente cobrados, estão os princípios **SOLID**: **S**ingle Responsibility (cada classe com uma única responsabilidade), **O**pen/Closed (aberta para extensão, fechada para modificação), **L**iskov Substitution (subclasses devem poder substituir suas superclasses sem quebrar o comportamento), **I**nterface Segregation (várias interfaces específicas são melhores que uma genérica) e **D**ependency Inversion (depender de abstrações, não de implementações concretas). Os padrões **GRASP** (Larman) orientam a atribuição de responsabilidades (Especialista, Criador, Controlador, Baixo Acoplamento, Alta Coesão, Polimorfismo, entre outros). Os **padrões de projeto do GoF** (_Gang of Four_: Gamma, Helm, Johnson e Vlissides, 1994) somam 23 padrões em três categorias: **criacionais** (como Singleton, Factory Method, Abstract Factory, Builder, Prototype), **estruturais** (como Adapter, Decorator, Facade, Composite, Proxy) e **comportamentais** (como Observer, Strategy, Command, Template Method, State). Os conceitos de **acoplamento e coesão**, estudados no projeto estruturado, **continuam valendo** na OO: busca-se baixo acoplamento entre classes e alta coesão dentro de cada uma.

### 2. A UML: conceito, origem e características

A **UML (Unified Modeling Language, Linguagem de Modelagem Unificada)** é uma **linguagem gráfica padronizada** para **especificar, visualizar, construir e documentar** os artefatos de um sistema de software. Ela surgiu da unificação, em meados dos anos 1990, dos métodos de **Grady Booch**, **James Rumbaugh** (OMT) e **Ivar Jacobson** (OOSE, de casos de uso), na empresa Rational. Foi adotada pela **OMG (Object Management Group)** em 1997 como padrão. A UML 2.0 foi lançada em 2005, e a versão mais recente é a 2.5.x. Ela também é padrão **ISO/IEC 19505**.

Um ponto crucial e muito cobrado: **a UML é uma linguagem de modelagem, e não uma metodologia ou um processo de desenvolvimento**. Ela diz **como desenhar** os modelos, mas **não diz o que fazer, em que ordem, nem quando**. Por isso, ela é combinada com um processo, como o **RUP (Rational Unified Process)** ou o **UP (Unified Process)**, que é **orientado a casos de uso, centrado na arquitetura, iterativo e incremental**.

Outras características: a UML é **independente de linguagem de programação** e de processo, é **extensível** (por meio de **estereótipos**, **valores atribuídos** e **restrições**, agrupados em **perfis**), e pode ser usada em três modos: como **esboço** (rascunho para comunicação), como **projeto detalhado** (para gerar código) e como **linguagem de programação** (modelos executáveis, mais raro). A **OCL (Object Constraint Language)** é a linguagem formal, parte da UML, usada para expressar **restrições** nos modelos. A **MDA (Model Driven Architecture)**, também da OMG, é a abordagem de desenvolvimento dirigido por modelos.

### 3. Os diagramas da UML 2

A UML 2 define **14 diagramas**, divididos em duas grandes categorias. A UML 1.x tinha **9**, e a prova pode cobrar essa diferença (na UML 2, o diagrama de colaboração passou a se chamar **diagrama de comunicação**, e surgiram, entre outros, os de estrutura composta, de visão geral de interação e de temporização).

**Diagramas estruturais** (descrevem a estrutura estática do sistema, os elementos e seus relacionamentos): **classes**, **objetos**, **componentes**, **implantação**, **pacotes**, **estrutura composta** e **perfil**. **Diagramas comportamentais** (descrevem o comportamento dinâmico e as interações): **casos de uso**, **atividades**, **máquina de estados**, e os quatro **diagramas de interação**: **sequência**, **comunicação**, **visão geral de interação** e **temporização**.

Um macete: os de interação são subconjunto dos comportamentais (sequência, comunicação, visão geral de interação e temporização), e os **sete estruturais** são: classes, objetos, componentes, implantação, pacotes, estrutura composta e perfil.

#### 3.1. Diagrama de casos de uso

Ele captura os **requisitos funcionais** do ponto de vista do usuário, mostrando **quem** usa o sistema e **o que** ele faz, sem entrar em como. É o diagrama mais usado na fase de análise e é a base dos processos como o RUP.

Seus elementos são o **ator** (um papel externo que interage com o sistema, que pode ser uma pessoa, outro sistema ou um dispositivo; representado por um bonequinho; **não é um usuário específico, mas um papel**), o **caso de uso** (uma funcionalidade que entrega **valor observável** ao ator; representado por uma **elipse**), a **fronteira do sistema** (um retângulo que delimita o escopo, com os casos de uso dentro e os atores fora) e os **relacionamentos**.

Os relacionamentos são: a **associação** (linha simples entre ator e caso de uso, indicando participação); a **inclusão (include)** (um caso de uso **sempre** executa outro; é **obrigatório**; a seta tracejada vai do **caso de uso base para o incluído**; serve para reaproveitar comportamento comum, como “Autenticar usuário”); a **extensão (extend)** (um caso de uso **pode** ser estendido por outro **sob certa condição**; é **opcional**; a seta tracejada vai do **caso de uso de extensão para o base**; modela comportamento excepcional ou opcional); e a **generalização** (relação de herança entre atores ou entre casos de uso, com seta de triângulo vazado).

O macete clássico: **include = obrigatório (sempre), extend = opcional (condicional)**, e as **setas apontam em sentidos opostos** (include vai para o incluído; extend vai para o base). Cada caso de uso é complementado por uma **descrição textual (especificação)**, com nome, atores, **pré-condições**, **fluxo principal (básico)**, **fluxos alternativos**, **fluxos de exceção** e **pós-condições**.

#### 3.2. Diagrama de classes

É o diagrama **central** da modelagem OO e o **mais cobrado**. Mostra as **classes**, seus **atributos**, **operações** e os **relacionamentos** entre elas, em uma visão **estática**.

A classe é um retângulo com três compartimentos: **nome** (em negrito, centralizado; em _itálico_ se for **abstrata**), **atributos** e **operações (métodos)**. A **visibilidade** de cada membro é indicada por um símbolo: **+** público, **−** privado, **#** protegido, **~** pacote. Os atributos têm formato `visibilidade nome : tipo = valorInicial`, e os métodos `visibilidade nome(parâmetros) : tipoRetorno`. Membros **estáticos** aparecem **sublinhados**. Uma **interface** é indicada com o estereótipo `<<interface>>`.

A **multiplicidade** indica quantas instâncias de uma classe se relacionam com a outra: **1** (exatamente um), **0..1** (zero ou um), ***** ou **0..*** (zero ou mais), **1..*** (um ou mais), e **n..m** (intervalo).

Os relacionamentos, com sua notação, são os pontos mais cobrados:

|Relacionamento|Notação|Significado|
|---|---|---|
|**Associação**|Linha simples (pode ter seta de navegabilidade)|Relação estrutural entre classes (“usa”, “conhece”)|
|**Agregação**|Linha com **losango vazio** no lado do **todo**|“Tem um”; partes existem sem o todo|
|**Composição**|Linha com **losango cheio** no lado do **todo**|“É parte de”; partes não existem sem o todo|
|**Generalização (herança)**|Linha **cheia** com **triângulo vazado** apontando para a superclasse|“É um”|
|**Realização**|Linha **tracejada** com **triângulo vazado** apontando para a interface|Classe implementa uma interface|
|**Dependência**|Linha **tracejada** com **seta aberta** apontando para o elemento do qual se depende|Uma classe usa outra temporariamente (como parâmetro, por exemplo)|

Uma **classe associativa** é a classe ligada a uma associação, para guardar atributos da própria relação (por exemplo, “Matrícula” entre Aluno e Disciplina, com a nota). O diagrama de classes pode ser usado em **três níveis**: **conceitual** (análise, sem detalhes técnicos), **de especificação/projeto** (com interfaces e métodos) e **de implementação** (com detalhes da linguagem).

#### 3.3. Diagrama de objetos

É um **retrato (snapshot)** do sistema em um **instante específico**, mostrando **instâncias** das classes e os valores dos seus atributos. A notação é parecida com a de classes, mas o nome do objeto aparece **sublinhado** no formato `nomeObjeto : NomeClasse`. É útil para **validar** o diagrama de classes com exemplos concretos.

#### 3.4. Diagrama de sequência

É o diagrama de interação **mais conhecido**. Mostra **como os objetos colaboram ao longo do tempo** para realizar um cenário, **enfatizando a ordem temporal** das mensagens. O tempo corre **de cima para baixo**.

Seus elementos são os **participantes** (objetos, atores, retângulos no topo), as **linhas de vida** (linhas tracejadas verticais que representam a existência do participante), as **barras de ativação (foco de controle)** (retângulos finos sobre a linha de vida, indicando que o objeto está executando algo) e as **mensagens** (setas horizontais). A **mensagem síncrona** (o remetente espera a resposta) tem **ponta cheia**; a **assíncrona** (o remetente segue sem esperar) tem **ponta aberta**; a **mensagem de retorno** é **tracejada**. Existe ainda a mensagem de **criação** (aponta para o início de um novo objeto) e de **destruição** (marcada com um **X**).

Os **fragmentos combinados** modelam lógica de controle dentro do diagrama: **alt** (alternativas, equivalente a if/else), **opt** (opcional), **loop** (repetição), **par** (paralelismo), **ref** (referência a outro diagrama), **break** e **neg**.

#### 3.5. Diagrama de comunicação (antigo colaboração)

Também é de interação, mas **enfatiza a organização estrutural** dos objetos (quem se relaciona com quem), e não o tempo. As mensagens são **numeradas** (1, 1.1, 2, ...) para indicar a ordem. Ele é **semanticamente equivalente** ao de sequência para muitos fins, mas com ênfase diferente. Macete: **sequência = ênfase no tempo; comunicação = ênfase nos relacionamentos**.

#### 3.6. Diagrama de atividades

Modela o **fluxo de trabalho** (_workflow_) ou a **lógica de um processo ou algoritmo**, mostrando a sequência de ações, decisões e paralelismos. É parecido com o fluxograma, mas com recursos para **concorrência**.

Seus elementos são o **nó inicial** (círculo cheio), o **nó final de atividade** (círculo com borda, “alvo”), o **nó final de fluxo** (círculo com X, encerra só aquele fluxo), a **ação/atividade** (retângulo arredondado), o **nó de decisão** (losango com uma entrada e várias saídas com **condições de guarda** entre colchetes), o **nó de junção/merge** (losango que reúne fluxos alternativos), a **barra de bifurcação (fork)** (divide um fluxo em fluxos **paralelos**), a **barra de junção (join)** (sincroniza fluxos paralelos, esperando todos terminarem), os **objetos** (dados que trafegam) e as **raias (partições, _swimlanes_)** (divisões que indicam **quem é responsável** por cada ação). Macete: **decisão/merge = losango; fork/join = barra**.

#### 3.7. Diagrama de máquina de estados

Mostra os **estados** pelos quais **um objeto** passa durante sua vida e as **transições** entre eles, disparadas por **eventos**. É ideal para objetos com **comportamento dependente do estado** (um pedido que passa por “Novo”, “Pago”, “Enviado”, “Entregue”, “Cancelado”).

Elementos: o **estado inicial** (círculo cheio), o **estado final** (círculo com borda), os **estados** (retângulos arredondados, que podem ter ações internas `entry/`, `do/` e `exit/`), as **transições** (setas) rotuladas com `evento [guarda] / ação`, e os **estados compostos** (que contêm subestados). Em UML 1.x, chamava-se **diagrama de estados** (_statechart_).

#### 3.8. Diagrama de componentes

Mostra a **organização física e modular** do software em **componentes** (unidades substituíveis que encapsulam implementação e expõem **interfaces fornecidas** e **requeridas**, representadas pela notação “pirulito” e “soquete”) e suas **dependências**. É usado na **arquitetura e no projeto**.

#### 3.9. Diagrama de implantação (deployment)

Mostra a **topologia física** do sistema em execução: os **nós** (hardware ou ambientes de execução, desenhados como cubos 3D, como servidores e dispositivos), os **artefatos** (arquivos executáveis, bibliotecas, bancos de dados) alocados nos nós, e as **conexões** (protocolos de comunicação) entre eles.

#### 3.10. Diagrama de pacotes

Organiza elementos do modelo em **pacotes** (agrupamentos lógicos, desenhados como uma pasta) e mostra as **dependências** entre eles. Serve para dar uma visão de **alto nível** da arquitetura em camadas e para **reduzir a complexidade** dos modelos grandes.

#### 3.11. Diagramas menos frequentes

O **diagrama de estrutura composta** mostra a **estrutura interna** de uma classe ou componente (suas **partes**, **portas** e **conectores**). O **diagrama de visão geral de interação** combina o diagrama de atividades com **referências a diagramas de interação**, mostrando como cenários se encadeiam. O **diagrama de temporização** mostra, com ênfase no **tempo**, as mudanças de estado de objetos ao longo de uma linha temporal, útil para sistemas de tempo real. O **diagrama de perfil** define **extensões** da UML (estereótipos, restrições, valores atribuídos) para um domínio específico.

#### Diagramas e técnicas relacionados

Fora dos 14, aparecem como correlatos os **cartões CRC (Classe-Responsabilidade-Colaborador)**, técnica simples de brainstorming para descobrir classes; a **visão 4+1 de Kruchten** (visões lógica, de processos, de desenvolvimento, física e de cenários/casos de uso, usada na arquitetura e no RUP); o **diagrama entidade-relacionamento (DER)**, que não é da UML, mas é comparado ao diagrama de classes; e o **modelo conceitual de domínio** do método de Larman.

#### Quadro-resumo: qual diagrama usar

| Quero mostrar...                              | Diagrama               |
| --------------------------------------------- | ---------------------- |
| Quem usa o sistema e para quê                 | **Casos de uso**       |
| Estrutura estática: classes e relacionamentos | **Classes**            |
| Exemplo concreto, em um instante              | **Objetos**            |
| Troca de mensagens no tempo                   | **Sequência**          |
| Troca de mensagens com ênfase nas ligações    | **Comunicação**        |
| Fluxo de trabalho, algoritmo, paralelismo     | **Atividades**         |
| Ciclo de vida de um objeto                    | **Máquina de estados** |
| Módulos de software e interfaces              | **Componentes**        |
| Hardware e distribuição física                | **Implantação**        |
| Agrupamento lógico do modelo                  | **Pacotes**            |