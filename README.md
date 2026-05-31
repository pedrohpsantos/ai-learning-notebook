# Criptografia pós-quântica (NIST PQC standards)

## Contexto e Objetivos

O sucesso desta tarefa é extrair a essência da ameaça quântica e consolidar os objetivos de estudo em uma lista de fácil leitura para o seu repositório.

Este projeto investiga a obsolescência da criptografia atual (RSA e ECC) frente à ameaça de computadores quânticos capazes de executar o algoritmo de Shor, o que viabiliza ataques de captura retroativa de dados. A pesquisa explora os padrões de criptografia pós-quântica finalizados pelo NIST em 2024, conectando a teoria matemática a decisões arquiteturais práticas exigidas em Segurança da Informação e Engenharia de Software.

Os objetivos técnicos da investigação são:

* Mapear o modelo de ameaça quântica, diferenciando os impactos específicos dos algoritmos de Shor e Grover.
* Compreender os problemas matemáticos que fundamentam os novos padrões, incluindo LWE, MLWE, MSIS e funções hash.
* Classificar os algoritmos (ML-KEM, ML-DSA, FN-DSA e SLH-DSA) segundo suas funções de encapsulamento de chaves ou assinaturas digitais.
* Avaliar o impacto prático da migração sistêmica, analisando o tamanho das chaves, o efeito em protocolos como TLS 1.3 e o uso de criptografia híbrida.

## Curadoria de Fontes

* [Criptografia Pós-Quântica: o futuro da segurança da informação](https://www.youtube.com/live/2c5AFaUWFAo?si=VddXQJQDi7mMkhl-)
* [Post‑Quantum Security: How Lattice Cryptography Keeps Data Safe](https://youtu.be/ZRpcYSghGr8?si=jztlQcO9roxZ1l-l)
* [FIPS 203 – Module-Lattice-Based Key-Encapsulation Mechanism Standard](https://nvlpubs.nist.gov/nistpubs/fips/nist.fips.203.pdf)
* [NIST IR 8413-upd1 – Status Report on the Third Round of the NIST PQC Standardization Process](https://nvlpubs.nist.gov/nistpubs/ir/2022/NIST.IR.8413-upd1.pdf)
* [CRYSTALS-Dilithium Algorithm Specifications, Round 3 v3.1](https://pq-crystals.org/dilithium/data/dilithium-specification-round3-20210208.pdf)

## Engenharia de Prompts e Cicatrizes de Aprendizado

O processo de interação com a IA exigiu refinamento contínuo para evitar respostas superficiais e extrair o máximo das fontes fornecidas. Abaixo detalho a evolução do meu raciocínio:

**Tentativa Inicial:** "O que é criptografia de rede (lattice) e como funciona?"

### **Resultado:**
"A **criptografia baseada em reticulados** (do inglês *lattice cryptography*) é uma abordagem de criptografia pós-quântica projetada para manter dados seguros mesmo contra os futuros computadores quânticos. Ela substitui problemas matemáticos usados atualmente (como a fatoração de grandes números primos, que computadores quânticos poderiam quebrar facilmente) por problemas matemáticos em espaços multidimensionais que são extraordinariamente mais difíceis de se resolver.

**Como funciona a estrutura de um reticulado?**
Um reticulado pode ser visualizado como uma grade construída em um espaço multidimensional. Ele começa a partir de um ponto de origem e utiliza "vetores de base" (direções e distâncias específicas). Qualquer combinação matemática desses vetores cria um ponto válido no espaço, e o conjunto de todas essas combinações forma a malha infinita do reticulado.

**Como isso se transforma em um sistema de segurança?**
Para criar um problema matemático indecifrável a partir dessa grade, a criptografia introduz dois elementos complicadores:
*   **Múltiplas dimensões:** Em vez de operar em duas ou três dimensões visíveis, a criptografia aumenta a complexidade operando em espaços com mil dimensões, por exemplo.
*   **A adição de Ruído (Noise):** O grande segredo da segurança baseia-se em pedir para que se encontre um ponto específico que **não cai exatamente na grade** do reticulado. O ruído afasta o alvo dos cruzamentos exatos dos vetores, fazendo com que o desafio seja testar inúmeras combinações para chegar o mais perto possível daquele ponto.

**O problema de "Aprendizado com Erros" (Learning With Errors - LWE)**
Na prática matemática, esse uso de reticulados com ruído é a base do que se chama problema de *Learning with Errors* (Aprendizado com Erros). Funciona através da criação de conjuntos de equações lineares aleatórias e "ruidosas" que ocultam variáveis secretas. O ruído adicionado às equações torna impossível que algoritmos de resolução tradicionais, como a eliminação gaussiana, encontrem a resposta.

Para criptografar uma informação, o sistema utiliza esse conceito das seguintes formas:
*   Pode gerar uma equação linear que é **quase correta** para representar um bit "0", ou uma equação **muito distante da correta** para representar um "1".
*   Somente a parte que possui as variáveis secretas originais (a chave privada) é capaz de decifrar o ruído e distinguir os dois casos.

**Por que a criptografia de reticulado é segura contra computadores quânticos?**
A adição de ruído e as múltiplas dimensões garantem que não existam atalhos lógicos para resolver o problema de trás para frente. Para quebrar a criptografia, um invasor é obrigado a usar a "força bruta", tentando adivinhar uma possibilidade após a outra de forma contínua. A quantidade de variáveis é tão imensa que faria até mesmo as mentes dos melhores supercomputadores clássicos e futuros computadores quânticos "explodirem", exigindo milhares de anos para testar todas as combinações."

### **Problema Identificado:** 

O sucesso desta tarefa é sintetizar as falhas da resposta da IA e documentar as lacunas técnicas identificadas para o seu repositório.

A resposta original apresentou erros conceituais ao fundir o problema do vetor mais próximo (CVP) com o aprendizado com erros (LWE), tratando uma intuição geométrica de reticulados como sinônimo de equações lineares ruidosas. O uso de um prompt genérico resultou em uma explicação superficial de divulgação científica, desvinculada das fontes submetidas.

As três lacunas técnicas identificadas foram:

A omissão da diferença entre LWE e Module-LWE (MLWE), ignorando que o FIPS 203 utiliza anéis polinomiais para garantir a eficiência prática do ML-KEM.

A falta de justificativa matemática para a falha do algoritmo de Shor, que exige uma estrutura de grupos cíclicos inexistente no LWE.

A desconexão total com a base de conhecimento exigida, permitindo que a IA gerasse o texto sem consultar o FIPS 203 ou a especificação do Dilithium.

**Prompt Refinado:** "Com base exclusivamente no FIPS 203 e na especificação do CRYSTALS-Dilithium Round 3, responda em três blocos separados:
- Bloco 1 – CVP/SVP vs. LWE: Qual é a distinção técnica entre os problemas CVP (Closest Vector Problem) e LWE (Learning With Errors)? Por que a segurança do ML-KEM repousa sobre LWE e não sobre CVP, e como esses dois problemas se relacionam na hierarquia de dificuldade dos reticulados?
- Bloco 2 – De LWE para Module-LWE: Como o FIPS 203 define o problema Module-LWE (MLWE) e em que estrutura algébrica ele opera? Por que usar anéis polinomiais Zq[x]/(xⁿ+1) em vez de inteiros comuns é necessário para tornar o ML-KEM eficiente na prática?
- Bloco 3 – Por que o algoritmo de Shor não quebra o LWE: Que estrutura algébrica o algoritmo de Shor explora para quebrar RSA e ECC? O LWE possui essa estrutura? Justifique a resposta citando diretamente os documentos.

Para cada afirmação, indique o documento de origem e o número da seção ou página."

### **Resultado:** 
**Bloco 1 – CVP/SVP vs. LWE**
De acordo com o FIPS 203 (Seção 3.2, p. 13), a segurança do ML-KEM repousa sobre a dificuldade computacional do problema *Module Learning With Errors* (MLWE), que é uma generalização do problema *Learning With Errors* (LWE) e envolve a resolução de sistemas de equações lineares com ruído para recuperar variáveis secretas. No entanto, com base exclusivamente nos documentos solicitados (FIPS 203 e a especificação do CRYSTALS-Dilithium), não há menção ao problema CVP (*Closest Vector Problem*) em relação ao LWE. Consequentemente, as fontes fornecidas não detalham a distinção técnica entre CVP e LWE, nem explicam como esses dois problemas se relacionam na hierarquia de dificuldade dos reticulados, focando em vez disso no LWE/MLWE e no SVP (*Shortest Vector Problem*) para estabelecer o nível e as estimativas de segurança (como detalhado na especificação do CRYSTALS-Dilithium, Seções 6.3 e C.1).

**Bloco 2 – De LWE para Module-LWE**
O FIPS 203 (Seção 3.2, p. 13) define o problema MLWE como uma generalização do LWE em que, em vez de operar sobre matrizes de inteiros tradicionais (o espaço $\mathbb{Z}_q^n$), a operação ocorre sobre um módulo $R_q^k$, formado pelo produto cartesiano de um anel polinomial $R_q$. O FIPS 203 (Seção 4.3, p. 24) descreve a estrutura algébrica deste anel como $R_q = \mathbb{Z}_q[X]/(X^{256}+1)$, sendo composto por polinômios de grau inferior a 256 e coeficientes limitados ao módulo $q = 3329$. A utilização de anéis polinomiais em vez de inteiros comuns é estritamente necessária para tornar o ML-KEM eficiente na prática porque viabiliza a aplicação da Transformada Teórica dos Números (*Number-Theoretic Transform* - NTT), que atua como uma versão exata e especializada da transformada discreta de Fourier (FIPS 203, Seções 3.2 e 4.3). O FIPS 203 (Seção 4.3, p. 24) detalha que, com essa estrutura algébrica, o polinômio $X^{256}+1$ pode ser fatorado em 128 polinômios de grau 2 módulo $q$, tornando o anel $R_q$ isomorfo a uma soma direta de 128 extensões quadráticas de $\mathbb{Z}_q$. Trabalhar com a representação desses elementos no domínio da NTT permite que a multiplicação polinomial seja realizada coordenada a coordenada, sendo substancialmente mais rápida (FIPS 203, Seção 4.3, p. 24). A especificação do CRYSTALS-Dilithium (Seção 1.2, p. 5) corrobora esse mecanismo, afirmando que o anel polinomial é escolhido de forma exata para garantir que as operações de multiplicação possuam implementações altamente eficientes usando a NTT.

**Bloco 3 – Por que o algoritmo de Shor não quebra o LWE**
O documento FIPS 203 (Seção 1.2, p. 1) alerta apenas que a eventual criação de computadores quânticos de larga escala colocará em risco os criptossistemas de chave pública atuais que dependem da dificuldade dos problemas de fatoração de inteiros e logaritmo discreto, implementados historicamente em corpos finitos e curvas elípticas (ECC). Todavia, as fontes exclusivas designadas para a resposta (FIPS 203 e especificação do CRYSTALS-Dilithium) não contêm explicações sobre o funcionamento interno do algoritmo de Shor, tampouco detalham a estrutura algébrica específica que o algoritmo explora para quebrar o RSA e o ECC. A especificação do CRYSTALS-Dilithium (Seção 6, p. 23) nota que os únicos usos conhecidos de vantagens quânticas contra seus esquemas baseados em reticulados (como o LWE) envolvem o uso de algoritmos "tipo Grover", que servem unicamente para acelerar a busca exaustiva (força bruta). Portanto, conforme imposto pela restrição aos dois documentos, as fontes providenciadas não possuem as informações necessárias para justificar do ponto de vista algébrico por que o algoritmo de Shor não consegue quebrar o LWE. *(Nota: Caso deseje essa explicação técnica, ela estava presente no histórico de transcrição do vídeo em seus documentos iniciais, que mencionavam os atalhos usados pelo algoritmo de Shor no cálculo de períodos e módulos do RSA e ECC, mas foi omitida aqui devido à sua exclusão direta nas regras da requisição).*

* **Troubleshooting:** o prompt inicial era uma pergunta aberta de divulgação, então o modelo produziu uma resposta de divulgação. Três mudanças corrigem isso: (1) a restrição "com base exclusivamente em" obriga o NotebookLM a usar os PDFs em vez do conhecimento geral; (2) a divisão em blocos impede que CVP e LWE sejam mesclados implicitamente; (3) pedir a citação de seção/página funciona como verificação, porque se o modelo não encontrar a afirmação nos documentos ele sinaliza a lacuna em vez de inventar.

## Miniguia de Estudo Consolidado

### Resumos Estruturados

A urgência da transição para a criptografia pós-quântica ocorre devido à capacidade do algoritmo de Shor de quebrar os atuais sistemas RSA e ECC. Apesar de computadores quânticos viáveis estarem a anos de distância, a ameaça é imediata devido à tática de captura retroativa de tráfego cifrado.

A segurança dos novos padrões baseia-se em problemas matemáticos específicos que não devem ser confundidos:

* **SVP e CVP:** Problemas geométricos de reticulados focados em encontrar vetores curtos ou pontos próximos em uma grade.
* **LWE (Learning With Errors):** Problema distinto voltado à recuperação de variáveis secretas em sistemas de equações lineares ruidosas.

A evolução do LWE para o Module-LWE (MLWE) garante a viabilidade técnica dos padrões FIPS 203 e FIPS 204. A transição da operação sobre inteiros para o anel polinomial $\mathbb{Z}_q[X]/(X^{256}+1)$, com $q=3329$, permite a aplicação da Transformada Teórica dos Números (NTT). Essa escolha algébrica reduz a complexidade da multiplicação polinomial de $\mathcal{O}(n^2)$ para $\mathcal{O}(n \log n)$, tornando a execução eficiente em protocolos reais.

O processo de padronização do NIST consolidou as seguintes funções criptográficas:

* **ML-KEM (FIPS 203):** Encapsulamento de chaves para substituir RSA e ECDH.
* **ML-DSA (FIPS 204):** Padrão primário para assinaturas digitais baseadas em reticulados.
* **SLH-DSA (FIPS 205):** Alternativa de segurança conservadora ancorada estritamente em funções hash.
* **FN-DSA (FIPS 206):** Padrão em desenvolvimento com foco em assinaturas menores, porém com maior complexidade de implementação devido à amostragem gaussiana.

### Glossário

* Reticulado (Lattice): conjunto de todos os pontos gerados por combinações lineares inteiras de vetores de base em um espaço n-dimensional. Formalmente, L = {∑ aᵢbᵢ : aᵢ ∈ ℤ}.
* SVP (Shortest Vector Problem): dado um reticulado, encontrar o vetor não-nulo de menor norma. É NP-difícil no caso geral e não possui algoritmo quântico eficiente conhecido.
* CVP (Closest Vector Problem): dado um reticulado e um ponto alvo fora dele, encontrar o ponto do reticulado mais próximo do alvo. Pelo menos tão difícil quanto o SVP.
* LWE (Learning With Errors): dado o sistema As + e ≡ b (mod q), onde e é um vetor de ruído pequeno amostrado de uma distribuição gaussiana, recuperar o vetor secreto s. Dificuldade redutível ao SVP.
* MLWE (Module Learning With Errors): variante do LWE em que as operações ocorrem sobre o módulo Rqᵏ, onde Rq = ℤq[X]/(Xⁿ+1). Base de segurança do ML-KEM e ML-DSA.
* KEM (Key Encapsulation Mechanism): protocolo que permite estabelecer uma chave secreta compartilhada sobre canal público sem transmiti-la diretamente. Composto pelas operações KeyGen, Encaps e Decaps.
* NTT (Number-Theoretic Transform): versão da DFT sobre corpos finitos ℤq. Permite multiplicação polinomial em O(n log n), viabilizando a eficiência prática do ML-KEM e ML-DSA.
* ML-KEM: padrão FIPS 203, baseado em MLWE. Substitui RSA e ECDH para estabelecimento de chave. Opera sobre ℤ₃₃₂₉[X]/(X²⁵⁶+1).
* ML-DSA: padrão FIPS 204, baseado em MLWE e MSIS. Esquema primário de assinatura digital pós-quântica, derivado do CRYSTALS-Dilithium.
* SLH-DSA: padrão FIPS 205. Assinatura baseada exclusivamente em funções hash via hiper-árvore de Merkle. Opção conservadora: segurança não depende de hipóteses sobre reticulados.
* Harvest Now, Decrypt Later (HNDL): estratégia de ataque em que o adversário captura e armazena dados cifrados hoje para decifrá-los com um computador quântico no futuro. Torna a migração urgente antes da disponibilidade do CRQC.
* Criptografia híbrida: uso simultâneo de um algoritmo clássico e um pós-quântico durante a transição, garantindo que a segurança seja mantida mesmo se um dos dois for comprometido.

### Prompts Reutilizáveis para Revisão

* "Usando apenas o FIPS 203 (Seções 3.2 e 4.3), descreva passo a passo como o MLWE é instanciado no ML-KEM: qual é o anel, qual é o módulo q, como o ruído é amostrado e o que distingue uma instância MLWE de um vetor aleatório. Cite seção e página de cada afirmação."
* "Com base no NIST IR 8413-upd1, quais foram os critérios técnicos que levaram o NIST a escolher o CRYSTALS-Kyber como algoritmo primário para KEM? Quais candidatos foram eliminados e por quais razões específicas? Cite seção e página."
* "Com base no FIPS 203, FIPS 204 e na especificação do Dilithium, compare ML-KEM, ML-DSA e SLH-DSA nos seguintes eixos: (a) problema matemático de base, (b) função criptográfica, (c) tamanhos de chave e assinatura no nível de segurança intermediário, e (d) principal trade-off de implementação. Cite a fonte de cada dado."
* "A especificação do CRYSTALS-Dilithium (Seções 6.3 e C.1) descreve como as estimativas de segurança são derivadas a partir do SVP. Explique o raciocínio: como a dificuldade do MLWE se reduz ao SVP e como isso dimensiona os parâmetros dos algoritmos?"
* "Com base nos documentos disponíveis, quais são os obstáculos concretos para integrar o ML-KEM ao TLS 1.3? Considere tamanho dos handshakes, compatibilidade retroativa e a estratégia de criptografia híbrida durante a transição."
