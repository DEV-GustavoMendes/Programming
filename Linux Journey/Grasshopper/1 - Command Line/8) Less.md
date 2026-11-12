# less

Aprenda a navegar, pesquisar e acompanhar arquivos de texto longos de forma interativa com less.

Quando um arquivo de texto é longo demais para uma única tela, `less` permite lê-lo sem enviar o arquivo inteiro pela rolagem do terminal. Seu nome inspirou a antiga piada do Unix “less is more”, pois `more` é outro paginador.

## Abertura de um Arquivo

Inicie o paginador fornecendo um nome de arquivo:

```bash
$ less /home/pete/Documents/text1
```

Enquanto `less` estiver ativo, as teclas controlarão o paginador em vez de iniciar comandos comuns do shell. Você retornará ao shell quando sair do paginador.

## Navegação no less

Use estas teclas enquanto o paginador estiver aberto:

- Use `Seta para cima`, `Seta para baixo`, `Page Up` e `Page Down` para avançar por linhas ou telas.
- Pressione `g` para ir ao início.
- Pressione `G` para ir ao final.
- Pressione `u` para subir meia tela ou `d` para descer meia tela.
- Pressione `h` para abrir a ajuda integrada.

## Pesquisa no less

Digite `/`, seguido de um padrão, e pressione Enter para pesquisar para a frente. Comece com `?` para pesquisar para trás.

- `/search_term`: pesquisa `search_term` para a frente.
- `?search_term`: pesquisa `search_term` para trás.
- `n`: repete a pesquisa na mesma direção.
- `N`: repete a pesquisa na direção oposta.

## Saída do less

Pressione `q` para sair de `less` e retornar ao prompt do shell.

## Inicialização do less com Opções

Opções e comandos iniciais podem mudar a forma como o paginador começa:

```bash
$ less -N file.txt
$ less +G file.txt
$ less +F /var/log/syslog
```

- `-N`: mostra números de linha.
- `+G`: abre no final do arquivo.
- `+F`: acompanha o novo conteúdo conforme ele é acrescentado, de modo semelhante a `tail -f`.

Ao acompanhar um arquivo com `+F`, pressione `Ctrl+C` para interromper o acompanhamento e voltar à navegação normal; depois, pressione `q` para sair. Use `-i` para pesquisas que ignorem maiúsculas e minúsculas, a menos que o padrão contenha uma letra maiúscula, ou `-I` para ignorá-las independentemente do padrão.

Os comandos também podem enviar sua saída por um pipe para `less`:

```bash
$ dmesg | less
```

Para praticar paginação, pesquisa e leitura de textos do sistema, experimente estes laboratórios:

1. **[Comando less do Linux: Paginação de Arquivos](https://labex.io/labs/linux-linux-less-command-file-paging-214301)** — Aprenda a usar `less` para visualizar e percorrer arquivos de texto com eficiência, incluindo pesquisa, números de linha e correspondência de padrões.
2. **[Visualização de Arquivos de Log e Configuração no Linux](https://labex.io/labs/linux-viewing-log-and-configuration-files-in-linux-387914)** — Aprenda habilidades essenciais para visualizar e percorrer arquivos de texto, inclusive logs e configurações do sistema, usando comandos como `cat`, `more` e `less`.
