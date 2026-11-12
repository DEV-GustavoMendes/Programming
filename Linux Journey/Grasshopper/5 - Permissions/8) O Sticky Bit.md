# O Sticky Bit

Aprenda como o sticky bit protege entradas em diretórios compartilhados com permissão de escrita, como `/tmp`.

Um diretório com permissão de escrita normalmente permite que um usuário autorizado remova ou renomeie as entradas contidas nele, mesmo quando esse usuário não é proprietário dos próprios arquivos. O sticky bit acrescenta uma restrição de propriedade que torna diretórios compartilhados com permissão de escrita mais seguros.

## Como o Sticky Bit Restringe a Remoção

Quando um diretório possui o sticky bit, o Linux geralmente permite que uma entrada seja removida ou renomeada apenas por um processo com privilégios adequados, pelo proprietário do diretório ou pelo proprietário da entrada. As permissões comuns de escrita e busca no diretório ainda são necessárias.

A restrição diz respeito às entradas do diretório. Ela não impede que o proprietário de um arquivo edite seu conteúdo quando as permissões do arquivo permitem essa operação, nem torna o diretório privado.

## Reconhecimento do Bit em `/tmp`

O diretório temporário do sistema é um exemplo comum:

```bash
$ ls -ld /tmp
drwxrwxrwt 17 root root 4096 Dec 15 11:45 /tmp
```

O `t` minúsculo final ocupa a posição de execução dos outros. Ele significa que tanto o sticky bit quanto a permissão de execução dos outros estão presentes. Um `T` maiúsculo significa que o sticky bit está definido, mas a permissão de execução dos outros está ausente.

Como `/tmp` normalmente permite escrita e busca para todos, vários usuários podem criar entradas nele. O sticky bit impede que um usuário comum remova as entradas de outro usuário apenas porque o diretório permite escrita para todos. As aplicações ainda devem criar objetos temporários com segurança, pois nomes previsíveis, links inseguros e modos de arquivos fracos geram riscos separados.

## Definição e Remoção do Sticky Bit

Defina o bit simbolicamente:

```bash
$ chmod +t shared-directory
```

Em um dígito octal inicial de bits especiais, o sticky contribui com `1`:

```bash
$ chmod 1777 shared-directory
```

O `1` inicial define o sticky, enquanto `777` fornece o modo comum. Esse modo é apropriado somente quando o diretório é compartilhado intencionalmente por todos os usuários locais. Para um diretório de equipe, permissões de grupo mais restritas podem ser preferíveis. Remova somente o sticky bit com `chmod -t shared-directory`.

## Verificação da Política Completa do Diretório

O sticky não concede acesso de escrita nem de busca; ele apenas restringe a remoção e a renomeação depois que as permissões comuns permitem modificar o diretório. Verifique em conjunto o proprietário, o grupo, o modo comum, as ACLs e o contexto de montagem do diretório. Teste com contas sem privilégios em um ambiente isolado, em vez de alterar `/tmp` em um sistema em uso.

Para praticar, crie um diretório compartilhado descartável, defina um modo comum apropriado e o sticky bit e teste a remoção de entradas com dois usuários sem privilégios. O laboratório [Exclusão e Movimentação de Arquivos](https://labex.io/labs/linux-delete-and-move-files-7777) pode reforçar as operações básicas de renomeação e exclusão.