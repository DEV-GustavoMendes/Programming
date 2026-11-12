# rpm e dpkg

Aprenda como `dpkg` e `rpm` inspecionam e modificam seus bancos de dados de pacotes nativos e arquivos locais.

`dpkg` é a ferramenta de pacotes de baixo nível dos sistemas da família Debian, enquanto `rpm` desempenha função semelhante nos sistemas da família RPM. Elas desempacotam arquivos nativos, executam ações do ciclo de vida dos pacotes e atualizam os bancos de dados de pacotes instalados. Ferramentas que conhecem repositórios, como APT e DNF, utilizam esses mecanismos de baixo nível.

## Inspeção de um Arquivo Antes da Instalação

Um arquivo de pacote não equivale a um único executável. Ele pode conter muitos arquivos de carga útil, metadados, tratamento de configurações e scripts privilegiados do ciclo de vida. Antes da instalação, inspecione sua origem, a assinatura ou o caminho de download autenticado, os metadados e o conteúdo.

```bash
Debian: $ dpkg-deb --info ./some-package.deb
Debian: $ dpkg-deb --contents ./some-package.deb
RPM:    $ rpm -qip ./some-package.rpm
RPM:    $ rpm -qlp ./some-package.rpm
```

O `p` nas formas de consulta do RPM exibidas significa “consultar um arquivo de pacote”, e não o banco de dados instalado. A saída da consulta ajuda a examinar um pacote, mas não comprova que seus scripts ou programas sejam seguros.

## Instalação de um Arquivo Local

A instalação direta de baixo nível usa:

```bash
Debian: $ sudo dpkg -i ./some-package.deb
RPM:    $ sudo rpm -U ./some-package.rpm
```

`dpkg -i` pode desempacotar e configurar o arquivo solicitado, mas não obtém dependências ausentes nos repositórios. De modo semelhante, o uso direto de `rpm` não oferece o fluxo de trabalho normal do resolvedor de repositórios. Em geral, um comando de nível mais alto é preferível para um arquivo local, pois pode resolver dependências usando as fontes configuradas:

```bash
Debian: $ sudo apt install ./some-package.deb
RPM:    $ sudo dnf install ./some-package.rpm
```

Examine a transação antes de confirmá-la. No APT, o prefixo `./` diferencia o caminho de um arquivo Debian local do nome de um pacote do repositório.

## Remoção de um Pacote Instalado

A remoção recebe o nome de um pacote instalado, não o nome do arquivo usado anteriormente:

```bash
Debian: $ sudo dpkg --remove package-name
RPM:    $ sudo rpm --erase package-name
```

No Debian, `--remove` normalmente preserva os arquivos de configuração classificados como conffiles; `--purge` também solicita a remoção deles, sujeita aos scripts do pacote e aos dados não gerenciados. Nenhum dos comandos garante a exclusão de dados criados pelo usuário. Em geral, `apt remove` ou `dnf remove` é uma opção melhor, pois pode avaliar os pacotes relacionados e apresentar a transação completa.

## Consulta do Estado Instalado

Liste os registros de pacotes instalados ou conhecidos com:

```bash
Debian: $ dpkg-query -l
RPM:    $ rpm -qa
```

Para uma inspeção direcionada, prefira um nome de pacote específico e um formato legível por máquina quando a confiabilidade de um script for importante. Os bancos de dados de pacotes descrevem o estado gerenciado, mas administradores locais ou aplicativos ainda podem modificar arquivos posteriormente; use os recursos de verificação quando precisar comparar os arquivos instalados com os metadados registrados.

Use [Gerenciamento de Pacotes com RPM](https://labex.io/labs/rhel-managing-packages-with-rpm-in-linux-590868) para praticar consultas de arquivos e verificações de integridade em um sistema isolado.