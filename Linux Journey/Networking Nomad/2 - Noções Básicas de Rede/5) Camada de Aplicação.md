# Camada de aplicação

Aprenda como os protocolos de aplicação definem mensagens de serviço, estado, nomenclatura e comportamento de segurança.

A camada de aplicação do TCP/IP contém os protocolos que as aplicações usam para solicitar e fornecer serviços de rede. Ela abrange muitas funções que a terminologia OSI separa nas camadas de aplicação, apresentação e sessão.

## Mensagens e semântica dos protocolos

Um protocolo de aplicação define como os pares interpretam mensagens e estados. O HTTP define solicitações, respostas, métodos, códigos de estado e campos. O DNS define consultas e registros de recursos. O SMTP define comandos e respostas para a transferência de mensagens.

Nem todo protocolo de aplicação acrescenta um “cabeçalho de aplicação” fixo. Alguns usam campos textuais, outros registros binários, outros vários formatos aninhados, e alguns transportam uma sequência contínua de mensagens por uma única conexão de transporte.

## Clientes, servidores e pares

Um cliente inicia uma solicitação ou conexão com um serviço; um servidor escuta ou aceita essa interação de outra forma. Esses são papéis em uma interação, não categorias permanentes de dispositivos. Um host pode ser cliente de DNS e servidor de SSH ao mesmo tempo, e alguns protocolos usam papéis ponto a ponto.

## Nomes, portas e seleção de serviços

Uma aplicação pode resolver um nome de serviço para um ou mais endereços IP e escolher um ponto de extremidade de transporte. Portas conhecidas fornecem valores padrão, não uma prova imutável de um protocolo. O HTTP geralmente usa a porta TCP 80 e o HTTPS, a porta TCP 443, mas ambos podem operar em outros lugares. O SMTP usa portas e políticas diferentes para retransmissão e envio de mensagens.

## Segurança e testes de ponta a ponta

O TLS pode acrescentar confidencialidade, integridade e identidade autenticada do par quando a validação do certificado e o nome do ponto de extremidade estão corretos. Ele não autoriza automaticamente todas as ações da aplicação. Teste o mesmo nome, família de endereços, porta, protocolo, credenciais e solicitação usados pelo cliente real.

Por exemplo, um diagnóstico de HTTPS pode verificar separadamente a resolução, a conexão TCP, o certificado e o nome TLS, a resposta HTTP e o conteúdo da aplicação. O sucesso em uma etapa delimita o problema, mas não comprova todas as etapas posteriores.

