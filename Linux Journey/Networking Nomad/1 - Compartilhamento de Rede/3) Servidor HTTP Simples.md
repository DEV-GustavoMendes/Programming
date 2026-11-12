# Servidor HTTP Simples

Aprenda a expor temporariamente um diretório controlado usando o servidor HTTP do Python.

O módulo `http.server` do Python pode servir arquivos estáticos para um teste curto ou transferência confiável. Não é servidor de produção e não oferece autenticação, autorização, TLS, limitação de taxa nem tratamento reforçado de tráfego hostil.

## Preparação do diretório compartilhado

Crie um diretório dedicado contendo apenas os arquivos destinados à exposição. Revise arquivos ocultos, links, permissões e metadados sensíveis. Não sirva o diretório pessoal, raiz de repositório, credenciais ou caminhos do sistema.

Use `--directory` para tornar a raiz explícita:

```bash
$ python3 -m http.server 8000 --directory /srv/temporary-share
```

Quando não há um arquivo de índice, o módulo normalmente gera uma listagem do diretório. Qualquer pessoa que consiga alcançar o socket em escuta pode enumerar e baixar o conteúdo servido.

## Controle do endereço de escuta

Vincule ao loopback quando somente o mesmo host deve acessar:

```bash
$ python3 -m http.server 8000 --bind 127.0.0.1 --directory /srv/temporary-share
```

Para compartilhar em rede confiável, vincule deliberadamente ao endereço apropriado e confirme o firewall. Sem bind restritivo, o comando costuma ouvir em todas as interfaces e pode expor o diretório além da rede pretendida.

## Teste do acesso

No host servidor, solicite um arquivo conhecido:

```bash
$ curl -f http://127.0.0.1:8000/example.txt
```

Para um teste remoto autorizado, use o endereço da interface selecionada em vez do loopback. Confirme tanto que o arquivo pretendido está acessível quanto que um arquivo fora da raiz de documentos não está. O sucesso no navegador por si só não comprova uma exposição apropriada nem a confidencialidade.

## Encerramento e limpeza

Execute o serviço temporário num terminal supervisionado e pare com `Ctrl-C` ao concluir. Confirme que o listener sumiu:

```bash
$ ss -ltn 'sport = :8000'
```

Remova cópias temporárias conforme a política e reverta regras de firewall. Para distribuição persistente, autenticada ou pública, use servidor mantido com controle de acesso e TLS.

