## Sistemas Gerenciadores de Banco de Dados (SGBD)

Este tópico faz fronteira com o anterior e é onde a prova da ESFCEx realmente cobra banco de dados. A análise de incidência está no final. Ela usa as provas de 2025 e 2026, que li na íntegra na etapa anterior.

### 1. Princípios e conceitos fundamentais

Um **banco de dados** (BD) é uma coleção organizada de dados inter-relacionados, que representa aspectos do mundo real (o “minimundo”), é projetada para uma finalidade específica e pode ser compartilhada por vários usuários e aplicações. Um **Sistema Gerenciador de Banco de Dados (SGBD)** é o software que permite **definir, construir, manipular, proteger e compartilhar** bancos de dados. Já o **sistema de banco de dados** é o conjunto formado pelo banco de dados, pelo SGBD e pelas aplicações que o utilizam. Essa distinção aparece em prova: o SGBD (Oracle, PostgreSQL, MySQL, SQL Server) **não é** o banco de dados, ele o gerencia.

Quatro verbos aparecem em todas as definições clássicas (Elmasri e Navathe):

- **Definir:** especificar tipos de dados, estruturas e restrições, o que é feito pela DDL.
- **Construir:** armazenar os dados em um meio controlado pelo SGBD.
- **Manipular:** consultar, atualizar e gerar relatórios, o que é feito pela DML.
- **Compartilhar:** permitir acesso simultâneo de vários usuários e programas.

#### A abordagem de arquivos e por que ela foi superada

Antes dos SGBDs, cada aplicação mantinha seus **próprios arquivos**, com formato embutido no código do programa. Isso gerava problemas que justificam a existência do SGBD: **redundância** (o mesmo dado repetido em vários arquivos), **inconsistência** (as cópias divergem), **dependência entre dados e programas** (mudar o formato do arquivo obriga a reescrever os programas), **dificuldade de acesso** (cada nova consulta exige um novo programa), **isolamento de dados**, **problemas de integridade** (as regras ficavam espalhadas no código), **atomicidade** das atualizações (uma queda no meio de uma transferência bancária deixava o sistema em estado inválido), **acesso concorrente** anômalo e **problemas de segurança**. Cada uma das características do SGBD é a resposta a um desses problemas.

#### Características da abordagem de banco de dados

**Natureza autodescritiva.** O SGBD guarda, além dos dados, a **descrição da estrutura** do banco. Esse conteúdo é o **catálogo** ou **dicionário de dados**, que contém os **metadados** (dados sobre os dados). Um exemplo é o código de criação de uma tabela, que fica armazenado no próprio banco.

**Independência entre programas e dados.** É a capacidade de mudar a estrutura de armazenamento sem alterar as aplicações. Fica mais claro com a arquitetura de três esquemas, que veremos adiante.

**Abstração de dados e múltiplas visões.** O usuário vê o que lhe interessa, sem saber como o dado é guardado. Cada tipo de usuário pode ter uma **visão (view)** diferente do mesmo banco.

**Compartilhamento e processamento de transações multiusuário.** Vários usuários acessam e alteram os dados ao mesmo tempo, com garantia de correção. O SGBD controla a **concorrência** e preserva as propriedades das transações.

### 2. Objetivos e vantagens dos sistemas de banco de dados

Os objetivos do SGBD, enunciados de formas ligeiramente diferentes pelos autores, são os seguintes:

**Controle de redundância.** Não se trata de eliminá-la a qualquer custo, mas de **controlá-la**. Em bancos operacionais, a normalização reduz a redundância. Em data warehouses, às vezes se redundam dados de propósito (desnormalização) para ganhar desempenho. Por isso “eliminar totalmente a redundância” é afirmação suspeita em prova.

**Consistência e integridade.** O SGBD impõe **restrições de integridade** (de domínio, de entidade, referencial e semânticas) e evita que o banco entre em estado inválido.

**Segurança.** Controla quem acessa o quê, por meio de autenticação, autorização, privilégios (`GRANT` e `REVOKE`), visões e, em muitos casos, criptografia e auditoria.

**Compartilhamento de dados.** Vários usuários e aplicações usam a mesma base, o que também facilita padronizar e centralizar o controle.

**Independência de dados.** Pode ser lógica ou física, como veremos.

**Backup e recuperação (recovery).** Após uma falha (queda de energia, erro de disco, falha de software), o SGBD restaura o banco a um estado consistente, usando **logs** (diários de transações), _checkpoints_ e cópias de segurança.

**Controle de concorrência.** Permite acessos simultâneos sem que as atualizações de um usuário corrompam as de outro.

**Eficiência no acesso.** O SGBD usa índices, otimização de consultas e _buffers_ para responder rapidamente.

**Múltiplas interfaces e representação de relacionamentos complexos.** Oferece linguagens de consulta, interfaces gráficas, APIs e a capacidade de representar relacionamentos entre os dados.

**Restrição a acessos não autorizados e inferência de novas informações** (dados derivados por regras e consultas) são objetivos citados em parte da literatura.

Como contrapartida, usar um SGBD tem **custos**: licenças e hardware, treinamento, **complexidade** de administração, **sobrecarga** em aplicações muito simples e a **dependência** do fornecedor. Existem situações em que **não** vale a pena usar um SGBD: aplicações simples, bem definidas e sem expectativa de mudança, requisitos de tempo real muito rigorosos (o SGBD adiciona sobrecarga) ou sistemas de usuário único sem necessidade de compartilhamento.

### 3. Arquitetura e componentes

#### Arquitetura de três esquemas (ANSI/SPARC)

Cobrada em quase toda prova de banco de dados, essa arquitetura separa o banco em três níveis:

- **Nível interno (físico):** descreve como os dados são **armazenados** (arquivos, índices, estruturas físicas). O **esquema interno** é definido pela DDL de armazenamento.
- **Nível conceitual (lógico):** descreve a **estrutura de todo o banco** para a comunidade de usuários, ocultando detalhes físicos. Contém entidades, atributos, relacionamentos, tipos de dados e restrições. É o **esquema conceitual**, tipicamente definido em um modelo como o relacional.
- **Nível externo (de visão):** descreve a parte do banco que interessa a cada grupo de usuários. Há um **esquema externo** (visão) para cada tipo de usuário.

Entre os níveis existem **mapeamentos** (transformações) que o SGBD executa: externo→conceitual e conceitual→interno.

Essa arquitetura viabiliza os dois tipos de **independência de dados**:

- **Independência lógica:** alterar o esquema conceitual (adicionar um atributo, por exemplo) sem alterar os esquemas externos e os programas. É mais difícil de obter.
- **Independência física:** alterar o esquema interno (reorganizar arquivos, criar um índice, trocar de disco) sem alterar o esquema conceitual. É a mais comum.

Macete: **lógica = conceitual intocado nas visões; física = interno muda e o conceitual não percebe.**

#### Esquema e instância

O **esquema** é a descrição da estrutura do banco, e raramente muda. A **instância** (ou _estado_) é o conjunto de dados armazenados em um dado momento, e muda o tempo todo. Uma mesma tabela mantém o esquema enquanto as linhas entram e saem.

#### Linguagens do SGBD

O SGBD oferece sublinguagens, hoje reunidas no **SQL**:

|Sigla|Nome|Função|Comandos típicos|
|---|---|---|---|
|**DDL**|Data Definition Language|Define esquemas e estruturas|`CREATE`, `ALTER`, `DROP`|
|**DML**|Data Manipulation Language|Manipula os dados|`INSERT`, `UPDATE`, `DELETE`, `SELECT` (alguns autores colocam o `SELECT` na DQL)|
|**DCL**|Data Control Language|Controla permissões|`GRANT`, `REVOKE`|
|**DTL/TCL**|Transaction Control Language|Controla transações|`COMMIT`, `ROLLBACK`, `SAVEPOINT`|
|**DQL**|Data Query Language|Consulta|`SELECT`|

Na terminologia clássica, havia também a **VDL** (linguagem de definição de visões) e a **SDL** (linguagem de definição de armazenamento). Hoje a DDL costuma abranger tudo.

A DML pode ser **procedural** (diz _como_ obter, como as linguagens navegacionais) ou **declarativa/não procedural** (diz _o que_ se quer, como o SQL).

#### Componentes de um SGBD

Os módulos internos de um SGBD tradicional incluem:

- **Compilador/pré-compilador de DDL e DML**, que traduz comandos em operações internas.
- **Processador de consultas**, com o **otimizador**, que escolhe o plano de execução mais barato.
- **Gerenciador de transações**, com o **controle de concorrência** e o **gerenciador de recuperação**.
- **Gerenciador de armazenamento**, com o **gerenciador de buffer**, o **gerenciador de arquivos** e os métodos de acesso (índices).
- **Dicionário de dados (catálogo)**, que guarda os metadados.
- **Mecanismos de segurança e autorização.**

#### Arquiteturas de sistema

Quanto à distribuição, o sistema de banco de dados pode ser **centralizado** (tudo em um servidor), **cliente-servidor** (o cliente envia consultas e o servidor as processa), em **duas ou três camadas** (a de três camadas separa apresentação, aplicação e dados), **paralelo** (várias CPUs e discos para ganhar desempenho) e **distribuído** (dados espalhados por vários locais, com transparência de localização).

### 4. Propriedades das transações (ACID)

Uma **transação** é uma unidade lógica de trabalho: uma sequência de operações que deve ser executada **por inteiro ou de forma nenhuma**. O exemplo clássico é a transferência de R$ 100 entre contas, com um débito e um crédito. As propriedades **ACID** são:

- **Atomicidade (Atomicity):** a transação é indivisível. Ou todas as operações são efetivadas, ou nenhuma. Garantida pelo **gerenciador de recuperação** (logs e _rollback_).
- **Consistência (Consistency):** a transação leva o banco de um estado consistente a outro estado consistente, respeitando as restrições de integridade.
- **Isolamento (Isolation):** transações concorrentes não interferem umas nas outras; cada uma deve ter a impressão de executar sozinha. Garantido pelo **controle de concorrência**.
- **Durabilidade (Durability):** depois do _commit_, as alterações **permanecem**, mesmo diante de falhas. Garantida pelo gerenciador de recuperação.

Macete: **A** de atomicidade, e não de “autenticação”. **I** de isolamento, e não de “indexação”. **C** de consistência, e não de “coesão”. **D** de durabilidade, e não de “dependência”. Essas são exatamente as alternativas erradas que a prova costuma usar.

#### Controle de concorrência

Sem controle, surgem anomalias: **atualização perdida**, **leitura suja** (ler dado ainda não confirmado), **leitura não repetível** e **fantasmas**. As técnicas principais de controle de concorrência são os protocolos baseados em **bloqueio (lock)**, como o **bloqueio em duas fases (2PL)**, com bloqueios compartilhados (leitura) e exclusivos (escrita). Os protocolos baseados em **timestamp** (marca de tempo) ordenam as transações por tempo de início. Os protocolos de **validação** (otimistas) e o **controle multiversão (MVCC)** são variantes importantes. O problema que o bloqueio pode causar é o **deadlock** (impasse), quando duas transações esperam uma pela outra. O SGBD o trata por **detecção** e abortando uma delas, ou por **prevenção**.

O critério de correção é a **serializabilidade**: o resultado da execução concorrente deve ser equivalente ao de alguma execução serial.

#### Recuperação

Técnicas de recuperação usam o **log** (registro sequencial das operações), com **escrita antecipada de log (WAL)**, _checkpoints_, _undo_ (desfazer o que não foi confirmado) e _redo_ (refazer o que foi confirmado mas não gravado em disco). Essas ideias sustentam a **durabilidade** e a **atomicidade**.

### 5. Usuários de bancos de dados

A literatura costuma dividir os usuários em duas grandes categorias, os que **projetam e administram** e os que **usam**. A divisão exata muda um pouco entre autores, mas o essencial é o seguinte.

**Administrador de Banco de Dados (DBA).** Responsável pelo **gerenciamento técnico** do ambiente. Autoriza acessos, define a estrutura física, monitora o desempenho, realiza backup e recuperação, cria índices, aplica _patches_ e coordena a segurança. Tem os privilégios mais altos sobre o SGBD. Existe também o **administrador de dados (AD)**, que cuida das decisões sobre _quais_ dados armazenar e das políticas de negócio, em oposição às decisões técnicas do DBA.

**Projetistas de banco de dados.** Identificam os dados que serão guardados e escolhem as estruturas adequadas para representá-los. Conversam com os futuros usuários para entender seus requisitos e produzem o modelo conceitual e lógico.

**Analistas de sistemas e programadores de aplicação.** Os analistas determinam as necessidades dos usuários finais e especificam as transações. Os programadores implementam essas especificações em código, usando linguagens como Java, Python ou PHP, com SQL embutido ou APIs como o JDBC.

**Usuários finais.** São os que usam o banco no dia a dia. Dividem-se em:

- **Casuais (ocasionais):** acessam de vez em quando, com necessidades variadas, normalmente gerentes. Usam linguagens de consulta.
- **Ingênuos ou paramétricos (naïve):** executam sempre operações previstas e padronizadas, por meio de telas prontas. Exemplos: caixas de banco, atendentes de reservas e funcionários de balcão.
- **Sofisticados:** engenheiros, cientistas e analistas que escrevem suas próprias consultas e conhecem bem os recursos do SGBD.
- **Autônomos (stand-alone):** mantêm bancos pessoais com pacotes prontos e interfaces simples.

**Pessoal de suporte e manutenção do sistema.** Inclui os **projetistas e implementadores de SGBD** (que constroem o próprio software do SGBD), os **desenvolvedores de ferramentas** (pacotes de projeto, monitoramento, interfaces) e os **operadores e pessoal de manutenção** (administração do hardware e do software do ambiente).

Para a prova, o gancho mais cobrado é a **diferença entre DBA e usuário final**, e entre usuário **ingênuo** (usa programas prontos) e **sofisticado** (escreve consultas diretamente).

### 6. Classificação dos SGBDs

Os SGBDs se classificam por critérios diferentes. Pelo **modelo de dados**, há SGBDs relacionais, hierárquicos, de rede, orientados a objetos, objeto-relacionais e **NoSQL** (chave-valor, documento, coluna larga e grafo). Pelo **número de usuários**, monousuário ou multiusuário. Pela **distribuição**, centralizados ou distribuídos. Pelo **propósito**, de uso geral ou especializados. Pelo **custo**, comerciais ou de código aberto (como PostgreSQL e MySQL). O NoSQL costuma relaxar o ACID em troca de escalabilidade, adotando o modelo **BASE** e a **consistência eventual**, em que as alterações não se propagam de forma imediata entre as réplicas.

### 7. Macetes e truques para a prova

- **SGBD ≠ banco de dados.** SGBD é o software. Um banco pode existir sem SGBD (arquivos), mas não a vantagem de gerenciar.
- **Dicionário de dados = catálogo = metadados.** Guarda as definições de esquemas, como o código de criação de uma tabela.
- **Cuidado com “elimina totalmente a redundância”.** O correto é “controla” ou “reduz”.
- **Lógica × física:** lógica mexe no conceitual sem afetar visões. Física mexe no interno sem afetar o conceitual.
- **Esquema = estrutura (estável). Instância = dados (muda).**
- **Siglas das linguagens:** DDL define, DML manipula, DCL controla acesso (GRANT/REVOKE), TCL controla transação (COMMIT/ROLLBACK).
- **ACID decorado como “Atomicidade, Consistência, Isolamento, Durabilidade”.** Qualquer outra palavra para a letra é a pegadinha.
- **Isolamento é do controle de concorrência. Atomicidade e durabilidade são da recuperação.**
- **Deadlock** aparece em bloqueio. **Timestamp** é o outro grande protocolo. As duas famílias aparecem juntas como alternativa certa.
- **Usuário ingênuo usa telas prontas. Sofisticado escreve consultas. DBA administra.**
- **Atributos de valor único × multivalorados; simples × compostos; derivados** (calculados a partir de outros, como a idade a partir da data de nascimento). Esse vocabulário de modelagem veio do ER e costuma ser cobrado junto com SGBD.
- **Alternativas absolutas** (“sempre”, “somente”, “nunca”) continuam sendo a principal armadilha nas questões da VUNESP.

### 8. O que mais cai: análise das provas

Pelas provas de **2025** e **2026** da ESFCEx (Oficial do Quadro Complementar, Informática, VUNESP), que li na íntegra, banco de dados ocupa **uma pequena fatia do bloco específico, em torno de 3 a 4 questões por prova**, concentradas no fim do caderno (questões 47 a 50). O quadro que consegui verificar é este:

|Ano|Questões de banco de dados identificadas|
|---|---|
|2025|Símbolos do diagrama entidade-relacionamento; comando SQL `CREATE TABLE`; propriedades ACID; modelo multidimensional (tabela fato e dimensão)|
|2026|Tipos de atributos no modelo entidade-relacionamento; comando SQL `CREATE TABLE`; protocolos de controle de concorrência (bloqueio e timestamp)|

Além delas, as duas provas trazem uma questão de **JDBC** em Java (o pacote `java.sql` em 2025 e o método `DriverManager.getConnection()` em 2026), que mistura banco de dados e programação.

Do que li, a leitura é clara: em 2025 e 2026 **não caiu** questão sobre “objetivos de um SGBD”, “usuários de banco de dados” ou “arquitetura de três esquemas” em forma direta. A cobrança foi **operacional e de modelagem**: SQL de criação de tabela (nas duas provas), ER (nas duas), ACID e concorrência (uma em cada prova). **O conceito de SGBD aparece como base para esses assuntos, e não como pergunta própria.** Isso é uma inferência de duas provas e não vale como estatística de sete anos. Para 2020 a 2024 não confirmei o conteúdo, e o portal da ESFCEx publica provas anteriores (2021, 2022 e 2023 aparecem) que você pode conferir.

Com base nisso, a minha recomendação, que é julgamento e não dado, é esta. Estude **este tópico com atenção para ACID, controle de concorrência e a arquitetura de três esquemas**, porque são os trechos que de fato se convertem em questão. Decore a distinção DDL, DML, DCL e TCL, já que `CREATE TABLE` é DDL e aparece em toda prova. Treine **escrever `CREATE TABLE` com chave primária** (a sintaxe correta é `Coluna TIPO PRIMARY KEY`), porque essa questão se repetiu com variações nas duas provas. O tema “usuários de banco de dados” e “objetivos do SGBD” merece uma leitura firme, mas é o que tem menor probabilidade de cair como questão direta, a julgar pelo que vi.