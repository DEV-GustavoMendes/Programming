# Sinais

Aprenda como o Linux gera, bloqueia, entrega e trata sinais para controle de processos e notificação de eventos.

Um sinal é uma notificação assíncrona entregue a um processo ou a uma thread específica. Os sinais informam eventos e solicitam ações, mas transportam apenas informações limitadas em comparação com mecanismos de comunicação entre processos voltados a dados.

## Origens dos Sinais

Os sinais podem vir de vários lugares:

- Um terminal pode gerar `SIGINT` para `Ctrl-C` ou `SIGTSTP` para `Ctrl-Z` e direcioná-lo ao grupo de processos em primeiro plano.
- O kernel pode gerar um sinal síncrono, como `SIGSEGV`, quando uma thread faz uma referência inválida à memória.
- Um processo pode enviar um sinal autorizado para outro processo ou grupo de processos.
- Temporizadores, alterações no estado de filhos e hangups de terminais podem gerar outros sinais.

O remetente precisa possuir as permissões adequadas, normalmente baseadas em credenciais ou capacidades. Portanto, os sinais são uma interface de controle intermediada pelo kernel, não mensagens irrestritas entre usuários arbitrários.

## Disposições e Ações Padrão

A maioria dos sinais possui uma disposição válida para todo o processo que seleciona uma de três respostas:

- realizar a ação padrão definida pelo sinal
- ignorar o sinal
- invocar um manipulador instalado pelo usuário

As ações padrão variam: um sinal pode encerrar, encerrar e criar um core dump, interromper, continuar ou ser ignorado. Capturar `SIGTERM` pode permitir que um programa inicie um encerramento ordenado, mas um manipulador deve seguir regras rígidas de segurança assíncrona de sinais, e o programa ainda pode adiar ou se recusar a terminar.

Os nomes dos sinais são mais portáveis e legíveis que os números. Embora arquiteturas Linux comuns usem 15 para `SIGTERM`, não presuma que todos os números de sinais, exceto os garantidos pelo padrão relevante, sejam idênticos em todos os lugares. Use `kill -l` para inspecionar o mapeamento local.

## Sinais Bloqueados e Pendentes

As threads possuem máscaras de sinais que podem bloquear temporariamente a entrega dos sinais selecionados. Um sinal bloqueado que foi gerado permanece pendente até que possa ser entregue, sujeito às regras dos sinais padrão e de tempo real. Sinais padrão do mesmo tipo podem ser combinados, em vez de enfileirados uma vez por ocorrência.

Em um processo multithread, um sinal direcionado ao processo pode ser entregue a uma thread elegível que não o bloqueie; um sinal direcionado a uma thread tem como destino a thread especificada. Portanto, um projeto correto de sinais exige mais que verificar se “o processo o bloqueou”.

## Sinais que Não Podem Ser Tratados

`SIGKILL` encerra um processo, e `SIGSTOP` o interrompe. Nenhum deles pode ser capturado, ignorado ou bloqueado. Isso garante que o kernel mantenha o controle definitivo, mas também significa que `SIGKILL` não oferece oportunidade para a limpeza no nível da aplicação.

Até mesmo `SIGKILL` pode não fazer uma tarefa desaparecer instantaneamente da perspectiva de um observador. Uma tarefa pode estar aguardando uma operação ininterruptível do kernel e, depois do encerramento, seu pai ainda precisa coletar seu status.

