# Visão Geral do Compartilhamento de Arquivos

Aprenda a escolher e executar com segurança uma transferência de arquivos por SSH usando scp.

A movimentação de arquivos em rede varia de cópias únicas a compartilhamentos montados continuamente e árvores sincronizadas. Escolha conforme direção, tamanho, frequência, modelo de identidade, confiança da rede, metadados e necessidade de acesso compartilhado ativo.

## Escolha do método de transferência

- `scp` ou SFTP oferece cópia autenticada por SSH ou transferência interativa.
- `rsync` reconcilia árvores eficientemente, localmente ou por SSH.
- NFS apresenta exports como sistemas montados, normalmente entre hosts Unix-like.
- SMB, implementado pelo Samba, oferece acesso compartilhado entre vários sistemas.
- HTTP fornece downloads simples, mas não é um sistema montado geral.

Uma cópia não é automaticamente um backup. Backup também exige retenção independente, testes de restauração, integridade e proteção contra a mesma exclusão ou invasão.

## Compreensão dos caminhos do scp

A forma geral é `scp SOURCE DESTINATION`. Um operando remoto costuma usar `user@host:path`:

```bash
$ scp -- report.txt alice@example.net:/srv/incoming/
$ scp -- alice@example.net:/srv/outgoing/result.txt ./result.txt
```

O primeiro envia um arquivo local; o segundo busca um remoto. Dois-pontos separam host e caminho. Coloque entre aspas caminhos com caracteres do shell e evite nomes não confiáveis ambíguos.

## Cópia de um diretório

Use modo recursivo para uma árvore:

```bash
$ scp -r -- project/ alice@example.net:/srv/incoming/
```

Antes de copiar, examine o tamanho dos dados, os links simbólicos, as permissões, os requisitos de propriedade, o espaço livre e os nomes no destino. O SCP não é uma política de sincronização; cópias repetidas de diretórios podem deixar no destino arquivos que já não existem na origem.

## Verificação da identidade e dos resultados

A verificação da chave do host SSH protege contra conexão ao servidor errado. Trate uma chave alterada como evento a confirmar por canal confiável, sem ignorar o aviso. Use contas de privilégio mínimo e proteja as chaves.

Depois da transferência, verifique o status de saída, os arquivos esperados, os tamanhos, os metadados e, quando os requisitos de integridade exigirem, hashes calculados independentemente nos dois lados. Confirme que a aplicação de destino realmente consegue ler os dados.

