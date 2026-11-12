# Registro Geral

Aprenda a descobrir, filtrar, acompanhar e correlacionar logs gerais de sistemas Linux.

Logs gerais combinam avisos rotineiros, alertas e erros de várias fontes. São bons pontos de partida, mas nomes e conteúdo dos arquivos são escolhas da política de roteamento, não garantias universais do Linux.

## Localização da fonte relevante

Conforme distribuição e configuração, mensagens gerais podem aparecer em `/var/log/syslog`, `/var/log/messages`, no journal ou em vários destinos. Primeiro identifique o host e o intervalo do incidente, depois examine as fontes:

```bash
$ ls -lh /var/log
$ journalctl --since '2026-08-31 09:00' --until '2026-08-31 09:15'
```

Logs de aplicativos podem ficar em subdiretórios próprios ou serviços externos. Registros de autenticação, auditoria, pacotes, banco de dados e servidor web podem ser separados do fluxo geral.

## Inspeção de logs de texto

Use `less` para navegação controlada e `tail` para os registros mais novos:

```bash
$ sudo less /var/log/syslog
$ sudo tail -n 100 /var/log/messages
```

Acompanhe linhas novas durante uma reprodução limitada com `tail -F FILE`. `-F` tenta novamente quando o arquivo é substituído na rotação. Pare com `Ctrl-C` e não deixe sessões privilegiadas amplas abertas.

## Filtragem sem perder o contexto

Pesquise um arquivo ou intervalo limitado, em vez de canalizar imediatamente um fluxo ilimitado:

```bash
$ grep -n -C 3 'connection refused' /var/log/example.log
$ journalctl -u example.service --since '10 minutes ago' --grep='connection refused'
```

Maiúsculas, redação, limites de frequência e localização podem tornar a busca literal incompleta. Registre eventos bem-sucedidos e falhos e mantenha as linhas próximas, pois a causa pode preceder o erro visível.

## Inclusão de logs rotacionados

Um incidente pode atravessar o limite de uma rotação. Arquivos ativos, arquivos numerados e arquivos comprimidos podem conter partes diferentes da mesma sequência. Ferramentas como `zgrep` e `zless` leem arquivos comprimidos com gzip:

```bash
$ sudo zgrep -n 'connection refused' /var/log/example.log*.gz
```

Ordene pelos horários reais, não apenas pelos sufixos. Antes de copiar evidências, preserve metadados e restrinja acesso, pois logs podem conter dados pessoais ou credenciais.

