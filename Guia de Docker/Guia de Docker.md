# Guia completo de Docker — conceitos, comandos e exemplos práticos

O Docker é uma das ferramentas mais importantes no desenvolvimento de software moderno. Ele permite criar, executar, distribuir e gerenciar aplicações em ambientes isolados chamados containers.

Este material está organizado como uma apostila de estudos, começando pelos conceitos fundamentais e avançando até a utilização prática, com exemplos de comandos e projetos.

# 1. O que é Docker?

![[Pasted image 20261112132754.png]]

O Docker é uma plataforma de conteinerização que permite empacotar uma aplicação junto com suas dependências, bibliotecas, configurações e arquivos necessários para sua execução.

Isso resolve um problema muito comum:

> Na minha máquina funciona, mas na máquina de outra pessoa não funciona.

Com Docker, você consegue executar a mesma aplicação em diferentes ambientes, mantendo suas dependências e configurações padronizadas.

### Docker x Máquina Virtual

Embora sejam tecnologias relacionadas, possuem diferenças importantes.
![[Pasted image 20261112132828.png|440]]

![[Pasted image 20261112132848.png|435]]

Máquina Virtual (VM)

- Virtualiza um computador inteiro.
    
- Possui sistema operacional próprio.
    
- Consome mais recursos.
    
- Inicialização geralmente mais demorada.
    

[Enterprise IT Glossary | HPE | HPE](https://images.openai.com/static-rsc-4/GAqZ7e9DOHGRX2_due7-bHJ2pEX6trJr0SZc9HkhX0Ca5r6QM0hZ9V7haBhyqjXTKa8o9THCjGv9iiKwnmu3l_tQrMaPtHLOfhehEFLLpRb8Y3N9pJNyd_6FPHU3pDhD-k1gJaItUCSZ6Voa14DibpubhrTm5jLXpiL5oP_kQs4?purpose=inline)

Container Docker

- Compartilha o kernel do sistema hospedeiro.
    
- Isola processos e recursos.
    
- Geralmente é mais leve.
    
- Inicialização rápida.
    

|Característica|Máquina virtual|Container|
|---|---|---|
|Sistema operacional|Completo|Compartilha o kernel|
|Inicialização|Mais lenta|Mais rápida|
|Consumo de recursos|Maior|Geralmente menor|
|Isolamento|Virtualização de hardware|Isolamento de processos|
|Portabilidade|Alta|Alta|

Importante: containers não são máquinas virtuais. Eles utilizam recursos do próprio sistema operacional para isolar aplicações.

# 2. Principais conceitos do Docker

Antes de começar a utilizar os comandos, é importante compreender os principais componentes.

## 2.1. Docker Engine

É o mecanismo responsável por executar e gerenciar containers.

Ele possui três componentes principais:

- Docker Daemon (`dockerd`): serviço que gerencia imagens, containers, redes e volumes.
    
- Docker CLI (`docker`): ferramenta de linha de comando utilizada pelo usuário.
    
- Docker API: interface que permite a comunicação entre a CLI e o daemon.
    

![[Pasted image 20261112132920.png]]
## 2.2. Image (Imagem)

Uma imagem é um modelo utilizado para criar containers.

Ela contém:

- Sistema de arquivos da aplicação.
    
- Bibliotecas e dependências.
    
- Arquivos de configuração.
    
- Instruções de inicialização.
    
- Ambiente necessário para executar o programa.
    

Exemplos:

- `ubuntu:24.04`
    
- `nginx:latest`
    
- `postgres:17`
    
- `python:3.13`
    
- `node:22`
    

Uma imagem não é a aplicação em execução. Ela é a base para criar um container.

## 2.3. Container

Um container é uma instância executável de uma imagem.

Por exemplo:

```
docker run nginx
```

Esse comando cria e inicia um container utilizando a imagem do Nginx.

Você pode criar vários containers utilizando a mesma imagem.

![[Pasted image 20261112132941.png]]

## 2.4. Dockerfile

É um arquivo de texto que contém instruções para construir uma imagem personalizada.

Exemplo:

```
FROM python:3.13

WORKDIR /app

COPY . .

RUN pip install flask

EXPOSE 5000

CMD ["python", "app.py"]
```

Cada instrução tem uma finalidade:

|Instrução|Finalidade|
|---|---|
|`FROM`|Define a imagem-base.|
|`WORKDIR`|Define o diretório de trabalho.|
|`COPY`|Copia arquivos para a imagem.|
|`ADD`|Copia arquivos e permite outras operações, como extração de arquivos compactados.|
|`RUN`|Executa comandos durante a construção.|
|`EXPOSE`|Documenta a porta utilizada pela aplicação.|
|`ENV`|Define variáveis de ambiente.|
|`CMD`|Define o comando padrão de inicialização.|
|`ENTRYPOINT`|Define o executável principal do container.|
|`ARG`|Define argumentos disponíveis durante o build.|
|`USER`|Define o usuário que executará os processos.|
|`VOLUME`|Declara um ponto de montagem de dados.|

## 2.5. Docker Hub e Registry

Um Registry é um serviço utilizado para armazenar e distribuir imagens Docker.

O mais conhecido é o Docker Hub.

![[Pasted image 20261112132958.png]]

Exemplos de imagens disponíveis:

- Ubuntu.
    
- Debian.
    
- Alpine.
    
- PostgreSQL.
    
- MySQL.
    
- Redis.
    
- Nginx.
    
- Python.
    
- Node.js.
    

Comando para baixar uma imagem:

```
docker pull ubuntu:24.04
```

## 2.6. Volumes

Containers são considerados descartáveis. Quando um container é removido, seus dados gravados apenas na camada gravável dele também são removidos.

Os volumes permitem persistir informações independentemente do ciclo de vida do container.

Exemplo:

```
docker volume create dados-postgres
```

Utilizando o volume:

```
docker run -d \
  --name meu-banco \
  -v dados-postgres:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=123456 \
  postgres:17
```

Mesmo removendo e recriando o container, os dados armazenados no volume podem continuar disponíveis.

## 2.7. Networks (Redes)

As redes permitem que containers se comuniquem entre si.

Por exemplo, uma aplicação pode conversar com um banco de dados sem precisar expor o banco diretamente à internet.

Tipos comuns:

|Rede|Descrição|
|---|---|
|`bridge`|Rede padrão para containers em um host Docker.|
|`host`|Compartilha a rede do host, sem isolamento de rede convencional.|
|`none`|Desabilita a conectividade de rede do container.|
|`overlay`|Conecta containers entre diferentes hosts Docker em ambientes compatíveis.|

Criando uma rede:

```
docker network create minha-rede
```

Executando containers nessa rede:

```
docker run -d \
  --name banco \
  --network minha-rede \
  postgres:17
```

```
docker run -d \
  --name aplicacao \
  --network minha-rede \
  nginx
```

Os containers podem se comunicar utilizando seus nomes como endereços DNS dentro da rede.

## 2.8. Docker Compose

O Docker Compose permite definir e executar aplicações compostas por vários containers utilizando um arquivo YAML.

Imagine um sistema que possui:

- Backend.
    
- Banco de dados.
    
- Frontend.
    
- Servidor web.
    

Em vez de executar cada container individualmente, você pode definir toda a infraestrutura em um único arquivo.

![[Pasted image 20261112133017.png]]

Exemplo de `compose.yaml`:

```
services:

  banco:
    image: postgres:17
    container_name: banco
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: 123456
      POSTGRES_DB: sistema
    volumes:
      - dados:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  backend:
    image: python:3.13
    container_name: backend
    working_dir: /app
    volumes:
      - ./backend:/app
    command: python app.py
    ports:
      - "5000:5000"
    depends_on:
      - banco

volumes:
  dados:
```

Para iniciar:

```
docker compose up -d
```

Para parar:

```
docker compose down
```

Para acompanhar os logs:

```
docker compose logs -f
```

# 3. Instalação do Docker

Como você utiliza Fedora, vou utilizar o Fedora como referência nos comandos de instalação. Os demais comandos deste guia são praticamente os mesmos em Linux, Windows e macOS, considerando que o Docker esteja disponível.

## 3.1. Instalar no Fedora

Para instalar o Docker Engine pelo repositório oficial:

```
sudo dnf -y install dnf-plugins-core
```

Adicionar o repositório:

```
sudo dnf-3 config-manager \
  addrepo \
  --from-repofile https://download.docker.com/linux/fedora/docker-ce.repo
```

Instalar os pacotes:

```
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Iniciar o serviço:

```
sudo systemctl enable --now docker
```

Verificar:

```
sudo systemctl status docker
```

Testar a instalação:

```
sudo docker run hello-world
```

## 3.2. Executar Docker sem sudo

Por padrão, o Docker pode exigir privilégios administrativos.

Para permitir a execução pelo usuário:

```
sudo usermod -aG docker $USER
```

Depois, encerre e inicie novamente a sessão ou execute:

```
newgrp docker
```

Teste:

```
docker run hello-world
```

**Atenção à segurança**

O grupo `docker` concede privilégios equivalentes aos de root em muitos cenários. Não adicione usuários não confiáveis a esse grupo.

# 4. Principais comandos Docker

Esta é a parte mais importante para trabalhar com Docker pelo terminal.

## 4.1. Informações gerais

|Comando|Função|
|---|---|
|`docker --version`|Mostra a versão instalada.|
|`docker version`|Mostra as versões do cliente e servidor.|
|`docker info`|Exibe informações do Docker Engine.|
|`docker help`|Mostra a ajuda geral.|
|`docker COMMAND --help`|Mostra a ajuda de um comando específico.|

Exemplo:

```
docker info
```

## 4.2. Comandos de imagens

|Comando|Função|
|---|---|
|`docker images`|Lista imagens locais.|
|`docker pull imagem`|Baixa uma imagem.|
|`docker build -t nome .`|Constrói uma imagem.|
|`docker rmi imagem`|Remove uma imagem.|
|`docker image inspect imagem`|Exibe detalhes da imagem.|
|`docker image history imagem`|Mostra o histórico de camadas.|
|`docker image prune`|Remove imagens não utilizadas.|

Exemplos:

```
docker pull nginx
```

```
docker images
```

```
docker rmi nginx
```

Construindo uma imagem:

```
docker build -t minha-aplicacao:1.0 .
```

## 4.3. Comandos de containers

|Comando|Função|
|---|---|
|`docker run`|Cria e executa um container.|
|`docker ps`|Lista containers em execução.|
|`docker ps -a`|Lista todos os containers.|
|`docker start`|Inicia um container parado.|
|`docker stop`|Para um container.|
|`docker restart`|Reinicia um container.|
|`docker rm`|Remove um container.|
|`docker rename`|Renomeia um container.|
|`docker pause`|Suspende os processos.|
|`docker unpause`|Retoma os processos.|
|`docker inspect`|Exibe informações detalhadas.|
|`docker stats`|Mostra consumo de recursos.|
|`docker top`|Mostra processos do container.|
|`docker logs`|Exibe os logs.|
|`docker attach`|Conecta ao processo principal.|
|`docker exec`|Executa um comando dentro do container.|

Exemplo de execução:

```
docker run -d --name servidor nginx
```

Verificar containers:

```
docker ps
```

Parar:

```
docker stop servidor
```

Iniciar novamente:

```
docker start servidor
```

Remover:

```
docker rm servidor
```

## 4.4. Parâmetros importantes do docker run

O comando `docker run` possui diversos parâmetros.

|Parâmetro|Função|
|---|---|
|`-d`|Executa em segundo plano.|
|`-it`|Mantém terminal interativo.|
|`--name`|Define o nome do container.|
|`-p`|Mapeia portas.|
|`-v`|Monta volumes ou diretórios.|
|`-e`|Define variáveis de ambiente.|
|`--rm`|Remove o container após sua finalização.|
|`--network`|Define a rede.|
|`--restart`|Define política de reinicialização.|
|`--memory`|Limita memória.|
|`--cpus`|Limita uso de CPU.|
|`--hostname`|Define o hostname.|
|`--user`|Define o usuário de execução.|
|`--read-only`|Torna o sistema de arquivos raiz somente leitura.|

Exemplo completo:

```
docker run -d \
  --name servidor-web \
  -p 8080:80 \
  --memory=512m \
  --cpus=1 \
  --restart unless-stopped \
  nginx
```

A aplicação estará disponível em:

```
http://localhost:8080
```

Entendendo o mapeamento de portas:

```
-p 8080:80
```

- `8080`: porta do computador (host).
    
- `80`: porta do container.
    

## 4.5. Executando comandos dentro de containers

O `docker exec` permite executar comandos dentro de um container em funcionamento.

Abrir um terminal Bash:

```
docker exec -it servidor-web bash
```

Se a imagem não possuir Bash:

```
docker exec -it servidor-web sh
```

Executar um comando diretamente:

```
docker exec servidor-web ls
```

Acessar um banco PostgreSQL:

```
docker exec -it banco psql -U admin
```

## 4.6. Logs e monitoramento

|Comando|Função|
|---|---|
|`docker logs container`|Exibe os logs.|
|`docker logs -f container`|Acompanha os logs em tempo real.|
|`docker logs --tail 100 container`|Mostra as últimas 100 linhas.|
|`docker stats`|Monitora CPU, RAM e rede.|
|`docker events`|Exibe eventos do Docker.|

Exemplo:

```
docker logs -f servidor-web
```

## 4.7. Comandos de volumes

|Comando|Função|
|---|---|
|`docker volume ls`|Lista volumes.|
|`docker volume create`|Cria um volume.|
|`docker volume inspect`|Exibe informações.|
|`docker volume rm`|Remove um volume.|
|`docker volume prune`|Remove volumes não utilizados.|

Exemplo:

```
docker volume create meus-dados
```

```
docker volume ls
```

## 4.8. Comandos de redes

|Comando|Função|
|---|---|
|`docker network ls`|Lista redes.|
|`docker network create`|Cria uma rede.|
|`docker network inspect`|Inspeciona uma rede.|
|`docker network connect`|Conecta um container.|
|`docker network disconnect`|Desconecta um container.|
|`docker network rm`|Remove uma rede.|
|`docker network prune`|Remove redes não utilizadas.|

Exemplo:

```
docker network create rede-projeto
```

## 4.9. Limpeza do Docker

O Docker pode acumular imagens, containers e volumes que não são mais utilizados.

|Comando|Função|
|---|---|
|`docker container prune`|Remove containers parados.|
|`docker image prune`|Remove imagens não utilizadas.|
|`docker volume prune`|Remove volumes não utilizados.|
|`docker network prune`|Remove redes não utilizadas.|
|`docker system df`|Mostra o espaço utilizado.|
|`docker system prune`|Limpa recursos não utilizados.|

Comando mais abrangente:

```
docker system prune -a
```

Cuidado

O comando acima pode remover imagens que você deseja manter em cache. Adicionar `--volumes` também pode remover volumes não utilizados, inclusive dados que você pretendia preservar. Verifique sempre antes de executar.

# 5. Construindo uma imagem com Dockerfile

Agora vamos entender como criar suas próprias imagens.

Imagine que você desenvolveu uma aplicação em Python e deseja executá-la dentro de um container.

## 5.1. Estrutura do projeto

Crie uma pasta:

```
meu-projeto/
│
├── Dockerfile
├── requirements.txt
└── app.py
```

Arquivo `app.py`:

```
from flask import Flaskapp = Flask(__name__)@app.route("/")def inicio():    return "Minha primeira aplicação Docker!"if __name__ == "__main__":    app.run(host="0.0.0.0", port=5000)
```

Arquivo `requirements.txt`:

```
flask
```

Arquivo `Dockerfile`:

```
FROM python:3.13-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

## 5.2. Construindo a imagem

Dentro da pasta do projeto, execute:

```
docker build -t minha-api:1.0 .
```

Explicando:

- `docker build`: constrói uma imagem.
    
- `-t`: define o nome e a versão.
    
- `minha-api:1.0`: nome e tag da imagem.
    
- `.`: indica o diretório atual como contexto de construção.
    

Verifique:

```
docker images
```

## 5.3. Executando a aplicação

```
docker run -d \
  --name minha-api \
  -p 5000:5000 \
  minha-api:1.0
```

Acesse:

```
http://localhost:5000
```

Resultado esperado

```
Minha primeira aplicação Docker!
```

## 5.4. Atualizando a aplicação

Se você modificar o código Python, será necessário reconstruir a imagem para incorporar as alterações:

```
docker build -t minha-api:2.0 .
```

Depois, remova o container antigo:

```
docker stop minha-api
docker rm minha-api
```

E execute a nova versão:

```
docker run -d \
  --name minha-api \
  -p 5000:5000 \
  minha-api:2.0
```

## 5.5. Arquivo .dockerignore

Assim como existe o `.gitignore` no Git, existe o `.dockerignore`.

Ele impede que arquivos desnecessários sejam enviados para o contexto de construção da imagem.

Exemplo:

```
.git
.gitignore
.env
venv/
__pycache__/
*.pyc
node_modules/
.idea/
.vscode/
```

Isso reduz o tamanho do contexto de build e evita copiar arquivos desnecessários ou sensíveis.

# 6. Casos práticos de utilização do Docker

Agora vamos ver situações reais nas quais o Docker é utilizado no desenvolvimento de software.

## Caso 1 — Executar um servidor web com Nginx

![[Pasted image 20261112133105.png]]

O Nginx é um servidor web muito utilizado para disponibilizar páginas e aplicações.

Baixe e execute:

```
docker run -d \
  --name meu-nginx \
  -p 8080:80 \
  nginx
```

Acesse:

```
http://localhost:8080
```

Visualizar os logs:

```
docker logs meu-nginx
```

Parar o servidor:

```
docker stop meu-nginx
```

Remover:

```
docker rm meu-nginx
```

Quando utilizar: hospedagem de páginas, servidores web, proxy reverso e distribuição de aplicações.

## Caso 2 — Criar um banco PostgreSQL

Imagine que você está desenvolvendo um sistema em Java, Python ou Rust e precisa de um banco de dados.

Em vez de instalar o PostgreSQL diretamente no sistema operacional, você pode utilizar Docker.

Executar:

```
docker run -d \
  --name meu-postgres \
  -e POSTGRES_USER=admin \
  -e POSTGRES_PASSWORD=senha123 \
  -e POSTGRES_DB=escola \
  -p 5432:5432 \
  -v postgres-dados:/var/lib/postgresql/data \
  postgres:17
```

Acessar o terminal do PostgreSQL:

```
docker exec -it meu-postgres psql -U admin -d escola
```

Criar uma tabela:

```
CREATE TABLE alunos (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100),
    idade INT
);
```

Inserir dados:

```
INSERT INTO alunos (nome, idade)
VALUES ('João', 22);
```

Consultar:

```
SELECT * FROM alunos;
```

Quando utilizar: desenvolvimento de sistemas que precisam de banco de dados, testes e ambientes de desenvolvimento.

Segurança

Em projetos reais, não utilize senhas simples nem deixe credenciais diretamente no arquivo de configuração versionado.

## Caso 3 — Executar MySQL

Criar um container:

```
docker run -d \
  --name meu-mysql \
  -e MYSQL_ROOT_PASSWORD=senha123 \
  -e MYSQL_DATABASE=empresa \
  -p 3306:3306 \
  mysql:8.4
```

Acessar:

```
docker exec -it meu-mysql mysql -u root -p
```

Verificar bancos:

```
SHOW DATABASES;
```

Criar tabela:

```
USE empresa;

CREATE TABLE funcionarios (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100),
    salario DECIMAL(10,2)
);
```

Quando utilizar: desenvolvimento de aplicações que utilizam MySQL como banco de dados.

## Caso 4 — Executar aplicações Java

Imagine que você desenvolveu uma aplicação Java 11.

Estrutura:

```
projeto-java/
│
├── Dockerfile
└── Main.java
```

Arquivo `Main.java`:

```
public class Main {

    public static void main(String[] args) {

        System.out.println("Aplicação Java no Docker!");

    }

}
```

Dockerfile:

```
FROM eclipse-temurin:11-jdk

WORKDIR /app

COPY Main.java .

RUN javac Main.java

CMD ["java", "Main"]
```

Construir:

```
docker build -t projeto-java .
```

Executar:

```
docker run --rm projeto-java
```

Resultado:

```
Aplicação Java no Docker!
```

Quando utilizar: padronização do ambiente Java, testes, execução de aplicações e distribuição de sistemas.

## Caso 5 — Executar Python sem instalar bibliotecas no computador

Imagine que você precisa testar um código Python que utiliza determinada biblioteca.

Você pode executar diretamente:

```
docker run --rm python:3.13 python -c "print('Olá Docker!')"
```

Ou executar um terminal interativo:

```
docker run -it --rm python:3.13 python
```

Agora você terá um interpretador Python dentro do container.

Exemplo:

```
print("Testando Python")
```

Para sair:

```
exit()
```

Quando utilizar: testes rápidos, experimentação, execução de scripts e ambientes temporários.

## Caso 6 — Desenvolver uma aplicação com frontend, backend e banco de dados

Este é um dos cenários mais importantes do Docker.

Imagine um sistema de gerenciamento de alunos:

[Docker For Dummies: Explained Simply](https://images.openai.com/static-rsc-4/Bicrkymhy8kZAMf1rj-Hga46y4mrEkQPS-uFL11y5qfKOfi_-mcggqT6bAXu238izb6CDMT6YAJE9znuzYRei4-YqV0y-zDNvTV3tTmpAGBtTqkOe1PtimpnG6Qm9DC8eQM2myZWkZ-Tm5pPz-MJEJfnl7Tg_6ELFPc7Sq_6qAk?purpose=inline)

[slothbytes.beehiiv.com](https://slothbytes.beehiiv.com/p/docker-for-dummies?utm_source=chatgpt.com)

A arquitetura:

|Componente|Tecnologia|Responsabilidade|
|---|---|---|
|Frontend|HTML, CSS, JavaScript|Interface do usuário.|
|Backend|Python Flask|Regras de negócio.|
|Banco de dados|PostgreSQL|Armazenamento.|

Você pode utilizar o Docker Compose para iniciar todos os serviços de uma só vez.

Estrutura:

```
sistema-alunos/
│
├── compose.yaml
│
└── backend/
    ├── Dockerfile
    ├── requirements.txt
    └── app.py
```

Arquivo `backend/requirements.txt`:

```
flask
psycopg2-binary
```

Arquivo `backend/app.py`:

```
from flask import Flaskimport osimport psycopg2app = Flask(__name__)@app.route("/")def inicio():    conexao = psycopg2.connect(        host=os.getenv("DB_HOST", "banco"),        database=os.getenv("DB_NAME", "escola"),        user=os.getenv("DB_USER", "admin"),        password=os.getenv("DB_PASSWORD")    )    cursor = conexao.cursor()    cursor.execute("SELECT version()")    resultado = cursor.fetchone()    cursor.close()    conexao.close()    return f"Conectado ao PostgreSQL: {resultado[0]}"if __name__ == "__main__":    app.run(host="0.0.0.0", port=5000)
```

Arquivo `backend/Dockerfile`:

```
FROM python:3.13-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

Arquivo `compose.yaml`:

```
services:

  banco:
    image: postgres:17
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: senha123
      POSTGRES_DB: escola
    volumes:
      - dados:/var/lib/postgresql/data

  backend:
    build: ./backend
    ports:
      - "5000:5000"
    environment:
      DB_HOST: banco
      DB_NAME: escola
      DB_USER: admin
      DB_PASSWORD: senha123
    depends_on:
      - banco

volumes:
  dados:
```

Executar o projeto:

```
docker compose up -d --build
```

Acessar:

```
http://localhost:5000
```

Visualizar os containers:

```
docker compose ps
```

Visualizar os logs:

```
docker compose logs -f
```

Parar os serviços:

```
docker compose down
```

O que aconteceu?

1. O Docker construiu a imagem do backend.
    
2. Baixou a imagem do PostgreSQL.
    
3. Criou os dois containers.
    
4. Criou uma rede para comunicação.
    
5. Criou um volume para persistir os dados.
    
6. Iniciou os serviços.
    

Observe que o backend acessa o banco utilizando o nome `banco`, e não `localhost`. Isso ocorre porque os serviços compartilham uma rede Docker.

Observação: `depends_on` controla a ordem de inicialização, mas não garante que o PostgreSQL já esteja pronto para aceitar conexões. Em aplicações reais, implemente tentativas de reconexão ou healthchecks.

# 7. Docker Compose — comandos essenciais

Como o Compose é muito utilizado em projetos reais, vale memorizar seus principais comandos.

|Comando|Função|
|---|---|
|`docker compose up`|Cria e inicia os serviços.|
|`docker compose up -d`|Executa em segundo plano.|
|`docker compose up --build`|Reconstrói as imagens e inicia.|
|`docker compose down`|Para e remove containers e redes do projeto.|
|`docker compose stop`|Para os containers.|
|`docker compose start`|Inicia serviços parados.|
|`docker compose restart`|Reinicia os serviços.|
|`docker compose ps`|Lista serviços e containers.|
|`docker compose logs`|Mostra logs.|
|`docker compose logs -f`|Acompanha logs em tempo real.|
|`docker compose exec`|Executa comandos em serviços.|
|`docker compose build`|Constrói as imagens.|
|`docker compose pull`|Baixa imagens.|
|`docker compose config`|Valida e exibe a configuração.|
|`docker compose rm`|Remove containers parados.|
|`docker compose down -v`|Remove containers, redes e volumes do projeto.|

Exemplos:

```
docker compose exec backend sh
```

```
docker compose exec banco psql -U admin -d escola
```

```
docker compose config
```

Diferença importante:

- `docker compose stop`: interrompe os serviços, preservando os containers.
    
- `docker compose down`: remove os containers e a rede criada pelo projeto.
    
- `docker compose down -v`: também remove os volumes do projeto. Isso pode apagar os dados persistidos.
    

# 8. Conceitos intermediários e avançados

Depois de aprender os comandos básicos, existem alguns conceitos importantes para trabalhar profissionalmente com Docker.

## 8.1. Camadas de imagens (Layers)

Uma imagem Docker é construída por camadas.

Por exemplo:

```
FROM python:3.13-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

CMD ["python", "app.py"]
```

Cada instrução que modifica a imagem pode gerar uma camada.

O Docker utiliza cache para evitar reconstruir camadas que não foram modificadas.

Boa prática: copie primeiro os arquivos de dependências e instale os pacotes antes de copiar o restante do código. Assim, alterações no código não obrigam a reinstalação das dependências.

## 8.2. Bind Mount x Volume

Ambos permitem compartilhar dados entre o computador e o container, mas possuem diferenças.

|Característica|Bind Mount|Volume|
|---|---|---|
|Localização|Caminho definido pelo usuário|Gerenciado pelo Docker|
|Controle|Maior controle sobre arquivos|Gerenciamento pelo Docker|
|Utilização|Desenvolvimento|Persistência de dados|
|Compartilhamento|Arquivos do projeto|Dados persistentes|

Exemplo de Bind Mount:

```
docker run -d \
  --name meu-nginx \
  -p 8080:80 \
  -v ./site:/usr/share/nginx/html:ro \
  nginx
```

O diretório `./site` do computador será disponibilizado dentro do container.

O `:ro` indica somente leitura.

Exemplo de Volume:

```
docker volume create banco-dados
```

```
docker run -d \
  --name postgres \
  -v banco-dados:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=123456 \
  postgres:17
```

## 8.3. Variáveis de ambiente

São utilizadas para configurar aplicações sem precisar modificar o código.

Exemplo:

```
docker run -d \
  --name minha-api \
  -e DATABASE_HOST=localhost \
  -e DATABASE_USER=admin \
  nginx
```

No Docker Compose:

```
services:
  app:
    image: nginx
    environment:
      APP_ENV: development
      DEBUG: "true"
```

Você também pode utilizar um arquivo `.env` para centralizar configurações.

Exemplo:

```
DB_USER=admin
DB_PASSWORD=senha123
DB_NAME=escola
```

E no Compose:

```
services:
  banco:
    image: postgres:17
    environment:
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
      POSTGRES_DB: ${DB_NAME}
```

O `.env` não deve ser enviado ao Git se contiver credenciais reais. Para produção, prefira mecanismos apropriados de gerenciamento de secrets.

## 8.4. Docker Registry e publicação de imagens

Você também pode publicar suas próprias imagens para outras pessoas utilizarem.

Exemplo utilizando Docker Hub:

Primeiro, faça login:

```
docker login
```

Crie uma tag:

```
docker tag minha-api:1.0 usuario/minha-api:1.0
```

Envie a imagem:

```
docker push usuario/minha-api:1.0
```

Outra pessoa poderá baixar:

```
docker pull usuario/minha-api:1.0
```

E executar:

```
docker run -d -p 5000:5000 usuario/minha-api:1.0
```

## 8.5. Docker multi-stage build

É uma técnica para criar imagens menores e mais eficientes.

Imagine uma aplicação Java: você precisa do JDK para compilar, mas não necessariamente precisa dele para executar o programa.

Exemplo:

```
FROM eclipse-temurin:11-jdk AS build

WORKDIR /app

COPY Main.java .

RUN javac Main.java


FROM eclipse-temurin:11-jre

WORKDIR /app

COPY --from=build /app/Main.class .

CMD ["java", "Main"]
```

Nesse exemplo:

1. O primeiro estágio compila o programa.
    
2. O segundo estágio utiliza apenas o ambiente necessário para executá-lo.
    
3. O arquivo compilado é copiado entre os estágios.
    

Isso pode reduzir o tamanho final da imagem.

## 8.6. Healthcheck

Permite verificar se uma aplicação dentro do container está funcionando corretamente.

Exemplo:

```
FROM nginx

HEALTHCHECK --interval=30s \
  --timeout=5s \
  --retries=3 \
  CMD curl -f http://localhost/ || exit 1
```

Consultar o estado:

```
docker ps
```

Um container pode apresentar estados como:

- `starting`
    
- `healthy`
    
- `unhealthy`
    

Healthcheck é particularmente útil em sistemas compostos por vários serviços.

## 8.7. Docker Swarm e Kubernetes

São tecnologias relacionadas à orquestração de containers.

[Docker Swarm · Docker Docs](https://images.openai.com/static-rsc-4/ZVA4v9xQXAiCSOO-jO5ZsKULkvIb0B36X4zGwvJj1ee7thyDuGGdkHAAI2RWJv0QRfOS6oUFS23pxZfotEvlDNIfHNqe3h-hv-kE8MpnTTPVqcM0XDaABywt6UerqE8FFGzIj3xwZeqqrsdHhMrEXbkpcm8OBoPxnhle42GSAD8?purpose=inline)

Docker Swarm

- Orquestração de containers.
    
- Gerenciamento de serviços.
    
- Distribuição entre nós.
    
- Escalabilidade.
    

[DevOps, Continuous Delivery... and You - Full Stack Python](https://images.openai.com/static-rsc-4/zVPRXH7i89DWyRHJSK01J9I4pV7GUi0RF_go3gRNhC30nxAOVPkZBY7JEycXEpHwiEQmcypgZmorm62_-uRdTGc4uKqeP_zP018BiKcf9je4ooH6OjsuVWOnYvPY9wv7Y5rS1aMwHS4Wczhe90dEaip9yTQH4ApWt4wdBI1GM_g?purpose=inline)

Kubernetes

- Orquestração avançada.
    
- Gerenciamento de clusters.
    
- Escalabilidade automática.
    
- Recuperação de aplicações.
    

O Docker é utilizado para construir e executar containers. Kubernetes e Swarm trabalham com a administração de aplicações conteinerizadas em ambientes distribuídos.

# 9. Boas práticas ao utilizar Docker

Algumas recomendações importantes para projetos profissionais:

1. Utilize imagens oficiais: prefira imagens confiáveis e verificadas.
    
2. Evite `latest`: escolha versões específicas para garantir previsibilidade.
    
3. Utilize `.dockerignore`: evite enviar arquivos desnecessários.
    
4. Não armazene senhas no Dockerfile: utilize variáveis de ambiente e secrets.
    
5. Utilize volumes: mantenha dados importantes fora da camada descartável do container.
    
6. Reduza o tamanho das imagens: prefira imagens menores e multi-stage builds quando apropriado.
    
7. Não execute tudo como root: utilize usuários sem privilégios administrativos.
    
8. Monitore os logs: acompanhe erros e problemas de execução.
    
9. Limite os recursos: configure CPU e memória quando necessário.
    
10. Mantenha as imagens atualizadas: aplique correções de segurança.
    

# 10. Problemas comuns e soluções

|Problema|Possível solução|
|---|---|
|Permission denied|Verificar permissões e grupo Docker.|
|Port already allocated|Alterar a porta do host.|
|Container exited|Consultar `docker logs`.|
|Image not found|Verificar nome e tag da imagem.|
|Connection refused|Verificar se o serviço está funcionando e aceitando conexões.|
|No space left on device|Verificar utilização com `docker system df`.|
|Container não inicia|Executar `docker inspect` e consultar logs.|
|Banco perde dados|Verificar configuração de volumes.|

### Comandos úteis para diagnóstico

```
docker ps -a
```

```
docker logs nome-container
```

```
docker inspect nome-container
```

```
docker stats
```

```
docker system df
```

```
docker events
```

# 11. Fluxo de trabalho de um desenvolvedor utilizando Docker

Em um projeto real, um fluxo comum seria:

1. Desenvolver a aplicação
    
    Criar o código utilizando Java, Python, Rust, JavaScript ou outra linguagem.
    
2. Criar o Dockerfile
    
    Definir o ambiente necessário para executar a aplicação.
    
3. Construir a imagem
    
    ```
    docker build -t minha-aplicacao:1.0 .
    ```
    
4. Executar o container
    
    ```
    docker run -d -p 8080:8080 minha-aplicacao:1.0
    ```
    
5. Testar e monitorar
    
    ```
    docker logs -f minha-aplicacao
    ```
    
6. Publicar a imagem
    
    ```
    docker push usuario/minha-aplicacao:1.0
    ```
    
7. Implantar a aplicação
    
    Executar a imagem no servidor de destino.
    

# 12. Resumo para consulta rápida

Esta tabela reúne os comandos que vale a pena memorizar.

|Objetivo|Comando|
|---|---|
|Ver versão|`docker --version`|
|Baixar imagem|`docker pull nginx`|
|Listar imagens|`docker images`|
|Construir imagem|`docker build -t app .`|
|Executar container|`docker run -d app`|
|Listar containers|`docker ps -a`|
|Parar container|`docker stop app`|
|Iniciar container|`docker start app`|
|Reiniciar container|`docker restart app`|
|Remover container|`docker rm app`|
|Remover imagem|`docker rmi app`|
|Acessar container|`docker exec -it app sh`|
|Ver logs|`docker logs app`|
|Monitorar recursos|`docker stats`|
|Criar volume|`docker volume create dados`|
|Criar rede|`docker network create rede`|
|Iniciar Compose|`docker compose up -d`|
|Parar Compose|`docker compose down`|
|Reconstruir projeto|`docker compose up -d --build`|
|Limpar recursos|`docker system prune`|
|Publicar imagem|`docker push usuario/app`|

# 13. Exercícios práticos para aprender Docker

A melhor maneira de aprender Docker é desenvolver pequenos projetos e praticar os comandos.

Seu progresso

0/7 concluídos

1. Primeiro container

Baixe a imagem Ubuntu e execute um terminal interativo.

2. Servidor web

Execute um Nginx na porta 8080 e acesse pelo navegador.

3. Banco de dados

Crie um PostgreSQL com usuário, senha e banco personalizados.

4. Persistência

Crie um volume e configure o PostgreSQL para utilizá-lo.

5. Dockerfile

Crie uma imagem personalizada de uma aplicação Java ou Python.

6. Docker Compose

Crie um projeto com backend e PostgreSQL.

7. Publicação

Publique uma imagem própria no Docker Hub.

Reiniciar progresso

# 14. O que você deve dominar?

Para trabalhar com Docker em desenvolvimento de software, recomendo seguir esta ordem:

|Nível|Conteúdo|
|---|---|
|Iniciante|Imagens, containers e comandos básicos.|
|Iniciante|Dockerfile e construção de imagens.|
|Intermediário|Volumes, redes e variáveis de ambiente.|
|Intermediário|Docker Compose.|
|Intermediário|Integração com Java, Python e bancos de dados.|
|Avançado|Multi-stage builds e otimização.|
|Avançado|Segurança, CI/CD e orquestração.|

Minha recomendação: como você estuda Análise e Desenvolvimento de Sistemas e trabalha com linguagens como Java, Python e Rust, procure dominar principalmente Dockerfile, volumes, redes e Docker Compose. Esses conhecimentos serão muito úteis para desenvolver projetos completos com banco de dados e backend sem precisar instalar todas as dependências diretamente no Fedora.