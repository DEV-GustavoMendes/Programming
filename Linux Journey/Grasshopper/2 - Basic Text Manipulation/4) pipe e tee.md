# pipe e tee

Aprenda como pipelines conectam comandos e como tee salva um fluxo enquanto o encaminha.

Pipelines conectam pequenos comandos para que os dados fluam entre eles sem um arquivo intermediário. O comando `tee` pode copiar parte desse fluxo para um arquivo enquanto continua a encaminhá-lo.

## Conexão de Comandos com |

Suponha que uma listagem de diretório seja longa demais para ler de uma vez:

```bash
$ ls -la /etc
```

Coloque o operador de pipe, `|`, entre os comandos para conectar stdout do comando à esquerda a stdin do comando à direita:

```bash
$ ls -la /etc | less
```

O shell inicia os comandos do pipeline e organiza a conexão do fluxo. Os comandos podem trabalhar simultaneamente: `less` pode começar a ler antes que `ls` produza toda a listagem.

## Manutenção de stderr Separada

Um `|` simples transporta apenas stdout. Stderr do comando à esquerda mantém seu destino anterior, geralmente o terminal:

```bash
$ find /etc -name "*.conf" | less
```

Os caminhos correspondentes passam pelo pipe, enquanto os diagnósticos de permissão ainda podem aparecer diretamente no terminal. Redirecione stderr separadamente quando precisar de outro comportamento:

```bash
$ find /etc -name "*.conf" 2> find-errors.log | less
```


## Cópia de um Fluxo com tee

`tee` lê stdin, grava uma cópia em cada arquivo indicado e também envia os mesmos dados a stdout:

```bash
$ ls | tee listing.txt
```

Aqui, `listing.txt` recebe a listagem e stdout de `tee` continua conectada ao terminal. Por padrão, `tee` cria ou trunca o arquivo indicado, assim como `>`.

Use `-a` quando o conteúdo deva ser acrescentado ao arquivo em vez de substituí-lo:

```bash
$ date | tee -a activity.log
```

## Salvamento de um Resultado Intermediário

Coloque `tee` no meio de um pipeline para salvar um fluxo intermediário e continuar seu processamento:

```bash
$ ls -la /etc | tee etc-listing.txt | grep "conf"
```

Esse pipeline:

1. Produz a listagem longa completa.
2. Salva o fluxo completo em `etc-listing.txt`.
3. Envia o mesmo fluxo a `grep`, que mostra apenas as linhas que contêm `conf`.

O arquivo contém os dados anteriores ao filtro de `grep`. Se quiser salvar apenas as linhas filtradas, coloque `tee` depois de `grep`.

Para praticar pipelines e cópias de fluxos, experimente estes laboratórios:

1. **[Redirecionamento de Entrada e Saída no Linux](https://labex.io/labs/comptia-redirecting-input-and-output-in-linux-590840)** — Pratique o controle do fluxo de dados dos comandos manipulando a saída padrão (stdout), o erro padrão (stderr) e a entrada padrão (stdin) com operadores como `>`, `>>`, `2>` e o comando `tee`.
2. **[Controle de Sequência e Pipeline](https://labex.io/labs/linux-sequence-control-and-pipeline-17994)** — Aprenda a controlar sequências de comandos, usar pipelines e aproveitar ferramentas de texto como `cut`, `grep`, `wc`, `sort` e `uniq`.
3. **[Redirecionamento de Fluxos de Dados](https://labex.io/labs/linux-data-stream-redirection-17995)** — Aprenda a arte do redirecionamento de fluxos no Linux, incluindo a manipulação dos fluxos de entrada, saída e erro padrão, a combinação de saídas e o uso de `/dev/null`.