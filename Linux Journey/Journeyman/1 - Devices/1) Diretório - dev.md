# Diretório /dev

Aprenda como o Linux expõe interfaces de dispositivos e pseudodispositivos por meio de nós em `/dev`.

O Linux expõe muitas interfaces de dispositivos do kernel por meio de objetos especiais do sistema de arquivos chamados nós de dispositivos. Eles normalmente aparecem em `/dev`, junto com links simbólicos úteis e endpoints de comunicação. Abrir um nó de dispositivo conecta uma aplicação a um driver do kernel, em vez de acessar bytes armazenados em um arquivo comum.

## Exploração de `/dev`

Liste o diretório sem desreferenciar nem ler dispositivos:

```bash
$ ls -l /dev
```

As entradas podem representar armazenamento físico, terminais, interfaces de entrada, dispositivos lógicos ou pseudodispositivos fornecidos pelo kernel. Nem todo componente de hardware precisa de seu próprio nó visível ao usuário, e um dispositivo pode ser representado por vários links ou interfaces.

O primeiro caractere de uma listagem longa identifica o tipo de objeto do sistema de arquivos. Os nós de dispositivos de caractere e de bloco aparecem como `c` e `b`; lições posteriores examinam esses tipos e seus números maiores e menores.

## Pseudodispositivos

Alguns nós fornecem serviços do kernel sem corresponder a um hardware físico. `/dev/null` aceita e descarta os dados gravados:

```bash
$ command > /dev/null
```

Outros exemplos conhecidos incluem `/dev/zero`, que produz bytes zero, e `/dev/urandom`, que fornece bytes aleatórios por meio do subsistema de aleatoriedade do kernel. Cada um possui uma semântica específica; não deduza seu comportamento apenas pelo nome do arquivo.

## Gerenciamento Dinâmico de Dispositivos

Em sistemas Linux modernos, o `devtmpfs`, apoiado pelo kernel, pode preencher os nós básicos de dispositivos à medida que eles aparecem. Um gerenciador de dispositivos no espaço do usuário, como o `udev`, processa eventos, aplica permissões e propriedades e cria links simbólicos úteis ou nomes definidos por políticas. As responsabilidades exatas variam conforme o sistema.

Links estáveis, como as entradas em `/dev/disk/by-id/` ou `/dev/disk/by-uuid/`, podem ser mais seguros em configurações do que nomes baseados na ordem de detecção, como `/dev/sda`, que podem mudar quando a topologia do hardware ou a ordem de descoberta é alterada.

## Interação Segura

Ferramentas comuns podem abrir nós de dispositivos, mas isso não torna seguras leituras e gravações arbitrárias. A leitura pode expor entradas ou dados confidenciais; a gravação em um disco, terminal ou interface de firmware pode corromper dados ou atrapalhar usuários. É por isso que permissões de nós de dispositivos, grupos, ACLs, capacidades e a intermediação por serviços restringem o acesso.

Use primeiro ferramentas de descoberta somente para leitura, confirme o nó exato e a identidade do dispositivo e siga a documentação específica dele. Nunca faça experiências redirecionando dados para uma entrada desconhecida de `/dev` em um sistema importante.

Use o laboratório [Exploração de Dispositivos de Hardware no Linux](https://labex.io/labs/comptia-explore-hardware-devices-in-linux-590861) para realizar uma inspeção somente para leitura em um ambiente controlado.
