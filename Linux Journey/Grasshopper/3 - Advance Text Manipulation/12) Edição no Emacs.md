# Edição no Emacs

Aprenda a mover o ponto, ativar uma região e usar os comandos do kill ring do Emacs para editar texto.

O Emacs chama a posição atual do cursor de **ponto**. Comandos de movimento reposicionam o ponto; comandos de edição inserem, excluem, recortam, copiam ou colam texto ao redor dele. Na notação abaixo, `C-` significa Control e `M-` significa Meta, normalmente Alt.

## Movimento por Caracteres e Linhas

As teclas de seta e outras teclas de navegação da plataforma podem funcionar, mas os comandos de movimento padrão do Emacs permanecem disponíveis em sessões de terminal e gráficas:

- `C-f`: avança um caractere.
- `C-b`: recua um caractere.
- `C-n`: vai à próxima linha.
- `C-p`: vai à linha anterior.
- `C-a`: vai ao início da linha.
- `C-e`: vai ao final da linha.

## Movimento por Palavras e Limites do Buffer

Comandos Meta se movem por unidades maiores:

- `M-f`: avança uma palavra.
- `M-b`: recua uma palavra.
- `M-<`: vai ao início do buffer.
- `M->`: vai ao final do buffer.

Em muitos teclados, Alt atua como Meta. Quando essa combinação não está disponível, pressionar `Esc` e depois a tecla seguinte frequentemente envia o comando Meta equivalente.

## Definição de uma Região

A **marca** é uma posição salva no buffer. O texto entre o ponto e a marca é a **região**. Pressione `C-SPC`, escrito como `C-space` em algumas documentações, para executar `set-mark-command`; depois, mova o ponto para ampliar a região ativa.

Em um terminal, `C-SPC` pode ser codificado como `C-@`. O destaque depende das configurações de transient-mark, mas o ponto e a marca ainda definem uma região.

## Recorte ou Cópia de uma Região

O Emacs armazena texto recortado e copiado no **kill ring**:

- `C-w`: recorta a região ativa, removendo-a e acrescentando-a ao kill ring.
- `M-w`: copia a região ativa para o kill ring sem removê-la.
- `C-k`: recorta do ponto até o final da linha; o uso repetido pode incluir a nova linha.

Recortar é mais do que uma exclusão comum, pois o texto removido é preservado para uma colagem posterior.

## Colagem a Partir do Kill Ring

Use `C-y` para colar no ponto a entrada mais recente do kill ring. Imediatamente depois de uma colagem, `M-y` substitui o texto inserido por uma entrada anterior; repetir `M-y` percorre as entradas.

```text
C-y
M-y
```

Se outro comando não relacionado ocorrer depois de `C-y`, `M-y` deixará de ter o mesmo contexto de yank-pop.

Pratique em `*scratch*` ou em um arquivo descartável: mova o ponto, defina a marca, copie uma região, recorte outra e cole as duas novamente. Salve apenas quando o arquivo resultante merecer ser preservado.