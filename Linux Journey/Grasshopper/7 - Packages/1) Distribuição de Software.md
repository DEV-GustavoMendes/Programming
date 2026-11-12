# Distribuição de Software

Aprenda como projetos upstream, mantenedores, pacotes e formatos formam a cadeia de suprimentos de software Linux.

Software Linux é normalmente entregue como pacotes gerenciados por ferramentas da distribuição. Um pacote agrupa arquivos instaláveis com metadados para rastrear versões, dependências, propriedade, checksums e ações de ciclo de vida.

## O que um pacote contém

Um pacote binário pode conter executáveis, bibliotecas, documentação, configuração padrão, definições de serviços e outros recursos. Também inclui metadados como:

- nome e versão
- arquitetura e contexto da distribuição
- dependências e conflitos declarados
- listas de arquivos e integridade
- scripts ou triggers de ciclo de vida

Nem todo pacote é aplicativo interativo. Ele pode fornecer biblioteca, componente do kernel, dados de linguagem, fontes, símbolos de debug ou metadados que dependem de outros pacotes.

## O que um pacote contém

Um pacote binário pode conter executáveis, bibliotecas, documentação, configuração padrão, definições de serviços e outros recursos. Também inclui metadados como:

- nome e versão
- arquitetura e contexto da distribuição
- dependências e conflitos declarados
- listas de arquivos e integridade
- scripts ou triggers de ciclo de vida

Nem todo pacote é aplicativo interativo. Ele pode fornecer biblioteca, componente do kernel, dados de linguagem, fontes, símbolos de debug ou metadados que dependem de outros pacotes.

## Formatos nativos comuns

Dois formatos amplamente usados:

- `.deb`, usado por Debian e derivados, como Ubuntu e Linux Mint
- `.rpm`, usado por Fedora, Red Hat Enterprise Linux e sistemas relacionados

Há outros formatos nativos e multiplataforma. Uma extensão correspondente não garante compatibilidade: arquitetura, versão, bibliotecas, políticas, assinaturas e dependências também importam.

## Por que a distribuição gerenciada importa

O gerenciador registra o estado e coordena mudanças. Repositórios confiáveis normalmente oferecem resolução consistente, verificação de assinaturas, atualizações de segurança e remoção limpa. Binário copiado ou instalação da fonte pode ser apropriado, mas não entra automaticamente nesse ciclo.

A confiança depende da configuração e das chaves. Uma assinatura válida associa o pacote a uma chave confiável; não prova que software arbitrário é seguro. Prefira repositórios da distribuição e avalie fontes externas antes de lhes dar privilégio de instalação.

Use o laboratório [Gerenciando Pacotes com RPM no Linux](https://labex.io/labs/rhel-managing-packages-with-rpm-in-linux-590868) para inspecionar metadados e integridade de pacotes, ou o laboratório [Compilar Software a Partir do Código-Fonte no Linux](https://labex.io/labs/comptia-build-software-from-source-code-in-linux-590853) para comparar um fluxo de código-fonte com pacotes gerenciados.