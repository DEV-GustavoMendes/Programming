# Inserção e Acréscimo de Texto no Vim

Aprenda como o Vim entra no modo de Inserção antes, depois, acima ou abaixo da posição atual do cursor.

No modo Normal, o Vim interpreta as teclas como comandos. O modo de Inserção insere o texto digitado no buffer. Vários comandos do modo Normal entram no modo de Inserção em posições diferentes, permitindo começar a digitar sem uma navegação separada.

Pressione `Esc` para sair do modo de Inserção e retornar ao modo Normal. Se não souber qual modo está ativo, pressionar `Esc` é uma forma segura de restabelecer o modo Normal, embora possa cancelar uma operação pendente.

## Inserção Antes ou Depois do Cursor

A partir do modo Normal:

- `i`: entra no modo de Inserção antes do cursor.
- `a`: entra no modo de Inserção depois do cursor.

Por exemplo, se o cursor estiver sobre `b` em `abc`, `i` começa antes de `b`, enquanto `a` começa depois. Os dois comandos mudam de modo; o texto digitado em seguida realiza a inserção.

## Inserção nos Limites da Linha

Comandos em maiúsculas indicam posições significativas da linha atual:

- `I`: entra no modo de Inserção antes do primeiro caractere não vazio.
- `A`: entra no modo de Inserção no final da linha.

Em uma linha recuada, `I` ignora o recuo e começa antes do primeiro texto não vazio. Use `0i` se precisar especificamente inserir na coluna zero.

## Abertura de uma Nova Linha

A partir do modo Normal:

- `o`: abre uma nova linha abaixo da atual e entra no modo de Inserção.
- `O`: abre uma nova linha acima da atual e entra no modo de Inserção.

O Vim aplica o recuo conforme as configurações atuais e as regras do tipo de arquivo. Uma quantidade pode repetir a operação de abertura, mas primeiro aprenda a forma de uma única linha para que a posição resultante do cursor seja previsível.

Para praticar a alternância entre os modos Normal e de Inserção, experimente este laboratório:

1. **[Edição de Arquivos de Texto no Linux com Vim e Nano](https://labex.io/labs/comptia-edit-text-files-in-linux-with-vim-and-nano-591076)** — Pratique a criação, edição, gravação e navegação de arquivos com vi/vim e nano e domine os fundamentos dos modos Normal e de Inserção.