# Guia de comandos Git – Resumo completo

O Git é um sistema de controle de versão utilizado para acompanhar alterações em projetos, organizar o desenvolvimento e permitir que várias pessoas trabalhem no mesmo código.

O GitHub é uma plataforma online que permite armazenar e compartilhar repositórios Git.

A seguir, você encontrará um resumo dos principais comandos, desde a configuração inicial até o envio de projetos para o GitHub.

# 1. Configuração inicial

Antes de começar a utilizar o Git, configure seu nome e e-mail. Essas informações serão utilizadas para identificar seus commits.

Configurar usuário:

```
git config --global user.name "Seu Nome"
```

Configurar e-mail:

```
git config --global user.email "seu@email.com"
```

Verificar configurações:

```
git config --list
```

Definir editor padrão:

```
git config --global core.editor "code --wait"
```

# 2. Criando um repositório

Um repositório é o local onde o Git acompanha as alterações do projeto.

Iniciar um repositório:

```
git init
```

Esse comando deve ser executado dentro da pasta do projeto.

Clonar um repositório existente:

```
git clone URL_DO_REPOSITORIO
```

Exemplo:

```
git clone https://github.com/usuario/projeto.git
```

Entrar na pasta do projeto:

```
cd projeto
```

# 3. Verificando alterações

Esses comandos permitem acompanhar o estado atual do projeto.

|Comando|Função|
|---|---|
|`git status`|Mostra arquivos modificados e novos arquivos.|
|`git diff`|Mostra as diferenças ainda não adicionadas ao stage.|
|`git diff --staged`|Mostra as diferenças preparadas para commit.|
|`git log`|Mostra o histórico de commits.|
|`git log --oneline`|Mostra o histórico resumido.|
|`git show`|Mostra detalhes de um commit.|

# 4. Adicionando arquivos (Add)

Antes de realizar um commit, é necessário adicionar os arquivos à área de preparação (Staging Area).

Adicionar um arquivo específico:

```
git add arquivo.java
```

Adicionar todos os arquivos modificados:

```
git add .
```

Adicionar todos os arquivos, incluindo alterações e exclusões:

```
git add -A
```

Remover um arquivo da área de preparação:

```
git restore --staged arquivo.java
```

# 5. Realizando commits

O commit registra um conjunto de alterações no histórico do projeto.

Criar um commit:

```
git commit -m "Mensagem do commit"
```

Exemplo:

```
git commit -m "Adiciona calculadora em Java"
```

Adicionar arquivos e realizar commit em arquivos já rastreados:

```
git commit -am "Corrige operações matemáticas"
```

Atenção: `-am` não adiciona arquivos novos que ainda não são rastreados pelo Git.

Alterar a mensagem do último commit:

```
git commit --amend -m "Nova mensagem"
```

# 6. Enviando arquivos para o GitHub (Push)

O comando `push` envia os commits locais para um repositório remoto.

Verificar os repositórios remotos:

```
git remote -v
```

Adicionar um repositório remoto:

```
git remote add origin URL_DO_REPOSITORIO
```

Exemplo:

```
git remote add origin git@github.com:usuario/projeto.git
```

Definir a branch principal como main:

```
git branch -M main
```

Enviar os arquivos pela primeira vez:

```
git push -u origin main
```

Enviar alterações posteriormente:

```
git push
```

# 7. Atualizando o projeto (Pull e Fetch)

Esses comandos permitem receber atualizações de um repositório remoto.

|Comando|Função|
|---|---|
|`git fetch`|Busca atualizações sem incorporá-las à branch atual.|
|`git pull`|Busca e integra atualizações do repositório remoto.|
|`git pull origin main`|Atualiza a branch atual usando a main remota.|

Exemplo:

```
git pull origin main
```

É recomendado atualizar seu projeto antes de começar a trabalhar quando outras pessoas também estão modificando o repositório.

# 8. Trabalhando com branches

Branches são ramificações do projeto que permitem desenvolver funcionalidades separadamente sem modificar diretamente a branch principal.

[Branching & Merging | GitHub | QuickLearnPro](https://images.openai.com/static-rsc-4/PQBuwj78zYyenkTzOP8tz4t6w2EMKCzI-6IvsOdUG-sw7-rEYFaq2xA3wS6ULn8WOsvT3gM3mqY0Q6jEx1XA7svKbLYQJL1EtDGGUhRN6ze2Ow4bOrz2yLIVQ-ltfCEoJ_m8LDE0lt9rH8FmcF7buq4N5i3Mq224X8mMBWtYsIg?purpose=inline)

[quicklearnpro.com](https://quicklearnpro.com/domains/developer-tools/github/branching-and-merging?utm_source=chatgpt.com)

|Comando|Função|
|---|---|
|`git branch`|Lista as branches.|
|`git branch nome`|Cria uma nova branch.|
|`git switch nome`|Alterna para outra branch.|
|`git switch -c nome`|Cria e acessa uma nova branch.|
|`git branch -d nome`|Exclui uma branch já integrada.|
|`git branch -D nome`|Força a exclusão de uma branch.|
|`git merge nome`|Incorpora outra branch à atual.|

Exemplo prático:

Criar uma branch para desenvolver uma nova funcionalidade:

```
git switch -c nova-funcionalidade
```

Depois de desenvolver e realizar os commits, voltar para a branch principal:

```
git switch main
```

Incorporar as alterações:

```
git merge nova-funcionalidade
```

# 9. Desfazendo alterações

O Git possui comandos para recuperar arquivos e desfazer operações.

|Comando|Função|
|---|---|
|`git restore arquivo`|Descarta alterações locais não preparadas.|
|`git restore --staged arquivo`|Retira arquivo do stage.|
|`git reset --soft HEAD~1`|Desfaz o último commit, mantendo alterações preparadas.|
|`git reset --mixed HEAD~1`|Desfaz o último commit, mantendo alterações nos arquivos, mas retirando-as do stage.|
|`git reset --hard HEAD~1`|Remove o último commit e descarta suas alterações locais.|
|`git revert ID`|Cria um novo commit que desfaz outro commit.|

Cuidado

O comando `git reset --hard` pode apagar alterações permanentemente. Utilize somente quando tiver certeza.

# 10. Trabalhando com arquivos

|Comando|Função|
|---|---|
|`git rm arquivo`|Remove arquivo e registra a remoção.|
|`git mv antigo novo`|Renomeia ou move um arquivo.|
|`git clean -n`|Mostra arquivos não rastreados que seriam removidos.|
|`git clean -f`|Remove arquivos não rastreados.|
|`git rm --cached arquivo`|Remove arquivo do rastreamento, mantendo-o localmente.|

# 11. Arquivo .gitignore

O `.gitignore` serve para informar ao Git quais arquivos e pastas não devem ser rastreados.

Crie um arquivo chamado:

```
.gitignore
```

Exemplo para projetos Java:

```
# Arquivos compilados
*.class

# Pastas de compilação
bin/
build/
target/

# Configurações de IDE
.idea/
.vscode/

# Arquivos do sistema
.DS_Store
Thumbs.db

# Arquivos temporários
*.tmp
```

Exemplo para projetos Python:

```
__pycache__/
*.pyc
venv/
.env
```

Atenção: o `.gitignore` não deixa de rastrear arquivos que já foram adicionados ao repositório anteriormente.

# 12. Comandos adicionais importantes

|Comando|Função|
|---|---|
|`git tag`|Lista as tags.|
|`git tag v1.0`|Cria uma tag.|
|`git push origin v1.0`|Envia uma tag ao remoto.|
|`git stash`|Guarda temporariamente alterações.|
|`git stash pop`|Recupera as alterações guardadas.|
|`git stash list`|Lista alterações guardadas.|
|`git blame arquivo`|Mostra quem modificou cada linha.|
|`git remote remove origin`|Remove um repositório remoto.|
|`git help`|Abre a ajuda do Git.|

# 13. Fluxo de trabalho completo

Este é o fluxo mais comum para trabalhar com Git e GitHub no dia a dia.

## Fluxo básico do Git

1. Working Directory

Você cria e modifica os arquivos do projeto.

git add

2. Staging Area

Você seleciona quais alterações serão registradas.

git commit

3. Local Repository

As alterações ficam registradas no histórico local.

git push

4. Remote Repository (GitHub)

Os commits são enviados para o repositório online.

## Exemplo prático: enviando um projeto para o GitHub

Imagine que você acabou de criar um projeto Java no seu computador e deseja enviá-lo para um repositório vazio no GitHub.

Execute os seguintes comandos dentro da pasta do projeto:

1. Inicializar o Git

```
git init
```

2. Adicionar todos os arquivos

```
git add .
```

3. Criar o primeiro commit

```
git commit -m "Primeiro commit"
```

4. Conectar ao GitHub

```
git remote add origin git@github.com:usuario/projeto.git
```

5. Definir a branch principal

```
git branch -M main
```

6. Enviar para o GitHub

```
git push -u origin main
```

Pronto! Seu projeto estará disponível no GitHub.

# 14. Resumo rápido para consultar

Estes são os comandos que você provavelmente utilizará com maior frequência:

|Comando|Para lembrar|
|---|---|
|`git init`|Iniciar projeto|
|`git clone`|Baixar projeto|
|`git status`|Verificar alterações|
|`git add .`|Adicionar arquivos|
|`git commit -m ""`|Registrar alterações|
|`git push`|Enviar para GitHub|
|`git pull`|Atualizar projeto|
|`git fetch`|Buscar atualizações|
|`git branch`|Ver branches|
|`git switch`|Trocar branch|
|`git merge`|Unir branches|
|`git log`|Ver histórico|
|`git diff`|Comparar alterações|
|`git restore`|Descartar alterações|
|`git stash`|Guardar alterações temporariamente|

## 15. Exercício para praticar

Experimente executar este pequeno exercício para fixar os comandos:

Checklist de prática

0/11 concluídos

Criar uma pasta chamada projeto-git.

Entrar na pasta pelo terminal.

Executar git init.

Criar um arquivo README.md.

Adicionar o arquivo com git add .

Criar um commit chamado Primeiro commit.

Criar uma branch chamada desenvolvimento.

Criar um arquivo chamado teste.txt.

Adicionar e realizar um commit nessa branch.

Voltar para main e incorporar a branch desenvolvimento.

Verificar o histórico usando git log --oneline.

Reiniciar exercício

Minha recomendação: aprenda primeiro `init`, `status`, `add`, `commit`, `push`, `pull` e `branch`. Esses comandos formam a base para trabalhar com controle de versão em praticamente qualquer projeto de programação.