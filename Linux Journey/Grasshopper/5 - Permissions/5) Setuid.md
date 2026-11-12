# Setuid

Aprenda como o bit de modo set-user-ID afeta programas executáveis e por que ele exige uma revisão cuidadosa de segurança.

Alguns programas precisam de um acesso rigorosamente controlado que seus solicitantes normalmente não possuem. Em um arquivo comum executável, o bit set-user-ID pode fazer com que um novo processo receba o ID de usuário do proprietário do arquivo como seu ID de usuário efetivo. O programa pode então realizar operações autorizadas para essa identidade enquanto mantém informações sobre o solicitante.

Setuid não é uma instrução geral para “executar como root”. Seu efeito depende do proprietário do executável, do sistema operacional, do sistema de arquivos e das opções de montagem, além da forma como o programa gerencia suas credenciais.

## Reconhecimento do Setuid

Em sistemas que usam um executável `passwd` com setuid, uma listagem longa pode se parecer com esta:

```bash
$ ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root 68248 Jan 10 09:30 /usr/bin/passwd
```

O `s` minúsculo na posição de execução do proprietário significa que tanto o setuid quanto a execução do proprietário estão definidos. Se o setuid estiver presente, mas a execução do proprietário estiver ausente, `ls -l` mostrará um `S` maiúsculo nessa posição.

Não presuma que todas as distribuições tenham o mesmo modo ou projeto de autenticação. Inspecione o sistema real em vez de depender do exemplo.

## Compreensão da Alteração de Credenciais

Quando o kernel respeita o setuid durante a execução, o novo processo normalmente recebe um ID de usuário efetivo baseado no proprietário do executável. Para um programa pertencente ao root, isso pode fornecer acesso autorizado como root, mas apenas enquanto o programa é executado e somente por meio das operações realizadas por seu código.

Esse mecanismo pode permitir que um programa cuidadosamente desenvolvido valide uma solicitação e faça uma alteração restrita em um estado protegido. Por exemplo, um utilitário local para alterar senhas pode precisar de acesso controlado a dados de autenticação que usuários comuns não podem editar diretamente. As implementações modernas também dependem do PAM, de bloqueio de arquivos, de políticas e de outras proteções; o setuid sozinho não explica todo o fluxo de trabalho.

## Definição e Remoção do Bit

Defina o setuid simbolicamente com:

```bash
$ sudo chmod u+s myfile
```

Na notação octal, o setuid contribui com `4` em um dígito inicial de bits especiais:

```bash
$ sudo chmod 4755 myfile
```

Neste caso, o `4` inicial define o setuid e `755` define os bits comuns do proprietário, do grupo e dos outros. Remova o setuid sem alterar os demais modos com `chmod u-s myfile`.

## Tratamento do Setuid como Recurso Sensível à Segurança

Uma falha em um programa setuid privilegiado pode se tornar um caminho de elevação de privilégios. Esses programas devem validar as entradas, controlar o ambiente e os caminhos de arquivos nos quais confiam, evitar comportamentos inseguros em subprocessos, minimizar o código privilegiado e descartar as credenciais elevadas assim que possível.

Normalmente, o Linux não respeita o setuid em scripts interpretados, pois sua implementação segura apresenta problemas de condições de corrida e relacionados ao interpretador. Sistemas de arquivos montados com `nosuid` também suprimem os efeitos de setuid e setgid. Prefira mecanismos mais restritos, como operações intermediadas por serviços, uma política `sudo` cuidadosamente delimitada ou capacidades, quando forem adequados ao requisito.

Nunca adicione setuid a um shell, interpretador ou programa copiado arbitrariamente como experimento em um sistema compartilhado. Audite os arquivos setuid existentes e pratique apenas em um ambiente isolado e descartável.