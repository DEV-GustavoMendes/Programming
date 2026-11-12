## PARTE G: GERENCIAMENTO DE SERVIÇOS DE TI E ITIL

### 1. Conceitos fundamentais

O **Gerenciamento de Serviços de TI (ITSM, _IT Service Management_)** é o conjunto de **capacidades organizacionais** (políticas, processos, funções, pessoas) usado para **projetar, entregar, gerenciar e melhorar os serviços de TI** em benefício do negócio. A ideia central é mudar o foco: em vez de gerenciar **tecnologia** (servidores, redes, aplicações), a TI passa a gerenciar **serviços** que geram **valor** para o cliente.

A **ITIL (_Information Technology Infrastructure Library_, Biblioteca de Infraestrutura de TI)** é o conjunto mais difundido de **boas práticas** para ITSM. Ela nasceu no fim dos anos 1980 no Reino Unido, na **CCTA** (agência do governo britânico, depois incorporada ao **OGC**), como resposta à má qualidade dos serviços de TI no setor público. Mais tarde, passou a ser mantida pela **AXELOS** e, depois de 2021, pela **PeopleCert**. Uma característica muito cobrada: a ITIL **não é uma norma** nem uma metodologia rígida, e sim um **framework de boas práticas**, **independente de fornecedor e de tecnologia**, que deve ser **adaptado** a cada organização. A norma ligada à ITIL, **que permite certificação** da organização, é a **ISO/IEC 20000**.

As versões principais são: **ITIL v1/v2** (anos 1990 e 2000, com os livros de Suporte a Serviços e Entrega de Serviços), **ITIL v3** (2007, atualizada em **2011**, organizada em **ciclo de vida do serviço**) e **ITIL 4** (2019, baseada em **cadeia de valor de serviço**). Como a v3 ainda é a mais cobrada, ela recebe mais espaço aqui.

#### Definições-chave (ITIL v3)

Um **serviço** é um **meio de entregar valor aos clientes**, facilitando os resultados que eles desejam alcançar, **sem que tenham de gerenciar os custos e riscos específicos**. O **valor** do serviço é formado por duas dimensões: a **utilidade (_utility_)**, ou “**adequação ao propósito**” (o que o serviço **faz**, ou seja, as funcionalidades), e a **garantia (_warranty_)**, ou “**adequação ao uso**” (como ele **é entregue**: disponibilidade, capacidade, continuidade e segurança). **Valor = utilidade + garantia**, e as duas são necessárias. Um **processo** é um conjunto estruturado de atividades projetado para atingir um objetivo específico, que transforma entradas em saídas, é mensurável, entrega resultados a um cliente e responde a um evento específico. Uma **função** é uma unidade organizacional com papéis e responsabilidades para executar um conjunto de atividades (por exemplo, a Central de Serviços). Os **papéis** relevantes incluem o **dono do serviço (_service owner_)**, o **dono do processo (_process owner_)** e o **gerente do processo (_process manager_)**. O modelo **RACI** (Responsável, Aprovador, Consultado, Informado) é a ferramenta para definir as responsabilidades.

Outros conceitos que caem em prova: o **SLA (_Service Level Agreement_, Acordo de Nível de Serviço)** é o acordo **entre o provedor de TI e o cliente**; o **OLA (_Operational Level Agreement_, Acordo de Nível Operacional)** é o acordo **interno**, entre áreas da própria organização de TI; o **UC (_Underpinning Contract_, Contrato de Apoio)** é o acordo com um **fornecedor externo**. A regra é que os OLAs e UCs **sustentam** o cumprimento do SLA. O **item de configuração (IC, _CI_)** é qualquer componente que precisa ser gerenciado para entregar um serviço. O **BDGC/CMDB (Banco de Dados de Gerenciamento da Configuração)** armazena os ICs e seus relacionamentos. O **SKMS (Sistema de Gerenciamento do Conhecimento do Serviço)** é o repositório mais abrangente, que inclui o CMDB. O **KEDB (banco de dados de erros conhecidos)** registra os erros conhecidos e seus _workarounds_. O **KPI** é o indicador-chave de desempenho e o **CSF/FCS** é o fator crítico de sucesso.

### 2. O ciclo de vida do serviço (ITIL v3, edição 2011)

A ITIL v3 se organiza em **cinco publicações (livros)**, que formam o **ciclo de vida do serviço**:

**1. Estratégia de Serviço (_Service Strategy_)** está no **centro** do ciclo e define **por que** e **para quem** o serviço existe, alinhando a TI à estratégia do negócio. Seus processos são: **gerenciamento da estratégia para serviços de TI**, **gerenciamento do portfólio de serviços**, **gerenciamento financeiro para serviços de TI**, **gerenciamento da demanda** e **gerenciamento do relacionamento com o negócio**.

**2. Desenho de Serviço (_Service Design_)** projeta os serviços novos ou alterados, os processos e a infraestrutura. Seus processos são: **coordenação do desenho**, **gerenciamento do catálogo de serviços**, **gerenciamento do nível de serviço (SLM)**, **gerenciamento da disponibilidade**, **gerenciamento da capacidade**, **gerenciamento da continuidade do serviço de TI (ITSCM)**, **gerenciamento da segurança da informação** e **gerenciamento de fornecedores**. Os **quatro Ps** do desenho são **pessoas, processos, produtos (tecnologia) e parceiros (fornecedores)**, e as **cinco perspectivas** do desenho de serviço incluem: soluções de serviço, sistemas de gerenciamento e ferramentas (portfólio), arquitetura tecnológica, processos, e métodos de medição e métricas.

**3. Transição de Serviço (_Service Transition_)** leva o serviço projetado ao ambiente de produção de forma controlada. Seus processos são: **planejamento e suporte à transição**, **gerenciamento de mudanças**, **gerenciamento de ativos de serviço e configuração (SACM)**, **gerenciamento de liberação e implantação**, **validação e teste de serviço**, **avaliação de mudança** e **gerenciamento do conhecimento**.

**4. Operação de Serviço (_Service Operation_)** entrega e gerencia os serviços no dia a dia. Seus processos são: **gerenciamento de eventos**, **gerenciamento de incidentes**, **gerenciamento de requisições de serviço**, **gerenciamento de problemas** e **gerenciamento de acesso**. As **funções** são: **Central de Serviços (_Service Desk_)**, **gerenciamento técnico**, **gerenciamento de operações de TI** e **gerenciamento de aplicações**.

**5. Melhoria Contínua de Serviço (_Continual Service Improvement_, CSI)** envolve todos os estágios e busca melhorar continuamente os serviços e processos, usando o **ciclo PDCA** e o **processo de melhoria em sete passos** (definir o que medir, definir o que pode ser medido, coletar os dados, processar os dados, analisar os dados, apresentar e usar a informação, implementar a melhoria). Ela se apoia no **modelo de melhoria de Deming**.

Macete para a ordem: **E**stratégia, **D**esenho, **T**ransição, **O**peração e **M**elhoria contínua, ou seja, “**E**u **D**evo **T**estar **O**s **M**ódulos”, ou invente o seu.

### 3. Processos mais cobrados em detalhe

#### Gerenciamento de incidentes

Um **incidente** é uma **interrupção não planejada** de um serviço de TI ou **redução da sua qualidade**. O objetivo do processo é **restaurar o serviço normal o mais rápido possível**, **minimizando o impacto no negócio**, e não encontrar a causa raiz (isso é papel do problema). O fluxo típico: identificação e registro, categorização, priorização, diagnóstico inicial, escalonamento (**funcional**, para um grupo técnico mais especializado, ou **hierárquico**, para a gerência), investigação e diagnóstico, resolução e recuperação, e encerramento. A **prioridade** é calculada pela combinação de **impacto** (efeito sobre o negócio) e **urgência** (rapidez necessária para resolver): **Prioridade = Impacto × Urgência**. A solução temporária que restaura o serviço sem eliminar a causa é o **contorno (_workaround_)**.

#### Gerenciamento de problemas

Um **problema** é a **causa raiz** de um ou mais incidentes. Os objetivos são **evitar** que os incidentes ocorram e **minimizar o impacto** dos que não puderem ser evitados. Há dois enfoques: **reativo** (analisa os incidentes ocorridos para achar a causa raiz) e **proativo** (identifica tendências e falhas potenciais antes que gerem incidentes). Quando a causa raiz é identificada e há um _workaround_, o problema passa a ser um **erro conhecido**, registrado no **KEDB**. Uma **requisição de mudança (RDM)** é aberta quando a solução definitiva exige alteração na infraestrutura. Uma técnica comum de análise é o **diagrama de Ishikawa** (causa e efeito) e os “5 porquês”. A confusão mais cobrada: **incidente é o sintoma (restaurar rápido); problema é a causa (investigar e eliminar)**.

#### Gerenciamento de requisições de serviço

Uma **requisição de serviço** é um **pedido formal de um usuário** para algo **padronizado e pré-aprovado**, como redefinir senha, instalar um software ou criar uma conta. **Não é um incidente** (não há falha) **nem uma mudança** (que exige avaliação e autorização mais complexas). O processo é geralmente mais **rápido e padronizado**, apoiado pelo **catálogo de serviços**.

#### Gerenciamento de mudanças

Uma **mudança** é a adição, modificação ou remoção de qualquer item que possa afetar os serviços de TI. O objetivo é **controlar o ciclo de vida de todas as mudanças**, permitindo que elas ocorram com **mínima interrupção** dos serviços. O instrumento é a **requisição de mudança (RDM, _RFC_)**, avaliada pelo **CAB (_Change Advisory Board_, Comitê Consultivo de Mudanças)**. Para **mudanças emergenciais**, convoca-se o **ECAB (CAB de emergência)**. Os tipos de mudança são: **padrão** (de baixo risco, **pré-autorizada**, com procedimento definido, que **não passa pelo CAB** a cada ocorrência); **normal** (segue o **processo completo** de avaliação, autorização e implantação); e **emergencial** (necessária com **urgência** para resolver um incidente grave ou risco crítico, com **procedimento acelerado**). Também existe o **modelo de mudança** (um procedimento predefinido para um tipo de mudança) e a **mudança de maior risco**, que exige avaliação e autorização em níveis mais altos. O gerenciamento de mudanças **não é o mesmo** que o de liberação: a mudança **autoriza**, e a liberação **planeja, constrói, testa e implanta**.

#### Gerenciamento de configuração e de ativos de serviço

O objetivo é **manter informações precisas sobre os ICs** e seus relacionamentos no **CMDB**, suportando os demais processos (como incidentes e mudanças). Suas atividades incluem **identificação**, **controle**, **registro de status**, **verificação** e **auditoria**. A **linha de base de configuração (baseline)** é a configuração aprovada, usada como referência para comparar e reverter.

#### Gerenciamento de liberação e implantação

Planeja, programa e controla a **construção, teste e implantação** de releases em produção. A **DML (_Definitive Media Library_, Biblioteca de Mídia Definitiva)** guarda as versões aprovadas do software, e o **DHS** guarda peças de hardware aprovadas. Os tipos de implantação se parecem com os estudados na Parte A (_big bang_, em fases, _push_ e _pull_, manual e automatizada).

#### Gerenciamento de níveis de serviço (SLM)

Negocia, acorda e monitora os **SLAs**, OLAs e UCs, e produz os **relatórios de nível de serviço**. O **SLR (_Service Level Requirements_)** é o conjunto de requisitos do cliente que dá origem ao SLA. O **Plano de Melhoria do Serviço (SIP)** acompanha as melhorias.

#### Gerenciamento da capacidade, disponibilidade e continuidade

O **gerenciamento da capacidade** garante que haja **capacidade suficiente**, no momento certo e ao **custo justificável** (subprocessos de capacidade de negócio, de serviço e de componente). O **gerenciamento da disponibilidade** garante que os serviços estejam **disponíveis** quando necessários, usando métricas como **MTBF** (tempo médio entre falhas, relacionado à **confiabilidade**), **MTRS** (tempo médio para restaurar o serviço, relacionado à **manutenibilidade**) e **MTBSI** (tempo médio entre incidentes do sistema). Os aspectos da disponibilidade são **disponibilidade, confiabilidade, manutenibilidade, facilidade de serviço (_serviceability_) e segurança**. O **gerenciamento da continuidade (ITSCM)** garante a recuperação dos serviços após um desastre, com base na **análise de impacto no negócio (BIA)** e na **análise de risco**; seus conceitos incluem **RTO** (tempo máximo tolerável para retomada) e **RPO** (perda máxima tolerável de dados). Os tipos de recuperação, do mais rápido ao mais lento, costumam ser: **espelhamento/_hot standby_**, **recuperação imediata**, **intermediária (_warm_)** e **gradual (_cold_)**.

#### Gerenciamento de eventos e acesso

Um **evento** é uma **ocorrência detectável** significativa para a gestão da infraestrutura (informativo, de alerta ou de exceção). O **gerenciamento de acesso** concede aos **usuários autorizados** o direito de usar um serviço, **impedindo** o acesso de não autorizados, e executa as políticas do gerenciamento de segurança.

#### A Central de Serviços (_Service Desk_)

É a **função** que atua como **ponto único de contato (SPOC)** entre os usuários e a TI. Seus tipos são: **local** (junto aos usuários), **centralizado** (um só local, atendendo várias unidades), **virtual** (a localização física é indiferente, atuando pela rede) e _**follow-the-sun**_ (atendimento 24 horas, repassando o atendimento entre fusos horários diferentes). **Nível 1** é o primeiro atendimento (registro e resolução simples), **nível 2** envolve suporte técnico mais especializado, e **nível 3** envolve especialistas ou fornecedores.

### 4. ITIL 4 (2019)

A **ITIL 4** modernizou o framework para a era da transformação digital, integrando ideias de **Lean, Agile e DevOps**. Suas principais estruturas são:

O **Sistema de Valor de Serviço (SVS, _Service Value System_)** descreve como todos os componentes e atividades da organização trabalham juntos para **cocriar valor**. Seus componentes são: os **princípios orientadores**, a **governança**, a **cadeia de valor de serviço**, as **práticas** e a **melhoria contínua**, tendo como entrada a **oportunidade e a demanda**, e como saída o **valor**.

Os **sete princípios orientadores** são: **focar no valor**, **começar de onde você está**, **progredir iterativamente com feedback**, **colaborar e promover visibilidade**, **pensar e trabalhar de forma holística**, **manter simples e prático** e **otimizar e automatizar**.

A **cadeia de valor de serviço** tem **seis atividades**: **planejar**, **melhorar**, **engajar**, **desenhar e fazer a transição**, **obter/construir** e **entregar e suportar**.

As **quatro dimensões** do gerenciamento de serviços são: **organizações e pessoas**, **informação e tecnologia**, **parceiros e fornecedores** e **fluxos de valor e processos**.

A ITIL 4 substitui os **processos** da v3 por **34 práticas** (gerais de gerenciamento, de gerenciamento de serviços e de gerenciamento técnico), como gerenciamento de incidentes, de problemas, de mudanças (agora **habilitação de mudanças**), central de serviço e gerenciamento de nível de serviço. Os **quatro conceitos principais** são **valor, cocriação de valor, organização e relacionamentos de serviço**. Macete: se a questão fala em **SVS, cadeia de valor e princípios orientadores**, é ITIL 4; se fala em **ciclo de vida com cinco livros e 26 processos**, é a v3.