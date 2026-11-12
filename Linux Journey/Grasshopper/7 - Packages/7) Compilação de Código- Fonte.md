# Compilação de Código-fonte

Aprenda a verificar, configurar, compilar, testar, preparar e acompanhar software compilado a partir do código-fonte.

Compilar a partir do código-fonte pode fornecer uma versão ou um recurso indisponível nos repositórios configurados, mas transfere da distribuição para você o trabalho de integração, atualização e confiança. Prefira um pacote compatível da distribuição quando ele atender à necessidade.

## Verificação e Leitura Antes da Compilação

Obtenha o código-fonte por um canal de versões autenticado do projeto original. Verifique sua assinatura ou soma de verificação por um caminho confiável e inspecione o arquivo antes de extraí-lo em um diretório de preparação sem privilégios. Leia arquivos como `README`, `INSTALL`, `SECURITY` e a documentação de compilação do projeto.

As instruções de compilação são código executável. Um script `configure`, uma definição de compilação, um teste ou um plugin do compilador pode executar comandos arbitrários como seu usuário. Não compile código-fonte não confiável e não execute a compilação com `sudo`.

## Instalação dos Requisitos de Compilação

Em um sistema de desenvolvimento da família Debian, um ponto de partida comum é:

```bash
$ sudo apt install build-essential
```

Esse comando instala um compilador básico e ferramentas de compilação, mas não todas as dependências exigidas por todos os projetos. Um projeto também pode precisar de ambientes de execução de linguagens, geradores, ferramentas do sistema de compilação, cabeçalhos de desenvolvimento ou versões exatas de bibliotecas. Instale os requisitos a partir de repositórios confiáveis e diferencie as dependências de compilação das dependências de execução.

## Configuração e Compilação

Um projeto tradicional no estilo Autoconf usa:

```bash
$ ./configure --prefix=/usr/local
$ make
```

`configure` verifica o ambiente e gera arquivos de compilação de acordo com as opções selecionadas. `make` lê regras de dependências e comandos, normalmente em um `Makefile`, e cria os alvos solicitados.

Essa sequência não é universal. Os projetos podem usar CMake, Meson, Ninja, ferramentas específicas de uma linguagem ou scripts personalizados. Siga a documentação da versão exata, em vez de executar `./configure` apenas por familiaridade. Quando o sistema de compilação permitir, um diretório de compilação separado da árvore de código pode manter os arquivos gerados isolados.

## Testes Antes da Instalação

Execute o alvo de testes documentado pelo projeto, por exemplo:

```bash
$ make check
```

O alvo real pode ser `test`, `check` ou um comando separado. Investigue as falhas em vez de instalar um resultado não testado. Os testes podem exigir acesso à rede, serviços, hardware especial ou isolamento; examine-os antes da execução, assim como faria com qualquer outro código de compilação.

## Preparação e Acompanhamento da Instalação

`sudo make install` pode copiar arquivos diretamente para prefixos do sistema sem registrá-los no banco de dados de pacotes nativo. Alvos de desinstalação são opcionais e podem ser incompletos, enquanto atualizações posteriores podem sobrescrever arquivos ou deixá-los órfãos.

Prefira uma destas abordagens controladas:

- crie um pacote nativo oficial usando as ferramentas de empacotamento da distribuição
- instale sob um prefixo claramente separado, como `/usr/local`, quando a política permitir
- prepare os arquivos em uma raiz temporária de empacotamento com um mecanismo compatível, como `DESTDIR`
- use um prefixo de usuário sem privilégios, um ambiente isolado ou um contêiner, quando apropriado

`checkinstall` pode criar um pacote simples para alguns fluxos com `make install`, mas não é universal e não substitui uma receita de pacote com qualidade de distribuição devidamente revisada. Nunca o trate como uma regra obrigatória. Antes de qualquer cópia com privilégios, inspecione a lista de arquivos preparados, seus proprietários, permissões e caminhos, além do plano de desinstalação ou atualização.

Use [Compilar Software a Partir do Código-fonte no Linux](https://labex.io/labs/comptia-build-software-from-source-code-in-linux-590853) em um ambiente descartável para praticar o fluxo sem misturar arquivos experimentais em um sistema de produção.