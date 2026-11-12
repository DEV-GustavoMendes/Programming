## O que É o Shell do Linux

Boas-vindas à sua jornada pelo Linux! O primeiro passo é entender o shell do Linux. Um shell é um programa que recebe os comandos digitados por você, pede ao sistema operacional que os execute e mostra o resultado no terminal.

Se você já usou uma interface gráfica, está acostumado a clicar em janelas, menus e botões. Na linha de comando, você digita instruções precisas. Aplicativos chamados “Terminal”, “Console” ou “Konsole” normalmente abrem uma sessão de shell para você.

O terminal é a janela ou o aplicativo em que você digita, enquanto o shell é o programa executado dentro dele.

O shell é útil porque é rápido, permite criar scripts e está disponível em quase todos os sistemas Linux. À medida que aprender mais comandos, você poderá combiná-los para inspecionar arquivos, gerenciar diretórios, pesquisar textos, instalar programas e automatizar tarefas repetitivas.

**Macete: O Terminal oferece a janela, enquanto o shell é executado dentro dela.**

## Interação com o Shell Bash

Neste curso, vamos nos concentrar no Bash, abreviação de Bourne Again Shell. O Bash é um dos shells mais comuns no Linux e oferece uma boa base, mesmo que mais tarde você use `zsh`, `fish` ou outro shell.

Ao abrir um terminal, você verá o prompt do shell. Sua aparência pode variar, mas ele costuma mostrar o nome do usuário, o nome da máquina e o diretório atual.

```plaintext
pete@icebox:/home/pete $
```

O símbolo `$` indica que o shell está pronto para receber sua entrada como um usuário comum. Você não digita esse símbolo ao inserir comandos; ele é mostrado pelo shell. Se vir `#`, normalmente estará trabalhando como usuário root, que tem mais poder e também envolve mais riscos.

Os comandos geralmente seguem este padrão:

```bash
command options arguments
```

Por exemplo, em `echo Hello World`, `echo` é o comando e `Hello World` é o texto fornecido a ele.

## Seu Primeiro Comando Linux

Vamos começar com um dos comandos Linux mais básicos para iniciantes: `echo`. Esse comando exibe no terminal o texto fornecido por você.

```bash
$ echo Hello World
Hello World
```

Experimente mais alguns exemplos:

```bash
$ echo Linux is fun
Linux is fun
$ echo "Hello from Bash"
Hello from Bash
```

**As aspas são úteis quando você quer que o shell trate várias palavras como um único trecho de texto.**

Para praticar essas habilidades, explore a abrangente [Trilha de Aprendizado do Shell](https://labex.io/learn/shell).

## Dicas Comuns para Iniciantes

- Pressione `Enter` para executar um comando.
- Use a tecla `Seta para cima` para recuperar um comando anterior.
- Comandos e nomes de arquivos diferenciam maiúsculas de minúsculas no Linux.
- Os espaços importam. `echo hello` e `echohello` são diferentes.
- Se um comando parecer travado, `Ctrl-C` geralmente o cancela.