# yum e apt

Aprenda os fluxos de trabalho do APT e do DNF para inspecionar, instalar, remover e atualizar pacotes por meio de repositórios.

Gerenciadores de pacotes que conhecem repositórios obtêm metadados, resolvem dependências, verificam conteúdo autenticado e coordenam transações. Os sistemas da família Debian normalmente usam APT. As versões atuais do Fedora e do Red Hat Enterprise Linux usam DNF; no RHEL atual, o comando `yum` continua disponível como um alias de compatibilidade para o DNF, enquanto sistemas mais antigos usavam a implementação YUM original.

Sempre siga a documentação da distribuição e da versão instaladas, em vez de presumir que um conjunto de comandos se aplica a todos os sistemas.

## Atualização e Inspeção de Metadados

O APT separa a atualização dos metadados da atualização dos pacotes:

```bash
Debian family: $ sudo apt update
```

Pesquise e inspecione antes de instalar:

```bash
Debian family: $ apt search package-name
Debian family: $ apt show package-name
RPM family:    $ dnf search package-name
RPM family:    $ dnf info package-name
```

A configuração dos repositórios determina o que esses comandos conseguem encontrar. Leia atentamente os nomes das fontes, as arquiteturas, as versões e os erros de assinatura.

## Instalação de Pacotes

Instale pelo nome do pacote no repositório com:

```bash
Debian family: $ sudo apt install package-name
RPM family:    $ sudo dnf install package-name
```

O gerenciador propõe as dependências e quaisquer conflitos ou substituições. Não confirme automaticamente antes de examinar a origem, a versão e a arquitetura do pacote, o tamanho do download, a alteração no espaço em disco, as remoções e as novas dependências que serão instaladas.

## Remoção de Pacotes

Solicite a remoção com:

```bash
Debian family: $ sudo apt remove package-name
RPM family:    $ sudo dnf remove package-name
```

A remoção pode afetar pacotes dependentes ou deixar dependências e configurações que deixaram de ser usadas. Examine a transação proposta, diferencie a semântica de remoção da de expurgo nos sistemas da família Debian e preserve os dados do aplicativo de acordo com seu próprio procedimento de backup e retenção. A remoção de um pacote não garante a exclusão dos dados criados pelo usuário.

## Aplicação de Atualizações

Em um sistema com APT, atualize os metadados e depois examine as atualizações como etapas separadas, ambas bem-sucedidas:

```bash
$ sudo apt update
$ apt list --upgradable
$ sudo apt upgrade
```

Em um sistema com DNF, inspecione e aplique as atualizações disponíveis usando o fluxo de trabalho documentado localmente:

```bash
$ dnf check-update
$ sudo dnf upgrade
```

Um comando de atualização pode alterar bibliotecas essenciais, serviços, kernels e dependências. Use backups, políticas de manutenção, notas de versão e planejamento de reinicializações de serviços ou do sistema adequados ao ambiente. Verifique a semântica do status de saída: por exemplo, algumas operações de “verificação de atualizações” usam um status diferente de zero para informar que há atualizações disponíveis, e não que ocorreu uma falha de execução.

## Escolha entre `dnf` e `yum`

Use `dnf` na documentação atual do Fedora e do RHEL. Em um sistema RHEL recente, o comando `yum` pode invocar o comportamento de compatibilidade do DNF, mas scripts não devem deduzir a implementação apenas pelo nome do executável. Em máquinas legadas, verifique a versão instalada e a sintaxe compatível antes de adaptar instruções.

Pratique o APT em [Instalação e Remoção de Pacotes](https://labex.io/labs/linux-installing-and-removing-packages-385380) e os conceitos da família DNF/YUM em [Consulta e Atualização de Pacotes com YUM](https://labex.io/labs/rhel-query-and-update-packages-with-yum-in-linux-590869).