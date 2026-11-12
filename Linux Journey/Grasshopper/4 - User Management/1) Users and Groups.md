# Usuários e Grupos

Aprenda como o Linux identifica usuários e grupos e como as credenciais dos processos afetam as decisões de acesso.

O Linux usa identidades de usuários e grupos para rotular processos, definir a propriedade de objetos do sistema de arquivos e tomar decisões de controle de acesso. Os nomes legíveis ajudam os administradores, enquanto o kernel trabalha principalmente com identificadores numéricos e credenciais de processos.

## Identificação de Usuários com UIDs

Cada conta possui um identificador numérico de usuário, ou **UID**. Os nomes são mapeados para UIDs pelos bancos de dados de contas do sistema. Os arquivos armazenam a propriedade numericamente, embora as ferramentas normalmente mostrem o nome correspondente.

Execute `id` para inspecionar as informações de identidade do processo atual:

```bash
$ id
uid=1000(alice) gid=1000(alice) groups=1000(alice),27(sudo)
```

Os valores variam conforme o sistema. Contas humanas de login geralmente possuem diretórios pessoais como `/home/alice`, mas uma conta pode usar outro caminho ou não ter um diretório pessoal comum. Contas de serviço frequentemente existem para executar software com uma identidade limitada, não para permitir login interativo.

## Organização do Acesso com Grupos

Um grupo possui um identificador numérico, ou **GID**. Uma conta normalmente tem um grupo primário e pode pertencer a grupos suplementares. A associação a grupos permite conceder acesso a um conjunto de usuários sem atribuir permissões individualmente a cada conta.

Inspecione as associações com:

```bash
$ id alice
$ groups alice
```

Esses comandos informam dados de identidade configurados ou resolvidos. Serviços de diretório e caches podem participar; por isso, ler diretamente `/etc/group` nem sempre mostra o quadro completo das associações efetivas.

## Compreensão das Credenciais dos Processos

Um processo possui credenciais como UIDs e GIDs reais e efetivos, além de grupos suplementares. As credenciais efetivas são centrais para muitas verificações de permissão. Um processo iniciado por um usuário geralmente herda as credenciais do pai, mas mecanismos controlados podem alterá-las.

Isso é mais preciso que dizer que um processo sempre é executado apenas “como o usuário que o iniciou”. Executáveis set-user-ID, gerenciadores de serviços, contêineres, namespaces e chamadas de sistema que alteram privilégios podem afetar as identidades visíveis ou efetivas em determinado contexto.

## Reconhecimento da Identidade Root

A conta tradicionalmente chamada `root` possui UID 0. O UID 0 recebe tratamento especial de muitos mecanismos de permissão do Linux e carrega amplo poder administrativo. O Linux moderno também pode dividir privilégios por capabilities, namespaces, controles de acesso obrigatórios e confinamento de serviços; portanto, “poder ilimitado em todos os contextos” é uma simplificação excessiva.

O trabalho cotidiano deve usar uma conta sem privilégios. A autoridade administrativa aumenta o impacto de erros de caminho, comandos não confiáveis e software comprometido.

## Uso de sudo sob uma Política

`sudo` pergunta à política configurada se o usuário chamador pode executar um comando como uma identidade de destino. O destino padrão costuma ser root, mas uma política ou `-u USER` pode escolher outra conta. As solicitações de autenticação e o registro também dependem da configuração.

Liste os comandos que a conta atual tem permissão para executar:

```bash
$ sudo -l
```

Use um comando administrativo permitido somente quando a tarefa exigir e você compreender seus efeitos. Não use `sudo` apenas para silenciar um erro de permissão nem exiba bancos de hashes de senhas, como `/etc/shadow`, como exercício casual.

Para praticar a administração de contas e grupos em um ambiente controlado, experimente estes laboratórios:

1. **[Gerenciamento de Contas Linux com useradd, usermod e userdel](https://labex.io/labs/comptia-manage-linux-user-accounts-with-useradd-usermod-and-userdel-590837)** — Pratique todo o ciclo de administração de usuários, da criação e proteção à modificação e remoção.
2. **[Gerenciamento de Grupos Linux com groupadd, usermod e groupdel](https://labex.io/labs/comptia-manage-linux-groups-with-groupadd-usermod-and-groupdel-590836)** — Obtenha experiência prática com os principais utilitários de linha de comando para administrar grupos, inclusive a criação de grupos, a modificação das associações de usuários e a remoção de grupos.
3. **[Configuração de Contas e Privilégios sudo no Linux](https://labex.io/labs/comptia-configure-user-accounts-and-sudo-privileges-in-linux-590856)** — Aprenda técnicas essenciais para gerenciar contas de usuários e privilégios `sudo`, inclusive a concessão de permissões administrativas, a fim de melhorar a segurança de um sistema Linux.