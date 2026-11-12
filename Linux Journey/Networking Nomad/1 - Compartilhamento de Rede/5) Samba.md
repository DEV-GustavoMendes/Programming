# Samba

Aprenda a configurar, validar, acessar e proteger um compartilhamento básico do Samba.

Samba implementa o protocolo Server Message Block em sistemas Unix-like, permitindo compartilhar arquivos e impressoras entre Linux, Windows, macOS e outros clientes. Implantações modernas usam dialetos SMB atuais; o termo antigo CIFS ainda aparece em ferramentas Linux, mas não é motivo para habilitar o obsoleto SMB1.

## Planejamento do compartilhamento

Antes de instalar ou mudar, defina clientes autorizados, identidades, leitura e escrita, zona de rede, proprietário dos dados, backup e dialeto exigido. Use um diretório dedicado, sem expor acidentalmente uma árvore pessoal ou do sistema.

O acesso é controlado pelas regras do Samba e pelas permissões do sistema de arquivos. Permitir escrita em `smb.conf` não concede acesso que a conta não possui localmente.

## Definição de um compartilhamento básico

A configuração principal costuma ser `/etc/samba/smb.conf`:

```ini
[team]
    path = /srv/samba/team
    browseable = yes
    read only = no
    valid users = @teamshare
```

Crie o diretório com propriedade e permissões revisadas:

```bash
$ sudo install -d -o root -g teamshare -m 2770 /srv/samba/team
```

O bit set-group-ID ajuda novas entradas a herdar o grupo do diretório, mas o acesso colaborativo também pode exigir uma ACL ou uma máscara de criação escolhida cuidadosamente. Teste os resultados reais dos arquivos e diretórios em vez de presumir que a herança é suficiente.

## Configuração da identidade

Num servidor standalone, a conta geralmente precisa de identidade Unix correspondente e credencial Samba habilitada:

```bash
$ sudo smbpasswd -a alice
```

Implantações em domínios de diretório usam outro projeto de identidade. Não coloque senhas no histórico do shell nem em configurações legíveis por usuários não relacionados e não presuma que uma senha do Samba seja automaticamente idêntica à senha da conta Unix.

## Validação e aplicação

Verifique a configuração interpretada antes de recarregar:

```bash
$ testparm -s
```

Revise padrões inesperados e erros e, em seguida, recarregue o serviço Samba da distribuição por meio de seu gerenciador de serviços. Os nomes dos serviços variam e normalmente incluem `smbd.service` ou `smb.service`. Um reload causa menos interrupção do que um restart quando há suporte, mas ainda assim verifique o estado, os sockets em escuta, o escopo do firewall e os logs.

Teste a partir de um cliente com um usuário explícito:

```bash
$ smbclient //server.example.net/team -U alice
```

## Montagem a partir do Linux

Clientes Linux usam o driver `cifs` e helpers. Evite senhas na linha de comando, pois vazam pelo histórico ou processos. Use arquivo de credenciais legível apenas pelo root ou mecanismo aprovado:

```bash
$ sudo mount -t cifs //server.example.net/team /mnt/team \
    -o credentials=/root/.smb-team,vers=3.1.1
```

Proteja o arquivo de credenciais, confirme o dialeto aceito pelos dois lados e defina deliberadamente os requisitos de UID, GID, permissões e criptografia. Depois da montagem, verifique-a com `findmnt`, faça testes autorizados de leitura e escrita e desmonte após coordenar os usuários ativos.

