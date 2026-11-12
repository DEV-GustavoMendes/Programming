# /etc/fstab

Aprenda a definir associações persistentes de sistemas de arquivos e swap em `/etc/fstab` e validá-las com segurança.

`/etc/fstab`, a tabela de sistemas de arquivos, declara sistemas de arquivos, áreas de swap, montagens bind, origens de rede e outras associações que as ferramentas do sistema podem montar ou ativar. As entradas podem participar da inicialização, mas opções como `noauto`, a integração com montagens automáticas e a política do gerenciador de serviços afetam quando ou se isso acontece.

## Os Seis Campos

Uma entrada convencional possui seis campos separados por espaços em branco:

```text
UUID=130b882f-7d79-436d-a096-1e594c92bb76 /data ext4 defaults,nosuid,nodev 0 2
```

1. **Origem**: um caminho de dispositivo, `UUID=`, `LABEL=`, uma origem de rede ou outra especificação compatível.
2. **Destino**: o ponto de montagem, ou `none` para usos como swap quando apropriado.
3. **Tipo**: o tipo do sistema de arquivos, `swap`, `none` ou um tipo automático aceito.
4. **Opções**: uma lista separada por vírgulas interpretada pelos auxiliares de montagem e camadas de integração.
5. **Campo dump**: controla historicamente o utilitário de backup `dump`; `0` normalmente desabilita a participação.
6. **Campo pass**: controla a ordem de `fsck` durante o boot quando aplicável; `0` desabilita a verificação automática por esse mecanismo.

Os espaços em branco dentro de um campo devem ser escapados com a sintaxe do fstab, como `\040` para um espaço. Um `#` inicia um comentário fora de um campo.

## Identificadores Estáveis da Origem

Para sistemas de arquivos locais, um UUID de sistema de arquivos costuma ser mais estável que a enumeração `/dev/sdX`:

```bash
$ lsblk -f
$ sudo blkid
```

Use `UUID=...` somente depois de confirmar que o identificador pertence ao sistema de arquivos pretendido. A reformatação cria um novo UUID, e clones no nível dos blocos podem duplicá-lo. Por sua vez, `PARTUUID=` identifica uma entrada da tabela de partições e possui uma semântica diferente.

## Opções de Montagem e Campos de Verificação

`defaults` se expande para um conjunto convencional de opções definido pela implementação; ele não é necessariamente a política mais segura para todas as montagens. Adicione opções com base na confiança e na carga de trabalho, como acesso somente para leitura ou restrições a nós de dispositivos e ao comportamento setuid. Sistemas de arquivos de rede e removíveis podem exigir políticas de timeout, dependências ou tolerância a falhas para que o boot não fique bloqueado inesperadamente.

Nos sistemas de arquivos compatíveis com `fsck`, o sistema de arquivos raiz usa convencionalmente pass `1`, e outros sistemas de arquivos locais verificados usam pass `2`. As práticas específicas de cada sistema podem ser diferentes — por exemplo, alguns tipos não usam o fsck genérico durante o boot — portanto siga a documentação do sistema de arquivos instalado e da distribuição, em vez de atribuir `2` mecanicamente.

## Edição com um Caminho de Recuperação

Uma entrada inválida para a raiz, o boot ou uma rede obrigatória pode interromper a inicialização. Antes de editar:

1. Confirme um backup atual e o acesso pelo console ou por um modo de resgate.
2. Copie o arquivo existente preservando as permissões.
3. Verifique a identidade da origem e crie o ponto de montagem pretendido.
4. Faça uma única alteração de escopo restrito.
5. Valide e teste antes de reiniciar.

Não coloque credenciais diretamente em uma entrada do fstab legível por todos. Use o mecanismo protegido de credenciais do auxiliar de montagem correspondente.

## Validação sem Presumir o Sucesso

Comece com uma verificação estática quando houver suporte:

```bash
$ sudo findmnt --verify --verbose
```

Em seguida, teste a nova entrada específica sob condições controladas, confirme-a com `findmnt` e desmonte se o teste for temporário. `mount -a` tenta várias entradas elegíveis e pode acessar redes ou anexar origens não pretendidas; ele também ignora entradas já montadas e com `noauto`, portanto não é um verificador inofensivo de sintaxe nem uma prova completa.

Em sistemas baseados no systemd, recarregue a configuração do gerenciador após editar o fstab para atualizar as unidades de montagem geradas e verifique as dependências e o comportamento do boot conforme a documentação local.

Pratique no laboratório [Gerenciamento de Partições e Sistemas de Arquivos Linux](https://labex.io/labs/comptia-manage-linux-partitions-and-filesystems-590845) com o armazenamento secundário seguro para recuperação fornecido pelo laboratório.