Topologia é a forma como os dispositivos de uma rede se conectam. Ela pode ser **física** (cabos e equipamentos de fato) ou **lógica** (como os dados circulam).

![[Pasted image 20261111094034.png]]

**Barramento (bus)**  
Todos compartilham um único cabo central.

- ✅ Barata e simples de montar
- ❌ Um rompimento derruba a rede, o desempenho cai com o número de dispositivos e é difícil diagnosticar falhas
- 📍 Quase extinta. Era comum nas redes Ethernet coaxiais antigas.

**Anel (ring)**  
Cada dispositivo se liga a dois vizinhos, e os dados circulam em uma direção (ou duas, no anel duplo).

- ✅ Tráfego organizado, sem colisões e com desempenho previsível
- ❌ Uma falha pode interromper o anel e adicionar nós é trabalhoso
- 📍 Usada em Token Ring (legado) e em backbones de fibra com anel duplo (FDDI, SONET/SDH), que resistem a falhas.

**Estrela (star)**  
Todos se conectam a um ponto central (switch).

- ✅ Fácil de administrar, expandir e diagnosticar, e a falha de um cabo afeta só um dispositivo
- ❌ O switch central é ponto único de falha e exige mais cabeamento
- 📍 **É a mais utilizada hoje**, em praticamente todas as redes locais (LANs) de casas, escritórios e escolas.

**Malha (mesh)**  
Os dispositivos têm múltiplas conexões entre si. Na malha completa, todos se ligam a todos. Na parcial, só os nós importantes.

- ✅ Alta redundância e tolerância a falhas
- ❌ Custo e complexidade altos, e o número de links cresce rápido (n·(n−1)/2 na malha completa)
- 📍 Usada na internet (roteadores), em backbones, em data centers e em redes Wi-Fi mesh.

**Árvore (tree)**  
É uma hierarquia de estrelas conectadas entre si.

- ✅ Escalável e organizada por níveis, com facilidade de isolar segmentos
- ❌ Se o nó raiz cair, a rede se divide, e a manutenção fica mais complexa em redes grandes
- 📍 Redes corporativas e de campus, com os modelos núcleo, distribuição e acesso.

**Híbrida**  
Combina duas ou mais topologias.

- ✅ Flexível e adaptável a cada necessidade
- ❌ Projeto e gestão mais complexos e custo maior
- 📍 É o que existe na prática em redes grandes: estrelas nos andares, árvore no prédio e malha no núcleo.

**Resumo:** nas LANs, estrela (ou árvore de estrelas) domina. Na parte que exige alta disponibilidade, como a internet, o núcleo e os data centers, a malha é a mais usada. Barramento e anel são sobretudo históricos ou de nicho.