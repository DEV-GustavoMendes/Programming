# Repositórios de Pacotes

Aprenda como os repositórios publicam índices de pacotes assinados e como o APT encontra as fontes configuradas em sistemas da família Debian.

Um repositório de pacotes publica pacotes junto com índices e metadados de versão. Um gerenciador de pacotes baixa esses índices, seleciona versões compatíveis com a distribuição e a arquitetura configuradas, verifica a autenticação do repositório e obtém os arquivos de pacote necessários.

## Metadados do Repositório e Catálogos Locais

Um repositório é mais do que um diretório de arquivos compactados. Seus metadados descrevem os nomes, as versões e as arquiteturas dos pacotes disponíveis, além de somas de verificação, dependências e seções do repositório. O cliente mantém um catálogo local em cache para pesquisar e resolver pacotes sem precisar baixar todos os arquivos primeiro.

Em um sistema da família Debian, atualize os metadados das fontes configuradas com:

```bash
$ sudo apt update
```

Esse comando atualiza os índices de pacotes locais; por si só, ele não instala todas as atualizações disponíveis. Examine as fontes e os erros de autenticação informados, em vez de ignorar entradas que falharam.

## Configuração de Fontes do APT

O APT lê as fontes configuradas em:

- `/etc/apt/sources.list`
- arquivos terminados em `.list` ou `.sources` dentro de `/etc/apt/sources.list.d/`

A extensão `.list` usa o formato tradicional de uma linha. A extensão `.sources` usa blocos no estilo deb822, recomendados pela documentação atual do APT para novas configurações. Uma distribuição pode colocar suas fontes padrão em qualquer um desses locais; portanto, `/etc/apt/sources.list` não necessariamente contém a configuração completa ou principal.

Uma fonte no estilo deb822 pode se parecer com:

```text
Types: deb
URIs: https://deb.example.invalid/repository
Suites: stable
Components: main
Signed-By: /etc/apt/keyrings/example.gpg
```

Esse exemplo serve apenas para ilustrar a sintaxe; o domínio reservado `.invalid` não corresponde a um repositório utilizável.

## Autenticação de Repositórios

O APT verifica os metadados de versão assinados do repositório e, em seguida, confere os arquivos de pacote baixados usando as somas de verificação autenticadas nesses metadados. O campo `Signed-By` pode restringir uma fonte a um chaveiro específico, em vez de confiar em todas as chaves configuradas globalmente para esse repositório.

Uma assinatura válida comprova que os metadados vieram do detentor de uma chave de assinatura aceita e não foram modificados sem que isso fosse detectado. Ela não comprova que o software do publicador esteja livre de defeitos, não seja malicioso ou seja adequado ao sistema. Confirme a impressão digital da chave e as instruções da fonte por meio de um canal confiável independente.

## Adição Cuidadosa de Fontes de Terceiros

Um repositório pode instalar pacotes e scripts de ciclo de vida com privilégios do sistema; portanto, adicioná-lo amplia o limite de confiança de software do sistema. Antes de fazer isso:

1. Prefira o repositório da distribuição quando ele atender à necessidade.
2. Confirme o publicador, a versão compatível, a arquitetura e a impressão digital da chave de assinatura.
3. Use um arquivo de fonte dedicado e um chaveiro com escopo restrito.
4. Examine os nomes dos pacotes e as alterações de dependências antes da instalação.
5. Documente como desativar a fonte e migrar ou remover seus pacotes.

Não copie instruções obsoletas que desativem a verificação de assinaturas nem envie um script remoto não auditado diretamente para um shell privilegiado.

Pratique a instalação por repositórios em [Instalação de Software no Linux](https://labex.io/labs/linux-software-installation-on-linux-18005) ou compare o fluxo de trabalho da família Red Hat em [Consultar e Atualizar Pacotes com o YUM](https://labex.io/labs/rhel-query-and-update-packages-with-yum-in-linux-590869). Para consultar a sintaxe exata do APT, use o manual local `sources.list(5)`.

