## PARTE F: QUALIDADE DE SOFTWARE

### 1. Conceitos básicos de qualidade

A **qualidade** tem várias definições clássicas, e é bom conhecer as duas mais citadas: para **Crosby**, qualidade é a **conformidade com os requisitos**; para **Juran**, é a **adequação ao uso**. Em software, ela se divide em **qualidade do produto** (características do software entregue: confiável, usável, manutenível) e **qualidade do processo** (como o software é produzido). A premissa central da engenharia de software é que **um bom processo tende a gerar um bom produto**.

É importante distinguir três conceitos. A **Garantia da Qualidade (QA, _Quality Assurance_)** é **preventiva** e foca no **processo**: estabelece padrões, procedimentos e auditorias para assegurar que o processo é seguido e produz qualidade. O **Controle da Qualidade (QC, _Quality Control_)** é **detectivo** e foca no **produto**: inspeciona e testa os artefatos para encontrar defeitos. A **melhoria contínua do processo** usa os dados coletados para aperfeiçoar o processo ao longo do tempo (o ciclo **PDCA** de Deming: Planejar, Executar, Verificar, Agir).

Outra distinção muito cobrada é entre **verificação** e **validação**. A **verificação** responde “**estamos construindo o produto corretamente?**”, ou seja, se o produto está em conformidade com a sua **especificação** (revisões, inspeções, análise estática, testes de unidade). A **validação** responde “**estamos construindo o produto correto?**”, ou seja, se o produto atende às **necessidades reais do usuário** (testes de aceitação, protótipos, uso real). Macete: **verificação = conformidade com a especificação; validação = atender ao usuário**.

O **custo da qualidade** divide-se em custo de **prevenção** (treinamento, padrões), de **avaliação** (testes, revisões) e de **falhas** (internas, antes da entrega; externas, depois). A regra é que **quanto mais tarde um defeito é descoberto, mais caro é corrigi-lo**.

### 2. Processo de software

O **processo de software** é o conjunto estruturado de **atividades, métodos, práticas e transformações** usado para desenvolver e manter software, incluindo normalmente especificação, projeto, implementação, validação e evolução. Os **modelos de maturidade e capacitação** avaliam e melhoram a qualidade do processo de uma organização.

#### 2.1. CMM (Capability Maturity Model)

O **CMM**, ou mais exatamente **SW-CMM** (_Capability Maturity Model for Software_), foi desenvolvido pelo **SEI (Software Engineering Institute)** da **Universidade Carnegie Mellon**, por encomenda do **Departamento de Defesa dos EUA**, com base nas ideias de **Watts Humphrey**, e publicado no início dos anos 1990. Seu objetivo era avaliar a **capacidade de fornecedores** de software e orientar a melhoria dos processos. Ele organiza a maturidade em **cinco níveis**:

|Nível|Nome|Característica|
|---|---|---|
|**1**|**Inicial**|Processo **ad hoc**, caótico, dependente de **heróis** individuais|
|**2**|**Repetível**|**Gerenciamento básico de projeto** (custo, prazo, requisitos); sucessos anteriores podem ser repetidos|
|**3**|**Definido**|Processo **padronizado e documentado** em toda a organização|
|**4**|**Gerenciado**|Processo e produto **medidos quantitativamente** e controlados|
|**5**|**Otimizado**|**Melhoria contínua** do processo, com base nos dados e na inovação|

Cada nível (exceto o 1) é composto por **áreas-chave de processo (KPAs)**. Macete: **Inicial, Repetível, Definido, Gerenciado, Otimizado** (a ordem é de maturidade crescente; o nível 2 é “repetível”, e não “gerenciado”).

#### 2.2. CMMI (Capability Maturity Model Integration)

O **CMMI** é o sucessor do CMM. Ele **integrou vários modelos** (SW-CMM, de engenharia de sistemas, de aquisição, entre outros) em um framework único, e **ampliou o escopo** além do software, cobrindo desenvolvimento (**CMMI-DEV**), aquisição (**CMMI-ACQ**) e serviços (**CMMI-SVC**). Também foi desenvolvido pelo SEI, e seu mantenedor passou depois ao **CMMI Institute**, adquirido pela **ISACA** em 2016. A versão mais cobrada em provas é a **1.3** (2010), embora existam versões mais recentes (2.0, de 2018, e 3.0, de 2023), que reorganizaram o modelo.

O CMMI oferece **duas representações**:

A **representação em estágios (staged)** classifica a **organização inteira** em **níveis de maturidade** de 1 a 5. A **representação contínua** avalia **cada área de processo individualmente**, em **níveis de capacidade** de 0 a 5, permitindo melhorar as áreas escolhidas pela organização.

**Níveis de maturidade (estágios), CMMI v1.3:**

|Nível|Nome|Foco|
|---|---|---|
|**1**|**Inicial**|Processos imprevisíveis e reativos|
|**2**|**Gerenciado**|Projetos são **planejados, executados, medidos e controlados** (a nível de projeto)|
|**3**|**Definido**|Processos **padronizados e proativos**, em nível organizacional|
|**4**|**Gerenciado quantitativamente**|Uso de **estatística e medições** para controlar processo e qualidade|
|**5**|**Em otimização**|**Melhoria contínua** focada em causas de variação e inovação|

**Níveis de capacidade (representação contínua):** **0** Incompleto, **1** Executado, **2** Gerenciado, **3** Definido, **4** Gerenciado quantitativamente, **5** Em otimização.

O CMMI-DEV v1.3 tem **22 áreas de processo (PAs)**, distribuídas pelos níveis de maturidade. Vale conhecer pelo menos as áreas que caracterizam cada nível:

No **nível 2**: gerenciamento de **requisitos (REQM)**, **planejamento de projeto (PP)**, **monitoramento e controle de projeto (PMC)**, **gerenciamento de acordos com fornecedores (SAM)**, **medição e análise (MA)**, **garantia da qualidade de processo e produto (PPQA)** e **gerenciamento de configuração (CM)**. No **nível 3**: **desenvolvimento de requisitos (RD)**, **solução técnica (TS)**, **integração de produto (PI)**, **verificação (VER)**, **validação (VAL)**, **foco no processo organizacional (OPF)**, **definição do processo organizacional (OPD)**, **treinamento organizacional (OT)**, **gerenciamento integrado do projeto (IPM)**, **gerenciamento de riscos (RSKM)** e **análise de decisão e resolução (DAR)**. No **nível 4**: **desempenho do processo organizacional (OPP)** e **gerenciamento quantitativo do projeto (QPM)**. No **nível 5**: **inovação e implantação organizacional (OID)** e **análise e resolução de causas (CAR)**.

**Diferenças CMM e CMMI** que costumam cair: o CMMI tem **duas representações**, o CMM só a em estágios; o nível 2 do CMM é “**Repetível**” e o do CMMI é “**Gerenciado**”; o nível 4 do CMM é “**Gerenciado**” e o do CMMI é “**Gerenciado quantitativamente**”; o nível 5 do CMM é “**Otimizado**” e o do CMMI é “**Em otimização**”; o CMM usa **KPAs (áreas-chave de processo)** e o CMMI usa **PAs (áreas de processo)**; e o CMMI é mais abrangente (além de software).

#### 2.3. MPS.BR (modelo brasileiro)

Como o concurso é brasileiro, vale conhecer o **MPS.BR (Melhoria de Processo do Software Brasileiro)**, coordenado pela **SOFTEX**, compatível com ISO/IEC 12207 e ISO/IEC 15504 e alinhado ao CMMI, pensado para a realidade das **pequenas e médias empresas** brasileiras. Ele tem **sete níveis de maturidade**, de **G (o mais baixo)** a **A (o mais alto)**: **G** Parcialmente Gerenciado, **F** Gerenciado, **E** Parcialmente Definido, **D** Largamente Definido, **C** Definido, **B** Gerenciado Quantitativamente e **A** Em Otimização. Macete: a **ordem alfabética é invertida** (começa no G e sobe até o A), e **mais letras de nível “perto do A” significa maior maturidade**.

#### 2.4. Desenvolvimento ágil

Os **métodos ágeis** surgiram como alternativa aos processos pesados e **orientados a planos** (cascata, RUP). Em **2001**, em Snowbird (Utah, EUA), um grupo de desenvolvedores publicou o **Manifesto Ágil**, que enuncia **quatro valores**: **indivíduos e interações** mais que processos e ferramentas; **software em funcionamento** mais que documentação abrangente; **colaboração com o cliente** mais que negociação de contratos; **responder a mudanças** mais que seguir um plano. O manifesto diz que **há valor nos itens à direita, mas valoriza mais os da esquerda** (uma pegadinha: ágil **não elimina** processos, documentação, contratos e planos, apenas os prioriza de modo diferente). Ele é complementado por **12 princípios**, entre eles: satisfazer o cliente com **entregas contínuas e frequentes**, acolher **mudanças de requisitos** mesmo tardias, entregar software funcionando em **intervalos curtos** (de semanas a poucos meses, preferindo o menor prazo), **trabalho conjunto diário** entre negócio e desenvolvedores, **simplicidade** (maximizar o trabalho não realizado), **ritmo sustentável**, **equipes auto-organizadas**, **reflexão regular** sobre como se tornar mais eficaz e **atenção contínua à excelência técnica**.

**Scrum** é o framework ágil mais difundido. Ele é **empírico** e se baseia em **transparência, inspeção e adaptação**, com **iterações de tamanho fixo chamadas Sprints** (de **no máximo um mês**). Segundo o **Guia do Scrum 2020**, o **Time Scrum** é formado por um **Product Owner (PO)**, que maximiza o valor do produto e gerencia o **Product Backlog**; um **Scrum Master (SM)**, responsável por promover e apoiar o Scrum, e que é um líder-servidor; e os **Desenvolvedores**, que criam o incremento a cada Sprint. O time é **auto-gerenciável e multifuncional**, e **não há hierarquia** nem subtimes (versões antigas do guia falavam em “Time de Desenvolvimento”, e a prova pode usar qualquer das terminologias, conforme a data do material).

Os **eventos** são: a **Sprint** (que contém todos os demais), a **Reunião de Planejamento da Sprint (Sprint Planning)**, a **Reunião Diária (Daily Scrum, de 15 minutos)**, a **Revisão da Sprint (Sprint Review, para inspecionar o incremento com os stakeholders e adaptar o Product Backlog)** e a **Retrospectiva da Sprint (Sprint Retrospective, para melhorar o processo e a equipe)**. Os **artefatos** são o **Product Backlog** (lista ordenada de tudo o que é necessário no produto), o **Sprint Backlog** (itens selecionados para a Sprint mais o plano de entrega) e o **Incremento** (soma dos itens concluídos, utilizável), cada um com seu **compromisso** respectivo: **Meta do Produto (Product Goal)**, **Meta da Sprint (Sprint Goal)** e **Definição de Pronto (Definition of Done)**.

**XP (Extreme Programming)**, de Kent Beck, é um método ágil focado em **práticas de engenharia**. Seus valores são **comunicação, simplicidade, feedback, coragem e respeito**. As práticas principais são: **programação em pares**, **desenvolvimento orientado a testes (TDD)**, **integração contínua**, **refatoração**, **releases pequenos**, **jogo do planejamento**, **propriedade coletiva do código**, **padrões de codificação**, **projeto simples**, **ritmo sustentável (semana de 40 horas)**, **cliente presente (on-site)** e **metáfora**.

**Kanban** é um método de gestão de fluxo visual, com **quadro**, **colunas** e **limite de trabalho em progresso (WIP)**, focado em **melhoria contínua do fluxo**, sem iterações fixas obrigatórias. **Lean** (do Sistema Toyota de Produção) adaptado ao software, tem como princípios eliminar desperdício, ampliar o aprendizado, decidir o mais tarde possível, entregar o mais rápido possível, dar poder à equipe, construir a integridade e ver o todo. Outros métodos ágeis: **FDD (Feature Driven Development)**, **DSDM**, **Crystal** e **ASD**. Complementam o ecossistema ágil o **DevOps**, a **integração e entrega contínuas (CI/CD)** e o **BDD**.

**Ágil versus tradicional:** nos métodos tradicionais (cascata, RUP), as fases são mais sequenciais, o planejamento é detalhado e inicial, a mudança é tratada como desvio e o cliente aparece no início e no fim. Nos ágeis, o desenvolvimento é **iterativo e incremental**, o planejamento é **adaptativo**, a mudança é **bem-vinda** e o cliente **colabora continuamente**.

### 3. Qualidade e normas ISO

As normas ISO/IEC dão **referências internacionais** de qualidade. As mais importantes para a prova são:

A **ISO 9001** é a norma de **Sistemas de Gestão da Qualidade (SGQ)** para **qualquer organização**, e não especificamente de software. Baseia-se em princípios como foco no cliente, liderança, engajamento das pessoas, abordagem de processo, melhoria, decisão baseada em evidências e gestão de relacionamentos. Ela **certifica a organização**, não o produto. A **ISO/IEC 90003** é a diretriz para aplicar a ISO 9001 a software.

A **ISO/IEC 12207** estabelece os **processos do ciclo de vida de software** (aquisição, fornecimento, desenvolvimento, operação, manutenção, e processos de apoio e organizacionais). A versão de 2017 foi alinhada à **ISO/IEC/IEEE 15288** (ciclo de vida de **sistemas**).

A **ISO/IEC 15504 (SPICE)** é a norma de **avaliação de processos de software**, usada para determinar a **capacidade** dos processos (níveis de 0 a 5). Foi substituída pela série **ISO/IEC 33000**.

A **ISO/IEC 9126** é o **modelo de qualidade de produto de software** clássico, com **seis características**: **funcionalidade, confiabilidade, usabilidade, eficiência, manutenibilidade e portabilidade**, cada uma com subcaracterísticas. O macete é a sigla **FCUEMP**, ou apenas decorar “**F**uncionalidade, **C**onfiabilidade, **U**sabilidade, **E**ficiência, **M**anutenibilidade, **P**ortabilidade”. Ela foi substituída pela **ISO/IEC 25010** (família **SQuaRE**, ISO/IEC 25000), cuja versão de 2011 define **oito características** de qualidade do produto: **adequação funcional, eficiência de desempenho, compatibilidade, usabilidade, confiabilidade, segurança, manutenibilidade e portabilidade**. A grande diferença em relação à 9126: a 25010 **acrescenta compatibilidade e segurança**, e **renomeia funcionalidade como adequação funcional** e eficiência como eficiência de desempenho. A 25010 também define o **modelo de qualidade em uso** (efetividade, eficiência, satisfação, ausência de riscos e cobertura de contexto). Uma revisão de 2023 atualizou o modelo (incluindo a segurança operacional/_safety_, entre outras mudanças), mas as provas em geral cobram as oito características da versão de 2011.

A **ISO/IEC 14598** tratava da **avaliação de produto de software** (substituída pela série 25040). A **ISO/IEC/IEEE 29119** é a norma de **teste de software**. A **ISO/IEC 27001** é a de **segurança da informação** (SGSI), e a **ISO/IEC 20000** é a de **gerenciamento de serviços de TI** (alinhada ao ITIL).

### 4. Metodologias e padrões

Neste tópico, o edital reúne os **referenciais de boas práticas**. Os principais são:

O **PMBOK** (_Project Management Body of Knowledge_), do **PMI**, é o guia de gerenciamento de **projetos** (áreas de conhecimento como escopo, cronograma, custos, qualidade, recursos, comunicações, riscos, aquisições e partes interessadas; e grupos de processos de iniciação, planejamento, execução, monitoramento e controle, e encerramento). O **ITIL** é o conjunto de boas práticas de **gerenciamento de serviços de TI**. O **COBIT** (ISACA) é o framework de **governança e gestão de TI**. O **RUP** é um processo iterativo e incremental, orientado a casos de uso e centrado na arquitetura, com quatro fases (**concepção, elaboração, construção, transição**). O **PSP/TSP** (_Personal/Team Software Process_, de Watts Humphrey) são processos de disciplina individual e em equipe. O **SWEBOK** é o corpo de conhecimento da engenharia de software (IEEE). As normas **IEEE 830** (especificação de requisitos, substituída pela **ISO/IEC/IEEE 29148**) e **IEEE 829** (documentação de testes, substituída pela **ISO/IEC/IEEE 29119-3**) aparecem em provas.

Complementam o tema as **revisões técnicas**: o **walkthrough** (o autor conduz, informal, para obter feedback e esclarecer), a **revisão técnica** (por pares, mais formal) e a **inspeção** (a mais formal e rigorosa, com papéis definidos e métricas, como a **inspeção de Fagan**). As **métricas de software** também caem: **pontos de função** (mede o **tamanho funcional** do software, independente de tecnologia, a partir dos requisitos: entradas externas, saídas externas, consultas externas, arquivos lógicos internos e arquivos de interface externa), **linhas de código**, **complexidade ciclomática de McCabe**, **COCOMO** (modelo de estimativa de esforço) e **GQM (Goal-Question-Metric)**, abordagem para definir métricas a partir de objetivos.

### 5. Teste de software

O **teste de software** é o processo de **executar um programa com a intenção de encontrar defeitos** (definição clássica de Glenford Myers). Seu objetivo é **revelar a presença de defeitos**, e **não provar que o software está livre deles**. Lembre-se da frase famosa de **Dijkstra**: testes podem mostrar a presença de defeitos, mas **nunca a ausência**.

#### Terminologia (muito cobrada)

Um **defeito** (_fault, bug_) é uma falha no código ou no artefato (a causa). Um **erro** (_error, mistake_) é a ação humana equivocada que introduz o defeito. Uma **falha** (_failure_) é o **comportamento incorreto observado** na execução quando o defeito é ativado (o efeito). A cadeia é: **erro humano → defeito → falha**. Um **caso de teste** é o conjunto de entradas, condições de execução e resultados esperados para verificar um objetivo. O **oráculo de teste** é o mecanismo que determina o **resultado esperado**. O **plano de teste** documenta escopo, abordagem, recursos e cronograma.

#### Princípios do teste

Os sete princípios amplamente adotados (ISTQB) são: **o teste mostra a presença de defeitos, não a sua ausência**; **o teste exaustivo é impossível**; **testar cedo** economiza tempo e dinheiro; **defeitos se agrupam** (poucos módulos concentram a maioria dos defeitos, o princípio de Pareto); o **paradoxo do pesticida** (repetir os mesmos testes leva a não encontrar novos defeitos, então é preciso rever e variar os casos); **o teste depende do contexto**; e a **falácia da ausência de erros** (encontrar e corrigir defeitos não ajuda se o sistema não atende às necessidades do usuário).

#### Níveis de teste

O **teste de unidade** verifica o **menor componente** isoladamente (função, método, classe), normalmente feito pelo **desenvolvedor**, em geral de **caixa-branca** e com apoio de frameworks (como JUnit). O **teste de integração** verifica a **interação entre módulos** (estratégias top-down, bottom-up, sanduíche e big bang, já vistas, com **stubs e drivers**). O **teste de sistema** verifica o **sistema completo** em relação aos requisitos, em ambiente similar ao de produção. O **teste de aceitação** verifica se o sistema atende às **necessidades do usuário/cliente**, para decidir sobre a **aceitação (validação)**; inclui o **teste alfa** (realizado no ambiente do desenvolvedor, por usuários) e o **teste beta** (realizado no ambiente do cliente, por usuários reais, sem a presença do desenvolvedor).

#### Tipos de teste

Quanto ao **objetivo**: o **teste funcional** verifica **o que** o sistema faz (requisitos funcionais); os **testes não funcionais** verificam **como** ele se comporta: **desempenho** (tempo de resposta), **carga** (comportamento sob o volume esperado), **estresse** (comportamento **além dos limites**, para ver onde e como quebra), **volume**, **usabilidade**, **segurança**, **compatibilidade**, **recuperação** e **confiabilidade**. O **teste de regressão** reexecuta testes após uma alteração para garantir que **nada que funcionava deixou de funcionar**, e é um forte candidato à **automação**. O **teste de confirmação (reteste)** verifica se um **defeito específico foi corrigido**. O **teste de fumaça (smoke)** é uma verificação **rápida e superficial** das funções mais críticas, para decidir se a versão é estável o bastante para testes mais profundos. O **teste de sanidade** verifica, de forma restrita, se uma correção ou funcionalidade específica funciona. O **teste exploratório** combina aprendizado, projeto e execução de testes simultaneamente, sem roteiro rígido. O **teste de mutação** insere pequenas alterações (mutantes) no código para avaliar **a qualidade da própria suíte de testes**.

#### Técnicas de teste

**Técnicas de caixa-preta (funcionais, baseadas na especificação):** o testador **não considera a estrutura interna**, apenas entradas e saídas. As principais são:

A **partição de equivalência** divide o domínio de entrada em **classes** em que todos os valores devem ser tratados do mesmo modo, e testa **um valor representativo** de cada classe (válidas e inválidas). A **análise de valor limite** testa os **valores nas fronteiras** das partições, onde os defeitos mais ocorrem. Por exemplo, para um campo que aceita idades de **1 a 100**, testam-se os valores **0, 1, 2, 99, 100 e 101**. A **tabela de decisão** testa as **combinações de condições**. O **teste de transição de estados** usa a máquina de estados. O **teste de caso de uso** deriva casos dos cenários. O **grafo de causa e efeito** relaciona causas (entradas) e efeitos (saídas).

**Técnicas de caixa-branca (estruturais, baseadas no código):** exigem conhecer a **estrutura interna**. Medem-se **critérios de cobertura**, do mais fraco ao mais forte, de modo geral:

|Critério de cobertura|O que exige|
|---|---|
|**Cobertura de instruções (comandos)**|Cada instrução executada ao menos uma vez|
|**Cobertura de decisões (ramos)**|Cada decisão assume **verdadeiro e falso** ao menos uma vez|
|**Cobertura de condições**|Cada **condição** individual de uma decisão assume V e F|
|**Cobertura de condição/decisão**|Combina as duas anteriores|
|**Cobertura de caminhos**|**Todos os caminhos** possíveis (geralmente inviável em programas grandes)|

Cobrir **todas as decisões (ramos)** **implica** cobrir todas as instruções, mas **o contrário não é verdadeiro**: cobertura de instruções **não garante** cobertura de decisões. A **complexidade ciclomática de McCabe**, **V(G) = E − N + 2P** (arestas, nós e componentes conexos do grafo de fluxo de controle; para um único programa, P = 1, o que dá E − N + 2) ou, equivalentemente, **número de decisões + 1**, indica o **número de caminhos linearmente independentes**, que é o **número mínimo de casos de teste** para a cobertura de ramos pelo critério do caminho básico, e também uma medida de **complexidade e manutenibilidade** do código.

O **teste de caixa-cinza** combina as duas visões. Os **testes baseados em experiência** (suposição de erros, exploratório) completam o conjunto.

#### Abordagens e práticas

O **TDD (Test-Driven Development)** segue o ciclo **Red-Green-Refactor**: escreve-se **primeiro um teste que falha** (vermelho), depois o **código mínimo para passar** (verde) e, por fim, **refatora-se**. O **BDD** descreve o comportamento em linguagem natural (Dado, Quando, Então). A **automação de testes** é mais vantajosa para **regressão**, e a **pirâmide de testes** recomenda muitos testes de unidade (rápidos e baratos), menos de integração e poucos de interface/ponta a ponta (lentos e caros). A **integração contínua** executa os testes automaticamente a cada mudança no código.

---

## MACETES E TRUQUES PARA A PROVA

**OO.** Memorize os quatro pilares: **abstração, encapsulamento, herança, polimorfismo**. Herança é “**é um**”; agregação e composição são “**tem um**”. Se a parte morre junto com o todo, é **composição** (losango **cheio**); se sobrevive, é **agregação** (losango **vazio**).

**UML não é metodologia.** Se a alternativa afirmar que a UML “define o processo de desenvolvimento”, está errada. UML 2 tem **14 diagramas** (7 estruturais e 7 comportamentais); a UML 1.x tinha **9**.

**Include versus extend.** **Include = obrigatório**, a seta vai **do base para o incluído**. **Extend = opcional/condicional**, a seta vai **da extensão para o base**.

**Setas do diagrama de classes.** Herança: **linha cheia + triângulo vazado**. Realização (interface): **linha tracejada + triângulo vazado**. Dependência: **tracejada + seta aberta**. Associação: **linha simples**.

**Sequência versus comunicação.** **Sequência = tempo**; **comunicação = organização/relacionamentos** (mensagens numeradas). Mensagem de **retorno** é **tracejada**.

**Atividades.** **Decisão/merge = losango**; **fork/join = barra**; **raias = responsáveis**.

**CMM/CMMI.** Decore as listas de nomes: CMM (**Inicial, Repetível, Definido, Gerenciado, Otimizado**) e CMMI (**Inicial, Gerenciado, Definido, Gerenciado quantitativamente, Em otimização**). O **nível 2** é o primeiro com **gerenciamento de projeto**; o **nível 3** é o da **padronização organizacional**; o **4** é **quantitativo**; o **5** é **melhoria contínua**. Se a questão falar em **heróis, caos, ad hoc**, é nível 1.

**MPS.BR.** Níveis de **G a A**, do menor ao maior.

**Ágil.** Se a questão disser que o Manifesto Ágil **abole documentação, planejamento ou contratos**, está errada: ele apenas **valora mais** os itens da esquerda. **Scrum** não é uma metodologia completa, mas um **framework**. **Sprint** tem **no máximo um mês**, e **Daily** tem **15 minutos**. **XP** é o das **práticas de engenharia** (TDD, par, refatoração, integração contínua).

**ISO.** **9001** = gestão da qualidade (organização). **12207** = ciclo de vida de software. **15504 (SPICE)** = avaliação de processo. **9126** = qualidade de produto (**6 características**, **FCUEMP**). **25010** = sucessora da 9126 (**8 características**: acrescenta **compatibilidade e segurança**). **29119** = teste.

**Verificação versus validação.** **Verificação** = “construímos **corretamente**?” (especificação). **Validação** = “construímos o **produto certo**?” (usuário).

**Erro, defeito e falha.** O **erro** é humano, o **defeito** está no código, a **falha** é o efeito visível na execução.

**Teste.** Teste **mostra defeitos, não a ausência**. **Alfa** = ambiente do desenvolvedor; **beta** = ambiente do cliente. **Stress** testa **além do limite**; **carga** testa o **volume esperado**. **Regressão** = não quebrar o que funcionava. **Valor limite** = testar as **bordas**. **Caixa-preta** = especificação; **caixa-branca** = código. Cobertura de **decisões** é mais forte que a de **instruções**. **V(G) = decisões + 1**.

**Palavras absolutas.** Cuidado com “**sempre**”, “**nunca**”, “**somente**” e “**garante**” (por exemplo, “o teste garante a ausência de defeitos” está errado).

---

## ANÁLISE DO QUE MAIS CAI (TENDÊNCIA, 2020 A 2026)

Reforço: análise qualitativa, e não estatística oficial.

**Incidência alta:** na parte de UML, o **diagrama de casos de uso** (include/extend, ator, fronteira) e o **diagrama de classes** (relacionamentos, visibilidade, multiplicidade, notação). Em qualidade, **teste de software** (níveis, tipos, caixa-preta e caixa-branca, regressão, valor limite, cobertura) e **desenvolvimento ágil/Scrum** (papéis, eventos, artefatos, valores do manifesto). Também tem peso alto **CMMI** (níveis de maturidade).

**Incidência média:** **diagramas de sequência, atividades e máquina de estados** (identificar qual diagrama usa qual elemento), **conceitos de OO** (os pilares, a diferença entre agregação e composição), **ISO/IEC 9126 e 25010** (as características) e **verificação versus validação**.

**Incidência menor, mas que aparece como pegadinha:** **MPS.BR**, **XP** e **Kanban**, **componentes e implantação**, **PMBOK/ITIL/COBIT** (como referenciais de metodologias e padrões), **ISO 12207/15504**, **métricas** (pontos de função, McCabe) e **padrões de projeto/SOLID**.

**Formato típico:** afirmativas certo/errado com troca de conceitos (include por extend, agregação por composição, stub por driver, validação por verificação), questões de associação entre diagrama e finalidade, e questões em que se descreve um **cenário** e se pede o diagrama, o tipo de teste ou o nível de maturidade correspondente.

**Estratégia de estudo:** concentre a maior parte do tempo em **casos de uso, diagrama de classes, Scrum, CMMI e tipos/técnicas de teste**, que reúnem a maioria das questões. Em seguida, fixe as **normas ISO** com a lista de características. Por fim, faça leitura rápida dos temas de baixa incidência. Pratique **desenhando** um diagrama de casos de uso e um de classes à mão, e resolva questões anteriores da ESFCEx e de bancas semelhantes para calibrar o nível de detalhe cobrado.