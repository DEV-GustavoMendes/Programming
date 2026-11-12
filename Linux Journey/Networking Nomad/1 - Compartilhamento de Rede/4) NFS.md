# NFS

Aprenda a descobrir, montar, validar e automatizar com segurança uma montagem de cliente NFS.

Network File System permite que um cliente acesse um export do servidor pelo namespace local de arquivos. O servidor controla os exports e boa parte da política; o cliente controla onde e quando um export autorizado é montado.

## Preparação do cliente

Instale os utilitários de cliente NFS da distribuição, normalmente empacotados como `nfs-common` em sistemas da família Debian ou `nfs-utils` em sistemas da família Red Hat. Confirme a acessibilidade pelo DNS ou endereço, as versões NFS permitidas, a política do firewall e o caminho exato da exportação com o administrador do servidor.

`showmount -e SERVER` pode listar exports pelo protocolo antigo de montagem, mas não é autoridade para todo servidor somente NFSv4. Uma falha não prova que não exista export NFSv4 autorizado.

## Montagem de um export

Crie um ponto vazio e dedicado e monte o export aprovado:

```bash
$ sudo mkdir -p /mnt/team
$ sudo mount -t nfs server.example.net:/srv/team /mnt/team
```

Especifique uma versão somente quando a política ou a compatibilidade exigirem, por exemplo, `-o vers=4.2`. Não tente adivinhar opções de desempenho nem de segurança. Confirme a origem, o tipo e as opções resultantes:

```bash
$ findmnt --target /mnt/team
```

## Identidade e permissões

O acesso combina regras do export, segurança do protocolo, identidades numéricas ou serviços de diretório e permissões. Nomes iguais nos dois hosts não garantem IDs iguais. `AUTH_SYS` envia IDs fornecidos pelo cliente e depende de confiança no cliente e na rede; ambientes mais fortes podem usar Kerberos configurado de ponta a ponta.

O servidor costuma mapear root remoto para identidade sem privilégio por root squashing. Não desative essa proteção para corrigir um erro; examine IDs, propriedade, política e modelo de segurança.

## Automação de montagens de rede

Uma montagem simples de inicialização em `/etc/fstab` pode atrasar a inicialização quando a rede ou o servidor estão indisponíveis. Dependendo do host, use `autofs` para mapas sob demanda ou opções de montagem do systemd como `_netdev,nofail,x-systemd.automount`, depois de testar a semântica exata delas:

```fstab
server.example.net:/srv/team/mnt/team nfs4 rw,_netdev,nofail,x-systemd.automount 0 0
```

Antes de editar fstab, preserve recuperação e valide com parser não destrutivo ou teste controlado. Automount melhora disponibilidade, mas não corrige autorização, DNS ou indisponibilidade.

## Desmontagem e verificação

Antes de desmontar, coordene processos que usam o compartilhamento e conclua escritas. Depois desmonte e confirme:

```bash
$ sudo umount /mnt/team
$ findmnt --target /mnt/team
```

Desmontagem forçada ou lazy pode ocultar referências ativas e causar erros; reserve-a para falha diagnosticada com plano explícito.



