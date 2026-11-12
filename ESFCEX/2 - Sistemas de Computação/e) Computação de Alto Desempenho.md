
# PARTE E: COMPUTAÇÃO DE ALTO DESEMPENHO (HPC)

### E.1 Conceitos

A Computação de Alto Desempenho (HPC) emprega processamento paralelo e agregação de recursos para resolver problemas grandes demais (ou lentos demais) para um único processador comum: simulação climática, dinâmica molecular, CFD, genômica, aprendizado de máquina, criptografia. O desempenho é medido em FLOPS (operações de ponto flutuante por segundo: giga, tera, peta e exa), e os maiores sistemas do mundo são listados no TOP500, ranqueado pelo benchmark LINPACK (HPL). A era dos supercomputadores já entrou na escala de exaflops.

O motivo de a computação paralela dominar é o fim da escalada de frequência (a barreira de potência e de dissipação, o “power wall”), que levou os fabricantes a aumentar o número de núcleos, e não a frequência.

#### Métricas

O speedup com p processadores é S(p) = T₁ / Tₚ, e a eficiência é E(p) = S(p)/p. O ideal é um speedup linear (S = p), mas na prática é limitado.

A Lei de Amdahl diz que, se uma fração f do programa é paralelizável e (1 − f) é serial, S(p) = 1 / ((1 − f) + f/p), e o limite quando p → ∞ é 1/(1 − f). Exemplo: com f = 0,9 e p = 10, S = 1/(0,1 + 0,09) ≈ 5,26, e o máximo possível, com infinitos processadores, é 10. A mensagem: a parte serial limita o ganho.

A Lei de Gustafson é uma visão complementar: ao ganhar mais processadores, o usuário costuma aumentar o tamanho do problema, mantendo constante o tempo de execução, e o speedup escalado é S(p) = p − α·(p − 1), em que α é a fração serial do tempo paralelo. Com α = 0,1 e p = 10, S = 10 − 0,9 = 9,1. É uma visão mais otimista, e as duas leis não são contraditórias, apenas partem de premissas distintas.

A escalabilidade forte (strong scaling) mantém o problema fixo e aumenta o número de processadores (regida por Amdahl). A escalabilidade fraca (weak scaling) aumenta o problema proporcionalmente aos processadores (Gustafson).

Outros fatores que prejudicam o ganho: o overhead de comunicação e sincronização, o desbalanceamento de carga, a granularidade (tarefas muito finas geram muita comunicação, tarefas muito grossas geram desbalanceamento), a contenção por memória e o false sharing. Os parâmetros de rede são a latência (tempo para o primeiro byte) e a largura de banda (vazão sustentada), e a regra de otimização é minimizar a comunicação e sobrepô-la ao cálculo.

As abordagens de decomposição são o paralelismo de dados (a mesma operação sobre partes distintas dos dados) e o paralelismo de tarefas (tarefas diferentes simultâneas). Os padrões comuns são mestre-escravo (master-worker), divisão e conquista, pipeline, MapReduce e SPMD (Single Program, Multiple Data, o mesmo programa em todos os processos, que se diferenciam pelo identificador).

### E.2 Arquiteturas paralelas

#### Taxonomia de Flynn (recapitulação)

SISD (monoprocessador sequencial), SIMD (uma instrução sobre múltiplos dados: unidades vetoriais SSE/AVX, GPUs), MISD (raro) e MIMD (múltiplos fluxos de instruções e dados: multiprocessadores e clusters). Existe ainda uma subclassificação do SIMD/MIMD, o SPMD.

#### Organização da memória

Memória compartilhada. Todos os processadores veem um espaço de endereçamento único. No modelo UMA/SMP (Symmetric Multiprocessing), o acesso à memória tem tempo uniforme (um barramento ou crossbar), mas a escalabilidade é limitada pela contenção. No NUMA (Non-Uniform Memory Access), cada processador tem memória local rápida e acesso mais lento à memória dos outros, o que escala melhor, e o ccNUMA mantém coerência de cache por hardware. A programação é mais simples (threads e OpenMP), mas é preciso cuidar da sincronização e da localidade de dados.

Memória distribuída. Cada nó tem sua memória privada, e a comunicação é feita por troca explícita de mensagens pela rede (MPI). Escala para milhares de nós, mas é mais trabalhosa de programar.

Híbrida. É o modelo dos supercomputadores atuais: nós de memória compartilhada (multicore, com GPUs) interligados por rede, usando MPI entre nós e OpenMP/CUDA dentro do nó. Existe ainda a memória compartilhada distribuída (DSM), que emula um espaço único sobre memória distribuída.

#### Multicore e manycore

Um processador multicore integra poucos núcleos (de 2 a algumas dezenas), cada um grande e poderoso, com execução fora de ordem, caches próprias e grande foco em baixa latência de uma thread. Compartilham tipicamente a cache L3 e o controlador de memória, com coerência por hardware (MESI). A tecnologia SMT (simultaneous multithreading, Hyper-Threading) faz um núcleo físico parecer dois lógicos.

Um processador manycore tem dezenas a milhares de núcleos simples, de menor frequência e complexidade, otimizados para vazão (throughput) e eficiência energética (exemplos: Intel Xeon Phi, hoje descontinuado, e as GPUs, que são o caso mais bem-sucedido). A distinção é de ênfase: poucos núcleos fortes (multicore) contra muitos núcleos fracos (manycore).

#### Clusters e grids

Um cluster é um conjunto de computadores independentes (nós), interligados por uma rede de alto desempenho, que funcionam como um recurso computacional único. Seus componentes são os nós de computação, os nós de acesso (login) e de gerência, o sistema de armazenamento compartilhado (sistemas de arquivos paralelos como Lustre e GPFS), a rede de interconexão (Ethernet, e redes de baixa latência como InfiniBand e Omni-Path) e o gerenciador de recursos/escalonador de jobs (SLURM, PBS, SGE). O Beowulf é o modelo clássico de cluster com hardware comum (commodity) e Linux. Em relação a um supercomputador monolítico, o cluster oferece melhor custo-benefício e escalabilidade, ao custo de comunicação mais lenta. Os clusters de alta disponibilidade (HA) focam em tolerância a falhas, e os de balanceamento de carga, em distribuir requisições. O grid é uma federação geograficamente distribuída de recursos heterogêneos de múltiplos domínios administrativos, e a nuvem é a evolução comercial desses conceitos.

#### Topologias de interconexão

As redes de interconexão são caracterizadas pelo grau (número de links por nó), diâmetro (maior distância mínima entre dois nós), largura de bisseção (capacidade de comunicação ao dividir a rede ao meio) e custo. As principais topologias são o barramento (simples, contenção), crossbar (conexão total, caro), anel, malha 2D/3D, toro (malha com bordas conectadas), hipercubo (2ⁿ nós, grau n, diâmetro n) e árvore gorda (fat tree), muito usada em clusters, pois mantém a largura de banda nos níveis superiores.

#### GPU

A GPU (Graphics Processing Unit) nasceu para renderização gráfica e se tornou uma plataforma de computação de propósito geral (GPGPU). Enquanto a CPU é otimizada para baixa latência (poucos núcleos fortes, grandes caches, controle sofisticado), a GPU é otimizada para alta vazão: milhares de núcleos simples que executam o mesmo código sobre muitos dados, escondendo a latência de memória com troca rápida entre muitas threads.

O modelo de execução é o SIMT (Single Instruction, Multiple Threads). Na GPU NVIDIA, os núcleos se agrupam em multiprocessadores de streaming (SMs). As threads são organizadas em blocos (thread blocks), e os blocos formam uma grade (grid). Dentro de um SM, as threads são executadas em grupos de 32, chamados warps, que executam a mesma instrução simultaneamente. Se as threads de um warp tomam caminhos diferentes num desvio (if), ocorre a divergência de warp, e os caminhos são serializados, degradando o desempenho. A hierarquia de memória tem registradores (por thread), memória compartilhada (shared memory, rápida, por bloco, gerenciada pelo programador), caches L1/L2, memória global (grande, de alta latência, acessível por todos) e memórias especiais de constantes e de texturas. O desempenho depende de coalescência (threads vizinhas acessando endereços contíguos, combinados numa única transação), do uso da memória compartilhada e da ocupação (occupancy) do SM. Os tensor cores aceleram multiplicação de matrizes para aprendizado profundo. Outras plataformas aceleradoras são as TPUs (Google), os FPGAs e os ASICs. As GPUs funcionam como aceleradores ligados à CPU (o host) pelo barramento PCIe (ou NVLink), e a transferência de dados host↔device é frequentemente o gargalo.

Macete: GPU é ótima para problemas com muito paralelismo de dados, alta intensidade aritmética e pouco desvio; é ruim para lógica sequencial, com muitos desvios ou com acesso irregular à memória.

### E.3 Ambientes e linguagens para arquiteturas de alto desempenho

A escolha da ferramenta segue a arquitetura: MPI para memória distribuída, OpenMP para memória compartilhada, CUDA/OpenCL para GPUs e aceleradores, e OpenGL para gráficos. Em aplicações reais, combinam-se (MPI + OpenMP + CUDA).

#### MPI (Message Passing Interface)

É um padrão de biblioteca (não uma linguagem) para troca de mensagens em memória distribuída, com bindings para C, C++ e Fortran. As implementações mais conhecidas são Open MPI e MPICH. O modelo é SPMD: lança-se o mesmo programa em N processos, cada um com um rank (identificador de 0 a N−1) dentro de um comunicador (o padrão é MPI_COMM_WORLD). Cada processo tem seu próprio espaço de memória, e os dados só se movem por mensagens explícitas.

Funções-chave: MPI_Init e MPI_Finalize (início e fim), MPI_Comm_rank e MPI_Comm_size (identidade e tamanho do grupo). A comunicação ponto a ponto usa MPI_Send e MPI_Recv (bloqueantes) e MPI_Isend/MPI_Irecv (não bloqueantes, permitem sobrepor comunicação e computação, e exigem MPI_Wait ou MPI_Test). As operações coletivas envolvem todos os processos do comunicador: MPI_Bcast (um envia a todos), MPI_Scatter (distribui partes de um vetor), MPI_Gather (junta partes no raiz), MPI_Reduce (combina valores com uma operação, como SUM ou MAX, no raiz), MPI_Allreduce (o resultado vai a todos), MPI_Alltoall e MPI_Barrier (sincronização). Um cuidado clássico: dois processos que fazem MPI_Send bloqueante um para o outro antes do MPI_Recv podem entrar em deadlock (dependendo do tamanho da mensagem e do buffer do sistema), e a correção é ordenar as chamadas ou usar MPI_Sendrecv.

Exemplo de soma paralela com redução:

#include <mpi.h>

#include <stdio.h>

int main(int argc, char **argv) {

    MPI_Init(&argc, &argv);

    int rank, size;

    MPI_Comm_rank(MPI_COMM_WORLD, &rank);

    MPI_Comm_size(MPI_COMM_WORLD, &size);

    double local = rank + 1, total = 0;

    MPI_Reduce(&local, &total, 1, MPI_DOUBLE, MPI_SUM, 0, MPI_COMM_WORLD);

    if (rank == 0) printf("Soma = %f\n", total);

    MPI_Finalize();

    return 0;

}

Compila-se com mpicc e executa-se com mpirun -np 4 ./programa.

#### OpenMP

É uma API baseada em diretivas de compilador (#pragma omp ...), biblioteca e variáveis de ambiente, para memória compartilhada em C, C++ e Fortran. O modelo é fork-join: o programa começa com uma thread mestre, que cria um grupo de threads ao entrar numa região paralela, e as threads se juntam ao final. Sua grande virtude é a paralelização incremental de código sequencial existente.

Diretivas e cláusulas principais: #pragma omp parallel (cria a região paralela), #pragma omp for ou parallel for (divide as iterações do laço entre as threads), sections (blocos distintos para threads distintas), single, master, barrier, e a sincronização com critical (exclusão mútua para um bloco), atomic (atualização atômica de uma variável) e reduction (combinação segura de resultados parciais). As cláusulas de escopo de dados são shared (compartilhada) e private (cópia por thread), e schedule(static|dynamic|guided) controla a distribuição das iterações (dynamic ajuda quando a carga das iterações é desigual). Funções úteis: omp_get_thread_num() e omp_get_num_threads(). As condições de corrida são o principal erro típico (por exemplo, acumular numa variável compartilhada sem reduction).

  

c

double soma = 0.0;

#pragma omp parallel for reduction(+:soma)

for (int i = 0; i < N; i++)

    soma += a[i];

  

Compila-se com a opção -fopenmp (GCC). Versões recentes do padrão incluem task (paralelismo de tarefas) e offloading para aceleradores (target).

#### CUDA

É a plataforma e o modelo de programação proprietários da NVIDIA para GPGPU, uma extensão de C/C++. O programa tem uma parte no host (CPU) e funções chamadas kernels executadas no device (GPU), declaradas com __global__ e lançadas com a sintaxe kernel<<<blocos, threads_por_bloco>>>(args). Cada thread descobre seu índice por variáveis embutidas: threadIdx, blockIdx, blockDim e gridDim. O fluxo típico é: alocar memória na GPU (cudaMalloc), copiar dados do host (cudaMemcpy HostToDevice), lançar o kernel, copiar o resultado de volta (DeviceToHost) e liberar (cudaFree). A memória compartilhada é declarada com __shared__ e a sincronização entre as threads de um bloco usa __syncthreads(). Não há sincronização global dentro de um kernel entre blocos diferentes.

c

__global__ void soma(float *a, float *b, float *c, int n) {

    int i = blockIdx.x * blockDim.x + threadIdx.x;

    if (i < n) c[i] = a[i] + b[i];

}

// lançamento: soma<<<(n + 255) / 256, 256>>>(d_a, d_b, d_c, n);

  

O cálculo (n + 255) / 256 é o arredondamento para cima do número de blocos. O ecossistema inclui as bibliotecas cuBLAS, cuFFT, cuDNN e Thrust.

#### OpenCL

É um padrão aberto do consórcio Khronos para programação heterogênea e portável (CPUs, GPUs de qualquer fabricante, FPGAs, DSPs). O modelo tem uma plataforma (host e dispositivos de computação), um contexto, filas de comando (command queues) para enviar trabalho aos dispositivos, programas e kernels escritos em OpenCL C (um dialeto do C), em geral compilados em tempo de execução para o dispositivo disponível. A execução é organizada em um NDRange (espaço de índices de 1 a 3 dimensões), com work-items (a menor unidade de execução, equivalente a uma thread) agrupados em work-groups (equivalentes aos blocos). O kernel obtém os índices com get_global_id() e get_local_id(). A hierarquia de memória é a global, a constante, a local (compartilhada no work-group) e a privada (por work-item).

Tabela de correspondência CUDA ↔ OpenCL (muito útil): thread = work-item; bloco = work-group; grid = NDRange; memória compartilhada (__shared__) = memória local (__local); SM = compute unit; threadIdx + blockIdx*blockDim = get_global_id. Enquanto o CUDA é mais maduro e produtivo em GPUs NVIDIA (com mais bibliotecas), o OpenCL é portável entre fabricantes, ao custo de ser mais verboso. Há ainda o OpenACC (diretivas para aceleradores, na linha do OpenMP) e o SYCL/HIP como alternativas.

#### OpenGL

O OpenGL (Open Graphics Library, Khronos) é uma API gráfica multiplataforma para renderização 2D e 3D acelerada por GPU. Não é uma linguagem de computação paralela geral, e este é um ponto de atenção, pois a prova pode misturá-lo com CUDA e OpenCL. Seu núcleo é o pipeline gráfico: especificação de vértices, vertex shader (transforma os vértices), montagem de primitivas, rasterização, fragment (pixel) shader (calcula a cor de cada fragmento) e testes de profundidade e mistura (blending). Os shaders são escritos em GLSL (OpenGL Shading Language). As versões modernas permitem compute shaders, usados para computação geral dentro do contexto gráfico. Seus pares são o Vulkan (sucessor de baixo nível, mais eficiente, também do Khronos) e o Direct3D/DirectX (Microsoft), e o WebGL é a versão para navegadores.

Macete para a prova: MPI = mensagens, memória distribuída, clusters; OpenMP = diretivas, memória compartilhada, multicore; CUDA = GPUs NVIDIA; OpenCL = aberto, heterogêneo, portável; OpenGL = gráficos 3D, não computação geral. Para “qual a melhor ferramenta em um cluster de vários nós com várias GPUs”, a resposta é a combinação MPI + CUDA (ou MPI + OpenMP + CUDA).

## PARTE F: COMPUTAÇÃO EM NUVEM

### F.1 Conceitos

A definição de referência é a do NIST (SP 800-145): computação em nuvem é um modelo que permite acesso por rede, ubíquo, conveniente e sob demanda, a um conjunto compartilhado de recursos computacionais configuráveis (redes, servidores, armazenamento, aplicações e serviços), que podem ser provisionados e liberados rapidamente com mínimo esforço de gerência ou interação com o provedor.

#### Cinco características essenciais (NIST)

Autoatendimento sob demanda (on-demand self-service): o usuário provisiona recursos sozinho, sem intervenção humana do provedor.

Amplo acesso pela rede (broad network access): acesso via padrões de rede, por diversos dispositivos.

Agrupamento de recursos (resource pooling): os recursos do provedor atendem vários clientes em um modelo multilocatário (multi-tenant), com atribuição dinâmica, e o cliente em geral não controla a localização exata.

Elasticidade rápida (rapid elasticity): os recursos podem ser expandidos e contraídos rapidamente, muitas vezes de forma automática, dando a impressão de capacidade ilimitada.

Serviço mensurado (measured service): o uso é medido, controlado e reportado, com pagamento pelo que se consome (pay-as-you-go).

Escalabilidade × elasticidade: escalabilidade é a capacidade de lidar com mais carga adicionando recursos (vertical = máquina maior; horizontal = mais máquinas), e elasticidade é a capacidade de ajustar automaticamente e em ambas as direções a quantidade de recursos à demanda. O modelo econômico troca CapEx (investimento em capital) por OpEx (despesa operacional).

#### Modelos de implantação

A nuvem pública é operada por um provedor e aberta a múltiplos clientes (AWS, Microsoft Azure, Google Cloud). A privada é dedicada a uma única organização (interna ou hospedada), com mais controle e maior custo. A comunitária é compartilhada por organizações com interesses em comum (segurança, conformidade). A híbrida combina duas ou mais (por exemplo, dados sensíveis na privada e picos de carga na pública), e o cloud bursting é o transbordo de carga para a pública. O termo multicloud refere-se ao uso de vários provedores públicos, e o vendor lock-in (dependência do fornecedor) é um dos riscos que ele busca mitigar.

#### Modelos de serviço

Cada modelo define até onde vai a responsabilidade do provedor.

IaaS (Infrastructure as a Service). O provedor entrega a infraestrutura virtualizada: máquinas virtuais, armazenamento e rede. O cliente gerencia o SO, o middleware, o runtime, os dados e as aplicações. Exemplos: Amazon EC2 e S3, Azure Virtual Machines, Google Compute Engine. É o mais flexível e o mais trabalhoso.

PaaS (Platform as a Service). O provedor entrega uma plataforma de desenvolvimento e execução (runtime, middleware, banco de dados gerenciado, escalabilidade automática). O cliente gerencia apenas suas aplicações e dados. Exemplos: Google App Engine, Heroku, AWS Elastic Beanstalk, Azure App Service.

SaaS (Software as a Service). O provedor entrega a aplicação pronta, acessada por navegador ou cliente leve, e o cliente apenas a usa e configura. Exemplos: Gmail, Microsoft 365, Salesforce, Google Workspace.

Modelos derivados: FaaS/serverless (executa funções por evento, sem gerenciar servidores, com cobrança por execução: AWS Lambda, Azure Functions), CaaS (contêineres como serviço), DBaaS (banco de dados como serviço) e o genérico XaaS. Macete visual: quanto mais alto o modelo (IaaS → PaaS → SaaS), menor a responsabilidade e o controle do cliente e maior a do provedor.

#### Conceitos de arquitetura

Os recursos se organizam em regiões (áreas geográficas) e zonas de disponibilidade (datacenters isolados dentro da região), e o projeto resiliente distribui a aplicação em várias zonas. O SLA (Service Level Agreement) define a disponibilidade contratada (por exemplo, 99,95%). Outros elementos: balanceadores de carga, auto scaling, CDN (rede de distribuição de conteúdo próxima ao usuário), e os índices de recuperação RPO (quantos dados se aceita perder, medido em tempo) e RTO (quanto tempo se aceita ficar fora do ar). Em sistemas distribuídos em nuvem vale o teorema CAP: diante de uma partição de rede, é preciso escolher entre consistência e disponibilidade, o que explica a consistência eventual de muitos bancos NoSQL.

### F.2 Segurança em nuvem

O princípio mais cobrado é o modelo de responsabilidade compartilhada: o provedor é responsável pela segurança da nuvem (instalações físicas, hardware, rede de base, hipervisor), e o cliente pela segurança na nuvem (dados, identidades, configuração, aplicações). A fronteira se desloca conforme o modelo: em IaaS o cliente cuida de mais coisas (SO, patches, firewall de host), e em SaaS cuida basicamente de dados, acessos e configurações. Um erro comum e perigoso é supor que o provedor protege tudo.

#### Principais ameaças e riscos

Os incidentes mais frequentes são causados por configuração incorreta (como um bucket de armazenamento aberto ao público), seguidos de credenciais fracas ou vazadas (chaves de API em repositórios), interfaces e APIs inseguras, vazamento de dados, ameaças internas (insiders), sequestro de contas, ataques de negação de serviço (DoS/DDoS), riscos do ambiente multilocatário (isolamento falho, ataques de canal lateral entre VMs), perda de dados, dependência do fornecedor e falta de visibilidade e governança (shadow IT).

#### Controles e boas práticas

Gestão de identidade e acesso (IAM): aplicar o princípio do menor privilégio, o controle de acesso baseado em papéis (RBAC), a autenticação multifator (MFA), a federação de identidades (SAML, OAuth 2.0, OpenID Connect) e a rotação de credenciais. Criptografia: dados em repouso (por exemplo, AES-256) e em trânsito (TLS), com gerenciamento de chaves por serviços de KMS ou HSM, e a opção de o cliente controlar as chaves (BYOK/HYOK). Rede: segmentação com redes virtuais privadas (VPC), security groups e listas de controle, WAF, proteção anti-DDoS, VPN e conexões dedicadas. Monitoramento e resposta: registros de auditoria (logs de acesso e de API), SIEM, ferramentas de CSPM (verificação contínua da postura de configuração), detecção de anomalias. Continuidade: backups, replicação entre regiões, planos de recuperação de desastres (DR) com RPO/RTO definidos. Arquitetura Zero Trust: nenhuma entidade é confiável por padrão, e todo acesso é verificado continuamente. DevSecOps: segurança integrada ao ciclo de desenvolvimento (varredura de dependências, de imagens de contêiner e de infraestrutura como código).

#### Conformidade e aspectos legais

Normas e certificações relevantes: ISO/IEC 27001 (gestão de segurança da informação), 27017 (controles específicos de nuvem) e 27018 (proteção de dados pessoais em nuvem pública), SOC 2, PCI DSS (cartões), GDPR (União Europeia). No Brasil, a LGPD (Lei nº 13.709/2018) impõe obrigações sobre o tratamento de dados pessoais, o que traz à tona a questão da soberania e residência dos dados (em qual país eles ficam e que leis se aplicam). Na administração pública federal, a Instrução Normativa nº 5/2021 do GSI/PR trata dos requisitos mínimos de segurança da informação para o uso de computação em nuvem, e vale conferir seu texto atual no seu estudo, por ser tema recorrente em concursos públicos federais.

### F.3 Desenvolvimento de aplicações em nuvem

Aplicações nativas de nuvem (cloud-native) são projetadas para aproveitar elasticidade, automação e resiliência. Os princípios mais conhecidos são os da metodologia Twelve-Factor App, entre eles: configuração via variáveis de ambiente (nunca no código), processos sem estado (stateless), estado em serviços externos, logs como fluxo de eventos, paridade entre desenvolvimento e produção, e descartabilidade (inicialização rápida e encerramento gracioso). A aplicação stateless é a que permite escalar horizontalmente com um balanceador de carga.

#### Arquiteturas

O monolito é uma aplicação única, mais simples de começar, mas difícil de escalar por partes e de evoluir com equipes grandes. Microsserviços dividem a aplicação em serviços pequenos, independentes, com banco de dados e ciclo de implantação próprios, comunicando-se por APIs (REST, gRPC) ou mensagens. Os ganhos são a escalabilidade e a implantação independentes e a tolerância a falhas isoladas, e o custo é a complexidade operacional, a latência de rede, a consistência de dados distribuídos e a necessidade de observabilidade. A arquitetura orientada a eventos e a mensageria (filas como SQS e RabbitMQ, e publish/subscribe e streaming como Kafka) desacoplam produtores e consumidores. O serverless (FaaS) executa o código sob demanda, sem gerenciar servidores, com escala automática (até zero) e cobrança por invocação, mas com cold start, limites de tempo de execução e risco de lock-in.

#### Contêineres e orquestração

Um contêiner Docker empacota a aplicação com suas dependências, descrito por um Dockerfile e instanciado de uma imagem (em camadas). Imagens ficam em registries. Um exemplo mínimo:

dockerfile

FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci --omit=dev

COPY . .

EXPOSE 3000

CMD ["node", "[server.js](http://server.js)"]

  

O Kubernetes orquestra contêineres em cluster: o Pod é a menor unidade implantável (um ou mais contêineres que compartilham rede e armazenamento); o Deployment declara o número desejado de réplicas e cuida de atualizações progressivas (rolling update) e rollback; o Service oferece um endereço estável e balanceamento para os pods; o Ingress expõe serviços HTTP externamente; o HPA (Horizontal Pod Autoscaler) escala automaticamente conforme métricas. Seu modelo é declarativo: descreve-se o estado desejado e o sistema trabalha continuamente para atingi-lo. Há ainda o Helm (empacotamento de aplicações).

#### Dados e armazenamento

Os tipos de armazenamento são objeto (S3, escalável e barato, acesso por API HTTP, ideal para arquivos, backups e conteúdo estático), bloco (EBS, discos para VMs, baixa latência) e arquivo (NFS/SMB gerenciado). Para bancos, há opções relacionais gerenciadas (RDS, Cloud SQL) e NoSQL (chave-valor, documento, colunar e grafo), escolhidos conforme o padrão de acesso, o volume e as garantias de consistência.

#### Ciclo de vida, automação e resiliência

DevOps e CI/CD automatizam a integração, o teste e a entrega contínua. A infraestrutura como código (IaC) descreve a infraestrutura em arquivos versionados e reproduzíveis (Terraform, CloudFormation, Ansible). A observabilidade se apoia em três pilares: logs, métricas e rastreamento distribuído (traces). Os padrões de resiliência incluem retry com backoff exponencial, circuit breaker (interrompe chamadas a um serviço falho para evitar falhas em cascata), timeouts, bulkhead (isolamento de recursos) e idempotência das operações (repetir uma requisição não altera o resultado, essencial com retries). As estratégias de implantação incluem blue/green, canary e rolling.

#### Migração

As estratégias de migração conhecidas como “6 Rs”: rehost (lift-and-shift, mover sem alterar), replatform (ajustes mínimos, como migrar para um banco gerenciado), refactor (reescrever para o modelo nativo de nuvem), repurchase (trocar por um SaaS), retire (desativar) e retain (manter onde está). O lift-and-shift é o mais rápido, mas colhe pouco dos benefícios da nuvem.

Macete sobre nuvem. Decore as 5 características NIST (autoatendimento, acesso amplo, agrupamento, elasticidade, serviço medido) e os 4 modelos de implantação. Em IaaS/PaaS/SaaS, pergunte sempre “o que o cliente ainda gerencia?”. Em segurança, responsabilidade compartilhada é a resposta de quase toda questão conceitual, e os dados e acessos são sempre do cliente. Serverless não significa “sem servidores”, e sim “sem gerenciar servidores”. Elasticidade ≠ escalabilidade.

## MACETES GERAIS DESTE BLOCO

Em SO, treine sempre os cálculos mecânicos: Gantt (espera e retorno), número de bits da página e do deslocamento, EAT com TLB, contagem de faltas de página (FIFO, LRU, OPT, e lembre da anomalia de Belady), tamanho máximo de arquivo no i-node, estado seguro no banqueiro e capacidade dos níveis de RAID. As afirmativas com “sempre” e “nunca” costumam estar erradas.

Em HPC, domine as fórmulas de Amdahl e Gustafson e a distinção entre as duas, as correspondências CUDA↔OpenCL e a regra de que cada ferramenta serve um tipo de arquitetura (MPI/distribuída, OpenMP/compartilhada, CUDA/OpenCL/GPU, OpenGL/gráficos). Lembre que warp = 32 threads na NVIDIA.

Em nuvem, o NIST, os modelos de serviço e a responsabilidade compartilhada respondem à maioria das questões. Para questões de arquitetura, quando aparecer “escalar horizontalmente”, pense em aplicação sem estado, balanceador e auto scaling.

-

## ANÁLISE DO QUE MAIS CAI (2020 A 2026)

Aqui vale a mesma transparência da primeira parte: não tenho acesso a um levantamento verificado, questão por questão, das provas da ESFCEx de 2020 a 2026, e não vou inventar percentuais. O que segue é uma estimativa de prioridade, construída a partir do conteúdo programático e do padrão geral de provas de Informática para formação de oficiais, e deve ser lida como orientação de estudo, não como estatística oficial.

Incidência alta (estimada). Em Sistemas Operacionais: processos e threads (estados, PCB, diferença entre processo e thread), escalonamento (FCFS, SJF, RR, prioridades, com cálculo), sincronização (semáforos, seção crítica, condições de corrida) e deadlock (condições de Coffman, banqueiro), memória virtual e paginação (cálculos de endereço, algoritmos de substituição, thrashing) e sistemas de arquivos (alocação, i-node, FAT/NTFS/ext4, journaling). Em Nuvem: as características e os modelos de serviço e implantação, e a segurança com responsabilidade compartilhada. Esses são os assuntos de entrada mais “conceituais e cobráveis”, com alta chance de aparecer em formato de afirmativas.

Incidência média. Virtualização (tipo 1 × tipo 2, VM × contêiner, paravirtualização), IPC (pipes, memória compartilhada × mensagens, sockets), E/S (escalonamento de disco, RAID, DMA, buffering/spooling), e em HPC os conceitos básicos (speedup, Amdahl, taxonomia de Flynn, cluster × multicore, GPU) e as diferenças entre MPI, OpenMP, CUDA e OpenCL. Em Nuvem, contêineres e Kubernetes e os conceitos de arquitetura (elasticidade, SLA, disponibilidade).

Incidência menor. Aspectos distribuídos avançados (relógios lógicos, eleição, consenso, Paxos/Raft), detalhes de programação em MPI e CUDA (sintaxe), OpenGL e pipeline gráfico, Gustafson, topologias de interconexão em detalhe, e padrões de resiliência e migração (6 Rs).

Como a prova é de Informática para formação de oficiais e tende a cobrar conhecimento conceitual com algum cálculo, a melhor aposta de custo-benefício é: dominar os cálculos mecânicos de SO, a nomenclatura de nuvem e as relações entre ferramentas e arquiteturas de HPC. Para trocar a estimativa por dado real, vale o procedimento que sugeri antes: reunir as provas anteriores da ESFCEx que você encontrar, marcar cada questão com o tópico do edital e contar. Se quiser, preparo uma planilha-modelo para isso e uma bateria de exercícios comentados com os temas de maior peso: escalonamento, paginação e substituição de páginas, deadlock e responsabilidade compartilhada em nuvem.

## Sistemas de Computação, parte 3: Segurança da Informação

Mantenho o formato das partes anteriores: o conceito central, depois os detalhes que as bancas cobrem, com exemplos resolvidos e os Macetes destacados ao longo do texto. No final há os macetes gerais e a análise de incidência. A seção de normas da ABNT foi conferida em fontes atuais, e destaco o que mudou nas versões mais recentes.

-

## G.1 Fundamentos: o vocabulário que sustenta tudo

A segurança da informação protege a informação, em qualquer meio, contra acesso, uso, alteração ou destruição indevidos. Seu alicerce é a tríade CID (em inglês, CIA):

Confidencialidade: a informação só é acessível a quem foi autorizado. É ameaçada por vazamentos e espionagem, e protegida por criptografia e controle de acesso.

Integridade: a informação não é alterada de forma não autorizada, mantendo-se exata e completa. É protegida por hash, assinatura digital e controle de versão.

Disponibilidade: a informação e os serviços estão acessíveis quando necessários. É ameaçada por DoS e falhas, e protegida por redundância, backup e planos de continuidade.

A esses três pilares somam-se propriedades complementares que a prova adora: a autenticidade (garantia da origem, de que quem diz ter produzido a informação realmente o fez), o não repúdio ou irretratabilidade (o autor não pode negar a autoria depois) e a legalidade. Acrescente o conjunto AAA: Autenticação (provar quem você é), Autorização (definir o que você pode fazer) e Accounting/Auditoria (registrar o que você fez).

Os termos de risco precisam estar bem distintos. Ativo é tudo que tem valor para a organização. Ameaça é a causa potencial de um incidente (um atacante, um malware, um incêndio). Vulnerabilidade é a fraqueza de um ativo que pode ser explorada por uma ameaça. Ataque é a tentativa concreta de explorar uma vulnerabilidade. Impacto é a consequência. Risco é a combinação da probabilidade de uma ameaça explorar uma vulnerabilidade com o impacto resultante. Os controles são preventivos (evitam), detectivos (identificam), corretivos (recuperam) e dissuasivos.

Os princípios de projeto recorrentes são a defesa em profundidade (várias camadas independentes de proteção, de modo que a falha de uma não comprometa tudo), o menor privilégio (cada usuário e processo recebe só o acesso estritamente necessário), a segregação de funções (nenhuma pessoa controla sozinha um processo crítico de ponta a ponta), o need to know (necessidade de conhecer), o padrão seguro (fail-safe defaults, negar por padrão) e o princípio de Kerckhoffs (a segurança de um sistema criptográfico deve depender apenas do segredo da chave, e não do segredo do algoritmo).

-

## G.2 Criptografia

Criptografia é a ciência de transformar a informação (texto claro) em uma forma ininteligível (texto cifrado) por meio de um algoritmo e de uma chave, e de revertê-la a quem possui a chave correta. A criptoanálise estuda como quebrar essas proteções. As cifras históricas já ilustram os dois grandes mecanismos: a substituição (a cifra de César desloca cada letra, e a de Vigenère usa uma chave repetida) e a transposição (reordena as letras). Elas caem facilmente por análise de frequência. O one-time pad (chave aleatória, do tamanho da mensagem, usada uma única vez) é teoricamente inquebrável, mas impraticável por causa da distribuição das chaves. A esteganografia é diferente: oculta a existência da mensagem (por exemplo, nos bits menos significativos de uma imagem), e não a torna ilegível.

### Criptografia simétrica

Usa a mesma chave para cifrar e decifrar. É rápida e eficiente para grandes volumes, mas tem o problema da distribuição segura da chave e da escalabilidade: numa rede de n participantes que querem comunicação sigilosa par a par, são necessárias n(n − 1)/2 chaves (para 10 pessoas, 45 chaves). Também não oferece não repúdio, pois as duas partes conhecem a chave.

Os algoritmos clássicos são os seguintes:

DES: cifra de bloco de 64 bits, chave de 56 bits efetivos (64 com paridade), estrutura Feistel de 16 rodadas. Hoje é considerado inseguro (quebrado por força bruta). O 3DES aplica o DES três vezes (cifra-decifra-cifra) e está em desuso.

AES (Rijndael): padrão atual, cifra de bloco de 128 bits com chaves de 128, 192 ou 256 bits, que executam 10, 12 ou 14 rodadas, respectivamente. É uma rede de substituição-permutação, e sem ataque prático conhecido.

RC4: cifra de fluxo, hoje proibida em protocolos como o TLS por vulnerabilidades. ChaCha20 é uma cifra de fluxo moderna e muito usada em dispositivos móveis.

Outros: Blowfish, Twofish, IDEA.

A diferença entre cifras de bloco (processam blocos fixos) e de fluxo (processam bit a bit ou byte a byte) cai com frequência. Para cifrar mensagens maiores que um bloco usam-se os modos de operação: o ECB cifra cada bloco independentemente, vaza padrões (blocos iguais geram cifras iguais, o que revela a “silhueta” de imagens) e não deve ser usado; o CBC encadeia os blocos com um vetor de inicialização (IV); o CTR transforma a cifra de bloco em de fluxo; e o GCM é um modo autenticado, que oferece confidencialidade e integridade simultaneamente (o recomendado atualmente).

### Criptografia assimétrica (de chave pública)

Usa um par de chaves matematicamente relacionadas: a pública (divulgada a todos) e a privada (mantida em segredo pelo titular). O que uma cifra, só a outra decifra, e é computacionalmente inviável descobrir a privada a partir da pública. Resolve a distribuição de chaves e viabiliza a assinatura digital, mas é centenas de vezes mais lenta que a simétrica.

Os dois usos fundamentais (e a prova adora trocá-los) são:

Confidencialidade: o remetente cifra com a chave pública do destinatário, e só o destinatário, com a sua privada, decifra.

Autenticidade, integridade e não repúdio (assinatura): o remetente “assina” com a sua chave privada, e qualquer um verifica com a chave pública do remetente.

Macete: para sigilo, cifre com a pública do destino; para assinar, use a sua privada. Nunca se cifra com a sua própria pública esperando que outros leiam, e nunca se compartilha a privada.

Os principais algoritmos são o RSA (baseado na dificuldade de fatorar o produto de dois primos grandes, hoje com chaves de no mínimo 2048 bits), o Diffie-Hellman (apenas acordo de chaves, não cifra nem assina: duas partes constroem um segredo compartilhado por um canal inseguro, baseado no problema do logaritmo discreto), o ElGamal, o DSA/ECDSA/EdDSA (assinatura) e a criptografia de curvas elípticas (ECC), que oferece segurança equivalente com chaves muito menores (uma chave ECC de 256 bits equivale aproximadamente a uma RSA de 3072 bits), sendo ideal para dispositivos limitados. Atenção: o Diffie-Hellman sozinho é vulnerável ao ataque man-in-the-middle sem autenticação das partes.

Exemplo de RSA com números pequenos (didático): escolhem-se os primos p = 3 e q = 11. Então n = p·q = 33 e φ(n) = (p−1)(q−1) = 20. Escolhe-se e = 3 (primo com 20), e calcula-se d tal que e·d ≡ 1 (mod 20), obtendo d = 7 (3 × 7 = 21 ≡ 1). A chave pública é (e = 3, n = 33) e a privada, (d = 7, n = 33). Para cifrar a mensagem m = 4: c = mᵉ mod n = 4³ mod 33 = 64 mod 33 = 31. Para decifrar: m = cᵈ mod n = 31⁷ mod 33 = 4.

### Criptografia híbrida e envelope digital

Na prática combinam-se as duas: usa-se a assimétrica apenas para trocar uma chave de sessão (ou para autenticar), e a simétrica, rápida, para cifrar os dados em si. É assim que funcionam o TLS/HTTPS, o PGP e o S/MIME. O envelope digital é exatamente isso: a mensagem é cifrada com uma chave simétrica aleatória, e essa chave é cifrada com a pública do destinatário.

### Funções de hash

Uma função hash criptográfica transforma uma entrada de qualquer tamanho num resumo (digest) de tamanho fixo. As propriedades exigidas: determinismo (mesma entrada, mesmo hash), resistência à pré-imagem (dado o hash, não se acha a entrada: é função de mão única), resistência à segunda pré-imagem e resistência a colisões (é inviável achar duas entradas distintas com o mesmo hash), além do efeito avalanche (alterar um bit da entrada muda drasticamente o hash). Hash não é criptografia: não há chave nem forma de “decifrar”.

Os algoritmos: MD5 (128 bits) e SHA-1 (160 bits) estão quebrados (colisões demonstradas) e não devem ser usados para segurança. A família SHA-2 (SHA-224/256/384/512) é o padrão corrente, e o SHA-3 (Keccak) é o mais recente. Usos: verificação de integridade de arquivos, assinatura digital, armazenamento de senhas e HMAC.

Para senhas, o hash puro é insuficiente, porque tabelas pré-calculadas (rainbow tables) e a força bruta rápida o derrotam. As boas práticas são acrescentar um salt (valor aleatório único por senha) e usar funções lentas e adaptativas (bcrypt, scrypt, Argon2, PBKDF2). O HMAC combina um hash com uma chave secreta, e oferece integridade e autenticidade simultaneamente (sem não repúdio, pois a chave é compartilhada).

### Assinatura digital

O processo: o remetente calcula o hash do documento e cifra esse hash com a sua chave privada, produzindo a assinatura, que segue anexa ao documento. O destinatário recalcula o hash do documento recebido, decifra a assinatura com a chave pública do remetente e compara os dois hashes. Se forem iguais, comprova-se a integridade (o documento não mudou), a autenticidade (só quem tem a privada poderia assinar) e o não repúdio. A assinatura digital não garante confidencialidade: o documento continua legível. Assina-se o hash, e não o documento inteiro, por eficiência.

### Ataques à criptografia e o futuro

Os ataques típicos: força bruta (testar todas as chaves, o que torna o tamanho da chave decisivo: cada bit a mais dobra o esforço), dicionário, texto claro conhecido/escolhido, ataque do aniversário (explora colisões de hash, com custo da ordem de 2ⁿ/² para um hash de n bits), man-in-the-middle e canal lateral (side-channel) (explora tempo de execução, consumo de energia). Na computação quântica, o algoritmo de Shor quebraria RSA, Diffie-Hellman e ECC, e o algoritmo de Grover reduz pela metade a segurança efetiva das chaves simétricas (por isso o AES-256 é considerado resistente). A resposta é a criptografia pós-quântica, em processo de padronização pelo NIST (algoritmos como ML-KEM, derivado do Kyber, para troca de chaves, e ML-DSA e SLH-DSA para assinaturas). O processo de migração é um tópico atual.

-

## G.3 Certificação digital

O problema que ela resolve: como ter certeza de que uma chave pública realmente pertence a quem alega? A resposta é um terceiro de confiança que atesta essa ligação, assinando-a. O certificado digital é o documento eletrônico que vincula uma chave pública a uma identidade (pessoa, empresa, servidor), assinado digitalmente por uma Autoridade Certificadora (AC).

O padrão de formato é o X.509. Um certificado contém: a versão, o número de série, o algoritmo de assinatura, o emissor (a AC), o período de validade, o titular (subject), a chave pública do titular, as extensões (uso da chave, nomes alternativos/SAN, pontos de distribuição da LCR) e a assinatura da AC. Ele não contém a chave privada, que permanece sempre com o titular.

### A infraestrutura de chaves públicas (PKI/ICP)

A ICP (Infraestrutura de Chaves Públicas) é o conjunto de entidades, políticas e procedimentos que emitem, gerenciam e revogam certificados. Seus componentes são a AC (emite e assina certificados e as listas de revogação), a AR (Autoridade de Registro) (faz a identificação presencial ou validada do solicitante e encaminha o pedido à AC, mas não emite o certificado), o repositório (onde ficam os certificados e as listas) e os mecanismos de revogação.

A confiança funciona em cadeia (hierarquia): a AC Raiz possui um certificado autoassinado, que é a âncora de confiança e já vem instalado nos sistemas e navegadores. Ela certifica ACs intermediárias, que emitem os certificados dos usuários finais. Para validar um certificado, verifica-se a assinatura de cada elo até chegar a uma raiz confiável, e também a validade e a não revogação. Quando um certificado é comprometido, perdido ou deixa de ser válido, ele é revogado. Os mecanismos são a LCR/CRL (Lista de Certificados Revogados, publicada periodicamente pela AC) e o OCSP (consulta em tempo real do status de um certificado, mais ágil). Há também o OCSP stapling, em que o servidor apresenta a resposta já assinada.

O carimbo do tempo (timestamp), emitido por uma Autoridade de Carimbo do Tempo (ACT/TSA), comprova que um documento existia em determinado instante, e é essencial para dar validade longa a assinaturas, mesmo depois de o certificado expirar.

### ICP-Brasil

A ICP-Brasil foi instituída pela Medida Provisória nº 2.200-2/2001 para garantir a autenticidade, a integridade e a validade jurídica de documentos eletrônicos e de aplicações com certificados digitais. Sua estrutura é: o Comitê Gestor da ICP-Brasil (define políticas e normas), a AC Raiz, exercida pelo ITI (Instituto Nacional de Tecnologia da Informação), autarquia federal que não emite certificados para o usuário final e só credencia e fiscaliza as ACs de primeiro nível, as ACs (entidades públicas ou privadas credenciadas, como Serpro, Serasa Experian, Certisign, Caixa e outras) e as ARs. A MP estabelece que os documentos eletrônicos assinados com certificado ICP-Brasil presumem-se verdadeiros em relação aos signatários.

Os tipos de certificado se diferenciam pela finalidade e pelo meio de armazenamento da chave privada:

Série A (assinatura/autenticação): A1 (chave gerada e armazenada no computador/software, em arquivo; é a de menor segurança física e de menor validade, tradicionalmente 1 ano), A3 (chave em hardware criptográfico, como cartão inteligente (smartcard) ou token USB, ou nuvem; a chave não pode ser exportada) e A4 (hardware, com chaves de 4096 bits). Os tamanhos de chave RSA mínimos são 2048 bits para A1 e A3.

Série S (sigilo): S1, S3 e S4, correspondentes, com finalidade de cifrar (confidencialidade), e não de assinar.

Série T: certificados de carimbo do tempo, emitidos às ACTs.

Os prazos de validade de cada tipo foram alterados por normativos do Comitê Gestor ao longo do tempo, então, para a prova, confirme os prazos vigentes no material mais atual do edital. A distinção que permanece estável é que o A1 é em software (menos seguro, mas fácil de usar e instalar em servidores) e o A3 é em hardware (mais seguro).

No plano legal, a Lei nº 14.063/2020 classifica as assinaturas eletrônicas em três níveis: a simples (identifica o signatário por meios simples, como login e senha), a avançada (usa certificados não ICP-Brasil ou mecanismos que garantem vínculo único com o signatário e detecção de alteração posterior) e a qualificada (usa certificado digital ICP-Brasil, o nível de maior força probatória). Os formatos de assinatura digital normatizados incluem CAdES (binário, CMS), XAdES (XML, usado em documentos fiscais) e PAdES (PDF), cada um com perfis para validade de longo prazo.

### Certificados em servidores web

Em HTTPS, o certificado do servidor autentica o site ao navegador. Os níveis de validação são DV (apenas controle do domínio), OV (valida a organização) e EV (validação estendida). Há certificados wildcard (*.exemplo.com, cobrem todos os subdomínios de um nível) e SAN (vários nomes num só certificado). A Let’s Encrypt oferece certificados DV gratuitos e automatizados (ACME). Ao acessar o site, o navegador verifica a cadeia, a validade, o nome e a revogação, e avisa o usuário se algo falhar. Medidas complementares são o HSTS (força o uso de HTTPS), o certificate pinning e o Certificate Transparency (registros públicos de certificados emitidos, para detectar emissões indevidas).

Macete: o certificado garante quem é o dono da chave pública, nada mais. Confiar numa chave é confiar na AC que a atestou. Se a questão perguntar “o que a AC assina?”, a resposta é o certificado (ou seja, a ligação identidade–chave pública), e não o documento do usuário.

-

## G.4 Políticas e protocolos de segurança

### Políticas e governança

A Política de Segurança da Informação (PSI) é o documento de mais alto nível que declara o compromisso da direção, os objetivos, as responsabilidades e as regras gerais de proteção. Ela se desdobra em uma hierarquia: a política (o “porquê” e as diretrizes estratégicas), as normas (regras obrigatórias específicas), os procedimentos (passo a passo operacional) e as instruções de trabalho. Para ser eficaz, precisa de apoio da direção, ser divulgada, revisada periodicamente e ter sanções definidas.

Componentes comuns: classificação da informação (pública, interna, confidencial, secreta, conforme o impacto de sua divulgação, com rotulagem e tratamento diferenciados, e, na administração pública federal, os graus de sigilo da Lei de Acesso à Informação e seu decreto: reservado, secreto e ultrassecreto), política de controle de acesso, política de senhas, política de uso aceitável, mesa limpa e tela limpa, uso de dispositivos móveis e BYOD, backup, gestão de incidentes, descarte seguro de mídias e conscientização e treinamento (o fator humano é o elo mais fraco, e a engenharia social explora isso).

Modelos de controle de acesso: o DAC (discricionário, o dono do recurso decide quem acessa, como as permissões de arquivos Unix), o MAC (obrigatório, o sistema impõe o acesso por rótulos de classificação, como no modelo militar), o RBAC (baseado em papéis, as permissões se associam a funções e os usuários a funções) e o ABAC (baseado em atributos e contexto). Os modelos clássicos de segurança formal incluem o Bell-LaPadula (confidencialidade: “sem leitura para cima, sem escrita para baixo”) e o Biba (integridade: o inverso).

Autenticação: os fatores são algo que você sabe (senha, PIN), algo que você tem (token, cartão, celular) e algo que você é (biometria). A autenticação multifator (MFA) combina fatores de categorias diferentes (duas senhas não são MFA). Em biometria, a FAR (taxa de falsa aceitação, aceita um impostor) e a FRR (taxa de falsa rejeição, rejeita um usuário legítimo) se movem em sentidos opostos, e o ponto em que se igualam é a EER. Gerenciadores de senha, SSO (login único), Kerberos, OAuth 2.0 (autorização delegada), OpenID Connect (autenticação sobre OAuth) e SAML (federação) são mecanismos comuns de identidade.

Backup: o completo copia tudo; o incremental copia o que mudou desde o último backup de qualquer tipo (rápido de fazer, lento de restaurar: exige o completo mais todos os incrementais); o diferencial copia o que mudou desde o último completo (restauração só com o completo mais o último diferencial). A regra 3-2-1: pelo menos 3 cópias, em 2 mídias diferentes, com 1 fora do local (hoje, com variantes que acrescentam uma cópia offline ou imutável, essencial contra ransomware). O backup só vale se a restauração for testada.

Continuidade: o PCN/BCP (Plano de Continuidade de Negócios) mantém as funções críticas durante uma crise, o DRP (Plano de Recuperação de Desastres) restaura a TI, e a análise de impacto no negócio (BIA) define o RTO (tempo máximo tolerável de indisponibilidade) e o RPO (perda máxima tolerável de dados, medida em tempo). Os sites de contingência são hot (pronto, espelhado), warm (parcialmente equipado) e cold (apenas espaço e infraestrutura básica).

Contexto normativo brasileiro (além da ABNT): a Política Nacional de Segurança da Informação (PNSI) foi instituída pelo Decreto nº 9.637/2018, com o GSI/PR como órgão central de coordenação. A LGPD (Lei nº 13.709/2018) regula o tratamento de dados pessoais e exige medidas de segurança e a comunicação de incidentes. O Marco Civil da Internet (Lei nº 12.965/2014) estabelece princípios, guarda de registros e responsabilidades. A Lei nº 12.737/2012 criou crimes informáticos (invasão de dispositivo informático), e foi agravada pela Lei nº 14.155/2021 (fraudes eletrônicas). Como esses dispositivos mudam, vale conferir a versão vigente de cada um ao estudar.

### Protocolos de segurança

SSL/TLS. Provê confidencialidade, integridade e autenticação sobre o TCP, e é a base do HTTPS (porta 443). O SSL (todas as versões) e o TLS 1.0/1.1 estão obsoletos; usam-se o TLS 1.2 e o TLS 1.3. No handshake, cliente e servidor negociam os parâmetros, o servidor apresenta seu certificado (autenticação), e eles estabelecem uma chave de sessão simétrica (por Diffie-Hellman efêmero ou RSA nas versões antigas); depois, os dados trafegam cifrados com a chave simétrica. O TLS 1.3 reduz o handshake, remove algoritmos fracos e exige sigilo perfeito (forward secrecy), em que o vazamento futuro da chave privada do servidor não permite decifrar sessões passadas.

SSH (porta 22). Acesso remoto seguro, substituto do Telnet (porta 23, texto claro) e do rlogin. Autentica o servidor por chave de host e o usuário por senha ou, melhor, por par de chaves. Permite também túneis (port forwarding) e o SFTP (transferência segura sobre SSH, distinto do FTPS, que é o FTP sobre TLS). O SCP copia arquivos por SSH.

IPsec. Conjunto de protocolos da camada de rede (IP) para segurança, muito usado em VPNs. Seus protocolos são o AH (Authentication Header, protocolo 51: integridade e autenticação, sem confidencialidade) e o ESP (Encapsulating Security Payload, protocolo 50: confidencialidade, e também integridade/autenticação), e o IKE (UDP 500, e 4500 com NAT-T) negocia as chaves e as associações de segurança (SA). No modo transporte, só a carga útil é protegida (entre hosts); no modo túnel, o pacote IP inteiro é encapsulado e protegido (entre gateways, o típico das VPNs site a site). Outras VPNs: OpenVPN, WireGuard, L2TP/IPsec, e o PPTP, considerado inseguro.

Segurança de e-mail: S/MIME (usa certificados X.509) e PGP/GPG (modelo de confiança descentralizado, a “teia de confiança”) cifram e assinam mensagens. SMTPS/STARTTLS, IMAPS (993) e POP3S (995) protegem o transporte. Contra falsificação de remetente, usam-se SPF, DKIM (assinatura do domínio) e DMARC.

DNS: o DNSSEC adiciona assinaturas digitais às respostas DNS, garantindo autenticidade e integridade (contra cache poisoning e spoofing), mas não cifra as consultas. DoH/DoT (DNS sobre HTTPS/TLS) cifram o tráfego DNS.

Redes sem fio: o WEP (RC4) é totalmente quebrado. O WPA foi transitório, o WPA2 (AES-CCMP) foi o padrão por anos, e o WPA3 (autenticação SAE, mais resistente a ataques de dicionário offline e com sigilo perfeito) é o mais recente. Em redes corporativas usa-se o WPA2/WPA3-Enterprise, com IEEE 802.1X, EAP e um servidor RADIUS (UDP 1812/1813).

Kerberos (porta 88). Protocolo de autenticação em rede baseado em tíquetes e criptografia simétrica, com um KDC (Key Distribution Center) formado pelo AS (Authentication Server, que emite o TGT, Ticket Granting Ticket) e pelo TGS (Ticket Granting Server, que emite os tíquetes de serviço). O usuário se autentica uma vez e usa o TGT para obter acesso a vários serviços (base do SSO no Active Directory). Depende de relógios sincronizados (tolerância de poucos minutos).

Outros: SNMPv3 (autenticação e cifragem, em oposição ao v1/v2c, em texto claro), HTTPS/HSTS, LDAPS (636) e RADIUS/TACACS+ (AAA de acesso à rede).

Macete de portas seguras: SSH 22; HTTPS 443; SMTPS 465/587; IMAPS 993; POP3S 995; LDAPS 636. Os equivalentes inseguros: Telnet 23, FTP 21, HTTP 80, POP3 110, IMAP 143.

-

## G.5 Mecanismos de detecção e proteção contra ataques e invasões

### Principais ataques

Engenharia social e phishing: manipulam pessoas, e não sistemas. O phishing engana a vítima por e-mail para que revele credenciais ou execute algo. As variantes são o spear phishing (direcionado a uma pessoa específica), o whaling (contra executivos), o vishing (por voz), o smishing (por SMS) e o pharming (redireciona o usuário a um site falso, por adulteração de DNS). Também há pretexting, baiting e tailgating (entrar logo atrás de alguém autorizado).

Negação de serviço (DoS/DDoS): esgota recursos para derrubar a disponibilidade. O DDoS usa muitas máquinas (uma botnet). O SYN flood explora o handshake de três vias do TCP, enviando muitos SYN sem concluir a conexão e lotando a tabela de conexões semiabertas. Os ataques de amplificação/reflexão (DNS, NTP, memcached) enviam pequenas requisições com o IP da vítima forjado, e servidores abertos respondem com grandes volumes. O Smurf usa ICMP broadcast com origem forjada. Mitigações: filtros de borda, rate limiting, SYN cookies, CDNs e serviços anti-DDoS.

Interceptação e falsificação: o sniffing captura tráfego (efetivo em protocolos em texto claro). O man-in-the-middle (MITM) se interpõe entre duas partes, lendo ou alterando a comunicação. O spoofing falsifica identidades: de IP, de ARP (ARP poisoning associa o MAC do atacante ao IP de outro host na LAN), de DNS (cache poisoning) e de e-mail. Defesas: criptografia de ponta a ponta (TLS), Dynamic ARP Inspection, port security, DNSSEC.

Ataques a aplicações web: a injeção de SQL insere comandos SQL por entradas não tratadas (por exemplo, ' OR '1'='1), e a defesa essencial é usar consultas parametrizadas (prepared statements), com validação de entrada e menor privilégio no banco. O XSS (Cross-Site Scripting) injeta scripts que executam no navegador de outras vítimas (refletido, armazenado ou baseado em DOM), com defesa por escape/codificação de saída e Content Security Policy. O CSRF induz o navegador autenticado da vítima a executar uma ação indesejada, e a defesa são tokens anti-CSRF e cookies SameSite. Outros: buffer overflow (escrever além do limite de um buffer, sobrescrevendo memória adjacente), inclusão de arquivos, SSRF, quebra de controle de acesso e directory traversal. O projeto OWASP Top 10 lista os riscos mais críticos de aplicações web; na edição de 2021, os dez itens foram: controle de acesso quebrado, falhas criptográficas, injeção, design inseguro, configuração incorreta de segurança, componentes vulneráveis e desatualizados, falhas de identificação e autenticação, falhas de integridade de software e dados, falhas de registro e monitoramento, e SSRF. Confira se há edição mais recente publicada.

Ataques a credenciais: força bruta, dicionário, credential stuffing (reaproveita credenciais vazadas em outros sites), password spraying (poucas senhas comuns contra muitas contas, para evitar bloqueio), keylogging e pass-the-hash (usa o hash da senha sem conhecê-la).

Outros: o zero-day (vulnerabilidade desconhecida do fabricante, sem correção disponível), a APT (Advanced Persistent Threat, ataque sofisticado, prolongado e direcionado, tipicamente de atores estatais ou organizados), o ataque à cadeia de suprimentos (compromete um fornecedor ou componente de software confiável), o escalonamento de privilégios e o movimento lateral dentro da rede.

### Mecanismos de proteção

Firewall. Controla o tráfego entre redes conforme regras. Evolui de filtro de pacotes (decide por IP, porta e protocolo, sem estado e muito rápido) para o stateful inspection (acompanha o estado das conexões, permite respostas legítimas a conexões iniciadas internamente), o proxy de aplicação (atua como intermediário na camada 7, inspeciona o conteúdo e esconde os clientes) e o NGFW (Next-Generation Firewall, que inclui identificação de aplicações, IPS, inspeção TLS e inteligência de ameaças). O WAF (Web Application Firewall) protege especificamente aplicações web contra injeção e XSS. A política recomendada é a de negar por padrão (default deny) e liberar apenas o necessário. No Linux, o iptables/nftables usa as cadeias INPUT, OUTPUT e FORWARD. Um firewall não protege contra ameaças internas, tráfego cifrado não inspecionado, malware trazido por mídia removível ou ataques que passam por portas permitidas.

DMZ (zona desmilitarizada). Segmento de rede intermediário entre a rede externa e a interna, onde ficam os serviços públicos (web, e-mail, DNS). Se um servidor da DMZ for comprometido, a rede interna continua protegida por outro firewall. As arquiteturas podem ter um firewall com três interfaces ou dois firewalls em série.

IDS e IPS. O IDS (Intrusion Detection System) detecta e alerta (é passivo, fica fora do caminho do tráfego ou recebe uma cópia), enquanto o IPS (Intrusion Prevention System) está em linha e bloqueia o que identifica como malicioso. Quanto ao local: NIDS/NIPS (de rede, monitora o tráfego do segmento) e HIDS/HIPS (de host, monitora logs, integridade de arquivos e chamadas de sistema da máquina). Quanto à técnica de detecção: por assinaturas (compara com padrões conhecidos: preciso para ameaças conhecidas, cego a novas), por anomalias (compara com um perfil do comportamento normal: detecta ataques novos, mas gera mais falsos positivos) e híbrida. Ferramentas conhecidas: Snort e Suricata (rede), OSSEC/Wazuh e Tripwire (host, integridade de arquivos). O desafio central é equilibrar falsos positivos (alarmes indevidos) e falsos negativos (ataques não detectados).

Honeypots e honeynets. Sistemas-isca, deliberadamente vulneráveis e monitorados, projetados para atrair atacantes, estudar suas técnicas e desviá-los de alvos reais. Qualquer acesso a eles é, por definição, suspeito.

SIEM e SOC. O SIEM centraliza e correlaciona logs de várias fontes para detectar incidentes e apoiar a conformidade. O SOC (Security Operations Center) é a equipe e o ambiente que monitora e responde. O EDR/XDR monitora e responde a ameaças em estações e em múltiplas camadas. A inteligência de ameaças (threat intelligence) alimenta tudo isso.

Segmentação e controle de rede: VLANs, NAC (Network Access Control, valida a postura do dispositivo antes de liberar o acesso), VPNs para acesso remoto seguro, proxies, filtros de conteúdo, DLP (prevenção de vazamento de dados) e a arquitetura Zero Trust (“nunca confie, sempre verifique”: nenhuma entidade é confiável por estar dentro da rede, e todo acesso é autenticado, autorizado e monitorado continuamente).

Gestão de vulnerabilidades: hardening (endurecimento: remover serviços e contas desnecessários, aplicar configurações seguras, como os benchmarks do CIS), gestão de correções (patch management), varreduras de vulnerabilidades (Nessus, OpenVAS) e testes de invasão (pentest), que se classificam em black box (sem conhecimento prévio), gray box (conhecimento parcial) e white box (conhecimento total). O Nmap é a ferramenta clássica de varredura de portas e descoberta de serviços. Atenção à distinção: a varredura de vulnerabilidades identifica fraquezas, e o pentest tenta explorá-las de forma controlada.

Modelos de referência de ataque: a Cyber Kill Chain (Lockheed Martin) descreve as fases de uma intrusão (reconhecimento, armamento, entrega, exploração, instalação, comando e controle, ações sobre os objetivos), e a base MITRE ATT&CK cataloga táticas e técnicas de adversários.

Resposta a incidentes (ciclo do NIST SP 800-61): preparação, detecção e análise, contenção, erradicação e recuperação e atividades pós-incidente (lições aprendidas). Na forense digital, vale a ordem de volatilidade (coletar primeiro o mais volátil: registradores e cache, memória RAM, tráfego de rede, processos, depois disco e mídias), a cadeia de custódia (registro documentado de quem manuseou a evidência, garantindo sua validade) e o uso de hash para comprovar que a imagem do disco não foi alterada. No governo federal, o CTIR Gov é o centro de tratamento e resposta a incidentes cibernéticos, e os ETIR são as equipes dos órgãos.

-

## G.6 Normas de segurança da informação no âmbito nacional (ABNT)

A ABNT publica no Brasil as versões nacionais das normas internacionais da família ISO/IEC 27000, com o prefixo ABNT NBR ISO/IEC. Conferi as edições vigentes em fontes atuais, porque houve revisões importantes recentemente. O quadro atual é o seguinte.

ABNT NBR ISO/IEC 27001:2022 (requisitos do SGSI). É a norma certificável, que especifica os requisitos para estabelecer, implementar, manter e melhorar continuamente um Sistema de Gestão de Segurança da Informação (SGSI). A ABNT a publica com o título “Segurança da informação, segurança cibernética e proteção à privacidade”, como tradução da edição internacional lançada pela ISO em outubro de 2022, que substituiu a anterior. Seus requisitos genéricos se aplicam a organizações de qualquer tipo, tamanho ou natureza. A estrutura segue as cláusulas de 4 a 10: 4 (contexto da organização, partes interessadas, escopo do SGSI), 5 (liderança, política de segurança, papéis e responsabilidades), 6 (planejamento: avaliação e tratamento de riscos, objetivos), 7 (apoio: recursos, competência, conscientização, comunicação, informação documentada), 8 (operação), 9 (avaliação de desempenho: monitoramento, auditoria interna, análise crítica pela direção) e 10 (melhoria: não conformidades, ação corretiva, melhoria contínua). O modelo de melhoria contínua é o ciclo PDCA (Plan-Do-Check-Act). Há ainda o Anexo A, que lista os controles de referência, e a Declaração de Aplicabilidade (SoA), que registra quais controles foram selecionados e por quê (e a justificativa de exclusões). Na edição de 2022 o Anexo A passou a refletir a estrutura da ISO 27002:2022. [globalsuitesolutions](https://www.globalsuitesolutions.com/pt/?p=117756)

ABNT NBR ISO/IEC 27002:2022 (código de prática, controles). Detalha os controles de segurança: define cada um, seu propósito e oferece orientação de implementação. A edição de 2022 organiza 93 controles em quatro temas: pessoais, físicos, tecnológicos e organizacionais, e chegou depois de um intervalo de quase dez anos desde a versão de 2013. A distribuição dos 93 é: 37 organizacionais, 8 de pessoas, 14 físicos e 34 tecnológicos. A versão anterior (2013) tinha 114 controles em 14 domínios, e a de 2022 consolidou e reorganizou, e acrescentou 11 controles novos: inteligência de ameaças (threat intelligence), segurança da informação para uso de serviços em nuvem, prontidão de TIC para a continuidade de negócios, monitoramento da segurança física, gerenciamento de configuração, exclusão de informação, mascaramento de dados, prevenção de vazamento de dados, atividades de monitoramento, filtragem da web e codificação segura. Os controles passaram também a ter atributos (tipo de controle, propriedades de segurança, conceitos de cibersegurança) para facilitar a filtragem e o cruzamento. [estrategiaconcursos](https://www.estrategiaconcursos.com.br/blog/?p=1277031)

ABNT NBR ISO/IEC 27005 (gestão de riscos de segurança da informação). Orienta o processo de gestão de riscos que sustenta o SGSI: estabelecer o contexto, identificar, analisar e avaliar riscos, tratá-los, comunicar e monitorar. A edição mais recente foi publicada pela ABNT como NBR ISO/IEC 27005:2023, cancelando e substituindo a versão anterior. As opções de tratamento são mitigar/modificar (aplicar controles), evitar (eliminar a atividade geradora do risco), compartilhar/transferir (seguro, terceirização) e reter/aceitar (assumir conscientemente), restando sempre o risco residual. Em termos gerais de gestão de riscos há também a ABNT NBR ISO 31000 (diretrizes gerais, não específicas de segurança da informação). [tre-sp](https://www.tre-sp.jus.br/transparencia-e-prestacao-de-contas/licitacoes/aquisicao-contratacao-direta-covid-19/inexigibilidade-de-licitacao-2025/etp-ilf-016-2025)

Outras normas da família que costumam ser citadas:

ABNT NBR ISO/IEC 27003: orientações para implementação de um SGSI. 27004: monitoramento, medição, análise e avaliação (métricas e indicadores).

ABNT NBR ISO/IEC 27701:2019: extensão da 27001 e da 27002 para a gestão da privacidade da informação (SGPI). A versão brasileira inclui um anexo de mapeamento com a LGPD, que ajuda a identificar quais controles atendem cada dispositivo da lei. [confidata](https://confidata.com.br/blog/iso-27701-certificacao-complementa-lgpd)

ABNT NBR ISO 22301: gestão de continuidade de negócios (SGCN). Substituiu a antiga NBR 15999.

ISO/IEC 27017 e 27018: controles de segurança para serviços em nuvem e proteção de dados pessoais em nuvem pública.

ISO/IEC 27035: gestão de incidentes de segurança. 27037: coleta e preservação de evidências digitais. 27014: governança da segurança da informação.

ABNT NBR ISO/IEC 27000: vocabulário e visão geral da família.

Macete das normas: 27001 = requisitos do SGSI (certifica, “o quê”); 27002 = controles e boas práticas (não certifica, “como”); 27005 = riscos; 27701 = privacidade/LGPD; 22301 = continuidade. Não confunda as edições: 27001 e 27002 são de 2022 (93 controles em 4 temas), e a versão de 2013 tinha 114 controles em 14 domínios. Nas provas mais antigas, as questões podem se referir à edição de 2013, por isso é útil conhecer as duas. Uma pegadinha clássica é dizer que a ISO 27002 é certificável, quando só a 27001 é.

-

## G.7 Antivírus e códigos maliciosos

### Classificação do malware

Malware (malicious software) é todo software projetado para causar dano ou obter vantagem indevida. Os tipos e o que os distingue:

Vírus: precisa de um hospedeiro (infecta arquivos ou programas) e de ação do usuário para se executar, e se propaga ao infectar outros arquivos. Há vírus de macro (documentos de escritório), de boot e de arquivo executável. Os polimórficos mudam sua aparência a cada infecção (cifrando-se com chaves distintas), e os metamórficos reescrevem seu próprio código, para fugir das assinaturas.

Worm (verme): se propaga sozinho pela rede, explorando vulnerabilidades, sem precisar de hospedeiro nem de ação do usuário. Consome banda e recursos. Esta é a distinção clássica: o vírus precisa de um hospedeiro, o worm é autônomo.

Cavalo de Troia (trojan): parece um programa legítimo, mas executa funções ocultas (instala backdoor, rouba dados). Não se replica sozinho.

Spyware: coleta informações do usuário sem consentimento. O keylogger registra teclas, e o screenlogger captura telas. Adware exibe publicidade indesejada.

Ransomware: cifra os arquivos da vítima (ou bloqueia o acesso) e exige resgate, geralmente em criptomoeda. Hoje é a ameaça mais visada, muitas vezes com dupla extorsão (ameaça de vazar os dados roubados).

Backdoor: abre uma porta oculta para acesso remoto posterior. Rootkit: conjunto de técnicas que esconde a presença do invasor e do malware no sistema (processos, arquivos, conexões), operando muitas vezes no nível do kernel, o que o torna difícil de detectar.

Bot e botnet: o computador infectado (zumbi) passa a ser controlado remotamente, e o conjunto de zumbis é usado em DDoS, spam e fraudes.

Bomba lógica: código dormente que dispara quando uma condição é satisfeita (uma data, a remoção de um usuário). Cryptojacking: usa os recursos da vítima para minerar criptomoedas. Fileless malware: age na memória e em ferramentas legítimas do sistema, sem deixar arquivos no disco.

### Como o antivírus detecta

Um antivírus (hoje em geral chamado anti-malware ou plataforma de proteção de endpoint) combina técnicas, porque nenhuma é suficiente sozinha.

Detecção por assinatura: compara os arquivos com um banco de assinaturas (padrões de bytes ou hashes de malware conhecido). É rápida e precisa para ameaças conhecidas, mas inútil contra malware novo (zero-day) e contra variantes polimórficas. Exige atualização constante do banco.

Heurística: procura características e instruções suspeitas em programas desconhecidos, sem depender de uma assinatura exata. Pode ser estática (analisa o código sem executar) ou dinâmica. Detecta variantes e ameaças novas, ao custo de mais falsos positivos.

Análise comportamental: observa o que o programa faz em execução (modificar muitos arquivos rapidamente, alterar o registro, tentar desabilitar a segurança, cifrar documentos em massa) e bloqueia comportamentos típicos de malware, o que é fundamental contra ransomware e malware sem arquivo.

Emulação e sandbox: executa o código suspeito num ambiente isolado e controlado, para observar seu comportamento sem risco ao sistema real. Alguns malwares detectam a sandbox e se escondem, e esse é um jogo de gato e rato.

Outras: aprendizado de máquina, reputação em nuvem (consulta a bases globais sobre a popularidade e o histórico do arquivo), verificação de integridade (compara hashes de arquivos críticos com os esperados) e quarentena (isola o arquivo suspeito, preservando-o e impedindo sua execução, em vez de apagá-lo de imediato).

O modo de operação pode ser a varredura sob demanda (o usuário ou uma tarefa agendada verifica o sistema) ou a proteção em tempo real (on-access), que verifica cada arquivo ao ser aberto, baixado ou executado. Em ambos pode haver falsos positivos (arquivo legítimo sinalizado) e falsos negativos (malware não detectado). O arquivo de teste EICAR é uma cadeia de texto padronizada, inofensiva, que os antivírus reconhecem como “vírus”, usada para verificar se a proteção está ativa.

### Limitações e boas práticas

O antivírus não é infalível: não detecta com certeza o malware inédito nem o ofuscado com packers e criptografia, não impede a engenharia social, não substitui o firewall nem as correções de segurança, e pode ser desativado por malware. Por isso ele é apenas uma camada da defesa em profundidade. As boas práticas: manter sistema e aplicativos atualizados (a maior parte das infecções explora vulnerabilidades conhecidas e com correção disponível), manter o antivírus e suas assinaturas atualizados, não executar anexos e links duvidosos, usar contas sem privilégio de administrador no uso diário, controlar mídias removíveis, manter backups offline/imutáveis testados (a defesa mais eficaz contra ransomware), e usar EDR/XDR em ambientes corporativos. Em ambientes corporativos, a gestão é centralizada (console que distribui políticas, atualizações e coleta alertas).

-

## MACETES GERAIS DESTE BLOCO

Criptografia. Simétrica = mesma chave, rápida, problema da distribuição. Assimétrica = par de chaves, lenta. Sigilo: cifra com a pública do destino. Assinatura: assina com a sua privada. Assinatura digital garante autenticidade, integridade e não repúdio, mas não sigilo. Hash não é reversível e não usa chave (HMAC usa). MD5 e SHA-1 estão quebrados. AES: bloco 128 bits, chaves 128/192/256. DES: chave de 56 bits, inseguro. Diffie-Hellman = acordo de chaves. Em criptografia híbrida (TLS), a assimétrica negocia a chave e a simétrica cifra os dados.

Certificação. O certificado liga identidade ↔ chave pública e é assinado pela AC. A AR identifica o solicitante, mas não emite. A AC Raiz da ICP-Brasil é o ITI. A chave privada nunca vai no certificado. A revogação se consulta por LCR ou OCSP. A1 = software; A3 = hardware (token/cartão).

Protocolos. SSH substitui Telnet. HTTPS = HTTP sobre TLS. IPsec: ESP cifra, AH só autentica, e o modo túnel protege o pacote inteiro. WPA3 > WPA2 >> WEP. Kerberos usa tíquetes e depende de relógio sincronizado.

Detecção. IDS detecta, IPS bloqueia. Assinatura = ameaças conhecidas, anomalia = novas (mais falsos positivos). Firewall stateful acompanha o estado da conexão. Honeypot = isca. Scan de vulnerabilidades ≠ pentest. SQL injection se previne com consultas parametrizadas, e XSS com escape de saída.

Malware. Vírus precisa de hospedeiro, worm é autônomo e se propaga pela rede, trojan não se replica, rootkit esconde, ransomware cifra, e backup offline é a melhor defesa contra ele.

Normas. 27001 = requisitos/certificável (2022), 27002 = controles (2022, 93 controles, 4 temas), 27005 = riscos (2023), 27701 = privacidade/LGPD, 22301 = continuidade.

Estratégia. Em questões de segurança, desconfie de afirmações absolutas como “garante totalmente”, “elimina” e “impede qualquer ataque”, pois segurança é redução de risco, e nunca ausência dele. Identifique sempre qual propriedade da tríade a questão discute e qual controle a protege.

-

## ANÁLISE DO QUE MAIS CAI (2020 A 2026)

Mantenho a mesma transparência das partes anteriores: não consegui acessar um levantamento verificado, questão por questão, das provas da ESFCEx de 2020 a 2026, e por isso não vou inventar percentuais. Verifiquei apenas as edições vigentes das normas da ABNT, que sustentaram a seção G.6. O que segue é uma estimativa de prioridade, baseada no conteúdo programático e no padrão geral de provas de Informática, e deve ser lida como orientação de estudo, e não como estatística oficial.

Incidência alta (estimada). Criptografia: a diferença entre simétrica e assimétrica, o uso correto das chaves (cifrar × assinar), funções de hash e assinatura digital, que são o tema mais recorrente e com mais chance de aparecer em formato de afirmativas e de cálculo conceitual. Certificação digital: AC, AR, cadeia de confiança, o conteúdo e o uso do certificado, a ICP-Brasil, os tipos A1/A3. Ataques e proteção: tipos de malware (vírus × worm × trojan), phishing, DoS/DDoS, injeção de SQL e XSS, e a diferença entre firewall, IDS e IPS. Normas da ABNT: 27001 e 27002, e o que cada uma trata, com atenção à atualização de 2022.

Incidência média. Protocolos (TLS/HTTPS, SSH, IPsec, WPA2/WPA3), políticas de segurança (classificação da informação, controle de acesso, MFA), backup (completo, incremental, diferencial) e continuidade (RTO e RPO), antivírus (métodos de detecção, limitações), gestão de riscos (27005, tratamento de risco) e DMZ.

Incidência menor, mas que aparece. Kerberos e AAA em detalhe, Bell-LaPadula e Biba, criptografia pós-quântica, forense digital (ordem de volatilidade, cadeia de custódia), Cyber Kill Chain e MITRE ATT&CK, OWASP Top 10 em detalhe, e a legislação (PNSI, LGPD, Marco Civil) quando o edital as cita.

Em segurança, o ponto mais importante é a atualização: as normas da ABNT mudaram em 2022 e 2023, e as provas mais antigas podem refletir a edição de 2013. Confira no edital qual edição ele indica. Para trocar a estimativa por dado real, repita o procedimento que sugeri antes: reunir as provas anteriores da ESFCEx que você encontrar, marcar cada questão com o tópico do edital e contar. Se quiser, posso montar a planilha-modelo para essa contagem e uma bateria de exercícios comentados com os temas de maior peso: criptografia e assinatura digital, certificação ICP-Brasil, IDS × IPS × firewall, tipos de malware e as normas 27001/27002.

  
  

