## PARTE J: ANÁLISE DE PONTOS DE FUNÇÃO (APF)

### 1. Conceitos e objetivos

A **Análise de Pontos de Função (APF)** é uma **técnica de medição do tamanho funcional** de um software, a partir da **visão do usuário**, **independentemente da tecnologia, da linguagem de programação e da forma de desenvolvimento**. Foi criada por **Allan Albrecht**, da **IBM**, em **1979**, e hoje é mantida pelo **IFPUG (_International Function Point Users Group_)**, por meio do **Manual de Práticas de Contagem (CPM, _Counting Practices Manual_)**. Também é padronizada pela **ISO/IEC 20926**. Existem outras variantes, como a **NESMA** (holandesa) e a **COSMIC**.

O que a APF **mede**: o **tamanho funcional**, ou seja, a **quantidade de funcionalidade** entregue ao usuário. O que ela **não mede**: esforço, prazo, custo ou produtividade diretamente. Esses valores são **derivados** do tamanho, com base em **dados históricos de produtividade** da organização (por exemplo, horas por ponto de função). Os principais **usos** são a **estimativa de esforço, prazo e custo**, a **comparação de produtividade**, a **gestão de contratos** (muito usada no governo brasileiro, como no **SISP**, para pagar por entrega, e não por hora trabalhada), o **acompanhamento do crescimento do escopo** e a **análise de qualidade** (defeitos por ponto de função).

Vantagens: independência de tecnologia, baseia-se em requisitos (pode ser feita **cedo**, antes da codificação), é **repetível**, entendida pelo usuário e **comparável**. Limitações: exige **contador treinado**, tem **certa subjetividade**, **não captura bem a complexidade algorítmica** (sistemas científicos e de tempo real) e **não mede requisitos não funcionais** de modo direto (só, em parte, por meio do fator de ajuste).

### 2. Visão geral do processo de contagem (IFPUG)

Os passos típicos são: **(1)** determinar o **tipo de contagem**; **(2)** identificar o **escopo da contagem e a fronteira da aplicação**; **(3)** contar as **funções de dados**; **(4)** contar as **funções transacionais**; **(5)** calcular os **pontos de função não ajustados (PFNA)**; **(6)** determinar o **valor do fator de ajuste (VAF)**; **(7)** calcular os **pontos de função ajustados (PFA)**.

#### Tipos de contagem

A contagem de **projeto de desenvolvimento** mede a funcionalidade entregue na **primeira instalação** do software; a de **projeto de melhoria** mede as funcionalidades **adicionadas, alteradas e excluídas** de uma aplicação existente; a de **aplicação** (_baseline_) mede o **tamanho total de uma aplicação instalada**.

#### Fronteira da aplicação

A **fronteira** é a **linha divisória entre a aplicação medida e o mundo externo** (usuários e outras aplicações). Ela é definida pela **visão do usuário**, e **não** pela estrutura técnica. É o ponto de partida para decidir o que é “interno” (ALI) e o que é “externo” (AIE, EE, SE, CE).

### 3. Funções de dados

As funções de dados representam o **requisito de armazenamento de dados** do usuário. São de dois tipos:

O **ALI (Arquivo Lógico Interno)** é um **grupo de dados ou informações de controle, logicamente relacionados, identificável pelo usuário, mantido (criado, alterado, excluído) pela aplicação dentro da fronteira**. Por exemplo, a tabela de clientes de um sistema de vendas. O **AIE (Arquivo de Interface Externa)** é um grupo de dados logicamente relacionados, identificável pelo usuário, **referenciado (somente lido) pela aplicação, mas mantido por outra aplicação**, ou seja, **fora da fronteira**. Por exemplo, a tabela de CEPs mantida pelos Correios ou um cadastro de outro sistema. A **diferença decisiva**: **o ALI é mantido pela aplicação medida, e o AIE é mantido por outra**. Um mesmo grupo de dados pode ser ALI em uma aplicação e AIE em outra.

A complexidade de cada função de dados depende de dois fatores: os **TR (Tipos de Registro, em inglês _RET_)**, que são subgrupos de dados reconhecidos pelo usuário (por exemplo, a entidade principal e as entidades-filhas dependentes, ou as subcategorias), e os **TD (Tipos de Dados, em inglês _DET_)**, que são os campos únicos reconhecidos pelo usuário, não repetidos.

**Matriz de complexidade de ALI e AIE:**

||**1 a 19 TD**|**20 a 50 TD**|**51 ou mais TD**|
|---|---|---|---|
|**1 TR**|Baixa|Baixa|Média|
|**2 a 5 TR**|Baixa|Média|Alta|
|**6 ou mais TR**|Média|Alta|Alta|

**Pontos por complexidade:**

|Função|Baixa|Média|Alta|
|---|---|---|---|
|**ALI**|**7**|**10**|**15**|
|**AIE**|**5**|**7**|**10**|

### 4. Funções transacionais

Representam as **funcionalidades de processamento** que a aplicação fornece ao usuário. Cada uma deve ser um **processo elementar**: a **menor unidade de atividade significativa para o usuário**, **completa em si mesma** e que **deixa a aplicação em estado consistente**. São três tipos:

A **EE (Entrada Externa)** é um processo elementar que **processa dados ou informações de controle vindos de fora da fronteira**, com o objetivo principal de **manter um ou mais ALIs** ou **alterar o comportamento do sistema**. Exemplos: incluir, alterar ou excluir um cliente (**cada uma dessas operações é uma EE separada**). A **SE (Saída Externa)** é um processo elementar que **envia dados ou informações de controle para fora da fronteira** e que contém **lógica de processamento adicional** além de uma simples recuperação: **cálculo ou fórmula matemática**, **criação de dados derivados**, **atualização de um ALI** ou **alteração do comportamento do sistema**. Exemplo: um relatório que calcula totais e comissões. A **CE (Consulta Externa)** é um processo elementar que **envia dados para fora da fronteira**, mas **por simples recuperação de dados**, **sem cálculo, sem dados derivados e sem atualizar ALI**. Exemplo: uma consulta de clientes pelo nome, exibindo os dados como estão armazenados.

O macete para distinguir **SE de CE**: se há **cálculo, derivação, atualização de ALI ou mudança de comportamento**, é **SE**; se é **só recuperação e exibição**, é **CE**. E a **EE** é a **que traz dados para dentro** e geralmente **mantém um ALI**.

A complexidade das funções transacionais depende dos **ALR/ARQ (Arquivos Referenciados**, em inglês _**FTR**_, _File Types Referenced_, que são os ALIs lidos ou mantidos e os AIEs lidos pela transação) e dos **TD (Tipos de Dados)**, os campos únicos que cruzam a fronteira, mais mensagens de erro, botões de ação etc.

**Matriz da EE:**

||**1 a 4 TD**|**5 a 15 TD**|**16 ou mais TD**|
|---|---|---|---|
|**0 a 1 ARQ**|Baixa|Baixa|Média|
|**2 ARQ**|Baixa|Média|Alta|
|**3 ou mais ARQ**|Média|Alta|Alta|

**Matriz da SE e da CE** (são iguais entre si):

||**1 a 5 TD**|**6 a 19 TD**|**20 ou mais TD**|
|---|---|---|---|
|**0 a 1 ARQ**|Baixa|Baixa|Média|
|**2 a 3 ARQ**|Baixa|Média|Alta|
|**4 ou mais ARQ**|Média|Alta|Alta|

**Pontos por complexidade:**

|Função|Baixa|Média|Alta|
|---|---|---|---|
|**EE**|**3**|**4**|**6**|
|**SE**|**4**|**5**|**7**|
|**CE**|**3**|**4**|**6**|

Macete dos pontos: **EE e CE têm os mesmos valores (3, 4, 6)**; **SE tem 4, 5, 7**; **ALI tem 7, 10, 15**; **AIE tem 5, 7, 10**. Repare que **a SE vale mais que a EE e a CE** porque tem lógica de processamento.

### 5. Pontos de função não ajustados (PFNA)

É a **soma** dos pontos de todas as funções de dados e transacionais, **de acordo com a complexidade de cada uma**. **PFNA = Σ pontos de ALI + AIE + EE + SE + CE.**

### 6. Fator de ajuste (VAF) e pontos de função ajustados

O **fator de ajuste (VAF, _Value Adjustment Factor_)** reflete a **influência de características gerais do sistema** (os requisitos não funcionais, em parte) sobre o tamanho. São **14 características gerais do sistema (CGS)**: **comunicação de dados**, **processamento distribuído**, **desempenho**, **utilização do equipamento (configuração altamente utilizada)**, **volume de transações**, **entrada de dados on-line**, **eficiência do usuário final**, **atualização on-line**, **processamento complexo**, **reusabilidade**, **facilidade de implantação**, **facilidade operacional**, **múltiplos locais** e **facilidade de mudanças**. Cada uma recebe um **grau de influência de 0 a 5** (0 = nenhuma influência; 5 = forte influência durante todo o ciclo). O **total de grau de influência (TDI)** é a soma dos 14 graus (varia de **0 a 70**).

**VAF = (TDI × 0,01) + 0,65**

O VAF varia, portanto, de **0,65** (TDI = 0) a **1,35** (TDI = 70). Ou seja, o ajuste **pode reduzir em até 35% ou aumentar em até 35%** o tamanho. Com TDI = 35 (valor médio), o VAF é 1,00 (sem ajuste).

**Pontos de função ajustados:**

|Tipo de contagem|Fórmula|
|---|---|
|**Desenvolvimento**|**PFA = (PFNA + PF das funções de conversão) × VAF**|
|**Aplicação**|**PFA = PFNA × VAF**|
|**Melhoria**|**PFA = (PF incluídos + PF alterados) × VAF depois + PF excluídos × VAF antes** (mais as funções de conversão, se houver)|

Observação importante: na prática, o **IFPUG** passou a tratar o **VAF como opcional** (e a norma ISO/IEC 20926 considera o tamanho funcional **sem o ajuste**), e muitos contratos, inclusive no governo brasileiro, usam o **PF não ajustado**. Mesmo assim, as provas ainda cobram a fórmula do VAF.

### 7. Exemplo completo de contagem

Imagine um sistema simples de cadastro de clientes e pedidos, com as seguintes funções (valores de TR/TD e ARQ/TD já definidos):

|Função|Tipo|Dados|Complexidade|Pontos|
|---|---|---|---|---|
|Cliente|**ALI**|1 TR, 12 TD|Baixa|**7**|
|Pedido (com itens)|**ALI**|2 TR, 25 TD|Média|**10**|
|Tabela de CEPs (externa)|**AIE**|1 TR, 4 TD|Baixa|**5**|
|Incluir cliente|**EE**|1 ARQ, 12 TD|Baixa|**3**|
|Alterar cliente|**EE**|1 ARQ, 12 TD|Baixa|**3**|
|Excluir cliente|**EE**|1 ARQ, 2 TD|Baixa|**3**|
|Registrar pedido|**EE**|3 ARQ, 20 TD|Alta|**6**|
|Relatório de pedidos por cliente (com total calculado)|**SE**|2 ARQ, 15 TD|Média|**5**|
|Consultar cliente|**CE**|1 ARQ, 10 TD|Baixa|**3**|

**PFNA = 7 + 10 + 5 + 3 + 3 + 3 + 6 + 5 + 3 = 45 pontos.**

Se as 14 CGS somarem TDI = 30, **VAF = 30 × 0,01 + 0,65 = 0,95**, e **PFA = 45 × 0,95 = 42,75 ≈ 43 PF**. Para estimar o esforço, se a produtividade histórica da organização for de **10 horas por PF**, o esforço estimado é de cerca de **430 horas**.

### 8. Contagem estimativa e indicativa (NESMA)

Quando é preciso estimar **cedo**, com poucas informações, a **NESMA** propõe dois métodos simplificados. A **contagem indicativa** estima o tamanho **apenas a partir do número de funções de dados**: **PF = 35 × (nº de ALI) + 15 × (nº de AIE)** (em outra variação para aplicações novas, usam-se **25 e 10**, para dados normalizados, mas o mais citado é 35 e 15). A **contagem estimada** identifica **todas as funções**, mas **assume complexidade baixa para as funções de dados** e **média para as transacionais** (**ALI = 7, AIE = 5, EE = 4, SE = 5, CE = 4**), sem calcular TR, TD e ARQ. Esses métodos são **mais rápidos, mas menos precisos**.

---

## MACETES E TRUQUES PARA A PROVA

**ITIL.** Os **cinco estágios** do ciclo de vida: **Estratégia, Desenho, Transição, Operação e Melhoria Contínua**. **Incidente** = restaurar rápido (sintoma); **problema** = causa raiz; **requisição** = pedido padrão sem falha; **mudança** = alteração controlada pelo CAB. **Mudança padrão** é pré-autorizada, **emergencial** passa pelo **ECAB**. **SLA** = cliente; **OLA** = interno; **UC** = fornecedor externo. **Prioridade = impacto × urgência**. **Utilidade = o que faz; garantia = como é entregue.** Se a questão falar em **SVS e 7 princípios**, é ITIL 4.

**COBIT.** **4.1**: 4 domínios **PO, AI, DS, ME**, **34 processos**, **7 critérios de informação**, maturidade 0 a 5. **5**: **5 princípios**, **7 habilitadores**, **37 processos**, **5 domínios (EDM, APO, BAI, DSS, MEA)**, capacidade 0 a 5 (ISO 15504). **2019**: **40 objetivos**, 5 domínios. **Governança** (EDM: avaliar, dirigir, monitorar) é diferente de **gerenciamento** (APO, BAI, DSS, MEA). O COBIT diz **“o que”** controlar; a ITIL diz **“como”** entregar serviços.

**PMBOK.** **49 processos, 5 grupos, 10 áreas** (6ª edição); **47 processos** (5ª edição). **Iniciação 2, Planejamento 24, Execução 10, Monitoramento e Controle 12, Encerramento 1.** O **termo de abertura** é emitido pelo **patrocinador**, não pelo gerente, e **autoriza** o projeto. **EAP é orientada a entregas**, não a atividades. **Caminho crítico = sequência mais longa, folga zero.** **Crashing** = mais recursos (custo); **fast tracking** = paralelismo (risco). **PERT = (O + 4M + P) / 6.** **Canais = n(n − 1) / 2.** **EVM**: variação = VA − algo; índice = VA / algo; > 1 ou positivo = bom. **Reserva de contingência** = riscos **identificados**; **reserva gerencial** = riscos **não identificados**. **Ameaças**: evitar, transferir, mitigar, aceitar; **oportunidades**: explorar, compartilhar, melhorar, aceitar. **Gerenciar a qualidade** = processo (preventivo); **controlar a qualidade** = produto (detectivo).

**Pontos de função.** Os **cinco componentes**: **ALI, AIE, EE, SE, CE**. **ALI é mantido pela aplicação; AIE é só referenciado.** **EE mantém ALI; SE tem cálculo/derivação; CE é só consulta.** Valores: **ALI 7/10/15; AIE 5/7/10; EE 3/4/6; SE 4/5/7; CE 3/4/6.** **VAF = TDI × 0,01 + 0,65**, entre **0,65 e 1,35**, com **14 CGS** de grau 0 a 5. A APF **mede tamanho, não esforço**: esforço é **derivado** com a produtividade histórica.

**Palavras absolutas.** Cuidado com “**sempre**”, “**nunca**”, “**somente**”, “**garante**”.

---

## ANÁLISE DO QUE MAIS CAI (TENDÊNCIA, 2020 A 2026)

Reforço: análise qualitativa de tendência, e não estatística oficial.

**Incidência alta:** na ITIL, o **ciclo de vida** (cinco livros e seus processos), as **definições de incidente, problema, mudança e requisição**, e o **SLA/OLA/UC**. No COBIT, a **estrutura em domínios** (PO/AI/DS/ME no 4.1 e EDM/APO/BAI/DSS/MEA no 5) e a **distinção entre governança e gerenciamento**. No PMBOK, os **grupos de processos e áreas de conhecimento**, **EAP**, **caminho crítico**, **EVM** e **gerenciamento de riscos**. Em pontos de função, a **identificação dos componentes (ALI, AIE, EE, SE, CE)** e o **cálculo do VAF**.

**Incidência média:** **ITIL 4** (SVS, princípios, cadeia de valor), **COBIT 5** (os cinco princípios e sete habilitadores), **estruturas organizacionais** e **termo de abertura** no PMBOK, **técnicas de cronograma** (PERT, crashing e fast tracking), **tipos de contrato**, **estratégias de resposta a riscos** e **pontos por complexidade** na APF.

**Incidência menor, mas que aparece como pegadinha:** **COBIT 2019**, **PMBOK 7ª edição**, **ISO/IEC 20000 e 38500**, **contagem NESMA** e **continuidade de serviço (RTO e RPO)**.

**Formato típico:** afirmativas certo/errado com troca de conceitos (incidente por problema, validação do escopo por controle da qualidade, ALI por AIE, governança por gestão), associação entre sigla e definição, **cálculos** (EVM, PERT, canais de comunicação, caminho crítico, pontos de função, VAF) e questões que dão um cenário e pedem o processo ou o componente correspondente. Como o tema é muito **numérico e decorativo**, o treino de questões faz grande diferença.

**Estratégia de estudo:** dê **prioridade** a **ITIL v3 (processos de operação e transição)**, **COBIT (domínios e a diferença entre versões)**, **PMBOK (áreas e processos, EVM, riscos, cronograma)** e **APF (identificação de componentes e cálculo)**. Para os cálculos, **resolva muitos exercícios**, porque a fórmula decorada sem prática não resiste à pressão da prova. Para as listas de processos e domínios, use **cartões de memorização (_flashcards_)** e **mnemônicos próprios**. Resolva questões anteriores da ESFCEx e de bancas parecidas para calibrar a profundidade.