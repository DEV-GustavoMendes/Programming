# traceroute

Aprenda como traceroute descobre saltos que respondem e como interpretar lacunas, tempos e variações de caminho.

`traceroute` envia sondagens com valores crescentes de TTL IPv4 ou Hop Limit IPv6. Roteadores onde o valor expira podem devolver Time Exceeded, revelando alguns pontos que respondem no caminho de ida.

## Como a descoberta de saltos funciona

As sondagens começam com limite um e aumentam. O primeiro roteador reduz para zero e pode retornar erro. Limite dois chega ao segundo antes de expirar, e o processo continua até resposta do destino ou limite máximo.

## Métodos de sondagem

O traceroute Linux tradicional costuma enviar UDP a portas altas. O destino pode encerrar com ICMP Port Unreachable. Opções usam ICMP Echo ou TCP SYN, que atravessam filtros de modo diferente:

```bash
$ traceroute -n example.com
$ traceroute -I -n example.com
$ traceroute -T -p 443 -n example.com
```

Os privilégios e as opções disponíveis variam. Use métodos autorizados para o destino e registre o método ao comparar os resultados.

## Interpretação dos asteriscos

Um asterisco significa que não se observou resposta antes do timeout. O roteador pode encaminhar tráfego enquanto filtra ou limita respostas diagnósticas. Se saltos posteriores respondem, o silencioso encaminhou ao menos algumas sondagens.

## Tempo e variação do caminho

Os tempos por salto medem ida e volta das respostas de controle, não latência acrescentada entre duas linhas. Roteadores podem despriorizar respostas. Balanceamento pode enviar sondagens por caminhos diferentes, e resolução de nomes acrescentar atraso; `-n` evita consultas reversas.

O retorno de cada resposta pode diferir do caminho de ida. Repita testes e correlacione com o tempo do aplicativo antes de apontar um gargalo.

## Comparação com o aplicativo

Traceroute pode chegar ao destino enquanto o serviço está bloqueado, e o serviço pode funcionar enquanto roteadores ocultam respostas. Teste a mesma família, destino, protocolo e porta do aplicativo e use traceroute como evidência auxiliar.

