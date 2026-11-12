# Permissões de Processos

Aprenda como IDs de usuário reais, efetivos e salvos ajudam os processos Linux a identificar solicitantes e gerenciar privilégios.

As verificações de autorização do Linux atuam sobre as credenciais dos processos, não diretamente sobre um nome de usuário digitado. Um processo possui vários IDs de usuário e grupo relacionados, cada um com uma função diferente. A maioria dos programas comuns começa com identidades correspondentes, enquanto programas privilegiados podem usar valores distintos deliberadamente.

## ID de Usuário Real

O ID de usuário real identifica a conta que iniciou o processo ou sua sessão de login ancestral. Os programas podem consultá-lo para diferenciar o solicitante de uma identidade efetiva elevada.

Para um comando comum iniciado pelo usuário Bob, o ID de usuário real normalmente corresponde ao UID de Bob. Criar outro processo não cria uma nova conta nem altera essa identidade por si só.

## ID de Usuário Efetivo

O ID de usuário efetivo é a credencial de usuário utilizada em muitas verificações de sistema de arquivos e privilégios. Normalmente, ele corresponde ao UID real. A execução de um programa setuid respeitado pode, em vez disso, inicializá-lo a partir do proprietário do executável.

Por exemplo, um utilitário de senhas cuidadosamente desenvolvido pode ser executado com um UID efetivo elevado para atualizar dados de autenticação protegidos. O programa ainda deve aplicar a política com base no solicitante, na conta requisitada, nos resultados do PAM e em outros contextos. Possuir um UID efetivo não torna automaticamente legítimas todas as operações solicitadas.

## ID Set-User-ID Salvo

O ID set-user-ID salvo permite que um programa mantenha uma identidade que poderá restaurar posteriormente, sujeito às regras das chamadas de sistema. Um programa privilegiado pode trocar temporariamente seu UID efetivo por um valor menos privilegiado, realizar um trabalho comum com autoridade reduzida e restaurar a identidade salva somente para uma operação de escopo restrito.

Isso é mais seguro do que manter autoridade elevada durante todo o programa, mas apenas quando implementado corretamente. Os programas devem descartar os privilégios permanentemente quando eles deixarem de ser necessários e verificar se cada chamada de alteração de credenciais falhou.

## IDs de Usuário São Apenas Parte do Conjunto de Credenciais

Os processos também possuem credenciais de grupo reais, efetivas, salvas e suplementares. IDs do sistema de arquivos, capacidades, namespaces, módulos de segurança, ACLs, opções de montagem e políticas de serviços podem afetar ainda mais a autorização. Portanto, “o UID permite” muitas vezes é apenas parte de uma explicação completa.

Use ferramentas como `ps` e `/proc/PROCESS/status` para inspecionar credenciais no Linux. A disponibilidade dos campos e os formatos de exibição variam, portanto consulte a documentação local e evite alterar credenciais apenas para fazer experiências em um sistema compartilhado.