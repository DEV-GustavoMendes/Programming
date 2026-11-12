## PARTE E: LINGUAGENS DE PROGRAMAÇÃO PARA A INTERNET

### 1. Contexto: como funciona uma aplicação web

Antes dos itens da lista, vale fixar o modelo em que todos eles se encaixam. Uma aplicação web segue a arquitetura **cliente-servidor** sobre o protocolo **HTTP** (HyperText Transfer Protocol): o navegador (cliente) envia uma **requisição** e o servidor devolve uma **resposta**. O HTTP é um protocolo da camada de aplicação, baseado em texto, e **sem estado** (_stateless_): cada requisição é independente, e o servidor, por padrão, não “lembra” das anteriores. Essa característica explica a existência de cookies e sessões, que veremos adiante.

Uma requisição tem linha inicial (método, recurso, versão), cabeçalhos e, eventualmente, corpo. Os métodos principais são:

- **GET:** obtém um recurso. Os parâmetros vão na URL, é idempotente e não deve alterar dados.
- **POST:** envia dados ao servidor no corpo da requisição, usado em formulários e para criar recursos.
- **PUT** (substitui um recurso), **DELETE** (remove), **HEAD** (só os cabeçalhos), **OPTIONS** e **PATCH**.

A resposta traz um **código de status**: 2xx sucesso (200 OK, 201 Created), 3xx redirecionamento (301, 302), 4xx erro do cliente (400, 401, 403, 404) e 5xx erro do servidor (500, 503).

O conteúdo pode ser **estático** (arquivo HTML pronto) ou **dinâmico** (gerado por um programa no servidor no momento da requisição). As tecnologias desta parte (Servlets, JSP, PHP, EJB) produzem conteúdo dinâmico. Elas se classificam como **programação do lado do servidor** (_server-side_), em oposição à do lado do cliente (_client-side_), como JavaScript no navegador, em que o código roda na máquina do usuário.

### 2. Java

#### 2.1 A plataforma

Java é uma linguagem orientada a objetos, criada na Sun Microsystems (James Gosling, 1995) e hoje mantida pela Oracle. Seu lema histórico, “escreva uma vez, execute em qualquer lugar” (_write once, run anywhere_), é viabilizado pela arquitetura:

1. O código-fonte (`.java`) é compilado pelo `javac` para **bytecode** (`.class`), uma representação intermediária independente de hardware.
2. O bytecode é executado pela **JVM (Java Virtual Machine)**, que existe para cada sistema operacional e interpreta ou compila o bytecode (compilação JIT, _Just-In-Time_) para código nativo.

Três siglas que caem em prova:

- **JVM:** a máquina virtual que executa o bytecode.
- **JRE (Java Runtime Environment):** JVM mais as bibliotecas necessárias para **executar** programas.
- **JDK (Java Development Kit):** JRE mais as ferramentas para **desenvolver** (compilador `javac`, `jar`, `javadoc`, depurador).

Características da linguagem: orientada a objetos, fortemente tipada, com **coleta de lixo automática**, sem ponteiros explícitos, com suporte nativo a threads e a segurança (verificador de bytecode e gerenciador de segurança). O código-fonte pode ter várias classes, mas **no máximo uma pública por arquivo, com o mesmo nome do arquivo**.

#### 2.2 As três edições

A plataforma se divide em edições, conforme a finalidade:

- **Java SE (Standard Edition):** o núcleo da linguagem e as bibliotecas fundamentais, para aplicações desktop e a base de tudo.
- **Java EE / Jakarta EE (Enterprise Edition):** a plataforma para aplicações corporativas e web distribuídas, construída sobre a SE.
- **Java ME (Micro Edition):** para dispositivos de recursos limitados (embarcados, celulares antigos).

Observação histórica sobre a web: os **applets** (pequenos programas Java embutidos em páginas, executados no navegador) foram a primeira presença do Java na internet, mas foram descontinuados e removidos. Hoje, o Java na web é predominantemente do lado do servidor.

### 3. J2EE (Java EE, Jakarta EE)

#### 3.1 O que é e como mudou de nome

**J2EE (Java 2 Platform, Enterprise Edition)** é a especificação de uma plataforma para desenvolver, implantar e executar aplicações corporativas distribuídas, multicamadas, escaláveis, transacionais e seguras. A trajetória do nome é cobrada com frequência:

- **J2EE** (1999 a 2005, versões 1.2, 1.3 e 1.4).
- **Java EE** (a partir da versão 5, em 2006, quando o “2” saiu do nome; passou por 5, 6, 7 e 8).
- **Jakarta EE** (a partir de 2018, quando a Oracle transferiu a tecnologia para a **Eclipse Foundation**). Como a marca “Java” ficou com a Oracle, o nome mudou. A partir do Jakarta EE 9, os pacotes migraram de `javax.*` para `jakarta.*` (por exemplo, `javax.servlet` virou `jakarta.servlet`), uma pegadinha prática importante.

J2EE é uma **especificação**, e não um produto: cada fornecedor a implementa em seu **servidor de aplicações**. Exemplos: Apache Tomcat (só o contêiner web: Servlets e JSP), Jetty, GlassFish/Payara, WildFly (antigo JBoss), IBM WebSphere e Oracle WebLogic.

#### 3.2 Arquitetura multicamadas

A plataforma promove a separação em camadas:

- **Camada de cliente:** navegador, aplicação desktop ou cliente de serviços.
- **Camada web (apresentação):** **Servlets** e **JSP** (e JSF), rodando no **contêiner web**. Recebe requisições HTTP e gera respostas.
- **Camada de negócio:** **EJB** (componentes com a lógica de negócio), rodando no **contêiner EJB**.
- **Camada de integração/dados (EIS, Enterprise Information System):** bancos de dados, sistemas legados, ERPs, acessados via JDBC, JPA, JMS, conectores.

Variações: arquitetura de **2 camadas** (cliente com a lógica + banco), **3 camadas** (apresentação, negócio, dados) e **n camadas**.

#### 3.3 Contêineres

Um **contêiner** é o ambiente de execução que gerencia o ciclo de vida dos componentes e lhes provê serviços (segurança, transações, concorrência, nomes, pool de conexões). O desenvolvedor foca na lógica de negócio, e o contêiner cuida da “infraestrutura”. É o conceito de **inversão de controle**: o contêiner é quem chama o componente, e não o contrário. Tipos:

- **Contêiner web:** hospeda Servlets e JSP.
- **Contêiner EJB:** hospeda os enterprise beans.
- **Contêiner de aplicação cliente** e **contêiner de applets**.

#### 3.4 Principais tecnologias da plataforma

Servlet, JSP, JSF (JavaServer Faces, framework de componentes de interface), **EJB**, **JPA** (Java Persistence API, mapeamento objeto-relacional), **JMS** (Java Message Service, mensageria assíncrona), **JTA** (Java Transaction API), **JNDI**, **JDBC**, JavaMail, **JAX-WS** (serviços SOAP), **JAX-RS** (serviços REST), JAXB (XML), CDI (injeção de dependência) e Bean Validation.

#### 3.5 Empacotamento e implantação (deploy)

- **JAR:** biblioteca ou módulo (incluindo EJBs).
- **WAR (Web Archive):** aplicação web (Servlets, JSP, HTML, recursos).
- **EAR (Enterprise Archive):** aplicação corporativa completa, agrupando WARs e JARs.

Estrutura de um WAR: o conteúdo público (HTML, JSP, imagens) fica na raiz, e a pasta **`WEB-INF`**, **inacessível diretamente pelo navegador**, guarda o descritor `web.xml`, as classes compiladas (`WEB-INF/classes`) e as bibliotecas (`WEB-INF/lib`). Os **descritores de implantação** (arquivos XML como `web.xml`, `ejb-jar.xml`) configuram a aplicação, hoje frequentemente substituídos por **anotações** (`@WebServlet`, `@Stateless`).

### 4. Servlets

#### 4.1 Conceito

Servlet é uma classe Java que roda **dentro do contêiner web** e responde a requisições, geralmente HTTP, gerando conteúdo dinâmico. Substituiu o CGI tradicional, com uma grande vantagem de desempenho: no CGI, cada requisição iniciava um **novo processo**, e no servlet cada requisição é atendida por uma **nova thread** de uma instância já carregada. Os servlets estendem `HttpServlet` (que estende `GenericServlet`, que implementa a interface `Servlet`).

#### 4.2 Ciclo de vida

O contêiner controla o ciclo, com três métodos principais:

1. **Carregamento e instanciação** (na primeira requisição ou na inicialização do contêiner, se configurado).
2. **`init()`:** chamado **uma única vez**, para inicializações (abrir recursos, ler configurações).
3. **`service()`:** chamado **a cada requisição**. No `HttpServlet`, ele despacha para `doGet()`, `doPost()`, `doPut()`, `doDelete()` etc., conforme o método HTTP.
4. **`destroy()`:** chamado **uma única vez**, ao descarregar o servlet, para liberar recursos.

Ponto crucial: existe **uma única instância** do servlet, atendendo muitas requisições **simultaneamente em threads distintas**. Portanto, **variáveis de instância são compartilhadas** entre todos os usuários e **não são seguras sem sincronização**. Armazene dados do usuário em variáveis locais, no objeto de requisição ou na sessão.

#### 4.3 Exemplo

java

```java
import java.io.IOException;
import java.io.PrintWriter;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.*;

@WebServlet("/ola")
public class OlaServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        String nome = req.getParameter("nome");        // lê parâmetro da requisição
        resp.setContentType("text/html;charset=UTF-8");
        try (PrintWriter out = resp.getWriter()) {
            out.println("<h1>Ola, " + nome + "!</h1>");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws IOException {
        doGet(req, resp);
    }
}
```

(Em servidores mais antigos, o pacote é `javax.servlet`.) Equivalente no descritor `web.xml`:

xml

```xml
<servlet>
    <servlet-name>OlaServlet</servlet-name>
    <servlet-class>pacote.OlaServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>OlaServlet</servlet-name>
    <url-pattern>/ola</url-pattern>
</servlet-mapping>
```

#### 4.4 Objetos e conceitos centrais

- **`HttpServletRequest`:** representa a requisição (parâmetros com `getParameter`, cabeçalhos, cookies, sessão, atributos).
- **`HttpServletResponse`:** representa a resposta (tipo de conteúdo, código de status, cabeçalhos, `getWriter()` para texto ou `getOutputStream()` para binário).
- **`ServletContext`:** compartilhado por **toda a aplicação** (escopo de aplicação).
- **`ServletConfig`:** configuração de **um** servlet específico (parâmetros de inicialização).
- **Escopos de atributos**, do mais curto ao mais longo: **request** (uma requisição), **session** (um usuário, várias requisições) e **application** (todos os usuários, enquanto a aplicação estiver no ar). Há também o escopo de página, só em JSP.
- **Controle de sessão:** como o HTTP não tem estado, usam-se: **cookies** (pequenos dados guardados no navegador; a sessão Java usa o cookie `JSESSIONID`), **reescrita de URL** (`encodeURL`, quando cookies estão desabilitados), **campos ocultos** de formulário e **`HttpSession`** (objeto no servidor, associado ao usuário por um identificador: `req.getSession()`, `setAttribute`, `getAttribute`, `invalidate()`).
- **Encaminhamento (`forward`) versus redirecionamento (`sendRedirect`):**
    - `RequestDispatcher.forward()`: acontece **no servidor**, na mesma requisição, e a URL no navegador **não muda**. Mantém os atributos da requisição.
    - `response.sendRedirect()`: instrui o navegador a fazer **uma nova requisição** à outra URL. A URL **muda**, e os atributos da requisição se perdem. É usado após um POST para evitar reenvio do formulário (padrão Post/Redirect/Get).
- **Filtros (`Filter`):** interceptam requisições e respostas antes e depois do servlet, usados para autenticação, log, compressão e codificação de caracteres.
- **Listeners:** reagem a eventos do ciclo de vida da aplicação, sessão ou requisição.

### 5. JSP (JavaServer Pages)

#### 5.1 Conceito

JSP é uma tecnologia que permite **misturar HTML com código Java** para gerar páginas dinâmicas, de forma mais cômoda que escrever HTML dentro de `println` de um servlet. Pense assim: **o servlet é Java com HTML dentro, e o JSP é HTML com Java dentro**. O ponto mais importante: **um JSP é, ao final, transformado em um servlet**.

#### 5.2 Ciclo de vida

1. **Tradução:** o contêiner converte o arquivo `.jsp` em código-fonte de um servlet (Java).
2. **Compilação** desse código para uma classe.
3. **Carregamento, instanciação e `jspInit()`**.
4. **Execução:** a cada requisição, chama-se **`_jspService()`**.
5. **`jspDestroy()`** na remoção.

A tradução e a compilação acontecem **na primeira requisição** (ou quando o arquivo é alterado), e por isso a primeira chamada é mais lenta. Os métodos `jspInit()` e `jspDestroy()` podem ser sobrescritos, mas o `_jspService()` **não**: é gerado pelo contêiner.

#### 5.3 Elementos de script e sintaxe

jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" import="java.util.*" %>
<%@ include file="cabecalho.jsp" %>

<%! int contador = 0;  /* declaração: vira membro da classe (compartilhado!) */ %>

<%
    // scriptlet: código Java dentro do método _jspService
    String nome = request.getParameter("nome");
    contador++;
%>

<html>
  <body>
    <h1>Bem-vindo, <%= nome %>!</h1>   <%-- expressão: resultado impresso na saída --%>
    <p>Total de acessos: <%= contador %></p>
  </body>
</html>
```

- **Diretivas** `<%@ ... %>`: instruções ao contêiner, em tempo de tradução. As três: **`page`** (configurações da página), **`include`** (inclusão **estática**, do conteúdo do arquivo no momento da tradução) e **`taglib`** (declara bibliotecas de tags).
- **Declaração** `<%! ... %>`: declara variáveis e métodos no nível da **classe**. Uma variável declarada assim é de instância do servlet gerado, portanto **compartilhada entre requisições** e sujeita aos mesmos problemas de concorrência.
- **Scriptlet** `<% ... %>`: código Java dentro de `_jspService`. Variáveis declaradas aqui são **locais**, seguras.
- **Expressão** `<%= ... %>`: avalia e imprime o resultado (sem ponto e vírgula).
- **Comentário JSP** `<%-- ... --%>`: não aparece no HTML enviado, ao contrário de `<!-- ... -->`, que aparece.
- **Ações padrão** `<jsp:...>`, em tempo de **requisição**: `<jsp:include page="x.jsp"/>` (inclusão **dinâmica**), `<jsp:forward>`, `<jsp:useBean>`, `<jsp:setProperty>`, `<jsp:getProperty>`, `<jsp:param>`.

**Macete: `<%@ include %>` (diretiva) é estática, na tradução; `<jsp:include>` (ação) é dinâmica, na requisição.**

#### 5.4 Objetos implícitos

Variáveis já disponíveis em scriptlets e expressões, sem declaração:

|Objeto|Tipo|Finalidade|
|---|---|---|
|`request`|HttpServletRequest|dados da requisição|
|`response`|HttpServletResponse|resposta|
|`out`|JspWriter|escrita na saída|
|`session`|HttpSession|sessão do usuário|
|`application`|ServletContext|escopo da aplicação|
|`config`|ServletConfig|configuração do servlet|
|`pageContext`|PageContext|acesso a todos os escopos|
|`page`|Object (equivale a `this`)|a própria página|
|`exception`|Throwable|só em páginas de erro (`isErrorPage="true"`)|

#### 5.5 Boas práticas: EL, JSTL, JavaBeans e MVC

Misturar muito Java em páginas JSP (scriptlets) é considerado má prática. As alternativas:

- **EL (Expression Language):** expressões como `${usuario.nome}` acessam atributos e propriedades de beans nos escopos, sem código Java.
- **JSTL (JSP Standard Tag Library):** conjunto de tags padronizadas, como `<c:if>`, `<c:forEach>`, `<c:out>` (que também escapa HTML) e tags de formatação e SQL.
- **JavaBeans:** classe com construtor sem argumentos, atributos privados e **getters/setters** públicos, que serve de portadora de dados.
- **Padrão MVC (Model-View-Controller)**, o arranjo clássico em Java web (“Model 2”):
    - **Model:** os dados e a lógica de negócio (JavaBeans, EJB, JPA).
    - **View:** a apresentação, tipicamente **JSP**.
    - **Controller:** o **Servlet**, que recebe a requisição, aciona o modelo, coloca os resultados em atributos e **encaminha (`forward`)** para o JSP.

Frameworks que implementam o padrão: Struts (histórico), JSF, Spring MVC.

**Servlet versus JSP:** servlets são mais indicados para **lógica de controle**, e JSP para **apresentação**. Funcionalmente, são equivalentes, já que o JSP vira servlet.

### 6. EJB (Enterprise JavaBeans)

#### 6.1 Conceito

EJB é a especificação de **componentes de negócio do lado do servidor**, executados no contêiner EJB. Encapsulam a **lógica de negócio** e recebem do contêiner, de forma declarativa, serviços como **gerenciamento de transações, segurança, concorrência, ciclo de vida, pool de instâncias e injeção de dependências**. Não confunda com **JavaBeans** (simples classes com getters/setters, usadas como objetos de dados), que nada têm a ver com o contêiner: **EJB e JavaBeans são coisas diferentes**.

#### 6.2 Tipos de beans

- **Session Bean (bean de sessão):** realiza lógica de negócio chamada por clientes. Divide-se em:
    - **Stateless (sem estado):** não guarda estado de conversação com o cliente. O contêiner mantém um **pool** de instâncias intercambiáveis, e qualquer instância pode atender qualquer chamada. É o mais escalável. Exemplo: calcular um frete.
    - **Stateful (com estado):** mantém o estado entre chamadas **de um mesmo cliente**, com uma instância dedicada a cada cliente. Exemplo: carrinho de compras.
    - **Singleton** (a partir do EJB 3.1): **uma única instância** por aplicação, compartilhada, com controle de concorrência.
- **Message-Driven Bean (MDB):** consome mensagens **assincronamente** de filas (`Queue`) ou tópicos (`Topic`) via **JMS**. Não é chamado diretamente pelo cliente e não tem interface de negócio.
- **Entity Bean (bean de entidade):** representava dados persistentes (linhas de tabela). Foi **substituído pela JPA** a partir do EJB 3.0 e removido nas versões posteriores. Hoje as “entidades” são classes simples anotadas com `@Entity`.

#### 6.3 Exemplo (EJB 3.x)

java

```java
import jakarta.ejb.Stateless;
import jakarta.persistence.*;

@Stateless
public class ContaService {

    @PersistenceContext
    private EntityManager em;          // injetado pelo contêiner

    public void transferir(Long origem, Long destino, double valor) {
        Conta o = em.find(Conta.class, origem);
        Conta d = em.find(Conta.class, destino);
        o.sacar(valor);
        d.depositar(valor);
        // transação gerenciada pelo contêiner: commit ao sair, rollback em exceção de sistema
    }
}
```

Um cliente (um servlet, por exemplo) o obtém por injeção: `@EJB private ContaService service;`.

#### 6.4 Interfaces e acesso

- **Interface local** (`@Local`): para clientes na **mesma JVM**, com chamada direta e rápida.
- **Interface remota** (`@Remote`): para clientes em **outra JVM**, via RMI. Os argumentos são passados **por valor** (serializados).
- **Visão sem interface** (_no-interface view_), desde o EJB 3.1: a própria classe é acessível localmente.

Um cliente localiza o bean por **injeção de dependência** ou por consulta **JNDI** (_lookup_). Nas versões antigas (EJB 2.x) era preciso também a **interface Home** (para criar/localizar beans) e a **interface Remote/Local**, além do descritor XML, daí a fama de complexidade que o EJB 3 reduziu bastante com anotações e POJOs.

#### 6.5 Transações

Duas formas de demarcação:

- **CMT (Container-Managed Transactions):** o **contêiner** controla, de forma declarativa (`@TransactionAttribute`). É o padrão.
- **BMT (Bean-Managed Transactions):** o **desenvolvedor** controla programaticamente, com `UserTransaction`.

Atributos transacionais (CMT): **`REQUIRED`** (padrão: usa a transação existente ou cria uma nova), **`REQUIRES_NEW`** (sempre cria uma nova, suspendendo a existente), **`MANDATORY`** (exige transação existente, senão lança exceção), **`SUPPORTS`** (usa se existir, senão executa sem), **`NOT_SUPPORTED`** (suspende a transação e executa sem) e **`NEVER`** (lança exceção se houver transação). Propriedades **ACID**: Atomicidade, Consistência, Isolamento e Durabilidade. O padrão do contêiner é fazer **rollback** em exceções de sistema (não verificadas) e **commit** em exceções de aplicação (verificadas), salvo configuração em contrário (`@ApplicationException(rollback=true)`).

#### 6.6 Segurança e outros serviços

Segurança declarativa por **papéis (roles)**: `@RolesAllowed`, `@PermitAll`, `@DenyAll`. Também há **timer service** (agendamento), **interceptadores** e **ciclo de vida** com callbacks (`@PostConstruct`, `@PreDestroy`, `@PrePassivate` e `@PostActivate` nos stateful: **passivação** é o salvamento do estado em disco para liberar memória).

### 7. JNDI (Java Naming and Directory Interface)

#### 7.1 Conceito

JNDI é uma API Java para acessar **serviços de nomes e de diretórios**. Um **serviço de nomes** associa **nomes a objetos**, permitindo localizá-los pelo nome, como a lista telefônica associa nomes a números ou o DNS associa nomes de domínio a endereços IP. Um **serviço de diretório** vai além: os objetos têm **atributos** pesquisáveis (como o LDAP, em que um usuário tem nome, e-mail, cargo). A grande vantagem é a **independência de implementação**: o mesmo código usa a mesma API, seja o serviço LDAP, DNS, o registro RMI ou o próprio servidor de aplicações.

#### 7.2 Arquitetura

Duas camadas: a **API JNDI** (usada pelas aplicações) e a **SPI (Service Provider Interface)** (implementada pelos fornecedores de cada serviço, como LDAP, DNS, RMI, CORBA). Os pacotes são `javax.naming` (nomes), `javax.naming.directory` (diretórios), `javax.naming.ldap` e `javax.naming.spi`.

#### 7.3 Conceitos e operações

- **Contexto (`Context`):** um conjunto de associações nome-objeto, ponto de partida para operações. **`InitialContext`** é o ponto de entrada.
- **Operações:** `lookup(nome)` (recupera o objeto), `bind(nome, obj)` (associa), `rebind` (associa, sobrescrevendo), `unbind` (remove), `list` (lista), `createSubcontext`.
- Para o JNDI funcionar, configuram-se propriedades como o **`INITIAL_CONTEXT_FACTORY`** e o **`PROVIDER_URL`** (em servidores de aplicação, o próprio servidor já provê essa configuração).
- **Namespace padrão de componentes:** nomes com o prefixo **`java:comp/env`** acessam o ambiente do componente (recursos declarados no descritor). Variações de escopo: `java:comp`, `java:module`, `java:app`, `java:global`.

**Exemplo clássico: obter um DataSource (pool de conexões) por JNDI**

java

```java
Context ctx = new InitialContext();
DataSource ds = (DataSource) ctx.lookup("java:comp/env/jdbc/MeuBanco");
Connection con = ds.getConnection();
```

O administrador configura o pool de conexões no servidor e o publica sob o nome `jdbc/MeuBanco`. A aplicação **não conhece** usuário, senha ou URL do banco, e isso é um enorme ganho de manutenção e segurança. Em Jakarta EE moderno, o mesmo resultado se obtém com `@Resource(lookup = "java:comp/env/jdbc/MeuBanco") DataSource ds;`.

**Papel integrador:** o JNDI é a “cola” da plataforma. É por ele que servlets localizam EJBs, DataSources, filas JMS e `UserTransaction`.

### 8. JDBC (Java Database Connectivity)

#### 8.1 Conceito

JDBC é a API Java (pacotes `java.sql` e `javax.sql`) para **acessar bancos de dados relacionais** com SQL, de forma independente do fabricante. O código usa a API, e um **driver JDBC** específico do banco faz a tradução para o protocolo do SGBD.

#### 8.2 Tipos de driver

- **Tipo 1 (ponte JDBC-ODBC):** traduz chamadas JDBC para ODBC. Dependente de plataforma, lenta e **removida do Java 8**.
- **Tipo 2 (API nativa):** usa bibliotecas nativas do fabricante (código C) instaladas na máquina cliente. Dependente de plataforma.
- **Tipo 3 (protocolo de rede ou middleware):** o cliente fala com um servidor intermediário (middleware), que se comunica com o banco. Puro Java no cliente.
- **Tipo 4 (protocolo nativo, _thin driver_):** **Java puro**, comunica-se **diretamente** com o banco por seu protocolo de rede. É o **mais usado** hoje e o mais portável e rápido (por exemplo, os drivers do PostgreSQL, MySQL e Oracle thin).

#### 8.3 Fluxo de uso

java

```java
String url = "jdbc:postgresql://localhost:5432/loja";

try (Connection con = DriverManager.getConnection(url, "usuario", "senha");
     PreparedStatement ps = con.prepareStatement(
         "SELECT id, nome FROM cliente WHERE cidade = ?")) {

    ps.setString(1, "Brasilia");            // parâmetros começam no índice 1
    try (ResultSet rs = ps.executeQuery()) {
        while (rs.next()) {                 // avança o cursor
            int id = rs.getInt("id");
            String nome = rs.getString("nome");
            System.out.println(id + " - " + nome);
        }
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

Os passos: (1) carregar o driver (automático desde o JDBC 4, que usa o mecanismo de serviços; antes se usava `Class.forName("...")`), (2) abrir a **`Connection`**, (3) criar o comando (`Statement`, `PreparedStatement` ou `CallableStatement`), (4) executar, (5) processar o **`ResultSet`**, (6) **fechar** os recursos (de preferência com _try-with-resources_, que fecha automaticamente).

A **URL de conexão** tem a forma `jdbc:subprotocolo://host:porta/banco`.

#### 8.4 Interfaces principais

- **`DriverManager`:** gerencia drivers e cria conexões (forma simples, sem pool).
- **`DataSource`:** forma moderna e preferida de obter conexões, tipicamente com **pool de conexões** e obtido por **JNDI**. O pool reaproveita conexões, evitando o alto custo de abrir uma nova a cada requisição.
- **`Connection`:** a sessão com o banco. Controla transações (`setAutoCommit`, `commit`, `rollback`).
- **`Statement`:** executa SQL estático. **`PreparedStatement`:** SQL **pré-compilado** com parâmetros `?`. **`CallableStatement`:** executa **procedimentos armazenados** (_stored procedures_).
- **Métodos de execução:** `executeQuery()` (SELECT, retorna `ResultSet`), `executeUpdate()` (INSERT, UPDATE, DELETE e DDL, retorna o número de linhas afetadas) e `execute()` (genérico).
- **`ResultSet`:** o resultado em forma de tabela, percorrido por um **cursor**. O cursor começa **antes da primeira linha**, e `next()` o avança, retornando `false` ao chegar ao fim. Por padrão, só avança (_forward-only_) e é somente leitura.
- **`ResultSetMetaData`** e **`DatabaseMetaData`:** informações sobre colunas e sobre o banco.

#### 8.5 Pontos de atenção para a prova

- **`PreparedStatement` é preferível ao `Statement`**, por dois motivos: **segurança** (previne **SQL Injection**, pois os parâmetros são tratados como dados e não como código SQL) e **desempenho** (a instrução é pré-compilada e reutilizável).
- **Os índices de coluna e de parâmetro no JDBC começam em 1**, e não em 0 (`setString(1, ...)`, `rs.getString(1)`). É uma pegadinha clássica para quem vem de vetores.
- **`autoCommit` vem ligado por padrão:** cada comando é uma transação própria. Para agrupar comandos numa transação: `con.setAutoCommit(false)`, executar, e então `con.commit()` ou, em caso de erro, `con.rollback()`.
- Esquecer de **fechar** `Connection`, `Statement` e `ResultSet` causa vazamento de recursos e esgota o pool.
- **Exemplo de SQL Injection:** concatenar `"... WHERE login='" + entrada + "'"` permite que a entrada `' OR '1'='1` altere a lógica da consulta.
- **JDBC versus JPA:** JDBC trabalha com SQL e tabelas (nível baixo), e JPA/ORM mapeia objetos para tabelas (nível mais alto), usando JDBC por baixo.
- **DAO (Data Access Object):** padrão que isola o acesso a dados numa classe própria, mantendo o resto da aplicação independente do SQL.

### 9. PHP

#### 9.1 Conceito

PHP é uma linguagem de **script do lado do servidor**, de código aberto, criada por **Rasmus Lerdorf** (1994-1995) e hoje bastante usada na web. O nome é um **acrônimo recursivo**: _PHP: Hypertext Preprocessor_ (originalmente _Personal Home Page_). Características:

- **Interpretada** (executada pelo motor **Zend Engine**, com otimizações como OPcache e, a partir do PHP 8, compilação JIT).
- **Tipagem dinâmica e fraca** (com tipagem opcional crescente: tipos declarados em parâmetros, retornos e propriedades, `declare(strict_types=1)`).
- Código embutido no HTML, entre as marcações `<?php ... ?>`.
- Sintaxe inspirada em C, Java e Perl; suporta orientação a objetos (classes, herança, interfaces, traits, namespaces).
- Roda tipicamente com **Apache** ou **Nginx** e é a base do clássico conjunto **LAMP** (Linux, Apache, MySQL, PHP). Sustenta sistemas como WordPress, Drupal e Moodle, e frameworks como Laravel e Symfony.
- **Ciclo de execução:** a cada requisição, o script é executado do início ao fim e **todo o estado é descartado** ao final (modelo “compartilha nada” entre requisições), o que simplifica a escalabilidade horizontal. O estado persistente fica em sessão, banco de dados ou cache.

#### 9.2 Sintaxe essencial

php

```php
<?php
$nome = "Ana";                       // variáveis começam com $
$idade = 30;
$notas = [8.5, 7.0, 9.2];            // array indexado
$aluno = ["nome" => "Ana", "curso" => "TI"];   // array associativo

echo "Ola, $nome!";                  // aspas duplas interpolam variáveis
echo 'Ola, $nome';                   // aspas simples NÃO interpolam
echo "Soma: " . (2 + 3);             // o operador de concatenação é o ponto

function dobro(int $x): int {
    return $x * 2;
}

foreach ($notas as $n) {
    echo dobro($n) . "<br>";
}

if ($idade >= 18) {
    echo "Maior de idade";
} elseif ($idade >= 16) {
    echo "Voto facultativo";
} else {
    echo "Menor";
}
?>
```

Pontos de sintaxe cobrados:

- **Variáveis** começam com `$`, diferenciam maiúsculas de minúsculas (os **nomes de função** e palavras-chave, não).
- **Concatenação com `.`** (ponto), e não com `+`.
- **`==` compara valor com conversão de tipo, e `===` compara valor e tipo.** `"5" == 5` é verdadeiro, mas `"5" === 5` é falso. Há também `!=` e `!==`.
- **`echo`** (construção da linguagem, aceita vários argumentos, sem retorno) e **`print`** (retorna 1).
- Tipos escalares: `int`, `float`, `string`, `bool`; compostos: `array`, `object`; especiais: `null`, `resource`. `var_dump()` mostra tipo e valor, e `print_r()` imprime estruturas de forma legível.
- **Escopo de variáveis:** variáveis dentro de funções são **locais**, e para acessar uma global é preciso usar `global $x` ou `$GLOBALS['x']`. `static` preserva valor entre chamadas.
- **`include`, `include_once`, `require`, `require_once`:** incluem outro arquivo. A diferença: se o arquivo não for encontrado, **`include` emite um aviso (warning) e continua**, enquanto **`require` gera um erro fatal e interrompe**. Os sufixos `_once` impedem inclusão duplicada.
- **Funções úteis:** `isset()` (existe e não é nulo), `empty()`, `count()`, `strlen()`, `str_replace()`, `explode()`, `implode()`, `htmlspecialchars()` (escapa HTML), `date()`.

#### 9.3 Variáveis superglobais

Arrays predefinidos, acessíveis em **qualquer escopo**:

- `$_GET` (parâmetros da URL), `$_POST` (corpo de formulário), `$_REQUEST` (GET + POST + COOKIE).
- `$_SESSION` (dados de sessão), `$_COOKIE`.
- `$_SERVER` (informações do servidor e da requisição, como o método e o IP), `$_FILES` (uploads), `$_ENV`, `$GLOBALS`.

#### 9.4 Sessões e cookies

php

```php
<?php
session_start();                      // deve vir ANTES de qualquer saída
$_SESSION['usuario'] = "ana";         // guarda dado no servidor
echo $_SESSION['usuario'];
session_destroy();                    // encerra a sessão
setcookie("tema", "escuro", time() + 3600);   // cookie no navegador, 1 hora
?>
```

Armadilha clássica: `session_start()` e `setcookie()`/`header()` precisam ser chamados **antes de qualquer saída** para o navegador (incluindo espaços e HTML antes do `<?php`), senão ocorre o erro “headers already sent”.

#### 9.5 Acesso a banco de dados

As extensões principais são **MySQLi** e **PDO (PHP Data Objects)**. O PDO oferece interface única para vários bancos, e ambos suportam **prepared statements**:

php

```php
<?php
$pdo = new PDO("mysql:host=localhost;dbname=loja;charset=utf8", "usuario", "senha");
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);

$stmt = $pdo->prepare("SELECT id, nome FROM cliente WHERE cidade = :cidade");
$stmt->execute([':cidade' => $_POST['cidade']]);

foreach ($stmt->fetchAll(PDO::FETCH_ASSOC) as $linha) {
    echo htmlspecialchars($linha['nome']) . "<br>";
}
?>
```

Como no JDBC, o uso de **consultas parametrizadas** é a defesa contra SQL Injection, e `htmlspecialchars` protege contra **XSS (Cross-Site Scripting)** ao imprimir dados do usuário. Para senhas, usam-se `password_hash()` e `password_verify()` (nunca MD5 ou SHA1 simples).

#### 9.6 Orientação a objetos em PHP

php

```php
<?php
abstract class Animal {
    public function __construct(protected string $nome) {}   // promoção de propriedade (PHP 8)
    abstract public function som(): string;
}
class Cachorro extends Animal {
    public function som(): string { return "{$this->nome} faz Au!"; }
}
$c = new Cachorro("Rex");
echo $c->som();
```

Notas: o construtor se chama `__construct`, acessa-se membros com `->` (instância) e `::` (estáticos e constantes), `$this` referencia o objeto, `extends` herda e `implements` implementa interfaces. **Herança simples** (uma classe só), mas há **traits** para reuso horizontal. Os modificadores são `public`, `protected` e `private`.

### 10. PHP versus Java web (comparação clássica)

|Aspecto|PHP|Java (Servlets/JSP/EJB)|
|---|---|---|
|Execução|Interpretado a cada requisição (script)|Compilado para bytecode, executado pela JVM em servidor de aplicações|
|Tipagem|Dinâmica (com tipos opcionais)|Estática e forte|
|Implantação|Copiar os arquivos para o servidor|Empacotar (WAR/EAR) e implantar no contêiner|
|Estado entre requisições|Nenhum por padrão (“compartilha nada”)|Instâncias de servlet e beans persistem, com escopos (aplicação, sessão)|
|Escalabilidade e recursos corporativos|Escala horizontalmente, com ferramentas de terceiros|Transações distribuídas, mensageria e segurança integradas na plataforma|
|Curva de aprendizado|Mais simples e rápida|Mais complexa, voltada a sistemas corporativos grandes|
|Multithreading|Cada requisição é isolada (um processo ou thread por requisição)|Um servlet compartilha uma instância entre threads (exige cuidado)|

### 11. XML (eXtensible Markup Language)

#### 11.1 Conceito

XML é uma **metalinguagem de marcação** recomendada pelo **W3C** para descrever, armazenar e **transportar dados de forma estruturada, legível por humanos e por máquinas**. Diferente do HTML, que tem um conjunto fixo de tags voltadas à **apresentação**, o XML permite **definir suas próprias tags**, com foco na **descrição e estrutura dos dados**. Ele é independente de plataforma, linguagem e fornecedor, e por isso se tornou base de integração (web services), de configuração (`web.xml`, `persistence.xml`, `pom.xml`) e de troca de documentos (a **NF-e brasileira**, por exemplo, é um documento XML com assinatura digital, o que liga seu tema ao seu trabalho com compliance fiscal).

#### 11.2 Sintaxe e regras de boa formação

xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- comentário -->
<biblioteca>
    <livro id="101" idioma="pt">
        <titulo>Estruturas de Dados</titulo>
        <autor>Maria Silva</autor>
        <preco moeda="BRL">89.90</preco>
        <disponivel/>                       <!-- elemento vazio -->
    </livro>
</biblioteca>
```

Um documento é **bem-formado** (_well-formed_) quando obedece às regras sintáticas:

1. Há **um único elemento raiz**.
2. Toda tag de abertura tem a de **fechamento** (ou é autofechada, `<tag/>`).
3. Os elementos estão **corretamente aninhados** (não se sobrepõem): `<a><b></a></b>` é inválido.
4. Os **valores de atributos vão entre aspas** (simples ou duplas).
5. É **sensível a maiúsculas e minúsculas**: `<Livro>` e `<livro>` são diferentes.
6. Os nomes não podem começar com número nem conter espaços.
7. Caracteres especiais usam **entidades**: `&lt;` (<), `&gt;` (>), `&amp;` (&), `&quot;` e `&apos;`. Trechos que não devem ser interpretados vão em **`<![CDATA[ ... ]]>`**.

Elementos **versus** atributos: elementos descrevem o conteúdo, atributos trazem metadados do elemento. Atributos não podem repetir no mesmo elemento nem ter estrutura interna. Também existem a **declaração XML** `<?xml ... ?>` (opcional, mas se presente deve ser a primeira coisa do documento), **instruções de processamento** e **comentários**.

#### 11.3 Bem-formado versus válido

- **Bem-formado:** segue as regras de sintaxe do XML. É o mínimo exigido.
- **Válido:** além de bem-formado, **obedece a uma gramática** (um esquema) que define quais elementos, atributos e tipos são permitidos. **Todo documento válido é bem-formado, mas o contrário não vale.**

#### 11.4 DTD e XML Schema

- **DTD (Document Type Definition):** a forma mais antiga de definir a estrutura. Usa sintaxe própria (que **não é XML**), tem poucos tipos de dados (basicamente texto) e **não suporta namespaces** de forma nativa.
- **XML Schema (XSD):** o sucessor, **escrito em XML**, com **tipos de dados ricos** (inteiro, data, decimal, tipos derivados e restrições como padrões e faixas), suporte a **namespaces** e maior poder de expressão. É a escolha dominante hoje (os leiautes da NF-e são publicados como arquivos XSD).

Trecho de XSD:

xml

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="livro">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="titulo" type="xs:string"/>
        <xs:element name="preco"  type="xs:decimal"/>
      </xs:sequence>
      <xs:attribute name="id" type="xs:integer" use="required"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

#### 11.5 Namespaces

Evitam **conflito de nomes** quando se combinam vocabulários XML diferentes. Associa-se um prefixo a um URI: `xmlns:fiscal="http://exemplo.com/fiscal"`, e usa-se `<fiscal:nota>`. O URI funciona como **identificador único**, não precisa apontar para uma página real.

#### 11.6 Tecnologias associadas

- **XPath:** linguagem de **consulta e navegação** em documentos XML, por expressões de caminho. Exemplos: `/biblioteca/livro/titulo` (seleciona títulos), `//livro[@id='101']` (livro com id 101), `//livro[preco>50]`.
- **XSLT:** linguagem para **transformar** XML em outro formato (HTML, outro XML, texto), usando folhas de estilo e XPath.
- **XSL-FO** (formatação para saída impressa) e **CSS** (apresentação).
- **XQuery:** linguagem de consulta mais poderosa, comparável ao SQL para XML.
- **Assinatura digital XML (XML-DSig)**, usada em documentos fiscais eletrônicos.

#### 11.7 Processamento de XML: parsers (analisadores)

Um **parser** lê o XML e o disponibiliza ao programa. As abordagens são:

- **DOM (Document Object Model):** carrega o documento inteiro na memória como uma **árvore de nós**. Permite **navegar livremente, modificar e criar** documentos, com acesso aleatório. Em contrapartida, **consome muita memória** (inadequado para arquivos enormes).
- **SAX (Simple API for XML):** **orientado a eventos** e sequencial: o parser percorre o documento e **dispara eventos** (`startElement`, `characters`, `endElement`), tratados por _callbacks_. Usa **pouca memória** e é rápido, adequado para arquivos grandes, mas é **somente leitura, para frente**, sem acesso aleatório. Modelo _push_ (o parser empurra os eventos).
- **StAX (Streaming API for XML):** também de fluxo, mas no modelo _**pull**_: o **programa** puxa os eventos conforme necessita, com mais controle que o SAX.

**Macete:** DOM = árvore na memória (cabe tudo, edita); SAX = eventos (economiza memória, só lê). Em Java, o conjunto de interfaces é o **JAXP (Java API for XML Processing)**. Em PHP, há extensões como `SimpleXML`, `DOMDocument` e `XMLReader`.

**Vinculação XML-objetos (data binding):** o **JAXB (Java Architecture for XML Binding)** converte automaticamente entre XML e objetos Java: **marshalling** (objeto para XML) e **unmarshalling** (XML para objeto), com anotações como `@XmlRootElement`.

#### 11.8 XML e web services

- **SOAP (Simple Object Access Protocol):** protocolo de troca de mensagens **em XML** (envelope com cabeçalho e corpo), geralmente sobre HTTP, com contratos rígidos.
- **WSDL (Web Services Description Language):** documento **XML que descreve** o serviço (operações, parâmetros, endereço).
- **UDDI:** registro para publicar e descobrir serviços (hoje pouco usado).
- **Java:** **JAX-WS** para SOAP e **JAX-RS** para REST.
- **REST** é um estilo arquitetural (não protocolo) baseado em recursos e nos métodos HTTP, que pode trafegar XML ou, mais comumente hoje, **JSON**.

**XML versus JSON:** o XML é mais **verboso**, tem **esquemas, namespaces e transformações** robustos, e é indicado para documentos complexos e integrações formais (como o governo e o setor financeiro). O JSON é mais **leve e compacto**, simples de usar em JavaScript e dominante em APIs REST modernas, mas tem menos recursos de validação nativos.

#### 11.9 Onde o XML aparece na pilha Java EE

`web.xml` (descritor web), `ejb-jar.xml` (descritor EJB), `persistence.xml` (JPA), `faces-config.xml` (JSF), `pom.xml` (Maven), e a formatação de mensagens SOAP e arquivos de configuração em geral.

### 12. Visão integrada: um fluxo típico em Java EE

Quando o usuário envia um formulário de transferência bancária, ocorre algo como: (1) o navegador envia um **POST** HTTP; (2) o **contêiner web** direciona a requisição para um **Servlet** (controller), que lê os parâmetros; (3) o servlet localiza o **EJB** (por injeção ou **JNDI**) e chama o método de negócio; (4) o EJB, dentro de uma **transação** gerenciada pelo contêiner, usa **JPA/JDBC** para acessar o banco, cuja conexão vem de um **DataSource** (obtido por **JNDI**); (5) o resultado volta ao servlet, que o coloca num atributo de requisição e faz **forward** para um **JSP** (view); (6) o JSP gera o **HTML** de resposta, e o contêiner o devolve ao navegador. Integrações com outros sistemas poderiam trafegar **XML** via SOAP ou arquivos como a NF-e.

---

## MACETES E ARMADILHAS PARA A PROVA

1. **J2EE é especificação, não produto.** O nome evoluiu para Java EE e depois Jakarta EE (Eclipse Foundation), com a mudança de `javax.*` para `jakarta.*` no Jakarta EE 9.
2. **Tomcat é só contêiner web** (Servlets e JSP), **sem contêiner EJB** (isso exige servidor como WildFly, GlassFish ou WebLogic, ou TomEE).
3. **Ciclo do servlet:** `init()` uma vez, `service()` a cada requisição (em threads), `destroy()` uma vez. **Uma instância, várias threads:** variáveis de instância não são thread-safe.
4. **JSP vira servlet.** Tradução e compilação ocorrem na primeira requisição, e `_jspService()` é gerado pelo contêiner.
5. **`<%@ include %>` é estático (tempo de tradução). `<jsp:include>` é dinâmico (tempo de requisição).**
6. **Declaração `<%! %>`** cria membros de classe (compartilhados). **Scriptlet `<% %>`** cria variáveis locais. **Expressão `<%= %>`** não leva ponto e vírgula.
7. **`forward` mantém a URL e a mesma requisição. `sendRedirect` troca a URL e cria nova requisição.**
8. **Escopos:** request < session < application.
9. **MVC em Java web:** Servlet = Controller, JSP = View, JavaBean/EJB = Model.
10. **EJB ≠ JavaBean.** Stateless = pool, sem estado de cliente. Stateful = estado por cliente. Singleton = instância única. **MDB** = mensagens assíncronas (JMS). Entity Bean foi substituído pela JPA.
11. **Transação no EJB: CMT (contêiner) é o padrão; atributo padrão é `REQUIRED`.**
12. **JNDI** associa **nomes a objetos**. A operação central é `lookup`. O contexto inicial é `InitialContext`, e o prefixo clássico é `java:comp/env`.
13. **JDBC: índices começam em 1; `executeQuery` para SELECT e `executeUpdate` para modificações; `PreparedStatement` previne SQL Injection; driver Tipo 4 é Java puro e o mais usado; `autoCommit` é verdadeiro por padrão.**
14. **PHP: variáveis com `$`, concatenação com `.`, `==` versus `===`, `include` (aviso) versus `require` (erro fatal), `session_start()` antes de qualquer saída.**
15. **PHP não guarda estado entre requisições** (cada execução é independente), ao contrário de objetos de aplicação e sessão de Java.
16. **XML: um único elemento raiz, tags fechadas, aninhamento correto, atributos entre aspas, sensível a maiúsculas.** Bem-formado ≠ válido (válido exige esquema).
17. **XSD > DTD:** o XSD é escrito em XML, tem tipos de dados e namespaces. O DTD não é XML.
18. **DOM = árvore em memória (edita, gasta memória). SAX = eventos (leve, só leitura, sequencial). StAX = pull.**
19. **XPath consulta, XSLT transforma, XSD valida.**
20. **SOAP usa XML, WSDL descreve o serviço, REST é estilo arquitetural.**

---

**Sobre incidência na prova:** vale o mesmo cuidado das partes anteriores. Não consegui contar por tema as questões de 2020 a 2026, então não afirmo percentuais. Pelo perfil da área, é plausível que questões sobre **ciclo de vida de servlets e JSP**, **diferença entre os tipos de EJB**, **JDBC e SQL Injection**, **XML (boa formação, DTD versus Schema, DOM versus SAX)** e **JNDI** apareçam como afirmações para julgar. Como o conteúdo programático da ESFCEx lista esses tópicos, vale conferir no edital vigente o texto exato e a ênfase de cada um. Se você me enviar as provas em PDF, faço a contagem real por assunto.