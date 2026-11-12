# /etc/shadow

Aprenda como os registros shadow locais representam hashes de senha e políticas de expiração sem expor dados confidenciais.

`/etc/shadow` armazena campos protegidos de hashes de senhas locais e de expiração de senhas. Separar esses valores do banco de dados `/etc/passwd`, geralmente legível, reduz a exposição a ataques de tentativa de descoberta de senhas offline.

## Proteção dos Dados do Shadow

As senhas não são armazenadas de forma reversível, “criptografadas” para exibição posterior. Uma entrada de senha local normalmente contém um hash unidirecional da senha, codificado com um identificador de algoritmo, salt e parâmetros. Um invasor que obtém hashes pode testar senhas candidatas offline, portanto o banco de dados deve permanecer restrito.

Os detalhes exatos de propriedade e permissão variam, mas o acesso costuma ser limitado ao root e a componentes do sistema com autorização restrita. Não imprima, copie, registre nem compartilhe o conteúdo do shadow apenas para inspecionar o estado de uma conta.

## Leitura do Formato de Nove Campos

Um registro shadow local contém nove campos separados por dois-pontos. Um registro esquemático se parece com este, com o hash omitido deliberadamente:

```text
alice:<password-field>:20000:0:90:7:14:20500:
```

Os campos são:

1. **Nome de login**.
2. **Hash da senha ou marcador especial de senha**.
3. **Última alteração da senha**, em dias desde 1970-01-01; em ferramentas comuns, `0` solicita uma alteração no próximo login autenticado por senha.
4. **Idade mínima da senha**, em dias.
5. **Idade máxima da senha**, em dias.
6. **Período de aviso** antes da expiração da senha, em dias.
7. **Período de inatividade** após a expiração da senha, em dias.
8. **Data de expiração da conta**, em dias desde 1970-01-01.
9. **Campo reservado**.

Campos vazios e valores numéricos especiais têm significados definidos que podem variar conforme o campo e a ferramenta. Use comandos de gerenciamento de contas em vez de editar os valores visualmente.

## Interpretação Cuidadosa do Campo de Senha

Um hash válido no campo 2 permite a verificação da senha Unix local. Um valor iniciado por `!` normalmente bloqueia esse hash de senha, enquanto `*` ou outro marcador de hash inválido impede a verificação bem-sucedida da senha por esse campo. Um valor vazio é sensível à segurança e pode permitir um comportamento sem senha, dependendo da política do PAM.

Esses marcadores descrevem o caminho da senha local, não todos os métodos de autenticação possíveis. Chaves públicas SSH, certificados, tokens e credenciais específicas de aplicações podem continuar disponíveis se não forem restringidos separadamente. A expiração da conta no campo 8 também é distinta do bloqueio da senha.

## Distinção entre Datas de Senha e de Conta

Os campos 3 a 7 dizem respeito à expiração da senha: quando ela foi alterada pela última vez, quando outra alteração é permitida, quando expira, quando começam os avisos e por quanto tempo após a expiração o login por senha permanece disponível. O campo 8 expira a conta em um dia absoluto, independentemente da idade da senha.

Por exemplo, uma idade máxima de senha de 90 dias não é o mesmo que uma data de expiração da conta. A primeira se desloca em relação à última alteração da senha; a segunda é uma data fixa até que um administrador a modifique.

## Inspeção e Alteração da Política por Ferramentas

Os administradores devem consultar apenas as informações necessárias para a tarefa:

```bash
$ sudo passwd -S alice
$ sudo chage -l alice
```

`passwd -S` resume o estado da senha local, enquanto `chage -l` lista as informações de expiração em um formato legível. Os formatos de saída e os requisitos de autorização podem variar conforme a distribuição.

Use `passwd`, `chage`, `usermod` e ferramentas de contas relacionadas para realizar alterações. Se o reparo manual do banco de dados shadow local for inevitável, `vipw -s` oferece bloqueio; valide os bancos de dados de contas com `pwck`. Mantenha uma sessão de recuperação antes de alterar a autenticação remotamente.

O PAM e o NSS podem integrar fontes de autenticação e identidade além dos arquivos shadow locais. Por isso, uma conta do sistema pode não ter um registro shadow local ou pode se autenticar por serviços adicionais.

Para praticar o estado de contas e as políticas de expiração em um ambiente controlado, experimente estes laboratórios práticos:

1. **[Gerenciamento de Contas de Usuário Linux com useradd, usermod e userdel](https://labex.io/labs/comptia-manage-linux-user-accounts-with-useradd-usermod-and-userdel-590837)** — Pratique todo o ciclo de administração de usuários, desde a criação e proteção de novas contas com `useradd` e `passwd` até sua modificação e exclusão.
2. **[Configuração de Contas de Usuário e Privilégios sudo no Linux](https://labex.io/labs/comptia-configure-user-accounts-and-sudo-privileges-in-linux-590856)** — Aprenda técnicas essenciais de gerenciamento de contas de usuário e privilégios sudo, incluindo a aplicação de políticas de senha e a proteção das contas.