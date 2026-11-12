## PARTE H: GOVERNANÇA DE TI E COBIT

### 1. Governança de TI: conceitos

A **governança corporativa** é o sistema pelo qual as organizações são **dirigidas, monitoradas e incentivadas**. A **governança de TI** é a parte dela que assegura que a TI **sustenta e amplia** a estratégia e os objetivos da organização, que seus **recursos são usados com responsabilidade** e que os **riscos são gerenciados**. A distinção clássica, cobradíssima, é esta: **governança** define **quem decide** e **como se prestam contas** (**direciona e monitora**; é atribuição do **conselho e da alta direção**); **gestão (gerenciamento)** **planeja, constrói, executa e monitora** as atividades dentro da direção traçada (é atribuição da **gerência executiva**). Em uma frase: **governança é “avaliar, dirigir e monitorar”; gestão é “planejar, construir, executar e monitorar”**.

As cinco **áreas de foco da governança de TI** (do IT Governance Institute, usadas no COBIT 4.1) são: **alinhamento estratégico**, **entrega de valor**, **gestão de recursos**, **gestão de riscos** e **mensuração de desempenho**. Macete: **A**linhamento, **V**alor, **R**ecursos, **R**iscos, **D**esempenho.

A norma **ISO/IEC 38500** (governança corporativa de TI) estabelece o modelo **Avaliar, Dirigir e Monitorar (EDM)** e **seis princípios**: **responsabilidade**, **estratégia**, **aquisição**, **desempenho**, **conformidade** e **comportamento humano**.

### 2. O COBIT

O **COBIT (_Control Objectives for Information and related Technology_)** é um **framework de governança e gestão de TI** criado pela **ISACA** (em conjunto com o **ITGI**, _IT Governance Institute_). Surgiu em 1996 como um conjunto de **objetivos de controle** para auditoria de TI e evoluiu para um modelo de governança. Seu foco é a **ligação entre os requisitos de negócio, os processos de TI e os recursos de TI**, com **controles** e **métricas**. Ele **não diz “como fazer”** em detalhe (diferente da ITIL, mais operacional); diz **“o que” deve ser controlado e medido**. Por isso, o COBIT é **complementar** à ITIL, ao PMBOK e às normas ISO.

Versões: **COBIT 1, 2, 3** (auditoria e controle), **COBIT 4.0/4.1** (2005/2007, governança de TI), **COBIT 5** (2012, governança e gestão corporativas de TI) e **COBIT 2019** (a mais recente). Cada uma tem estrutura própria.

### 3. COBIT 4.1

Foi a versão mais cobrada por muito tempo. Sua estrutura central é o chamado **cubo do COBIT**, com três dimensões: os **critérios de informação**, os **recursos de TI** e os **processos de TI**.

Os **sete critérios de informação** são: **eficácia**, **eficiência**, **confidencialidade**, **integridade**, **disponibilidade**, **conformidade** e **confiabilidade**. Os **quatro recursos de TI** são **aplicativos**, **informações**, **infraestrutura** e **pessoas**. Os **34 processos** se agrupam em **quatro domínios**, que formam um ciclo:

|Domínio|Sigla|Foco|
|---|---|---|
|**Planejar e Organizar**|**PO**|Estratégia, táticas, organização, arquitetura, riscos, projetos|
|**Adquirir e Implementar**|**AI**|Soluções de TI, aquisição, desenvolvimento, mudanças|
|**Entregar e Suportar**|**DS**|Entrega dos serviços, suporte, segurança, operação|
|**Monitorar e Avaliar**|**ME**|Monitoramento do desempenho, controle interno, conformidade, governança|

Macete: **P-A-E-M** (Planejar, Adquirir, Entregar, Monitorar), que acompanha o ciclo PDCA. O COBIT 4.1 usa um **modelo de maturidade** com **seis níveis (0 a 5)**: **0 Inexistente**, **1 Inicial/_ad hoc_**, **2 Repetível, porém intuitivo**, **3 Processo definido**, **4 Gerenciado e mensurável** e **5 Otimizado**. Ele também define **metas de TI, metas de processo e metas de atividade**, medidas por **indicadores de resultado (_outcome measures_, ou KGIs)** e **indicadores de desempenho (_performance drivers_, ou KPIs)**. Outros elementos incluem os **objetivos de controle**, as **práticas de controle** e o modelo **RACI**.

### 4. COBIT 5 (2012)

Reposicionou o COBIT como **framework de governança e gestão corporativa de TI**, integrando o **Val IT** (valor de investimentos de TI) e o **Risk IT** (riscos), e alinhando-se a normas como a ISO/IEC 38500 e a ISO/IEC 15504.

Os **cinco princípios** são (muito cobrados):

1. **Atender às necessidades das partes interessadas** (por meio da **cascata de objetivos**).
2. **Cobrir a organização de ponta a ponta** (não só a área de TI).
3. **Aplicar um modelo único e integrado** (alinhado a outros padrões).
4. **Habilitar uma abordagem holística** (por meio dos habilitadores).
5. **Distinguir governança de gerenciamento.**

A **cascata de objetivos (_goals cascade_)** traduz as **necessidades das partes interessadas** em **metas corporativas**, depois em **metas relacionadas à TI** e depois em **metas dos habilitadores**, conectando negócio e TI. Os **sete habilitadores (_enablers_)** são: **princípios, políticas e frameworks**; **processos**; **estruturas organizacionais**; **cultura, ética e comportamento**; **informação**; **serviços, infraestrutura e aplicativos**; e **pessoas, habilidades e competências**.

O **modelo de referência de processos** tem **37 processos**, divididos em **governança** e **gestão**:

|Área|Domínio|Sigla|Nº de processos|
|---|---|---|---|
|**Governança**|**Avaliar, Dirigir e Monitorar**|**EDM**|5|
|**Gestão**|**Alinhar, Planejar e Organizar**|**APO**|13|
|**Gestão**|**Construir, Adquirir e Implementar**|**BAI**|10|
|**Gestão**|**Entregar, Serviço e Suporte**|**DSS**|6|
|**Gestão**|**Monitorar, Avaliar e Medir**|**MEA**|3|

Total: 5 + 13 + 10 + 6 + 3 = **37**. Macete: **governança tem um domínio (EDM) e gestão tem quatro (APO, BAI, DSS, MEA)**, em correspondência com os domínios PO, AI, DS e ME do COBIT 4.1.

O COBIT 5 adota o **modelo de avaliação da capacidade de processos** baseado na **ISO/IEC 15504**, com **seis níveis (0 a 5)**: **0 Incompleto**, **1 Executado**, **2 Gerenciado**, **3 Estabelecido**, **4 Previsível** e **5 Otimizando**. Ele **substituiu** o antigo modelo de maturidade do COBIT 4.1.

### 5. COBIT 2019

Aprimorou o COBIT 5 e o tornou mais flexível e personalizável. Trabalha com **seis princípios do sistema de governança** (fornecer valor às partes interessadas; abordagem holística; sistema de governança dinâmico; governança distinta de gerenciamento; adaptado às necessidades da empresa; sistema de governança de ponta a ponta) e **três princípios do framework** (baseado em um modelo conceitual; aberto e flexível; alinhado aos principais padrões). O modelo de processos passa a ter **40 objetivos de governança e gestão**, nos **mesmos cinco domínios** (EDM, APO, BAI, DSS, MEA). Introduz os **fatores de design** (para adaptar o sistema de governança ao contexto da organização) e os **componentes** (processos, estruturas organizacionais, fluxos de informação, pessoas e habilidades, políticas e procedimentos, cultura e comportamento, e serviços, infraestrutura e aplicações), além de oferecer um modelo de **desempenho de gestão** e **níveis de capacidade** de 0 a 5.

### 6. COBIT, ITIL, PMBOK e ISO: como se relacionam

Um quadro de memorização:

|Referencial|Foco|Pergunta que responde|
|---|---|---|
|**COBIT**|Governança e gestão de TI (controle)|**“O que** devemos controlar e medir?”|
|**ITIL**|Gerenciamento de **serviços** de TI|**“Como** entregar bons serviços?”|
|**PMBOK**|Gerenciamento de **projetos**|**“Como** gerenciar projetos?”|
|**ISO/IEC 27001**|Segurança da informação|Como proteger a informação?|
|**ISO/IEC 20000**|Gestão de serviços (certificável)|A organização atende ao padrão de ITSM?|
|**ISO/IEC 38500**|Governança corporativa de TI|Como dirigir e monitorar a TI?|
|**CMMI/MPS.BR**|Maturidade do processo de software|Qual a maturidade dos processos?|