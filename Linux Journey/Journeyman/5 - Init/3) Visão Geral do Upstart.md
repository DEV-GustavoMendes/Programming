# Visão Geral do Upstart

Aprenda como o sistema init legado Upstart conecta expressões de eventos aos objetivos do ciclo de vida das tarefas.

O Upstart é um sistema legado de init e gerenciamento de serviços baseado em eventos, desenvolvido pela Canonical. Versões antigas do Ubuntu e várias outras distribuições o utilizaram, mas as versões atuais do Ubuntu usam systemd. Estude o Upstart ao manter um host legado confirmado, não como uma suposição padrão para uma instalação moderna.

## Confirmação de um Host Upstart Legado

Inspecione o PID 1 e a interface de controle ativa:

```bash
$ ps -p 1 -o pid,comm,args=
$ readlink /proc/1/exe
$ initctl version
```

O último comando só retorna informações significativas quando o serviço de controle e o cliente do Upstart estão presentes. Um diretório como `/usr/share/upstart` ou arquivos restantes em `/etc/init` são evidências fracas, pois pacotes e vestígios de migrações podem permanecer depois que outro sistema init assume o controle.

## Tarefas e Eventos

Uma **tarefa** do Upstart descreve um serviço ou trabalho, incluindo os comandos de seu processo e as condições do ciclo de vida. Um **evento** é uma notificação nomeada com variáveis de ambiente opcionais. A configuração da tarefa pode expressar quando seu objetivo deve se tornar iniciar ou interromper.

Os arquivos de tarefas do sistema normalmente ficam em `/etc/init/` com o sufixo `.conf`. Por exemplo:

```text
description "Example worker"
start on runlevel [2345]
stop on runlevel [016]
exec /usr/local/sbin/example-worker
```

Isso usa eventos de runlevels como entradas de compatibilidade. O Upstart também pode reagir a eventos do sistema de arquivos, dispositivos, rede ou definidos por aplicações, dependendo do que o sistema emite.

## Inicialização Orientada a Eventos

Durante a inicialização, o Upstart carrega as definições das tarefas e recebe eventos. As expressões `start on` ou `stop on` correspondentes atualizam os objetivos das tarefas; as transições podem emitir eventos adicionais que liberam outros trabalhos. Tarefas independentes podem progredir simultaneamente.

Esse modelo evita uma única sequência global de scripts codificada, mas pode ser difícil de diagnosticar quando os nomes dos eventos, a ordem e as condições são implícitos. Os eventos não são, por padrão, uma fila persistente de mensagens, portanto uma tarefa adicionada ou uma condição alterada posteriormente não deve presumir que todos os eventos anteriores serão repetidos.

## Migração e Compatibilidade

O systemd pode oferecer compatibilidade limitada com alguns scripts de serviços legados, mas não executa a sintaxe das tarefas do Upstart como unidades nativas do systemd. Ao migrar, traduza as condições do ciclo de vida, o ambiente, a política de respawn, o registro, as dependências e as semânticas de prontidão, em vez de renomear arquivos mecanicamente.

