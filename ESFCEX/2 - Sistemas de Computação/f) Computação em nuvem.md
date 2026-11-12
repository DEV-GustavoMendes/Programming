
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

`FROM node:20-alpine`

`WORKDIR /app`

`COPY package*.json ./`

`RUN npm ci --omit=dev`

`COPY . .`

`EXPOSE 3000`

`CMD ["node", "[server.js](http://server.js)"]`

  
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

**