# Processo de Inicialização: Init

Aprenda como o PID 1 inicializa o espaço do usuário, supervisiona serviços, recolhe processos-filhos e coordena o desligamento.

O kernel inicia o primeiro processo do espaço do usuário como PID 1 em um namespace de PIDs. Em um sistema Linux completo, esse processo init estabelece o ambiente de serviços. Em um container, o PID 1 pode ser um pequeno wrapper de init ou o próprio aplicativo, mas ainda tem responsabilidades especiais sobre sinais e processos-filhos.

## Responsabilidades do PID 1

Um sistema init geralmente:

- inicia e supervisiona serviços, logins, montagens e outras unidades de trabalho
- ordena o trabalho conforme dependências e o estado de destino configurado
- adota e recolhe processos-filhos órfãos
- reage a falhas de serviços conforme a política
- coordena desligamentos e reinicializações ordenados

O limite exato varia. Gerenciamento de dispositivos, rede, logs e tarefas agendadas podem ser programas separados supervisionados pelo init, não código incorporado ao PID 1.

## System V init e runlevels

O sysvinit tradicional usa configurações como `/etc/inittab` e scripts de inicialização e desligamento específicos de cada runlevel. Um runlevel representa um modo de operação, mas o significado dos níveis numerados pode variar entre distribuições. A ordem dos scripts segue convenções e pode ser ampliada ou paralelizada pelas ferramentas da distribuição.

Não deduza o init ativo apenas porque `/etc/init.d/` existe; scripts de compatibilidade podem permanecer em sistemas cujo PID 1 usa outra implementação.

## Sistemas baseados em eventos e dependências

O Upstart introduziu um modelo de jobs orientado a eventos e foi usado em versões antigas do Ubuntu e em outros sistemas. Hoje, ele tem interesse principalmente histórico ou para operação de sistemas legados.

O systemd é amplamente usado por distribuições atuais de uso geral. Ele modela serviços, sockets, montagens, temporizadores, dispositivos, targets e outros recursos como units. Dependências declarativas e mecanismos de ativação permitem que trabalhos independentes avancem em paralelo sem perder a ordem necessária.

Outros projetos ativos incluem OpenRC, runit, s6 e BusyBox init. “Mais recente” não é uma regra útil de compatibilidade; identifique o sistema realmente em execução e consulte sua documentação.

## Identificação do init em execução

Examine o PID 1 em vez de adivinhar pelos arquivos instalados:

```bash
$ ps -p 1 -o pid,comm,args=
$ readlink /proc/1/exe
```

Permissões, containers e namespaces afetam o que você vê. Um comando dentro de um container informa o PID 1 daquele namespace, não necessariamente o init do host. Depois de identificá-lo, use suas ferramentas nativas de estado e logs.

