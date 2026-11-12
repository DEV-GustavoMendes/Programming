## PARTE A: SISTEMAS DE INFORMAÇÃO

### 1. Conceitos e definições básicas

Para entender um sistema de informação, é preciso começar pela distinção entre dado, informação e conhecimento, porque a prova adora cobrar esse trio.

**Dado** é um registro bruto, isolado, sem contexto e sem significado por si só. O número “38” ou a palavra “Pouso Alegre”, sozinhos, são dados. **Informação** é o dado processado e contextualizado, de modo que passa a ter significado e utilidade para quem vai tomar uma decisão. “A temperatura em Pouso Alegre hoje é 38 graus” é informação. **Conhecimento** é a informação incorporada ao raciocínio de uma pessoa ou organização, combinada com experiência, valores e contexto, de forma que permite agir. Saber que, com 38 graus, o consumo de energia do data center sobe e é preciso reforçar a refrigeração é conhecimento. A hierarquia clássica vai de dado para informação, para conhecimento e, em alguns autores, para sabedoria.

Um **sistema**, na definição da Teoria Geral de Sistemas (Ludwig von Bertalanffy), é um conjunto de elementos interdependentes que interagem para atingir um objetivo comum. Todo sistema tem **entradas** (insumos), **processamento** (transformação), **saídas** (produtos), **feedback** (retroalimentação, que permite ajustar o sistema), **fronteira** (o limite que separa o sistema do que está fora dele) e **ambiente** (tudo o que está fora da fronteira e interage com o sistema). Um sistema pode ser aberto, quando troca matéria, energia e informação com o ambiente, ou fechado, quando isso não ocorre. As organizações são sistemas abertos.

Um **Sistema de Informação (SI)** é um conjunto de componentes inter-relacionados que coleta, processa, armazena e distribui dados e informações para apoiar a tomada de decisão, a coordenação, o controle, a análise e a visualização em uma organização. Repare que a definição não menciona computador. Essa é uma pegadinha clássica: **um sistema de informação não precisa ser informatizado**. Um arquivo de fichas em papel, com regras de preenchimento e consulta, é um sistema de informação. Quando o sistema usa computadores, fala-se em **sistema de informação baseado em computador** ou sistema informatizado.

Também é importante distinguir **Sistema de Informação** de **Tecnologia da Informação**. A TI é o conjunto de recursos tecnológicos (hardware, software, redes, bancos de dados) que **suporta** os sistemas de informação. O SI é mais amplo, pois envolve também pessoas, processos e a organização. A TI é uma das partes do SI, e não o contrário.

Os sistemas de informação desempenham três papéis principais nas organizações: apoiar as **operações** (processos do dia a dia), apoiar a **tomada de decisão** (em todos os níveis) e apoiar a **estratégia** (gerar vantagem competitiva). Três atividades básicas compõem um SI: **entrada** (captação dos dados), **processamento** (transformação em informação) e **saída** (distribuição da informação), com **armazenamento** e **controle/feedback** como atividades de apoio.

### 2. Componentes de um sistema de informação

A literatura divide os componentes de maneiras ligeiramente diferentes, mas o conjunto que mais aparece é este.

**Hardware** é a parte física: computadores, servidores, dispositivos de entrada e saída, armazenamento e equipamentos de rede. **Software** é a parte lógica: sistemas operacionais, aplicativos, sistemas gerenciadores de bancos de dados e utilitários. **Dados** são a matéria-prima, normalmente organizados em bancos de dados. **Pessoas** (também chamadas de _peopleware_) incluem usuários, analistas, programadores, administradores de banco de dados e gestores. Para muitos autores, são o componente mais importante e o mais difícil de gerenciar. **Procedimentos** (ou processos) são as regras, rotinas e políticas que definem como o sistema é usado e como as atividades são executadas. **Redes e telecomunicações** são a infraestrutura que conecta os demais componentes e permite o compartilhamento de recursos e informações.

Existem variações de lista que caem em prova. O modelo de **O’Brien**, por exemplo, fala em cinco recursos: humanos, de hardware, de software, de dados e de rede. Outras bancas citam apenas **hardware, software, dados, pessoas e procedimentos**. Se a questão disser que o SI é composto por “hardware, software, dados, pessoas e procedimentos”, considere correto. Se disser que o SI é composto “somente por hardware e software”, está errado.

### 3. Sistemas de informação operacional e gerencial

Essa classificação se apoia na **pirâmide organizacional**, que divide a empresa em três níveis de decisão: **operacional** (base), **tático ou gerencial** (meio) e **estratégico** (topo). Cada nível demanda um tipo diferente de sistema.

#### Sistemas de nível operacional

Os sistemas operacionais apoiam o nível mais baixo da organização, que executa as atividades rotineiras e registra as transações. O principal exemplo é o **Sistema de Processamento de Transações (SPT, em inglês TPS, _Transaction Processing System_)**. Ele registra e processa as transações diárias da empresa: uma venda, um pagamento, a emissão de uma nota fiscal eletrônica, um saque em caixa eletrônico, o lançamento de ponto de um funcionário, a baixa de um item de estoque.

As características principais são: trabalha com **grande volume de dados detalhados**, atende a **decisões estruturadas e repetitivas** (aquelas em que o procedimento é conhecido e padronizado), tem **foco no curto prazo**, exige **alta velocidade, confiabilidade e integridade** e serve de **fonte de dados** para os demais sistemas. Pense no PDV (ponto de venda) de um supermercado: cada leitura de código de barras é uma transação, e o somatório delas alimenta os sistemas gerenciais.

Um conceito ligado ao TPS que cai muito é o das propriedades **ACID** (atomicidade, consistência, isolamento e durabilidade) das transações em bancos de dados, que garantem a integridade quando o sistema registra, por exemplo, uma transferência bancária em que o débito de uma conta e o crédito de outra precisam acontecer juntos ou não acontecer.

#### Sistemas de nível gerencial (tático)

Os **Sistemas de Informação Gerencial (SIG, em inglês MIS, _Management Information System_)** atendem ao nível tático. Eles pegam os dados dos sistemas operacionais e os transformam em **relatórios resumidos e consolidados** que ajudam os gerentes a monitorar, controlar e planejar. Normalmente, atendem a **decisões estruturadas ou semiestruturadas**, com foco no médio prazo.

Os relatórios do SIG costumam ser classificados em quatro tipos: **periódicos** (gerados em intervalos fixos, como o relatório mensal de vendas), **sob demanda** (gerados quando o gestor solicita), **de exceção** (gerados apenas quando algum indicador foge do padrão, como “produtos com estoque abaixo do mínimo”) e **de indicadores-chave** (que resumem o desempenho). A diferença fundamental em relação ao SPT é que o SIG trabalha com **dados agregados e sintetizados**, enquanto o SPT trabalha com **dados detalhados e transacionais**.

#### Sistemas de nível estratégico

Embora o tópico do edital cite operacional e gerencial, vale conhecer o topo da pirâmide para não errar questões comparativas. Os **Sistemas de Informação Executiva (SIE, em inglês EIS ou ESS, _Executive Support System_)** atendem à alta administração. Lidam com **decisões não estruturadas**, de longo prazo, com dados internos e externos (tendências de mercado, concorrência, legislação), geralmente apresentados em **painéis (_dashboards_)** gráficos e fáceis de navegar.

#### Outros tipos de sistemas que aparecem em prova

**Sistemas de Gestão Empresarial (ERP)** integram em uma única base de dados todos os processos da empresa (financeiro, estoque, produção, compras, recursos humanos). **CRM** gerencia o relacionamento com clientes. **SCM** gerencia a cadeia de suprimentos. **Sistemas Especialistas** reproduzem o raciocínio de um especialista humano em um domínio restrito, usando base de conhecimento e motor de inferência. **Sistemas de Automação de Escritório** apoiam o trabalho administrativo (editores de texto, planilhas, e-mail). **Sistemas de Gestão do Conhecimento** capturam e compartilham conhecimento organizacional.

#### Quadro comparativo para memorizar

|Característica|Operacional (SPT)|Gerencial (SIG)|Apoio à Decisão (SAD)|Executivo (SIE)|
|---|---|---|---|---|
|Nível|Operacional|Tático|Tático/estratégico|Estratégico|
|Dados|Detalhados|Resumidos|Resumidos + modelos|Agregados, internos e externos|
|Decisões|Estruturadas|Estruturadas/semiestruturadas|Semiestruturadas/não estruturadas|Não estruturadas|
|Horizonte|Curto prazo|Médio prazo|Variável|Longo prazo|
|Exemplo|Folha de pagamento, PDV|Relatório mensal de vendas|Simulação de cenários|Painel de indicadores|

### 4. Sistema de Apoio à Decisão (SAD)

O **SAD (em inglês DSS, _Decision Support System_)** é um sistema **interativo**, baseado em computador, que ajuda os tomadores de decisão a usar dados e **modelos** para resolver problemas **semiestruturados e não estruturados**. A diferença para o SIG é que o SIG responde a perguntas pré-definidas por meio de relatórios fixos, enquanto o SAD permite que o usuário **explore**, **simule** e **teste hipóteses**, fazendo perguntas que não foram previstas na construção do sistema.

Para entender o que é decisão estruturada, semiestruturada e não estruturada, recorra ao modelo de **Herbert Simon**. Em uma decisão **estruturada**, o problema é rotineiro, há um procedimento conhecido e a resposta pode ser automatizada (conceder um desconto previsto em tabela). Em uma decisão **não estruturada**, o problema é novo e complexo, e não há procedimento definido; é necessário julgamento, avaliação e intuição (entrar ou não em um novo mercado). Na **semiestruturada**, parte do problema pode ser resolvida por procedimento conhecido, mas parte exige julgamento humano (definir o orçamento de marketing com base em projeções).

Simon também descreveu as **fases do processo decisório**: **inteligência** (identificar e compreender o problema), **projeto** (desenvolver e analisar alternativas), **escolha** (selecionar a melhor alternativa) e **implementação/monitoramento** (executar a decisão e acompanhar). Cai em prova a ordem delas.

#### Componentes do SAD

A arquitetura clássica do SAD tem três subsistemas principais. O **banco de dados** (ou subsistema de gerenciamento de dados) armazena os dados internos e externos que alimentam as análises. O **banco de modelos** (subsistema de gerenciamento de modelos) contém os modelos matemáticos, estatísticos, financeiros e de otimização usados nas simulações. O **subsistema de interface** (ou de diálogo) permite a interação amigável entre o usuário e o sistema. Alguns autores adicionam o **subsistema de gerenciamento de conhecimento** e os **usuários** como elementos da arquitetura.

#### Técnicas e ferramentas associadas

A análise _what-if_ (“e se?”) permite alterar variáveis e observar o efeito sobre o resultado (e se o dólar subir 10%?). A análise de sensibilidade verifica o quanto o resultado muda ao se variar uma única variável. A análise de metas (_goal seeking_) faz o caminho inverso: dado o resultado desejado, descobre qual valor as variáveis devem assumir. A otimização busca a melhor solução dentro de restrições. Em ambientes de BI, o SAD usa **Data Warehouse** (repositório integrado, histórico e orientado a assuntos), **Data Mart** (subconjunto do warehouse voltado a uma área), **OLAP** (processamento analítico online, com operações como _drill-down_, _roll-up_, _slice_ e _dice_) e **Data Mining** (mineração de dados, que descobre padrões e correlações não evidentes). Existem ainda os **GDSS**, ou SAD em grupo, que apoiam decisões tomadas por equipes.

Um caso real: uma rede de varejo usa um SAD para simular o impacto de uma promoção. O gestor altera o percentual de desconto, a duração e a categoria de produtos, e o sistema, com base em dados históricos de vendas e em modelos de elasticidade de preço, projeta o volume de vendas e a margem esperada.

### 5. Ciclo de vida de sistemas de informação

O **ciclo de vida de um sistema** (SDLC, _Systems Development Life Cycle_) descreve as fases pelas quais um sistema passa, desde a concepção até a desativação. Existem diversas formas de organizar essas fases, e é aí que moram as pegadinhas, pois a prova pode usar nomenclaturas diferentes para a mesma ideia.

#### Fases clássicas (modelo em cascata)

O **modelo em cascata** (_waterfall_), descrito por Winston Royce em 1970, organiza o desenvolvimento em fases sequenciais, em que cada fase só começa quando a anterior termina. As fases típicas são:

1. **Planejamento / estudo de viabilidade / levantamento preliminar:** define o escopo e avalia se o projeto vale a pena. A viabilidade pode ser **técnica** (temos tecnologia e competência?), **econômica** (o benefício supera o custo?), **operacional** (os usuários e a organização vão aceitar e usar?), **legal** (atende à legislação e a contratos?) e de **cronograma** (é possível entregar no prazo?).
2. **Análise:** levanta e especifica **o que** o sistema deve fazer, ou seja, os requisitos funcionais e não funcionais.
3. **Projeto (design):** define **como** o sistema vai fazer, ou seja, arquitetura, modelagem de dados, interfaces, módulos e tecnologias.
4. **Implementação (codificação):** os programadores constroem o sistema.
5. **Testes:** verificam se o sistema atende aos requisitos (unidade, integração, sistema e aceitação).
6. **Implantação:** coloca o sistema em produção.
7. **Manutenção:** corrige, adapta e evolui o sistema ao longo da vida útil.

Uma frase que ajuda a gravar: **a análise responde “o quê?” e o projeto responde “como?”**.

Os **requisitos funcionais** descrevem o que o sistema faz (emitir nota fiscal, calcular imposto). Os **requisitos não funcionais** descrevem qualidades e restrições (desempenho, segurança, usabilidade, disponibilidade, portabilidade). Essa distinção cai com frequência.

#### Estratégias de implantação

Na implantação (_cutover_), existem quatro estratégias principais. A **direta** (ou _big bang_) desliga o sistema antigo e liga o novo de uma vez, sendo mais rápida e mais arriscada. A **paralela** mantém os dois sistemas rodando simultaneamente por um período, sendo mais segura e mais cara. A **piloto** implanta o novo sistema em uma unidade ou grupo restrito antes de expandir. A **faseada** implanta módulo por módulo ou etapa por etapa.

#### Tipos de manutenção

A manutenção é a fase mais longa e cara do ciclo de vida, e a prova costuma cobrar seus quatro tipos. A **corretiva** corrige defeitos. A **adaptativa** adapta o sistema a mudanças no ambiente (nova legislação, novo sistema operacional, nova versão do banco de dados), e por isso é muito associada ao contexto fiscal. A **perfectiva** (ou evolutiva) melhora o sistema ou acrescenta funcionalidades novas, a pedido do usuário. A **preventiva** reestrutura o sistema para evitar problemas futuros e facilitar manutenções posteriores (por exemplo, refatoração).

#### Outros modelos de ciclo de vida

O **modelo de prototipação** constrói protótipos para validar requisitos com o usuário, útil quando os requisitos são incertos. O **modelo incremental** entrega o sistema em partes sucessivas, cada uma agregando funcionalidades. O **modelo iterativo** repete ciclos de desenvolvimento refinando o sistema. O **modelo espiral** (Barry Boehm, 1986) combina prototipação e cascata com ênfase na **análise de riscos**, percorrendo ciclos com quatro quadrantes (planejamento, análise de riscos, engenharia e avaliação pelo cliente). O **RUP (Rational Unified Process)** é iterativo e incremental, dirigido por casos de uso, com quatro fases (concepção, elaboração, construção e transição) e várias disciplinas. Os **métodos ágeis** (Scrum, XP, Kanban), baseados no Manifesto Ágil de 2001, valorizam indivíduos e interações, software funcionando, colaboração com o cliente e resposta a mudanças.

A **ISO/IEC 12207** é a norma internacional que define os processos do ciclo de vida de software, e a **ISO/IEC 25010** trata da qualidade de produto de software. Vale ter esses números na memória, pois aparecem como alternativas em questões.