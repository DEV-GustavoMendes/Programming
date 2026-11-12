# Módulos do Kernel

Aprenda a inspecionar, carregar, configurar e remover com segurança módulos específicos da versão do kernel Linux.

Um módulo carregável é código privilegiado que amplia o kernel em execução com um driver, sistema de arquivos, recurso de rede ou outro subsistema. Os módulos evitam incorporar todo recurso opcional à imagem principal, mas cada módulo carregado aumenta a superfície confiável de ataque.

## Listagem e inspeção de módulos

Liste os módulos carregados:

```bash
$ lsmod
```

A saída deriva do estado do kernel, como `/proc/modules`, e inclui nome, tamanho e contagem de uso ou dependências. Uma contagem aparentemente zero não prova que a remoção é segura; um driver ainda pode controlar dispositivos ativos ou participar do estado de um subsistema.

Inspecione um módulo disponível para o kernel atual com:

```bash
$ modinfo MODULE_NAME
```

`modinfo` pode mostrar arquivo, aliases, parâmetros, licença, descrição e assinatura. Esses metadados são descritivos, não prova de confiança ou compatibilidade com a carga de trabalho.

## Carregamento com `modprobe`

Carregue um módulo pelo nome:

```bash
$ sudo modprobe MODULE_NAME
```

`modprobe` consulta índices de dependências, aliases e configurações do kernel atual em `/lib/modules/$(uname -r)/`. Ele carrega dependências e passa parâmetros configurados. Já `insmod` insere diretamente um arquivo específico e não oferece o mesmo fluxo de resolução.

Antes de carregar, confirme procedência, política de assinatura, compatibilidade da versão, parâmetros, vínculo esperado com hardware e rollback. Secure Boot ou lockdown podem rejeitar módulos não assinados; forçar código incompatível arrisca travamento ou comprometimento.

## Parâmetros e carregamento no boot

Parâmetros e aliases persistentes ficam em um arquivo `.conf` de `/etc/modprobe.d/`:

```text
options example_module mode=careful
```

Essa linha afeta como o módulo é carregado; sozinha, não solicita carregamento no boot. Uma lista simples para o boot costuma ficar em `/etc/modules-load.d/`:

```text
example_module
```

Aliases de hardware muitas vezes acionam carregamento automático sem lista explícita. Para módulos necessários no início do boot, atualize o initramfs pelo processo documentado da distribuição.

## Blacklist e suas limitações

Uma configuração pode conter:

```text
blacklist example_module
```

Isso normalmente suprime o carregamento automático pelos aliases. Não descarrega um módulo ativo, não o remove do initramfs nem necessariamente impede uma carga explícita pelo nome ou como dependência. Hardening exige uma combinação específica de disponibilidade, assinaturas, initramfs, parâmetros de boot e política.

## Remoção segura de um módulo

Solicite a remoção com:

```bash
$ sudo modprobe -r MODULE_NAME
```

Modprobe pode remover dependências que ficaram sem uso. O kernel recusa quando o rastreamento comum mostra que o módulo está ocupado, mas isso não deve ser a única verificação. Pare serviços, desmonte sistemas de arquivos, desconecte dispositivos, estabilize a rede e confirme outro driver ou caminho de recuperação.

Nunca force a remoção em um sistema que precisa preservar. Falhas ou atividades pendentes podem travar o kernel ou corromper dados.

Use [Gerenciar Módulos do Kernel no Linux](https://labex.io/labs/comptia-manage-kernel-modules-in-linux-590865) para praticar com módulos definidos como seguros pelo laboratório.