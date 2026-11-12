### 1. Conceito e definições

Para a computação, um **dado** é um fato bruto registrado, como o número 1500 ou o texto “Maria”. A **informação** é o dado interpretado em um contexto, como “o salário de Maria é R$ 1.500”. Um **arquivo** é uma coleção nomeada de dados relacionados, gravada em memória secundária (disco magnético, SSD, fita, mídia óptica), que persiste mesmo com o computador desligado.

A hierarquia clássica de estruturação vai do menor para o maior. O **bit** forma o **byte**, o byte forma o **campo** (a menor unidade de informação com significado, como “nome” ou “CPF”), o conjunto de campos forma o **registro** (os dados de uma entidade, como um aluno), o conjunto de registros forma o **arquivo**, e o conjunto de arquivos inter-relacionados forma o **banco de dados**.

Alguns conceitos aparecem o tempo todo nas questões:

- **Chave** é o campo, ou combinação de campos, usado para identificar ou localizar registros. A chave primária identifica o registro de forma única. A chave secundária pode se repetir e serve de caminho alternativo de busca.
- **Bloco** (ou página) é a unidade de transferência entre disco e memória. O sistema nunca lê um byte isolado do disco. Ele lê um bloco inteiro.
- **Fator de bloco** é o número de registros que cabem em um bloco, calculado como o piso de B dividido por R, em que B é o tamanho do bloco e R o tamanho do registro. Com bloco de 4096 bytes e registro de 100 bytes, o fator é 40.
- **Registros de tamanho fixo e variável.** Nos fixos, localizar o n-ésimo registro é simples aritmética. Nos variáveis, é preciso delimitadores ou um prefixo com o tamanho.
- **Organização lógica e organização física.** A lógica é como o programador enxerga os dados (sequência de registros, tabela). A física é como eles realmente ficam nos setores, trilhas e cilindros.
- **Heap file.** É o arquivo sem ordenação, em que os registros ficam na ordem de inserção.

O custo de acessar um arquivo é medido em **número de acessos a disco**, não em operações de CPU. Em um disco magnético, um acesso soma o tempo de busca (_seek_, o deslocamento do braço até a trilha), a latência rotacional (esperar o setor passar sob a cabeça) e o tempo de transferência. O _seek_ domina. Por isso toda a engenharia de organização de arquivos tenta reduzir o número de blocos lidos. Em SSD não há partes móveis, mas o raciocínio por blocos e páginas continua valendo.

As operações básicas sobre arquivos são inserção, remoção, alteração, consulta (pontual, por intervalo ou total), ordenação e reorganização. Cada organização que veremos é, no fundo, um compromisso diferente entre o custo dessas operações.

### 2. Estruturas de armazenamento de dados

O padrão **ANSI/SPARC** descreve três níveis de abstração. O **nível externo** (visões) é o que cada usuário enxerga. O **nível conceitual** descreve o banco inteiro (entidades, relacionamentos, restrições). O **nível interno** (físico) descreve como os dados são armazenados, com arquivos, índices e blocos. Disso vem a **independência de dados**. A lógica é a capacidade de mudar o esquema conceitual sem quebrar as visões dos usuários. A física é a capacidade de mudar a organização dos arquivos sem alterar o esquema conceitual. Esse é um tema recorrente em prova.

Em termos de estruturas, vale separar duas ideias. A **organização primária do arquivo** define onde cada registro é fisicamente colocado: heap, ordenado ou por hash. Os **caminhos de acesso secundários** são estruturas auxiliares que aceleram buscas por outros campos: índices (esparsos, densos, multinível, árvores B e B+) e arquivos invertidos. Um mesmo arquivo de dados pode ter uma organização primária e vários índices secundários.

Por baixo disso estão as estruturas de dados clássicas aplicadas a disco: listas encadeadas (cadeias de overflow), tabelas de espalhamento (hash) e árvores balanceadas. O SGBD também mantém um **buffer pool** em memória, que guarda blocos recentes para evitar leituras repetidas, e um **dicionário de dados** (catálogo) com as definições de esquemas, tabelas, índices e permissões.

### 3. Modelos de dados

Um **modelo de dados** é um conjunto de conceitos para descrever a estrutura, as restrições e as operações de um banco de dados. Os três primeiros modelos do concurso são, em ordem histórica, o hierárquico, o de rede e o relacional.

#### Modelo hierárquico

Surgiu nos anos 1960, e o exemplo clássico é o **IMS** da IBM. Os dados formam uma **árvore**. Existe um segmento raiz, e cada segmento filho tem exatamente **um pai**. Só existem relacionamentos 1:N, no sentido pai→filho. Pense em um organograma, ou no sistema de pastas de um computador. Um departamento contém funcionários, e cada funcionário contém dependentes.

A consulta é feita por **navegação**: parte-se da raiz e desce-se pelo caminho até o segmento desejado. O problema aparece quando o mundo real tem relacionamentos N:M, como alunos e disciplinas. A estrutura força a **duplicar dados** em várias árvores, com redundância e risco de inconsistência. Há também anomalias de inserção (não dá para cadastrar um filho sem pai) e de remoção (apagar o pai apaga os filhos). O acesso depende do caminho e é rápido para o que foi previsto, mas ruim para consultas não planejadas.

#### Modelo de rede

Foi padronizado pelo grupo **CODASYL/DBTG** no início dos anos 1970. Os dados formam um **grafo**. Os registros se ligam por **conjuntos (sets)**, em que cada conjunto tem um registro **proprietário (owner)** e vários **membros (members)**. A diferença decisiva para o hierárquico é que um registro pode participar de vários conjuntos e ter **mais de um proprietário**. Isso permite representar N:M, normalmente com um registro de ligação (por exemplo, “matrícula” ligando aluno e disciplina).

Exemplos históricos são IDS, IDMS e TOTAL. A manipulação é **navegacional**: o programador percorre ponteiros de registro em registro, mantendo um “registro corrente”. É mais flexível que o hierárquico e eficiente em acessos previstos. Em contrapartida, é complexo de projetar e programar, e qualquer mudança estrutural obriga a reescrever programas, porque há baixa independência de dados.

#### Modelo relacional

Foi proposto por **E. F. Codd em 1970** e é o dominante até hoje. Os dados ficam em **relações (tabelas)**, formadas por **tuplas (linhas)** e **atributos (colunas)**, cada atributo com um **domínio** de valores permitidos. Não há ponteiros visíveis. O relacionamento entre tabelas é feito por **valores**: a chave estrangeira de uma tabela referencia a chave primária de outra.

As restrições de integridade são três:

- **Integridade de entidade:** a chave primária não pode ser nula.
- **Integridade referencial:** toda chave estrangeira deve corresponder a uma chave primária existente (ou ser nula, se permitido).
- **Integridade de domínio:** os valores devem pertencer ao domínio do atributo.

A manipulação é **declarativa**, ou seja, diz-se _o que_ se quer, não _como_ obter. A base teórica é a **álgebra relacional** (seleção, projeção, junção, união, diferença, produto cartesiano), e a linguagem prática é o **SQL**. Um exemplo:

sql

```sql
CREATE TABLE Aluno (
  matricula INT PRIMARY KEY,
  nome CHAR(40),
  curso_id INT,
  FOREIGN KEY (curso_id) REFERENCES Curso(id)
);
SELECT nome FROM Aluno WHERE curso_id = 3;
```

Completam o modelo a **normalização** (1FN, 2FN, 3FN, Boyce-Codd, para eliminar redundâncias e anomalias), as propriedades **ACID** das transações (atomicidade, consistência, isolamento, durabilidade) e a alta independência de dados. A desvantagem histórica era o desempenho, que foi superado com otimizadores de consulta e índices.

#### Comparação entre os três modelos

|Aspecto|Hierárquico|Rede|Relacional|
|---|---|---|---|
|Estrutura|Árvore|Grafo|Tabelas (relações)|
|Relacionamentos|1:N (um pai)|1:N e N:M (vários donos)|Qualquer um, via chaves|
|Ligação entre dados|Ponteiros pai-filho|Ponteiros em sets|Valores (chave estrangeira)|
|Acesso|Navegacional|Navegacional|Declarativo (SQL)|
|Independência de dados|Baixa|Baixa/média|Alta|
|Redundância|Alta em N:M|Menor|Controlada por normalização|
|Exemplos|IMS|IDS, IDMS|Oracle, PostgreSQL, MySQL, SQL Server|
|Base teórica|Empírica|Empírica (CODASYL)|Matemática (álgebra e cálculo relacional)|

Resumindo: **hierárquico e rede são navegacionais e dependem de ponteiros físicos**. O relacional é declarativo e baseado em teoria matemática. O de rede é uma generalização do hierárquico, porque aceita mais de um pai.

#### Modelo orientado a objetos

O banco de dados orientado a objetos (**SGBDOO**) nasceu nos anos 1980 e 1990, com o _Manifesto dos SGBDs Orientados a Objetos_ (Atkinson e outros, 1989). A ideia é guardar **objetos** diretamente, com os mesmos conceitos da programação orientada a objetos: classes, **identidade de objeto (OID)**, encapsulamento, herança, polimorfismo e tipos complexos (objetos dentro de objetos, coleções). Os **métodos** também podem ficar armazenados no banco. O padrão de referência é o **ODMG**, com a linguagem de consulta **OQL**.

A grande vantagem é eliminar o **descasamento de impedância** (_impedance mismatch_), a dificuldade de traduzir objetos da aplicação em linhas de tabelas. Isso é útil em CAD/CAM, multimídia e sistemas de engenharia, com dados complexos e aninhados. As desvantagens são a ausência de uma teoria formal tão sólida quanto a do relacional, a falta de padronização plena e a menor adoção no mercado. Exemplos: db4o, ObjectDB, GemStone, Versant. Existe ainda o modelo **objeto-relacional**, em que SGBDs relacionais (PostgreSQL, Oracle) ganham tipos definidos pelo usuário, herança de tabelas e métodos. É a solução híbrida mais comum.

### 4. Organização de arquivos

#### Arquivos sequenciais

No **arquivo sequencial** os registros são armazenados um após o outro e lidos nessa ordem. Ele pode ser **serial** (ordem de chegada, o heap) ou **sequencial ordenado** (ordenado por uma chave). A busca em arquivo desordenado é linear, com custo O(n) blocos. Em arquivo ordenado, pode-se usar **busca binária** sobre os blocos, com custo O(log n), desde que o meio permita acesso direto a qualquer bloco (o que não vale para fita magnética).

A inserção em arquivo ordenado é cara, porque é preciso abrir espaço e deslocar registros. Na prática, usa-se um **arquivo mestre** ordenado e um **arquivo de transações** (movimento), também ordenado. Periodicamente os dois são **intercalados (merge)** para gerar um novo mestre. É o clássico processamento em lote (_batch_), típico de folha de pagamento e faturamento em fita.

As vantagens são a simplicidade, o ótimo aproveitamento de espaço e a excelente eficiência quando se processa **todo** o arquivo. As desvantagens são a lentidão para consultas pontuais e para atualização individual.

#### Arquivos sequenciais indexados

O **sequencial indexado** une o melhor dos dois mundos: o arquivo de dados fica **ordenado pela chave** e existe um **índice** que aponta para os blocos, permitindo tanto acesso sequencial quanto direto. O exemplo histórico é o **ISAM** (_Indexed Sequential Access Method_) da IBM, e seu sucessor, o **VSAM**.

Os índices podem ser **densos**, com uma entrada para cada registro, ou **esparsos**, com uma entrada para cada bloco, apontando para a menor chave do bloco. Só pode haver índice esparso sobre campo pelo qual o arquivo está ordenado. Quando o índice fica grande, cria-se um **índice do índice**, o **multinível**. O arquivo se organiza em uma **área primária** (dados), uma **área de índices** e uma **área de overflow**, que recebe os registros que não couberam no lugar certo nas inserções. Com o tempo, as cadeias de overflow degradam o desempenho, e é preciso **reorganizar** o arquivo periodicamente. Essa degradação é exatamente o ponto fraco que a árvore B resolve.

Os índices também se classificam em **primário** (sobre a chave de ordenação, única), de **agrupamento** (_clustering_, sobre campo de ordenação não único) e **secundário** (sobre campo qualquer, sempre denso).

#### Arquivos relativos

No **arquivo relativo**, cada registro é localizado pelo seu **número relativo** (RRN, _relative record number_) dentro do arquivo. Com registros de tamanho fixo, o endereço é calculado diretamente: posição = (RRN − 1) × tamanho do registro. Não há busca, só um deslocamento. É o `RELATIVE` do COBOL.

A limitação é que o programa precisa **conhecer o número do registro**. Funciona muito bem quando a chave do negócio já é um inteiro denso, como o código de produto de 1 a 5000, e mal quando a chave é “CPF” ou “nome”.

#### Arquivos diretos (hashing)

No **arquivo direto**, também chamado de **arquivo hash** ou de **espalhamento**, uma **função de hash** transforma a chave em um endereço: `endereço = h(chave)`. Um exemplo simples é h(k) = k mod m, com m preferencialmente primo. A busca é feita em **tempo próximo de O(1)**, isto é, um ou poucos acessos a disco, independentemente do tamanho do arquivo.

O problema central são as **colisões**, quando duas chaves distintas geram o mesmo endereço. As soluções principais são:

- **Encadeamento (chaining):** os colididos formam uma lista ligada a partir do endereço, em área de overflow.
- **Endereçamento aberto:** procura-se outra posição livre, por sondagem linear, quadrática ou _hash_ duplo.
- **Reespalhamento (rehashing).**

Uma boa função de hash distribui uniformemente. O **fator de carga** (registros ÷ capacidade) controla o desempenho: perto de 100% as colisões explodem. O hash **estático** tem tamanho fixo. Os **dinâmicos** (hash extensível e hash linear) crescem sem reorganizar tudo, usando um diretório.

Há dois pontos fracos. Hash não atende bem **consultas por intervalo** (do código 100 ao 200), porque destrói a ordem das chaves. E o arquivo precisa de espaço sobrando para funcionar bem. Em compensação, é imbatível em busca exata por chave.

Uma observação sobre terminologia, que cai em prova: na literatura de concurso, **“arquivo relativo”** e **“arquivo direto”** às vezes são tratados quase como sinônimos, porque ambos dão acesso aleatório por posição. A distinção rigorosa é que no relativo a chave **é** a posição, e no direto a posição é **calculada** por hash a partir da chave. Leia sempre o enunciado para ver qual definição ele adota.

#### Arquivos invertidos

No **arquivo invertido**, em vez de partir do registro e descobrir seus valores, parte-se do **valor** e descobre-se **quais registros o contêm**. Para cada valor de um atributo (ou para cada palavra de um texto), o índice guarda uma **lista de ponteiros** (postings) para os registros que o possuem. A figura mental é o **índice remissivo no fim de um livro**: em vez de ler o livro procurando “hash”, você vai ao índice e vê as páginas onde a palavra aparece.

É a estrutura por trás dos **motores de busca textual** (Lucene, Elasticsearch, Solr) e é muito útil em **consultas por múltiplos atributos**. Para “cor = vermelho E ano = 2020”, basta **interseccionar** as duas listas. Pode ser **total** (todos os atributos) ou **parcial** (só alguns). O ponto fraco é a manutenção: cada inserção ou alteração obriga a atualizar várias listas.

#### Árvore B

A **árvore B** foi proposta por **Bayer e McCreight em 1972** e é a estrutura de índice mais importante em bancos de dados. É uma **árvore de busca balanceada e multicaminho**, pensada para disco. Cada **nó** corresponde a **um bloco**, e um nó guarda **várias chaves**, não apenas uma, o que torna a árvore larga e muito baixa. Quanto mais baixa a árvore, menos acessos a disco.

As propriedades de uma árvore B de **ordem m** (definição com m igual ao máximo de filhos) são:

- Cada nó tem **no máximo m filhos** e, portanto, no máximo **m − 1 chaves**.
- Cada nó interno, exceto a raiz, tem **no mínimo ⌈m/2⌉ filhos**.
- A raiz tem pelo menos 2 filhos, se não for folha.
- **Todas as folhas estão no mesmo nível**, o que garante o balanceamento perfeito.
- As chaves em cada nó ficam ordenadas, e os filhos separam os intervalos entre elas.

A busca percorre da raiz até a folha em **O(log n) acessos a disco**, com a base do logaritmo igual ao grau do nó. Com nós de centenas de chaves, uma árvore de três ou quatro níveis indexa milhões de registros.

A **inserção** sempre ocorre em uma folha. Se o nó **estoura** (excede o máximo), ele é **dividido (split)**: a chave do meio **sobe** para o pai e as demais ficam em dois nós. Esse processo pode se propagar até a raiz, e é assim que a árvore **cresce pela raiz**, sempre mantendo o balanceamento. Um exemplo com ordem 3 (máximo de 2 chaves por nó), inserindo 10, 20, 30, 40, 50:

1. Inserem-se 10 e 20: nó `[10, 20]`.
2. Insere-se 30: o nó estoura (`[10, 20, 30]`). A chave do meio, 20, sobe e vira a raiz, com filhos `[10]` e `[30]`.
3. Inserem-se 40: `[30, 40]`.
4. Insere-se 50: o nó estoura (`[30, 40, 50]`). A chave 40 sobe, e a raiz fica `[20, 40]` com filhos `[10]`, `[30]` e `[50]`.

A **remoção** é mais delicada. Se o nó fica com menos que o mínimo, ele tenta **redistribuir** (emprestar chave de um irmão, passando pelo pai). Se não der, os nós **se fundem (concatenação)**, e a fusão pode se propagar para cima.

Há variantes que caem em prova:

- **Árvore B+:** todas as chaves (com os dados ou ponteiros) ficam nas **folhas**, e os nós internos guardam só chaves de roteamento. As folhas são **encadeadas entre si**, o que torna as **consultas por intervalo** e a varredura sequencial muito eficientes. É a mais usada em SGBDs.
- **Árvore B*:** exige que os nós fiquem ocupados no mínimo 2/3 (contra 1/2), usando redistribuição entre irmãos antes de dividir.

Árvores B e B+ são usadas em índices de PostgreSQL, MySQL/InnoDB, Oracle e SQL Server, e em sistemas de arquivos como NTFS, HFS+, Btrfs e ext4 (na forma de HTree para diretórios).

Um cuidado importante com a **ordem**. Knuth define ordem como o máximo de filhos. Cormen usa **grau mínimo t**, com no mínimo t − 1 chaves e no máximo 2t − 1 por nó. As duas definições aparecem em livros e provas. Se o enunciado não disser, olhe os dados da questão.

#### Compressão de dados

**Compressão** é a representação da mesma informação com **menos bits**, eliminando redundância. Ela reduz espaço em disco, tempo de transferência e uso de banda, ao custo de CPU para comprimir e descomprimir. A **razão de compressão** é o tamanho original dividido pelo comprimido. A teoria por trás é a **entropia** de Shannon: o limite teórico de compressão sem perdas depende da previsibilidade dos dados.

**Compressão sem perdas (lossless)** recupera exatamente os dados originais. É obrigatória para texto, código, bancos de dados e planilhas. As técnicas principais são:

- **RLE (_Run-Length Encoding_):** troca sequências repetidas por um par (símbolo, repetições). “AAAABBBCC” vira “4A3B2C”. Funciona bem em imagens com grandes áreas uniformes e mal em texto comum, em que pode até aumentar o tamanho.
- **Huffman:** é um código de **comprimento variável e livre de prefixo**. Símbolos mais frequentes recebem códigos mais curtos. Constrói-se uma árvore juntando sempre os dois nós de menor frequência. Em “AAAABBC” (A=4, B=2, C=1), um código possível é A=0, B=10, C=11, que usa 4×1 + 2×2 + 1×2 = **10 bits**, contra 56 bits em ASCII de 8 bits. Nenhum código é prefixo de outro, e por isso a decodificação não é ambígua.
- **Família Lempel-Ziv (LZ77, LZ78, LZW):** substitui sequências repetidas por referências a ocorrências anteriores (janela deslizante no LZ77, dicionário construído dinamicamente no LZ78/LZW). O LZW é usado no GIF.
- **Deflate:** combina LZ77 com Huffman. É a base do ZIP, do gzip e do PNG.
- **Codificação aritmética** e a **transformada de Burrows-Wheeler (BWT)**, usada no bzip2.

**Compressão com perdas (lossy)** descarta informação que a percepção humana dificilmente nota. É usada em imagem (**JPEG**, com DCT e quantização), áudio (**MP3**, **AAC**) e vídeo (**MPEG**, **H.264**). Não serve para dados que exigem exatidão.

Em bancos de dados, a compressão aparece em páginas de índice (compressão de prefixo), em colunas com poucos valores distintos (codificação por dicionário, RLE) e em armazenamento colunar.

### 5. Macetes e truques para a prova

- **Ordem do menor para o maior:** campo → registro → arquivo → banco de dados. A “menor unidade de informação com significado” em um arquivo de banco é o **campo**, não o registro.
- **Hierárquico = 1 pai, árvore. Rede = vários pais, grafo.** Se a questão fala em “conjuntos owner-member” ou CODASYL, é rede. Se fala em “segmentos” e IMS, é hierárquico. Se fala em Codd, tabelas e chaves estrangeiras, é relacional.
- **Navegacional (hierárquico e rede) × declarativo (relacional).** Alternativa que diz que o relacional “usa ponteiros físicos entre registros” está errada.
- **“Descasamento de impedância”** é o gancho do banco orientado a objetos.
- **Índice esparso só funciona em arquivo ordenado pela chave do índice.** Índice secundário é sempre denso.
- **Hash é ótimo para busca exata e péssimo para intervalo.** Árvore B+ é ótima para os dois, e por isso domina os SGBDs.
- **Árvore B tem todas as folhas no mesmo nível.** Qualquer alternativa que fale em árvore B desbalanceada é falsa. Ela cresce pela raiz, não pelas folhas.
- **Na árvore B, “dividir” é inserção e “concatenar/redistribuir” é remoção.**
- **B+: dados nas folhas, folhas encadeadas.** B*: ocupação mínima de 2/3.
- **Arquivo invertido = índice remissivo de livro.** Aparece em buscas por múltiplos atributos e em motores de busca.
- **Huffman: código de comprimento variável, sem prefixo, símbolo frequente = código curto.** RLE pode **piorar** dados sem repetição. JPEG e MP3 são **com perdas**. ZIP e PNG são **sem perdas**.
- **Sequencial:** processa o arquivo todo muito bem. **Relativo/direto:** acesso aleatório rápido. **Indexado:** os dois, ao custo de manter o índice.
- **Cuidado com a palavra “sempre”, “nunca”, “somente”.** Em questões VUNESP de múltipla escolha, as alternativas absolutas costumam ser a pegadinha.

### 6. Análise do que mais cai (2020 a 2026)

As provas de 2025 e 2026 para Oficial do Quadro Complementar (Informática), aplicadas pela **VUNESP**, têm 50 questões objetivas e 4 horas de duração. As questões de conhecimentos específicos vão da 21 à 50. Abri as duas provas completas. Nelas, **não há questão específica de organização de arquivos** (sequencial, indexado, hash, árvore B ou compressão), nem sobre os modelos hierárquico e de rede.

O que apareceu na área de banco de dados:

- **2025:** diagrama entidade-relacionamento (símbolos gráficos), comando SQL `CREATE TABLE`, propriedades ACID e modelo multidimensional de data warehouse (tabela fato e tabelas dimensão).
- **2026:** modelo entidade-relacionamento (tipos de atributos), `CREATE TABLE` em SQL e protocolos de controle de concorrência (bloqueio e timestamp).
- **Nas duas provas:** uma questão de JDBC em Java.

Na vizinhança do tema, ainda apareceram **RAID** (nas duas provas) e **SSD e TRIM** (2026), que são armazenamento. Isso confirma que o edital cobra a “organização de arquivos” mais como conceito de base do que como questão direta.

Do que li, a leitura mais segura para o seu estudo é esta. O tópico, isolado, tem **baixa incidência direta**. Quando banco de dados cai, cai pelo lado **relacional e transacional**: SQL, modelagem ER, ACID, concorrência e data warehouse. Isso é uma inferência de duas provas, não uma estatística de sete anos. Para 2020 a 2024, o site da ESFCEx publica provas anteriores (2021, 2022 e 2023 aparecem no portal), e vale conferir se alguma delas traz questão direta.

A minha sugestão de prioridade dentro deste tópico é um **julgamento meu, não um dado**. Primeiro o **modelo relacional**, que é a ponte para SQL, ER, ACID e normalização. Depois **índices, árvore B/B+ e hashing**, que são os conceitos que mais “reaparecem” em outras questões de banco e sistema de arquivos. Em terceiro, a **comparação hierárquico × rede × relacional**, que é teoria clássica e fácil de acertar. Por último, **modelo orientado a objetos, arquivo invertido e compressão de dados**, que merecem uma leitura firme dos conceitos, mas não horas de resolução de exercícios.