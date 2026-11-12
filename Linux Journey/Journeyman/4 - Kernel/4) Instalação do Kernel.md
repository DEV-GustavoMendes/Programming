# Instalação do Kernel

Aprenda a instalar, inicializar, verificar e manter um kernel da distribuição com uma alternativa testada.

As distribuições empacotam kernels com módulos, integração do initramfs, atualizações do carregador, assinaturas e política de suporte. Use esse fluxo gerenciado, salvo quando estiver desenvolvendo ou testando deliberadamente um kernel personalizado e souber recuperar a máquina.

## Kernels em execução e instalados

Veja a versão do kernel em execução:

```bash
$ uname -r
6.8.0-00-generic
```

Isso não lista todos os kernels instalados nem muda assim que um pacote novo é instalado. O sistema precisa iniciar a nova imagem antes que `uname -r` a informe. Consulte pacotes e entradas de boot com as ferramentas da distribuição.

## Prefira o pacote de acompanhamento da distribuição

Instale ou mantenha o meta-pacote de kernel compatível para continuar recebendo atualizações de segurança. Os nomes dependem da versão, arquitetura, hardware e variante. O Ubuntu costuma oferecer `linux-generic`, mas sistemas cloud, low-latency, HWE, OEM, real-time e de outras arquiteturas usam pacotes diferentes.

Não transforme diretamente uma string de versão de `uname -r` em um operando de `apt install` presumindo que ela seja válida. Consulte a documentação atual da distribuição e examine os candidatos com o gerenciador de pacotes antes da instalação.

## Preparação da alteração

Antes da transação:

1. Confirme repositórios, assinaturas, ciclo de suporte e variante pretendida.
2. Garanta espaço suficiente em `/boot` ou na ESP.
3. Preserve um kernel funcional e uma entrada selecionável.
4. Verifique console, gerenciamento remoto, mídia de resgate, chaves de recuperação e rollback.
5. Confira módulos fora da árvore, drivers de armazenamento e rede, assinatura para Secure Boot, hibernação e compatibilidade com virtualização.

A transação deve gerar um initramfs correspondente e atualizar entradas pelos hooks da distribuição. Leia todo erro; o pacote constar como instalado não basta se a geração falhou.

## Inicialização e validação

Agende uma reinicialização controlada, considerando as partes interessadas e as cargas de trabalho ativas. Garanta que o console possa escolher a entrada antiga caso a padrão falhe. Depois do boot:

```bash
$ uname -r
$ journalctl -k -b
$ systemctl --failed
```

Use equivalentes em sistemas sem systemd. Valide armazenamento, rede, gráficos, entrada, módulos de segurança, módulos externos, containers, VMs e aplicativos. Um prompt de login não é validação completa.

## Remoção de kernels antigos

Use o fluxo de limpeza do gerenciador somente depois de validar o novo kernel. Nunca remova o kernel em execução, a única alternativa funcional nem pacotes exigidos pelo meta-pacote ativo. Revise a remoção e as entradas resultantes.

Apagar manualmente arquivos de `/boot` deixa pacotes e carregador inconsistentes. Se já não houver espaço, prepare a recuperação antes de mudar arquivos.

O laboratório [Personalizar o Menu de Inicialização GRUB2 no Linux](https://labex.io/labs/comptia-customize-the-grub2-boot-menu-in-linux-590859) oferece um ambiente seguro para entender várias entradas.