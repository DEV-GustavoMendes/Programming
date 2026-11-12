# man

Aprenda a abrir, navegar, pesquisar e selecionar seções das páginas de manual instaladas.

Muitos comandos, interfaces, arquivos de configuração e ferramentas administrativas do Linux possuem uma documentação de referência instalada chamada página de manual, ou página man. O comando `man` localiza e exibe essas páginas.

## Abertura de uma Página de Manual

Forneça o nome de um tópico a `man`. Por exemplo, abra a página de `ls` com:

```bash
$ man ls
```

As páginas de manual normalmente incluem uma sinopse, descrição, opções, arquivos relacionados e referências cruzadas, embora as seções exatas variem.

## Navegação e Pesquisa em uma Página

Em muitos sistemas, `man` exibe as páginas por meio de um paginador como `less`. Enquanto uma página estiver aberta, você poderá rolar com as teclas de seta ou de página e usar estes controles:

Dentro de uma página man:

- Digite `/pattern` e pressione Enter para pesquisar para a frente.
- Pressione `n` para repetir a pesquisa na mesma direção.
- Pressione `N` para repeti-la na direção oposta.
- Pressione `q` para sair.

O paginador pode variar conforme o sistema ou ambiente; por isso, suas teclas exatas não são garantidas em todos os lugares. Os controles acima se aplicam à configuração comum com `less`.

## Seleção de uma Seção do Manual

O manual é organizado em seções numeradas. Algumas seções comuns são:

- `1`: comandos de usuário.
- `2`: chamadas de sistema.
- `3`: funções de biblioteca.
- `5`: formatos de arquivos.
- `8`: comandos de administração do sistema.

O mesmo tópico pode aparecer em mais de uma seção. Coloque a seção antes do tópico para selecionar uma explicitamente:

```bash
$ man 5 passwd
$ man 1 passwd
```

O primeiro comando abre a página do formato de arquivo `passwd` na seção 5. O segundo abre a página do comando de usuário na seção 1. Referências como `passwd(5)` usam a mesma notação `tópico(seção)`.

## Quando uma Página Está Ausente

Nem todo nome de comando possui uma página de manual instalada separadamente. Se `man` informar que não existe uma entrada:

- Execute `type NAME` para descobrir como o Bash resolve o nome.
- Use `help NAME` quando for um comando interno do Bash.
- Tente `NAME --help` quando um programa externo aceitar essa convenção.
- Verifique se a distribuição oferece um pacote separado de documentação.