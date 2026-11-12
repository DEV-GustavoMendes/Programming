## PARTE B: LEVANTAMENTO DE SISTEMAS

### Contexto e objetivo

O **levantamento de sistemas** (também chamado de levantamento de requisitos, elicitação de requisitos ou levantamento de fatos) é a atividade em que o analista coleta informações sobre o sistema atual, o ambiente organizacional e as necessidades dos usuários, para entender o problema e especificar o que o novo sistema deve fazer. Falhas nessa etapa são a principal causa de projetos de software que estouram prazo e orçamento, porque um erro de requisito descoberto tardiamente custa muito mais para corrigir.

O analista precisa escolher as técnicas conforme o contexto: número de pessoas envolvidas, grau de conhecimento dos usuários, tempo e orçamento disponíveis e complexidade do problema. Raramente uma técnica sozinha basta, e o mais comum é combinar várias.

### 1. Observação pessoal

Na **observação pessoal**, o analista acompanha diretamente as pessoas trabalhando, no próprio local, para ver como as atividades realmente acontecem, em vez de apenas ouvir uma descrição delas. Ela pode ser **passiva** (o analista apenas observa, sem interferir) ou **ativa/participante** (o analista executa parte das tarefas para vivenciá-las).

Entre as **vantagens**, a principal é que ela revela **a realidade, e não a versão relatada**. As pessoas muitas vezes descrevem o processo como ele deveria ser, e não como ele é de fato, e a observação expõe os desvios, os atalhos informais, os gargalos e as interrupções. Também permite perceber o ambiente físico, o fluxo de documentos e a interação entre pessoas, e fornece dados de primeira mão.

Entre as **desvantagens**, ela é **demorada**, pode **interferir no comportamento** das pessoas observadas (o chamado **Efeito Hawthorne**, em que as pessoas, sabendo que estão sendo observadas, alteram o modo de trabalhar), pode não captar situações raras ou sazonais (como o fechamento anual) e depende da percepção do observador, com risco de interpretação subjetiva. Além disso, é difícil usá-la para atividades que envolvem raciocínio, que não são visíveis.

Um caso real: para automatizar o atendimento em uma agência bancária, o analista passa dias no atendimento e percebe que os caixas anotam informações em papéis paralelos porque o sistema legado não tem um campo necessário. Isso nunca apareceria em uma entrevista, porque os próprios caixas já não percebem como um problema.

### 2. Questionário

O **questionário** é um conjunto padronizado de perguntas respondidas por escrito, em papel ou meio eletrônico, normalmente sem a presença do analista. Ele é ideal quando é preciso colher informações de **muitas pessoas**, **dispersas geograficamente**, com **baixo custo**.

As perguntas podem ser **abertas** (o respondente escreve livremente, o que fornece riqueza de detalhes mas dificulta a tabulação) ou **fechadas** (o respondente escolhe entre opções pré-definidas, incluindo escalas, múltipla escolha e sim/não, o que facilita a análise estatística mas limita a profundidade). Também existem questionários **mistos**.

Entre as **vantagens**: baixo custo, rapidez para atingir muita gente, padronização das respostas (facilita a comparação e a tabulação), possibilidade de **anonimato** (que estimula respostas mais sinceras) e conveniência para o respondente, que escolhe quando responder.

Entre as **desvantagens**: **baixo índice de retorno** (muitas pessoas não respondem), **impossibilidade de esclarecer dúvidas** ou aprofundar uma resposta na hora, risco de **interpretação errada** das perguntas, respostas superficiais, difícil captar nuances e dependência da boa elaboração do instrumento (um questionário mal construído gera dados inúteis). Também não é adequado para entender processos complexos.

Boas práticas: perguntas claras, curtas, sem ambiguidade nem indução de resposta; ordem lógica; teste-piloto antes da aplicação ampla; e linguagem adequada ao público.

### 3. Entrevista

A **entrevista** é uma conversa dirigida entre o analista e o entrevistado, com o objetivo de obter informações sobre processos, necessidades, problemas e expectativas. É a técnica mais usada e uma das mais ricas, pois permite interação direta.

Quanto à estrutura, a entrevista pode ser **estruturada** (roteiro fixo de perguntas, na mesma ordem para todos, facilitando a comparação mas reduzindo a flexibilidade), **não estruturada** (conversa livre, só com o tema definido, o que dá flexibilidade e profundidade mas exige experiência e é difícil de comparar e documentar) ou **semiestruturada** (roteiro-base com liberdade para explorar pontos que surgirem, que é o formato mais comum).

O processo costuma ter quatro etapas: **preparação** (definir objetivos, escolher e agendar os entrevistados, estudar o assunto e preparar o roteiro), **condução** (apresentar o objetivo, criar clima de confiança, fazer perguntas, ouvir ativamente e observar a linguagem não verbal), **encerramento** (resumir o que foi entendido e agradecer) e **documentação** (registrar e validar com o entrevistado logo após).

Entre as **vantagens**: permite **esclarecer dúvidas na hora**, aprofundar respostas, captar a linguagem não verbal, obter informações que as pessoas não escreveriam, ajustar as perguntas conforme as respostas e criar vínculo com o usuário, o que favorece o seu envolvimento no projeto.

Entre as **desvantagens**: é **demorada e cara** (exige tempo do analista e do entrevistado), depende da **habilidade do entrevistador**, pode haver **viés** (do entrevistador ou do entrevistado), o entrevistado pode omitir ou distorcer informações por receio, e a análise de várias entrevistas dá trabalho. Também há o risco de conflito entre versões de diferentes entrevistados, que precisam ser reconciliadas.

Dica prática: entrevistar tanto o **nível gerencial** (que conhece os objetivos e as regras de negócio) quanto o **nível operacional** (que conhece como o trabalho realmente é feito), porque as visões costumam divergir.

### 4. JAD (Joint Application Design)

O **JAD** é uma técnica **em grupo**, criada pela **IBM** no fim dos anos 1970 (por Chuck Morris e Tony Crawford), que reúne em **sessões estruturadas e facilitadas** os usuários, os analistas, os gestores e demais interessados, para levantar requisitos e definir o projeto do sistema de maneira conjunta e rápida. Em vez de várias entrevistas individuais separadas, concentra-se todos em **workshops intensivos**, normalmente em uma sala preparada para isso, com recursos como quadros, projetores e material de apoio, e longe das interrupções do dia a dia.

Os papéis típicos de uma sessão JAD são: o **facilitador (líder da sessão)**, que conduz o encontro de forma neutra, mantém o foco e media conflitos (e deve conhecer a técnica, não necessariamente o negócio); o **patrocinador executivo**, que tem autoridade e dá apoio ao projeto; os **usuários** e **gestores**, que conhecem o negócio e definem as necessidades; o **escriba** (ou secretário), que documenta tudo o que for decidido; e os **analistas e desenvolvedores/técnicos**, que participam, esclarecem as restrições técnicas e depois transformam as decisões em especificações.

O JAD costuma seguir três grandes etapas: **adaptação/preparação** (definir objetivos e escopo, selecionar participantes, preparar o material e o local), **sessão** (realização dos workshops, com discussão estruturada e registro) e **finalização** (consolidar a documentação, validar com os participantes e obter a aprovação).

Entre as **vantagens**: **rapidez** (condensa semanas de entrevistas em poucos dias), **consenso** (os conflitos de visão aparecem e são resolvidos na própria sessão), **maior envolvimento e comprometimento** dos usuários com o sistema, **redução de erros e retrabalho** de requisitos, melhor comunicação entre a área de negócio e a de TI, e documentação produzida no próprio processo.

Entre as **desvantagens**: **dificuldade de reunir** todos os participantes ao mesmo tempo (agendas de gestores), **custo elevado** da parada de pessoas-chave por períodos longos, exigência de um **facilitador experiente**, possibilidade de **dominância** de participantes mais fortes ou hierarquicamente superiores inibirem os demais, e o risco de a sessão ser improdutiva se for mal preparada.

JAD é uma técnica **colaborativa** e está ligada à ideia de **participação do usuário**, o que a aproxima das práticas ágeis, embora seja anterior a elas.

### Outras técnicas de levantamento (para não errar por exclusão)

Mesmo que o edital cite apenas quatro, a prova pode trazer outras como alternativas. A **análise de documentos** examina formulários, manuais, relatórios e regulamentos existentes. A **prototipação** constrói modelos do sistema para o usuário reagir. O **brainstorming** gera ideias em grupo, sem críticas iniciais. A **etnografia** é uma observação aprofundada e prolongada do ambiente de trabalho. Os **cenários e casos de uso** e as **histórias de usuário** descrevem interações com o sistema. A **análise de sistemas similares** estuda soluções já existentes. A **reunião/workshop** é mais genérica e menos estruturada que o JAD.

### Comparação rápida das quatro técnicas

|Técnica|Melhor uso|Ponto forte|Ponto fraco|
|---|---|---|---|
|Observação pessoal|Entender o que realmente acontece|Dados reais, sem filtro|Demorada e sujeita ao Efeito Hawthorne|
|Questionário|Muitas pessoas, dispersas|Baixo custo e padronização|Baixo retorno, sem esclarecimento|
|Entrevista|Detalhar processos e necessidades|Flexibilidade e profundidade|Cara, demorada e dependente do entrevistador|
|JAD|Consenso rápido entre muitos envolvidos|Rapidez, consenso, envolvimento|Difícil de agendar e custoso|

---

## MACETES E TRUQUES PARA A PROVA

**Dado, informação e conhecimento.** Associe: dado é o bruto, informação é o dado com contexto e conhecimento é a informação com experiência para agir. Se a questão disser que “dado é o resultado do processamento da informação”, inverteram: está errada.

**SI não é sinônimo de computador.** Sempre que a alternativa afirmar que um sistema de informação “necessariamente utiliza computadores”, desconfie.

**Pirâmide e sistemas.** Decore a ligação: **operacional é SPT** (transações, dado detalhado, decisão estruturada); **tático é SIG** (relatórios, dado resumido); **apoio à decisão é SAD** (simulação, semiestruturada); **estratégico é SIE** (painel, não estruturada, dados externos).

**SIG versus SAD.** Se a questão fala em **relatórios pré-definidos**, é SIG. Se fala em **modelos, simulações, interatividade e “e se?”**, é SAD.

**Os três componentes do SAD.** Lembre-se de **dados, modelos e interface**.

**Análise é “o quê”, projeto é “como”.** Questões sobre a fase em que se definem requisitos (análise) ou arquitetura (projeto) usam essa diferença.

**Manutenção.** Memorize por palavra-chave: corretiva é **erro**, adaptativa é **mudança de ambiente/legislação**, perfectiva é **melhoria/nova funcionalidade** e preventiva é **evitar problemas futuros**. Mudança em lei fiscal que exige alterar o sistema é adaptativa.

**Cascata.** Se a questão diz “permite retornar facilmente a fases anteriores sem custo”, desconfie, pois o cascata puro é rígido e sequencial. O **espiral** é o que tem **análise de riscos**. A **prototipação** serve para **requisitos incertos**.

**Observação pessoal.** Palavra-chave: **Efeito Hawthorne**, ou seja, a pessoa observada muda seu comportamento.

**Questionário.** Palavras-chave: **muitas pessoas, baixo custo, baixo retorno, não permite esclarecimento**. Perguntas **fechadas** facilitam tabulação, **abertas** dão riqueza.

**Entrevista.** Palavras-chave: **flexibilidade, esclarecimento imediato, depende do entrevistador, demorada**. Estruturada tem roteiro fixo, não estruturada é livre.

**JAD.** Palavras-chave: **IBM, sessões em grupo, facilitador, consenso, rapidez, envolvimento do usuário**. Se a questão fala em “técnica em que o analista trabalha isolado do usuário”, está errada, pois o JAD é justamente a participação conjunta.

**Cuidado com as palavras absolutas.** Alternativas com “sempre”, “nunca”, “somente”, “exclusivamente” costumam estar erradas em questões de teoria de sistemas.

---

## ANÁLISE DO QUE MAIS CAI (TENDÊNCIA, 2020 A 2026)

Como mencionei no início, esta análise é qualitativa. Em provas de Informática para concursos militares e de bancas comparáveis, o comportamento deste conteúdo tende a ser o seguinte.

**Incidência alta:** o **ciclo de vida e os modelos de desenvolvimento** (cascata, espiral, prototipação, incremental, ágil) e a **ordem e o conteúdo das fases** (levantamento, análise, projeto, implementação, testes, manutenção). É o tema mais recorrente da área, e as questões exploram a diferença entre análise e projeto e os tipos de manutenção. Também tem peso alto a **classificação dos sistemas por nível organizacional** (SPT, SIG, SAD, SIE), muitas vezes em formato de “associe o sistema à descrição”.

**Incidência média:** **conceitos básicos** (dado, informação, conhecimento; componentes do SI), as **técnicas de levantamento** (principalmente comparação de vantagens e desvantagens, e a identificação da técnica a partir de uma descrição), e o **SAD** (componentes e tipos de decisão).

**Incidência menor, mas que aparece como pegadinha:** o **JAD** em si (costuma ser cobrado como “qual técnica reúne os envolvidos em sessões estruturadas”), a **teoria geral de sistemas** (feedback, fronteira, ambiente) e as **estratégias de implantação**.

**Formato típico das questões:** afirmativas certo/errado com troca sutil de conceitos (por exemplo, trocar manutenção adaptativa por corretiva), questões de associação, e descrições curtas de um cenário em que o candidato precisa identificar a técnica ou o tipo de sistema.

**Estratégia de estudo:** invista mais tempo no ciclo de vida e nos modelos, dedique atenção ao quadro comparativo de sistemas por nível, e treine a leitura de questões buscando a **palavra-chave** que revela o conceito. Em seguida, resolva questões de provas anteriores da própria ESFCEx e de bancas semelhantes para calibrar o nível de detalhe cobrado.