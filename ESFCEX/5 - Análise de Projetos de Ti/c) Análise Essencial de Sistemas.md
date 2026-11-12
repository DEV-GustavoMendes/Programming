## PARTE C: ANÁLISE ESSENCIAL DE SISTEMAS

### 1. Conceitos e definições básicas

Para entender a análise essencial, é preciso voltar um passo na história. A **Análise Estruturada** nasceu nos anos 1970, com Tom DeMarco (_Structured Analysis and System Specification_, 1978) e Gane e Sarson (1979), como resposta ao caos do desenvolvimento da época. A ideia central era modelar o sistema de forma gráfica, dividindo-o em partes menores (**particionamento**) e representando o que ele faz por meio do fluxo de dados, antes de se escrever qualquer código. O problema da abordagem clássica é que ela começava modelando o **sistema atual** (físico, com seus computadores, formulários e departamentos), depois o sistema atual lógico, depois o novo sistema lógico e por fim o novo sistema físico. Isso gerava muito trabalho de modelagem sobre algo que seria descartado e deixava os analistas presos à tecnologia existente.

A **Análise Essencial**, proposta por **Stephen McMenamin e John Palmer** (_Essential Systems Analysis_, 1984) e depois incorporada por **Edward Yourdon** na **Análise Estruturada Moderna** (1989), propõe um caminho mais direto: modelar **somente a essência** do sistema, ou seja, aquilo que ele **precisa fazer independentemente da tecnologia usada para fazê-lo**. A essência é o que o sistema faria se fosse implementado com **tecnologia perfeita**. A análise essencial, portanto, **não passa pela modelagem do sistema físico atual**; ela vai direto ao modelo lógico do que o negócio exige.

O conceito de **tecnologia perfeita** é fundamental e cai em prova. Imagine um **processador perfeito** (que executa qualquer processamento instantaneamente, sem custo, sem falhas) e uma **memória perfeita** (que armazena qualquer quantidade de dados, sem custo, sem perda, com acesso imediato). Com essa tecnologia hipotética, não haveria necessidade de arquivos de backup, índices, controles de erro de hardware, paginação ou otimizações de desempenho. Tudo o que sobra no modelo, ou seja, as funções e dados que o sistema continuaria precisando, é a **essência**. Os requisitos que existem apenas por causa das limitações da tecnologia real são chamados de **requisitos de implementação** e ficam para a fase de projeto.

O **modelo essencial** é composto por dois submodelos, e a prova gosta muito dessa divisão:

O **modelo ambiental** define a **fronteira** entre o sistema e o mundo externo. Responde à pergunta “o que está dentro e o que está fora do sistema, e como eles se comunicam?”. É composto pela **declaração de objetivos** (um texto curto e claro com o propósito do sistema), pelo **diagrama de contexto** (que mostra o sistema como um único processo cercado de entidades externas) e pela **lista de eventos** (que enumera os estímulos aos quais o sistema deve responder).

O **modelo comportamental** define **o que acontece dentro** da fronteira, ou seja, como o sistema responde internamente aos estímulos. É composto pelo **DFD** (diagrama de fluxo de dados) particionado por eventos, pelo **DER** (diagrama de entidade-relacionamento, com a estrutura dos dados armazenados), pelo **dicionário de dados** (que define cada elemento de dado) e pelas **especificações de processo** (que descrevem a lógica de cada processo primitivo, usando português estruturado, tabelas ou árvores de decisão). Em algumas abordagens, entra também o **DTE** (diagrama de transição de estados), para sistemas com comportamento dependente do tempo e do estado.

### 2. Estratégias e ferramentas da modelagem essencial

#### Estratégia de particionamento por eventos

Na análise estruturada clássica, o sistema era dividido de cima para baixo (_top-down_) de modo mais intuitivo e funcional, o que dependia muito da percepção do analista e produzia modelos diferentes conforme quem os fizesse. A análise essencial adota o **particionamento por eventos** (_event partitioning_), uma estratégia mais sistemática: identifica-se primeiro **todos os eventos** aos quais o sistema precisa responder e, para **cada evento**, cria-se **um processo** no DFD, que representa a resposta do sistema a esse evento. O resultado é um modelo mais objetivo e reprodutível.

Um **evento** é algo que acontece no mundo externo e exige uma resposta do sistema. Eles se classificam em três tipos:

Os **eventos externos (ou de fluxo de dados)** são disparados por uma entidade externa, que envia dados ao sistema (o cliente faz um pedido, o funcionário informa as horas trabalhadas). Os **eventos temporais** são disparados pela passagem do tempo (gerar a folha de pagamento todo dia 30, emitir um relatório às 18h). Os **eventos de controle** (ou de fluxo de controle) são disparados por uma condição interna ou sinal (o estoque ficou abaixo do mínimo e é preciso gerar um pedido de compra). Na prática, o ponto mais cobrado é a ideia de que **o evento temporal é disparado pelo tempo, e não por uma pessoa**.

Cada evento é descrito na **lista de eventos** com a sua **resposta** esperada. Um exemplo: “Cliente faz pedido” é o evento e “o sistema registra o pedido e reserva o estoque” é a resposta.

#### Sequência da construção do modelo essencial

A ordem típica de construção é: declaração de objetivos, diagrama de contexto, lista de eventos, DFD preliminar (um processo por evento), DFD refinado e nivelado, DER e dicionário de dados, e por fim as especificações de processo. Os passos são iterativos e a validação com o usuário ocorre ao longo do caminho.

#### Ferramentas da modelagem essencial

As ferramentas principais são o **diagrama de contexto**, o **DFD**, o **DER**, o **DTE**, o **dicionário de dados**, a **especificação de processos** (português estruturado, tabelas de decisão, árvores de decisão) e a **lista de eventos**.

### 3. Atividades essenciais

As **atividades essenciais** são as funções que o sistema precisaria executar mesmo com tecnologia perfeita. Elas se dividem em dois grupos, e essa divisão é um dos pontos mais cobrados do tema:

As **atividades fundamentais** (_core business_, ou atividades essenciais de negócio) são as que realizam o **propósito do sistema**, ou seja, as que respondem diretamente às necessidades do negócio e ao motivo pelo qual o sistema existe. Em um sistema de vendas, são “registrar pedido”, “faturar”, “calcular comissão”. Uma forma de testar: se a atividade existe para cumprir a missão do sistema, ela é fundamental.

As **atividades custodiais** são as que **mantêm os dados** necessários para que as atividades fundamentais possam ser executadas. São as atividades de **incluir, alterar, excluir e manter** os dados de apoio, como cadastros de clientes, produtos e fornecedores. Elas “tomam conta” (_custody_, custódia) dos dados. O cadastro de cliente, por si só, não é a razão de existir do sistema de vendas, mas é indispensável para ele.

Uma **dica de memória**: **fundamental é o “fazer o negócio”; custodial é o “cuidar dos dados”**.

Existe ainda um conceito relacionado que costuma aparecer: o **modelo de implementação do usuário** (ou modelo de implementação), que é a etapa posterior ao modelo essencial, em que se define **como** o sistema será implementado com a **tecnologia real**: o que será automatizado e o que será manual, a interface com o usuário, as restrições de desempenho e a fronteira homem-máquina. Esse modelo é construído **depois** do essencial e **parte dele**.

### 4. Diagrama de Fluxo de Dados (DFD)

O **DFD** é a ferramenta gráfica central da análise estruturada. Ele mostra **como os dados fluem** pelo sistema: de onde vêm, que processos os transformam, onde são armazenados e para onde vão. É uma visão **funcional**, voltada para dados em movimento, e **não mostra** a sequência de execução, o controle ou as decisões, ou seja, não é um fluxograma.

#### Componentes

O DFD tem quatro elementos, e a notação gráfica muda conforme o autor:

|Elemento|Yourdon/DeMarco|Gane & Sarson|
|---|---|---|
|**Processo** (transforma dados)|Círculo (bolha)|Retângulo com cantos arredondados|
|**Fluxo de dados** (dados em movimento)|Seta com nome|Seta com nome|
|**Depósito de dados** (dados em repouso)|Duas linhas paralelas|Retângulo aberto à direita|
|**Entidade externa** (terminador, fonte ou destino)|Retângulo|Retângulo (ou quadrado)|

O **processo** recebe dados de entrada, transforma-os e gera dados de saída. Seu nome deve ser **verbo + objeto** (“Calcular imposto”, “Emitir nota fiscal”). O **fluxo de dados** transporta dados entre os demais elementos e deve ter nome que descreva o conteúdo, normalmente um substantivo. O **depósito de dados** armazena dados para uso posterior (arquivo, tabela, banco de dados) e seu nome é um substantivo no plural (“Clientes”, “Pedidos”). A **entidade externa** (também chamada de **terminador**) é uma pessoa, organização ou outro sistema **fora da fronteira** do sistema que **fornece dados ou os recebe**. Ela não faz parte do sistema, e por isso o analista **não modela o que acontece dentro dela**.

#### Níveis do DFD e nivelamento

O DFD é construído em **níveis**, uma técnica chamada **nivelamento** (ou decomposição): partindo de uma visão geral e detalhando progressivamente.

O **diagrama de contexto** (também chamado de **nível 0** ou DFD de nível zero, dependendo do autor) mostra o sistema inteiro como **um único processo**, cercado de entidades externas, com os fluxos de entrada e saída. Ele define a fronteira do sistema e **não mostra depósitos de dados**. O **diagrama 0** (ou nível 1) é a **primeira explosão** do processo único do contexto, mostrando os principais processos, depósitos e fluxos. Cada processo do diagrama 0 pode então ser detalhado em um **diagrama filho** de nível inferior, e assim sucessivamente, até chegarem aos **processos primitivos** (ou funcionais), que não são mais decompostos e que têm uma **especificação de processo**.

A numeração segue a hierarquia: o processo 3 do diagrama 0 se detalha em 3.1, 3.2, 3.3, e assim por diante. Uma regra prática é manter cada diagrama com **no máximo cerca de 7 ± 2 processos**, para preservar a legibilidade (referência ao limite cognitivo de Miller).

#### Regras e erros comuns do DFD (muito cobrados)

O **balanceamento** é a regra mais importante: os fluxos de entrada e saída de um processo no nível superior devem **corresponder exatamente** aos fluxos de entrada e saída do diagrama que o detalha no nível inferior. Se isso não ocorre, o modelo está **desbalanceado**.

Outras regras e erros típicos: um processo **sem saída** é chamado de **buraco negro** (consome dados e nunca produz nada); um processo **sem entrada** é chamado de **milagre** (gera dados do nada); um processo com **entradas insuficientes** para gerar a saída é um **milagre parcial** (ou “cinza”). **Não pode haver fluxo direto entre dois depósitos**, **nem entre duas entidades externas**, **nem entre uma entidade externa e um depósito**: todo fluxo precisa passar por um processo. Todo **processo** precisa ter pelo menos uma entrada e uma saída. **Nomes de fluxos e processos devem ser significativos** e únicos. Um fluxo que sai de um depósito e volta para ele não tem sentido sem um processo no meio. Um depósito que só tem fluxos de entrada ou só de saída merece atenção (pode indicar dado que nunca é consultado ou lido sem nunca ter sido gravado).

#### O que o DFD não mostra

Ele **não mostra** controle, sequência temporal, decisões nem repetições, e por isso não se confunde com fluxograma. Também **não representa a estrutura interna dos dados**, que fica a cargo do DER e do dicionário de dados.

### 5. Dicionário de dados

O **dicionário de dados (DD)** é um repositório que contém a **definição de todos os elementos de dados** usados no modelo: fluxos de dados, depósitos de dados, elementos de dados e, em algumas abordagens, também processos e entidades. Ele é o elemento que **dá precisão e consistência** ao DFD: o DFD diz que existe um fluxo chamado “Pedido”, e o dicionário diz **exatamente de que ele é composto**.

Sem o dicionário, o mesmo termo poderia ter significados diferentes para pessoas diferentes. Ele serve como contrato entre analistas, usuários e programadores.

#### O que o dicionário descreve

Para cada item, tipicamente registra o **nome**, **aliases** (outros nomes pelos quais é conhecido), a **descrição** em linguagem natural, a **composição** (de quais itens menores ele é formado), os **valores e unidades** possíveis, o **tamanho e formato**, onde é **usado** e informações complementares.

#### Notação do dicionário de dados (Yourdon/DeMarco)

Essa notação é cobrada com frequência, e vale decorar os símbolos:

|Símbolo|Significado|
|---|---|
|**=**|“é composto de” (definição)|
|**+**|“e” (concatenação, sequência)|
|**[ \| ]**|“ou” (seleção, uma das alternativas)|
|**{ }**|repetição (iteração, zero ou mais ocorrências; pode vir com limites, como 1{...}10)|
|**( )**|opcional (pode ou não estar presente)|
|_**...**_|comentário|
|**@**|identificador (campo-chave) em depósitos|

Um exemplo de definição:

```
Pedido = Número-do-Pedido + Data + Cliente + 1{Item-do-Pedido}20 + (Observação)
Cliente = Código + Nome + [Pessoa-Física | Pessoa-Jurídica]
Documento = [CPF | CNPJ]
```

Lendo a primeira linha: um pedido é composto de número, data, cliente, de um a vinte itens e, opcionalmente, uma observação.

#### Construção e utilização

A construção do dicionário é **incremental e paralela** à construção do DFD: à medida que surgem fluxos e depósitos, eles são definidos. A definição segue **top-down**: começa-se pelos itens de nível mais alto e decompõe-se até os **elementos de dados (itens elementares)**, que **não são mais divisíveis** no contexto do sistema (por exemplo, “Nome”, “CPF”, “Data”). O dicionário deve ser **mantido consistente** com o DFD (todo fluxo e depósito do DFD deve estar definido, e todo item definido deve ser usado em algum lugar) e **revisado com os usuários**. Na utilização, ele serve à **validação do modelo** (conferir consistência e completude), à **comunicação** da equipe, ao **projeto** do banco de dados e das interfaces, à **geração de código** em algumas ferramentas CASE e à **manutenção**, porque documenta o significado dos dados ao longo da vida do sistema.

### 6. Tabelas de decisão e árvores de decisão

Nos processos primitivos, é preciso descrever a **lógica** de transformação dos dados. Quando essa lógica envolve **muitas condições e combinações**, o texto corrido fica ambíguo e difícil de verificar. Para isso existem as ferramentas de especificação lógica: **português estruturado** (linguagem de programação simplificada, com sequência, decisão e repetição), **tabelas de decisão** e **árvores de decisão**.

#### Tabela de decisão

A **tabela de decisão** organiza em formato tabular as **condições** que podem ocorrer, as **ações** a executar e a **relação** entre elas. Ela tem quatro quadrantes: **condições** (parte superior esquerda, listando as condições a testar), **entradas de condição** (parte superior direita, com os valores V/F, S/N ou outros para cada regra), **ações** (parte inferior esquerda, listando as ações possíveis) e **entradas de ação** (parte inferior direita, marcando com X as ações a executar em cada regra). Cada coluna da parte direita é uma **regra**.

Um exemplo de política de desconto:

||R1|R2|R3|R4|
|---|---|---|---|---|
|Cliente é preferencial?|S|S|N|N|
|Compra acima de R$ 1.000?|S|N|S|N|
|**Desconto de 15%**|X||||
|**Desconto de 10%**||X|X||
|**Sem desconto**||||X|

Para **n condições binárias** (duas respostas possíveis cada), o número de regras possíveis (combinações) é **2ⁿ**. Com duas condições, quatro regras; com três, oito; com quatro, dezesseis. Esse cálculo cai em prova.

A tabela de decisão ajuda a verificar três problemas: **completude** (todas as combinações possíveis foram previstas?), **redundância** (duas regras com as mesmas condições e as mesmas ações) e **contradição/inconsistência** (duas regras com as mesmas condições e **ações diferentes**). Também existe a **tabela de decisão com condições indiferentes** (traço “–” indica que o valor da condição não importa para aquela regra) e a **consolidação (compactação) de regras**, que agrupa regras com ações idênticas e uma condição indiferente.

Ela é indicada quando há **muitas condições combinadas** e deve ser evitada quando a lógica é basicamente sequencial.

#### Árvore de decisão

A **árvore de decisão** representa a lógica como um **diagrama em forma de árvore**, em que cada **nó** é uma condição, cada **ramo** é um valor possível da condição, e cada **folha** é uma ação ou resultado. Lê-se da raiz para as folhas.

Ela é **visualmente mais intuitiva** que a tabela, especialmente para mostrar a **sequência de testes** e a **dependência** entre as condições. Em compensação, ela pode **crescer rapidamente** (explosão combinatória) e **repetir ações** em várias folhas, tornando-se grande demais quando há muitas condições.

#### Quando usar cada uma

|Ferramenta|Melhor uso|
|---|---|
|**Português estruturado**|Lógica sequencial, com repetições e cálculos|
|**Tabela de decisão**|Muitas condições combinadas; checagem de completude e consistência|
|**Árvore de decisão**|Poucas condições e necessidade de mostrar a sequência de testes de forma visual|