# Gerenciamento de Arquivos de Log

Aprenda a configurar, testar e verificar com segurança a rotação de logs de texto usando logrotate.

Logs de texto sem limite podem esgotar um sistema de arquivos; exclusões agressivas podem remover evidências operacionais ou obrigatórias. `logrotate` aplica políticas de tamanho, tempo, compressão, propriedade e retenção a logs baseados em arquivos.

## Entendendo a rotação

Uma rotação típica renomeia o arquivo ativo, cria outro, opcionalmente pede ao aplicativo que o reabra, comprime gerações antigas e remove as excedentes. Tudo depende da configuração. Rotação não é backup: cópias retidas ainda podem ser apagadas, corrompidas ou perdidas com o host.

## Localização da configuração

O arquivo principal costuma ser `/etc/logrotate.conf`, com trechos de pacotes ou aplicações em `/etc/logrotate.d/`. Uma política simplificada pode ter esta aparência:

```text
/var/log/example/app.log {
    daily
    rotate 7
    compress
    delaycompress
    missingok
    notifempty
    create 0640 example adm
}
```

Isso solicita avaliação diária, sete rotações, compressão adiada por uma geração, tolerância a arquivo ausente ou vazio e um novo arquivo com modo e proprietário explícitos. A rotação real também depende do estado registrado e do agendador.

## Coordenação com o processo que escreve

Depois da renomeação, um daemon pode continuar escrevendo pelo descritor aberto. Um script `postrotate` costuma enviar um sinal documentado de reload ou reabertura. Confirme o comportamento do aplicativo e mantenha o script limitado.

`copytruncate` copia e trunca o original quando o aplicativo não consegue reabrir logs. Escritas podem ser perdidas ou duplicadas nessa janela; é um compromisso, não um padrão universalmente seguro.

## Teste antes da ativação

Use o modo debug para examinar decisões sem rotacionar:

```bash
$ sudo logrotate -d /etc/logrotate.conf
```

A saída de depuração não comprova que permissões, scripts, espaço livre ou a reabertura pela aplicação funcionarão durante uma execução real. Teste uma nova regra em um ambiente controlado e, depois da execução, examine o arquivo ativo, a geração rotacionada, a propriedade, a compressão, a saída da aplicação e o estado do logrotate. `-f` força uma rotação e altera o estado; não o confunda com uma execução de teste.

## Outros armazenamentos

Logrotate gerencia arquivos nomeados por suas políticas. O journal tem configuração própria de tamanho e retenção; bancos e serviços remotos têm outros controles. Monitore capacidade e saúde do logging para detectar writers travados ou rotação falha antes de esgotar espaço.

