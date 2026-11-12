# Visão Geral do System V

Aprenda como o init tradicional System V usa runlevels e links ordenados para scripts de serviços.

O init System V, normalmente chamado de SysV init ou sysvinit, é um projeto tradicional de PID 1 e inicialização de serviços. Ele continua importante em sistemas legados e por meio de scripts de compatibilidade, mas a presença de arquivos no estilo SysV não comprova que o sysvinit seja o PID 1 em execução.

## Identificação do Sistema Init Ativo

Inspecione o PID 1 ativo:

```bash
$ ps -p 1 -o pid,comm,args=
$ readlink /proc/1/exe
```

Um arquivo `/etc/inittab` ou diretório `/etc/init.d/` é apenas uma evidência auxiliar. O systemd e outros sistemas init podem manter esses arquivos para compatibilidade, e contêineres podem mostrar um namespace de PIDs diferente do host.

## Runlevels

Um runlevel é um modo operacional numérico nomeado. As configurações SysV tradicionalmente usam os níveis `0` a `6` e níveis especiais, mas seus significados são políticas da distribuição, não uma regra universal. Algumas convenções comuns são:

- `0`: transição para parada ou desligamento
- `1` ou `S`: modo de usuário único ou resgate
- `2` a `5`: modos multiusuário definidos pela distribuição
- `6`: transição para reinicialização

Historicamente, sistemas da família Debian tratam os níveis 2 a 5 de forma semelhante, enquanto as convenções da família Red Hat diferenciam os modos de texto e gráfico. Inspecione `/etc/inittab`, a documentação do init e os diretórios de runlevels no host real.

## Scripts Init e Links de Runlevels

Os scripts de serviços normalmente ficam em `/etc/init.d/`. Diretórios de runlevels, como `/etc/rc2.d/` ou `/etc/rc.d/rc2.d/`, contêm links cujos nomes codificam a ação e a ordem da transição:

- Links `SNNname` solicitam uma ação de início.
- Links `KNNname` solicitam uma ação de parada.
- `NN` fornece uma ordenação lexical entre os links daquela transição.

O algoritmo e os diretórios exatos variam. As dependências também podem ser expressas nos cabeçalhos dos scripts e processadas por ferramentas da distribuição, e algumas implementações paralelizam o trabalho. O SysV não deve ser reduzido a uma garantia de que todos os serviços sejam iniciados estritamente um por vez.

## Transição entre Runlevels

Quando o init muda de runlevel, o mecanismo rc da distribuição interrompe os serviços que deixaram de ser necessários e inicia os exigidos pelo novo modo. Os scripts precisam ser suficientemente idempotentes para lidar com operações repetidas de estado ou transição e retornar status significativos.

Solicitar o runlevel 0 ou 6 é uma ação destrutiva de disponibilidade para todo o sistema. Use a interface de desligamento do sistema, notifique os usuários, preserve o trabalho ativo e verifique o acesso remoto ao console, em vez de invocar casualmente transições brutas do init.

## Limites da Compatibilidade

Em um host com systemd, os scripts SysV podem ser encapsulados como unidades geradas, mas as dependências, os timeouts, o registro e as semânticas de estado do systemd ainda se aplicam. Executar diretamente um script legado pode ignorar o acompanhamento do gerenciador de serviços. Identifique o gerenciador ativo e use sua interface nativa quando possível.

