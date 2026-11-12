# Níveis de Privilégio

Aprenda como o privilégio do processador separa a execução de usuário da execução confiável do kernel.

Os processadores oferecem modos de privilégio que restringem instruções sensíveis e acesso à memória. O Linux usa esse limite do hardware para que falhas comuns em aplicativos não sobrescrevam diretamente a memória do kernel nem reconfigurem dispositivos. O kernel controla as transições para a execução privilegiada.

## Modo usuário

Um processo comum é executado em modo usuário dentro de seu espaço de endereços virtual. Ele pode realizar cálculos e acessar os mapeamentos concedidos pelo kernel, que podem ser grandes; modo usuário não significa “apenas pouca memória”. Ele não acessa diretamente qualquer endereço físico, mapeamentos privados de outro processo nem controles privilegiados do processador.

Tabelas de páginas e bits de proteção impõem o acesso. Se uma thread referencia um endereço inválido ou proibido, o processador entra no kernel, que pode resolver um page fault válido ou entregar um sinal como `SIGSEGV`.

## Modo kernel

O modo kernel permite instruções privilegiadas e acesso aos mapeamentos protegidos necessários para memória, escalonamento, interrupções e drivers. Em x86, a divisão do Linux costuma ser descrita como ring 0 para o kernel e ring 3 para processos de usuário. O Linux normalmente não usa rings 1 e 2 para isolamento comum.

Outras arquiteturas têm nomes e mecanismos diferentes, como exception levels. A virtualização acrescenta relações entre hypervisor e guests que não cabem num desenho simples de dois anéis. A ideia essencial é o privilégio controlado, não os números do x86.

## Transições controladas

Vários eventos transferem o controle a um ponto de entrada do kernel:

- uma instrução de chamada de sistema solicita um serviço
- uma exceção informa um page fault ou instrução inválida
- uma interrupção de hardware informa um evento externo

O processador salva o contexto, muda o privilégio pelo mecanismo configurado e começa a executar código confiável do kernel. O kernel valida o pedido, realiza ou rejeita o trabalho e retorna ao modo usuário quando apropriado.

O aplicativo não se transforma temporariamente em código do kernel. A CPU executa um handler do kernel em nome da thread, com pilhas e mapeamentos controlados pelo kernel.

## Privilégio da CPU não é identidade do usuário

Um aplicativo executado como usuário `root` continua normalmente em modo usuário. O UID 0 influencia verificações de autorização, mas não permite que suas instruções acessem diretamente a memória do kernel. Inversamente, o código do kernel executa em modo privilegiado qualquer que seja o usuário que causou a chamada.

Capabilities, namespaces, seccomp, módulos de segurança e cgroups restringem ainda mais o que um processo solicita. Essa política em camadas é separada do limite de hardware.

## Por que o limite importa

O limite reduz o dano de falhas comuns e cria um ponto para verificações de acesso, mas vulnerabilidades no kernel e módulos maliciosos podem derrotá-lo. Atualize kernel e firmware por canais confiáveis, reduza o código privilegiado e não carregue módulos desconhecidos.

Execução especulativa e canais laterais também mostram que o isolamento exige mitigação contínua; “outro anel” é uma base, não uma prova completa de segurança.

