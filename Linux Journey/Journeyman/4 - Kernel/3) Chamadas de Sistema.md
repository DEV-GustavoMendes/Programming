# Chamadas de Sistema

Aprenda como o código do espaço do usuário invoca serviços do kernel Linux e como inspecionar chamadas com segurança usando `strace`.

Uma chamada de sistema é uma entrada definida no kernel pela qual o código do espaço do usuário solicita uma operação, como abrir um arquivo, mapear memória, criar um processo ou enviar dados pela rede. Antes de agir, o kernel valida argumentos, credenciais, estado do objeto e política de segurança.

## Bibliotecas e a ABI de chamadas de sistema

Aplicativos costumam chamar funções da biblioteca C, em vez de escrever instruções de entrada específicas da arquitetura. Um wrapper prepara registradores e memória conforme a ABI, entra no kernel e traduz o resultado para a convenção da linguagem.

A relação nem sempre é de uma função para uma syscall:

- uma função de biblioteca pode combinar várias chamadas
- algumas funções trabalham inteiramente no espaço do usuário
- uma função vDSO otimizada pode obter certos dados mantidos pelo kernel sem uma transição completa
- uma chamada pode sustentar várias APIs de alto nível

## Entrada e retorno do kernel

O wrapper coloca o número da chamada e seus argumentos nos locais definidos pela arquitetura, depois executa uma instrução como `syscall` em x86-64 ou `svc` em AArch64. O processador muda para um ponto privilegiado configurado e o kernel despacha a solicitação.

Ao terminar, o kernel retorna um valor ou indicação de erro. Wrappers da biblioteca C normalmente retornam `-1` e definem o `errno` local da thread. Outras linguagens expõem erros de formas diferentes.

Chamar toda entrada de “interrupção de software” é impreciso em arquiteturas atuais; traps, instruções rápidas e supervisor calls implementam transições controladas relacionadas, mas diferentes.

## Números e compatibilidade

Números e convenções de chamadas são específicos da arquitetura. A mesma chamada simbólica pode ter outro número ou layout em outra ABI. Versões do kernel podem acrescentar syscalls, enquanto ABIs estáveis procuram preservar comportamentos existentes.

Um processo não privilegiado não insere handlers arbitrários na tabela do kernel em execução. Ampliar a interface exige código do kernel e projeto cuidadoso da ABI. O seccomp pode filtrar chamadas permitidas, mas não cria novas implementações.

## Rastreamento com `strace`

Rastreie um comando simples e salve a saída separadamente:

```bash
$ strace -o trace.log -- ls
```

Siga processos-filhos autorizados com `-f` ou filtre a saída:

```bash
$ strace -f -e trace=%file -o trace.log -- command
```

`strace` pode revelar caminhos, argumentos, dados do ambiente, endereços de rede, fragmentos de arquivos e credenciais passadas indevidamente. Proteja os traces e remova-os conforme a política de dados.

## Interpretação cuidadosa dos traces

O rastreamento altera o tempo e pode causar sobrecarga. Uma chamada com falha pode ser uma sondagem esperada, e o erro final pode resultar de operação anterior ou política do aplicativo. Interprete descritores, relações entre processos e logs em conjunto.

Permissões e políticas de ptrace limitam os processos rastreáveis. Não anexe a processos de outro usuário ou de produção sem autorização; pausas e mudanças de tempo podem afetar o serviço.

