# Reparo de Sistemas de Arquivos

Aprenda a diagnosticar danos no sistema de arquivos e escolher um fluxo de reparo offline, específico do tipo e com backups.

O reparo de um sistema de arquivos reescreve metadados para restaurar sua consistência interna. Ele pode descartar referências ou dados danificados e agravar a perda quando o hardware de armazenamento está falhando. Trate o reparo como uma operação de recuperação: preserve primeiro as evidências e os dados recuperáveis e só então use a ferramenta documentada para o sistema de arquivos exato.

## Diagnóstico antes do Reparo

Sintomas como erros de E/S, remontagens somente para leitura, arquivos ausentes ou falhas de montagem não comprovam todos uma corrupção do sistema de arquivos. Primeiro, reúna evidências somente para leitura:

```bash
$ findmnt --target /affected/path
$ lsblk -f
$ journalctl -k -b
```

Verifique a pilha de armazenamento, a integridade do dispositivo, os cabos ou o caminho de rede, o estado do RAID, a criptografia e os eventos recentes. Se o dispositivo estiver falhando, verificações repetidas podem consumir sua vida útil restante. Quando possível, capture uma imagem ou um clone com uma ferramenta voltada à recuperação e trabalhe sobre a cópia.

## Identificação do Sistema de Arquivos e do Dispositivo Exatos

Determine se o sistema de arquivos reside em uma partição, volume lógico, dispositivo RAID, mapeamento criptografado ou disco inteiro. Não execute um verificador em `/dev/sda` apenas porque uma partição filha como `/dev/sda1` está afetada.

Use `lsblk -f`, `blkid`, `findmnt` e ferramentas das camadas de armazenamento para mapear o destino. As assinaturas de detecção podem estar desatualizadas, portanto compare-as com a configuração conhecida e os backups.

## Colocação do Sistema de Arquivos Offline

A maioria dos verificadores tradicionais de consistência exige que o sistema de arquivos esteja desmontado. Um sistema montado muda enquanto o verificador o lê, e as gravações de reparo podem entrar em conflito com o estado em cache do kernel, causando corrupção.

Interrompa os serviços dependentes, desmonte os sistemas de arquivos aninhados, mova os diretórios de trabalho dos processos e desative as camadas superiores conforme necessário. Para o sistema de arquivos raiz, inicialize um ambiente de resgate ou use o mecanismo documentado de verificação offline da distribuição. Confirme com `findmnt` que o destino não esteja montado no namespace relevante.

## Uso da Ferramenta Específica do Sistema de Arquivos

`fsck` é uma interface capaz de invocar auxiliares específicos de sistemas de arquivos. Ele não é um único mecanismo universal de reparo. Alguns exemplos de fluxos distintos são `e2fsck` para sistemas ext, `xfs_repair` para XFS e as ferramentas de diagnóstico e recuperação específicas do Btrfs.

Opções com nomes semelhantes podem possuir semânticas diferentes. Em particular, não aplique opções `--repair` ou de força copiadas do guia de outro sistema de arquivos. Leia o manual instalado e a documentação atual do projeto ou da distribuição sobre recuperação. Comece por um modo sem modificações ou de diagnóstico quando a implementação oferecer um modo confiável, capture a saída e compreenda os reparos propostos.

## Verificação e Restauração do Serviço

Registre a ferramenta de reparo, a versão, as opções, a saída e o status de retorno. Após o reparo, repita as verificações da integridade do dispositivo, monte primeiro somente para leitura quando for apropriado, inspecione os dados essenciais e compare-os com backups conhecidos. Depois, restaure gradualmente as montagens e os serviços normais enquanto monitora os logs do kernel e das aplicações.

Um sistema de arquivos voltar a ser montável não comprova que todos os arquivos estejam corretos. Restaure a partir dos backups os dados de aplicações perdidos ou danificados e valide-os no nível da aplicação.

