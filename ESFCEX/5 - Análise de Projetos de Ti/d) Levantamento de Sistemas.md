## PARTE D: PROJETO ESTRUTURADO DE SISTEMAS

### 1. Conceitos e definições básicas

O **Projeto Estruturado** (_Structured Design_) nasceu com **Larry Constantine** e foi desenvolvido por **Edward Yourdon**, Glenford Myers e Meilir Page-Jones, nos anos 1970 e 1980. Ele é o **complemento natural da análise estruturada**: enquanto a análise responde ao “**o quê**” o sistema deve fazer (e produz o modelo essencial), o projeto responde ao “**como**” o sistema será construído com a tecnologia real (e produz o **modelo de implementação**, mais especificamente a **arquitetura de módulos** do software).

Em termos simples, o projeto estruturado **transforma o DFD em uma hierarquia de módulos**, que será implementada em código. Seu objetivo central é produzir um software com **estrutura clara, de fácil entendimento, teste e manutenção**, e isso é obtido por meio de **modularização** (dividir o sistema em módulos), com módulos **independentes entre si** (baixo acoplamento) e **internamente focados** (alta coesão).

Um **módulo** é uma unidade de programa com **identificador próprio**, que pode ser chamada por outro módulo e cuja execução pode ser tratada de forma independente (uma função, procedimento, sub-rotina). As propriedades de um módulo bem projetado incluem **entrada e saída bem definidas**, **função clara**, **lógica interna** e **dados internos** (escondidos de fora).

A modularização traz os benefícios da **divisão para conquistar**: facilita o entendimento, permite trabalho paralelo em equipe, reaproveitamento de módulos, testes isolados e manutenção localizada.

### 2. Ferramentas do projeto estruturado

A ferramenta principal é o **diagrama de estrutura** (_structure chart_, também chamado de **carta de estrutura** ou **diagrama hierárquico de módulos**). Ele mostra a **hierarquia de módulos**, as **chamadas** entre eles e os **dados e sinais** que trafegam nessas chamadas. Seus elementos são os seguintes: o **módulo** (um retângulo com o nome), a **chamada** (uma linha ou seta ligando o módulo chamador ao chamado), os **parâmetros de dados** (setas com **círculo vazio** na extremidade, indicando dados que trafegam entre os módulos) e os **parâmetros de controle (flags)** (setas com **círculo cheio**, indicando informação de controle, como um sinal de “fim de arquivo” ou um código de erro). Também existem notações para **decisão** (um losango no módulo chamador, indicando chamada condicional) e **repetição** (uma seta curva sobre as chamadas, indicando que o módulo chama os subordinados em laço).

O diagrama de estrutura mostra **quem chama quem**, mas **não mostra** a ordem exata nem o conteúdo interno de cada módulo, e por isso **não é um fluxograma**. A leitura costuma ser **de cima para baixo e da esquerda para a direita**.

Termos de medida da estrutura, frequentes em provas: a **profundidade** (número de níveis de hierarquia), a **largura** (número de módulos em um mesmo nível), o **fan-out** (número de módulos **subordinados** diretamente a um módulo, ou seja, quantos ele chama) e o **fan-in** (número de módulos **superiores** que chamam um dado módulo, ou seja, quantos o utilizam). **Fan-in alto é bom**, porque indica **reaproveitamento** do módulo. **Fan-out muito alto é ruim**, porque indica que o módulo está controlando coisas demais; a recomendação clássica é manter o fan-out em torno de **7 ± 2**. Há ainda o **escopo de controle** (o conjunto de todos os módulos subordinados, direta ou indiretamente, a um módulo) e o **escopo de efeito** (o conjunto de módulos afetados por uma decisão tomada em determinado módulo). A **regra de ouro** é que **o escopo de efeito de uma decisão deve estar contido no escopo de controle do módulo que a toma**; do contrário, surge a necessidade de passar flags de controle de forma complicada.

Outras ferramentas de apoio ao projeto são o **português estruturado/pseudocódigo**, as **tabelas e árvores de decisão** (já vistas), o **fluxograma** e o **diagrama de Nassi-Shneiderman (Chapin)** para detalhar a lógica interna de módulos, e o **HIPO** (_Hierarchy plus Input-Process-Output_, técnica da IBM de documentação em hierarquia com entrada-processamento-saída).

### 3. Especificação estruturada e métodos para especificação de módulos

A **especificação estruturada** é o conjunto de técnicas que descreve **de forma precisa e sem ambiguidade** a lógica interna de cada módulo (ou, na análise, de cada processo primitivo), usando **construções estruturadas** de programação: **sequência**, **decisão (seleção)** e **repetição (iteração)**. Esse conjunto de três construções é o núcleo da **programação estruturada** (teorema de Böhm-Jacopini), e **dispensa o uso do GOTO**, que torna a lógica difícil de seguir.

Os métodos para especificar módulos incluem o **português estruturado** (também chamado de **linguagem estruturada** ou **pseudocódigo**, que combina frases em linguagem natural com as construções estruturadas, como SE...ENTÃO...SENÃO, ENQUANTO...FAÇA, PARA...ATÉ), as **tabelas de decisão**, as **árvores de decisão**, o **fluxograma estruturado**, o **diagrama de Nassi-Shneiderman (Chapin)** e as **pré e pós-condições** (descrição formal do que deve ser verdadeiro antes e depois da execução do módulo, sem descrever o algoritmo, o que é útil para especificar **o que** o módulo faz sem dizer **como**). A escolha do método depende da natureza da lógica: algoritmos sequenciais combinam com português estruturado, lógica com muitas combinações de condições combina com tabelas, e relações simples de decisão se mostram bem em árvores.

### 4. Acoplamento

O **acoplamento** mede o **grau de dependência entre módulos diferentes**, isto é, o quanto um módulo depende de outro. O objetivo do projeto é o **baixo acoplamento** (módulos independentes), porque módulos fracamente acoplados podem ser **entendidos, alterados e testados isoladamente**, e uma mudança em um tem **pouco efeito** nos demais (fenômeno chamado de **efeito cascata** ou _ripple effect_).

Os tipos de acoplamento, **do pior (mais forte) para o melhor (mais fraco)**, são:

|Tipo|Descrição|Qualidade|
|---|---|---|
|**Acoplamento de conteúdo**|Um módulo **acessa ou altera o interior de outro** (código ou dados internos), ou desvia para o meio dele|Pior|
|**Acoplamento comum**|Módulos **compartilham dados globais** (uma área de dados comum)|Muito ruim|
|**Acoplamento externo**|Módulos compartilham um **formato de dados, protocolo ou dispositivo externo**|Ruim|
|**Acoplamento de controle**|Um módulo **passa um flag** que **controla a lógica interna** do outro|Médio-ruim|
|**Acoplamento de dados estruturados (stamp/carimbo)**|Módulos compartilham uma **estrutura de dados composta** (registro), mas o módulo usa só parte dela|Aceitável|
|**Acoplamento de dados**|Módulos se comunicam apenas por **parâmetros simples**, contendo só os dados necessários|**Melhor**|

Existe ainda o **“sem acoplamento”** (módulos totalmente independentes), que na prática não ocorre em um sistema, pois os módulos precisam cooperar.

**Macete para memorizar a ordem**: pense em **“Conteúdo, Comum, Externo, Controle, Carimbo (Stamp), Dados”**, do pior para o melhor. Uma forma de gravar: o acoplamento piora quanto mais o módulo **“sabe sobre o outro”**; no acoplamento de conteúdo, ele sabe tudo (inclusive o interior); no de dados, ele só sabe o mínimo.

### 5. Coesão

A **coesão** mede o **grau de relacionamento entre os elementos internos de um módulo**, isto é, o quanto as instruções e os dados dentro de um módulo **pertencem juntos** e contribuem para **uma única tarefa**. O objetivo é a **alta coesão**: um módulo deve fazer **uma coisa bem feita**.

Os tipos de coesão, **do pior (mais fraca) para o melhor (mais forte)**, são:

|Tipo|Descrição|Qualidade|
|---|---|---|
|**Coincidental**|Os elementos estão juntos **sem nenhuma relação**, por acaso (o módulo “utilidades diversas”)|Pior|
|**Lógica**|Os elementos executam **funções do mesmo tipo/categoria**, e a função específica é escolhida por um **parâmetro** (por exemplo, um módulo “processar entrada” que lê de teclado, disco ou rede conforme um flag)|Ruim|
|**Temporal**|Os elementos são executados **no mesmo momento** (módulo de “inicialização” que abre arquivos, zera contadores e limpa a tela)|Fraca|
|**Procedural**|Os elementos seguem uma **sequência de controle**, mas **sem compartilhar dados** necessariamente|Fraca/média|
|**Comunicacional**|Os elementos operam **sobre os mesmos dados de entrada ou geram a mesma saída**|Boa|
|**Sequencial**|A **saída de um elemento é a entrada do seguinte**, como em uma linha de montagem|Muito boa|
|**Funcional**|**Todos os elementos contribuem para uma única função bem definida**|**Melhor**|

**Macete para memorizar a ordem da coesão**, do pior ao melhor: **C**oincidental, **L**ógica, **T**emporal, **P**rocedural, **C**omunicacional, **S**equencial, **F**uncional. Uma frase possível: “**C**ada **L**ouco **T**em **P**roblemas, **C**ompreenda **S**eus **F**atos”. Esse tipo de mnemônico é pessoal; crie o seu se preferir.

**Regra de ouro:** o projetista busca **alta coesão e baixo acoplamento**. Os dois conceitos são complementares: módulos coesos tendem a precisar de menos comunicação com os demais, o que reduz o acoplamento. Uma forma de testar a coesão funcional é tentar **descrever o módulo em uma frase simples (verbo + objeto)**, sem usar “e”, “ou” ou vírgulas; se for preciso usar, a coesão provavelmente é menor.

### 6. Análise de transformação

A **análise de transformação** é a **estratégia principal** (e mais comum) para **derivar o diagrama de estrutura a partir de um DFD**. Ela é indicada quando o DFD tem um **fluxo de dados principal** em que os dados entram, são transformados e saem, ou seja, um fluxo do tipo **entrada, processamento, saída**.

Os passos típicos são os seguintes. Primeiro, **revisar e refinar o DFD**, garantindo que ele está correto e balanceado. Segundo, **identificar o fluxo principal** e determinar, nele, os **três segmentos**: o **fluxo de entrada** (aferente, que traz os dados e os prepara), a **central de transformação** (o núcleo, onde ocorre a lógica principal de processamento) e o **fluxo de saída** (eferente, que formata e entrega os resultados). Os **limites** entre eles são os pontos em que os dados passam de uma forma “bruta/externa” para uma forma “processada” e vice-versa; o **fluxo aferente** termina no ponto em que os dados atingem sua **forma mais abstrata e processada, ainda como entrada**, e o **eferente** começa no ponto em que os dados estão na forma **mais pura de saída**. Terceiro, **criar o módulo principal (de controle)** no topo da estrutura, com um módulo de coordenação. Quarto, **criar um módulo subordinado para cada segmento**: um módulo de **obtenção de entrada** (que chama módulos subordinados para ler e validar), um módulo de **transformação** (central) e um módulo de **produção de saída**. Quinto, **detalhar** cada um, criando módulos para os processos menores. Sexto, **refinar** a estrutura conforme os critérios de acoplamento, coesão, fan-in e fan-out.

O resultado é uma estrutura em que o módulo no topo coordena três ramos: **obter dados, transformar dados e produzir saída**. Em termos de macete: “**Aferente, Central, Eferente**” ou, mais facilmente, “**entrada, transformação, saída**”.

### 7. Análise de transação

A **análise de transação** é a estratégia usada quando o DFD (ou parte dele) tem um **processo que, dependendo do tipo de dado ou evento de entrada, escolhe uma entre várias trajetórias de processamento alternativas**. É a situação típica de um sistema que **recebe diferentes tipos de transação** e **despacha cada uma** para a rotina adequada: um caixa eletrônico (saque, depósito, consulta de saldo, transferência) ou um menu de opções.

O ponto no DFD em que ocorre a bifurcação é chamado de **centro de transação** (_transaction center_). A estrutura derivada tem **um módulo despachante (dispatcher)** no topo do ramo, que **recebe a transação, identifica o seu tipo e chama o módulo de processamento correspondente**. Os módulos subordinados são os **módulos de transação** (um para cada tipo, cada um tratando seu caso).

Os passos básicos: identificar o centro de transação no DFD, identificar os caminhos de ação (um por tipo de transação), criar um módulo despachante que obtém a transação e a encaminha, e criar um módulo para cada caminho de ação, detalhando em seguida cada um. Em uma estrutura real, as duas análises costumam se **combinar**: um sistema pode ter uma estrutura global de transformação, com um de seus ramos usando análise de transação.

**Macete de distinção:** **transformação** = **fluxo linear** (entrada, processo, saída); **transação** = **ramificação por tipo** (um despachante escolhendo o caminho).

### 8. Packaging

O **packaging** (empacotamento) é a atividade de **agrupar os módulos do projeto em unidades físicas de execução**, como **programas, jobs, rotinas executáveis ou partições de memória**, considerando as **restrições do ambiente real** (tamanho de memória, tempo de execução, volume de dados, limitações do sistema operacional e do hardware). Em uma era em que a memória era um recurso escasso, esse agrupamento tinha um papel crucial, definindo, por exemplo, quais módulos caberiam em cada área de memória ou em cada **passo de um processamento em lote (_batch_)**.

Os critérios para o empacotamento incluem **tamanho do módulo/programa**, **frequência de execução e uso** (módulos usados em conjunto tendem a ser agrupados), **dependência de dados**, **volatilidade** (módulos que mudam com frequência podem ser isolados para facilitar a manutenção), **requisitos de desempenho** e **restrições do ambiente de execução** (linguagem, sistema operacional, memória, janelas de processamento). Um ponto de atenção: o empacotamento é uma decisão de **implementação** e **não deve comprometer** a estrutura lógica de módulos com alta coesão e baixo acoplamento definida no projeto, mas ele pode **motivar ajustes** na estrutura.

### 9. Implementação

A **implementação** do projeto estruturado trata da **ordem e da forma de codificar, testar e integrar** os módulos definidos no diagrama de estrutura. Uma das decisões mais importantes é a **estratégia de integração e testes**:

A **integração top-down (descendente)** começa pelo **módulo de topo** e desce pela hierarquia, integrando e testando os módulos nível a nível. Os módulos inferiores que ainda não existem são simulados por **stubs** (módulos “esqueleto”, que retornam respostas fixas). A vantagem é que **testa cedo as interfaces e a lógica de controle principal**, e permite ter um protótipo funcional rápido; a desvantagem é a necessidade de muitos stubs e o fato de que os módulos de baixo nível (frequentemente os mais críticos, como entrada e saída) são testados por último.

A **integração bottom-up (ascendente)** começa pelos **módulos de nível mais baixo** (as folhas da árvore) e sobe. Como os módulos superiores ainda não existem, usam-se **drivers** (módulos “condutores”, que chamam o módulo testado e fornecem dados de teste). A vantagem é testar primeiro os módulos básicos e utilitários, sem precisar de stubs; a desvantagem é que o **sistema como um todo só existe no fim**, e os problemas de interface e controle de alto nível aparecem tarde.

A **integração sanduíche (híbrida)** combina as duas: **top-down nos níveis superiores e bottom-up nos inferiores**, encontrando-se no meio. A **integração big bang** (explosão) junta todos os módulos de uma vez e testa o conjunto; é a mais arriscada, porque dificulta localizar a origem dos erros.

**Macete:** **stub** = módulo **substituto de quem é chamado** (usado no top-down); **driver** = módulo **que chama** o módulo em teste (usado no bottom-up). Uma frase de memória: “no top-down eu **simulo o que está embaixo (stub)**; no bottom-up eu **simulo o que está em cima (driver)**”.

Também fazem parte da implementação a **codificação estruturada** (usando só sequência, decisão e repetição), a **documentação**, as **revisões de código** e os **testes** em seus níveis (unidade, integração, sistema, aceitação).

### 10. Otimização

A **otimização** é a atividade de **melhorar o desempenho** (velocidade, uso de memória, uso de recursos) do sistema depois de ele estar funcionando. O princípio clássico é de que **não se deve otimizar prematuramente**: primeiro, **faça o sistema correto, claro e funcional**; depois, **meça** e **otimize apenas onde for necessário**. Uma frase famosa, atribuída a Donald Knuth, diz que a **otimização prematura é a raiz de todo mal** em programação, justamente porque otimizar cedo demais complica o código e a manutenção sem benefício comprovado.

Outro princípio relevante é a **regra 80/20 (princípio de Pareto)** aplicada ao software: cerca de **80% do tempo de execução costuma ser gasto em apenas cerca de 20% do código**, de modo que o esforço de otimização deve focar nesses **pontos críticos (_hot spots_)**, identificados por **medição (profiling)**, e não por intuição. A otimização deve ser **guiada por medições**, e não deve **degradar a estrutura** (acoplamento e coesão) a menos que seja indispensável, e, se isso ocorrer, a decisão deve ser **documentada**. As ações típicas são trocar algoritmos ineficientes, ajustar estruturas de dados, reduzir acessos a disco/banco, usar _cache_, reorganizar o empacotamento e eliminar processamento redundante.

### 11. Administração do projeto estruturado

A **administração (gerência) do projeto estruturado** trata de **planejar, organizar, acompanhar e controlar** o projeto de desenvolvimento, usando as técnicas estruturadas como base para esse gerenciamento. Entre as atividades de gerenciamento estão o **planejamento** (definição de escopo, atividades, cronograma e recursos, apoiada na decomposição do sistema em módulos, que facilita a **estimativa** de esforço, prazo e custo), o **acompanhamento e controle** (medição do progresso, comparando o planejado com o realizado, e tratamento dos desvios), a **gestão de riscos**, a **gestão de configuração e mudanças** (controle de versões e das alterações de requisitos), a **garantia da qualidade** e a **gestão da equipe**.

Um ponto distintivo do ciclo estruturado são as **revisões estruturadas** (_structured walkthroughs_): reuniões formais, mas não punitivas, em que o autor apresenta seu trabalho (modelo, diagrama, especificação ou código) a um grupo de colegas, que **procuram erros** o mais cedo possível. O objetivo é **detectar defeitos, não corrigi-los na reunião**, e **avaliar o produto, não o autor**. Quanto mais cedo o erro é encontrado, **menor o custo de correção**. Essa ideia se relaciona com as **inspeções** (mais formais) e as **revisões por pares**. A modularização, por ser decomposição, também facilita a **divisão do trabalho** e o **acompanhamento** por módulo concluído.

---

## MACETES E TRUQUES PARA A PROVA

**Análise essencial.** Memorize as palavras-chave: **tecnologia perfeita**, **essência**, **particionamento por eventos**, **modelo ambiental (fronteira) e comportamental (interior)**. Se a questão disser que a análise essencial **começa modelando o sistema físico atual**, está errada: ela vai direto ao modelo lógico essencial.

**Atividade fundamental versus custodial.** **Fundamental** realiza o **propósito do negócio**; **custodial** **mantém os dados** (cadastros, inclusão, alteração, exclusão).

**Tipos de evento.** **Externo** (vem de entidade externa), **temporal** (vem do tempo) e **de controle** (vem de condição interna).

**DFD.** Se a alternativa disser que o DFD mostra **sequência, decisão ou controle**, está errada, pois o DFD mostra **fluxo de dados**. Memorize os erros: **buraco negro** (só entra), **milagre** (só sai) e **desbalanceamento**. Lembre-se também de que o diagrama de contexto tem **um único processo** e **não tem depósitos**, e de que **não há fluxo direto entre dois depósitos, duas entidades externas ou entidade e depósito**.

**Dicionário de dados.** Decore a notação: **=** (é composto de), **+** (e), **[ | ]** (ou), **{ }** (repetição), **( )** (opcional), _**...**_ (comentário). A pegadinha clássica é trocar o significado de **{ }** e **( )**: chaves é **repetição**, parênteses é **opcional**.

**Tabela de decisão.** **2ⁿ regras** para n condições binárias. **Redundância** é regras repetidas com as mesmas ações; **contradição** é mesmas condições com **ações diferentes**; **incompletude** é combinação não prevista.

**Acoplamento.** Lembre da sequência do pior ao melhor: **Conteúdo, Comum, Externo, Controle, Carimbo, Dados**. Uma questão pode perguntar qual é o **melhor** (dados) e qual é o **pior** (conteúdo).

**Coesão.** Do pior ao melhor: **Coincidental, Lógica, Temporal, Procedural, Comunicacional, Sequencial, Funcional**. O melhor é a **funcional** e o pior a **coincidental**. Uma pegadinha comum é confundir **coesão lógica** (elementos de mesma categoria, escolhidos por parâmetro) com **coesão funcional**.

**Objetivo geral.** Sempre que a alternativa falar em **“alto acoplamento e baixa coesão”** como meta, é erro; o objetivo é **baixo acoplamento e alta coesão**.

**Fan-in e fan-out.** **Fan-in** = quantos módulos **chamam** este módulo (alto é bom, reaproveitamento). **Fan-out** = quantos módulos este módulo **chama** (muito alto é ruim).

**Transformação versus transação.** **Transformação** = fluxo linear, três segmentos (**entrada, central de transformação, saída**). **Transação** = **centro de transação**, **dispatcher** e ramos por tipo de transação.

**Stub versus driver.** **Stub** substitui o módulo **chamado** (top-down). **Driver** substitui o módulo **chamador** (bottom-up).

**Otimização.** **Primeiro funcionar, depois otimizar**; **otimização prematura é um erro**; foque nos **20% do código** que consomem **80% do tempo**.

**Revisão estruturada.** **Procura erros, não os corrige na reunião, avalia o produto e não o autor.**

**Palavras absolutas.** Cuidado com “**sempre**”, “**nunca**” e “**somente**” nas alternativas.

---

## ANÁLISE DO QUE MAIS CAI (TENDÊNCIA, 2020 A 2026)

Mantenho o aviso: é uma análise qualitativa de tendência, e não um levantamento estatístico oficial.

**Incidência alta:** o **DFD** (componentes, notação, níveis, balanceamento, erros típicos) é, de longe, o tópico mais cobrado desta parte, muitas vezes em questões que apresentam um diagrama e pedem a identificação de erros. Também têm peso alto **acoplamento e coesão**, principalmente a **ordem dos tipos** e a identificação do tipo a partir de uma descrição, e o **dicionário de dados**, em especial a notação dos símbolos.

**Incidência média:** **tabelas e árvores de decisão** (com o cálculo de 2ⁿ regras e a identificação de redundância e contradição), os **conceitos de análise essencial** (tecnologia perfeita, modelo ambiental e comportamental, eventos, atividades fundamentais e custodiais), a **diferença entre análise de transformação e de transação** e o **diagrama de estrutura** (fan-in, fan-out, flags e dados).

**Incidência menor, mas que aparece como pegadinha:** **packaging**, **stubs e drivers**, **otimização** e **administração do projeto estruturado** (revisões estruturadas). São assuntos de teoria que costumam surgir como alternativas de conceito, em geral em itens certo/errado.

**Formato típico:** afirmativas certo/errado com troca sutil de termos (coesão por acoplamento, stub por driver, atividade fundamental por custodial), questões de associação entre conceitos e definições, e questões que apresentam um pequeno DFD, uma tabela ou uma notação do dicionário de dados e pedem uma interpretação.

**Estratégia de estudo:** invista a maior parte do tempo em **DFD, dicionário de dados, acoplamento e coesão**, pois reúnem a maior parte das questões. Depois, fixe as **tabelas de decisão** e a **diferença entre transformação e transação**. Por último, passe pelos temas de baixa incidência com leitura rápida dos macetes. Resolva questões anteriores da própria ESFCEx e de bancas parecidas, e **desenhe** pelo menos um DFD e um diagrama de estrutura à mão, porque a prática fixa muito mais do que a leitura.