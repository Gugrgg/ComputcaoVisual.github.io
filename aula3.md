## Amostragem e Quantização de Imagens

Expressamos imagens digitais como funções bidimensionais na forma $f(x,y)$, onde o valor da amplitude nas coordenadas espaciais $(x,y)$ é uma quantidade escalar positiva. Quando gerada a partir de processos físicos, a intensidade é proporcional à energia irradiada, dependendo da iluminação (incidência, de 0 a infinito) e da refletância (reflexão, de 0 a 1).

Para converter a cena contínua em formato digital, aplicamos dois processos essenciais:

* **Amostragem:** A digitalização das coordenadas espaciais (transformar o espaço em uma grade de pixels).
* **Quantização:** A digitalização dos valores de amplitude (atribuir um nível discreto de intensidade a cada pixel).

A origem convencional de uma imagem digital fica no canto superior esquerdo — $f(0,0)$ — baseada no comportamento dos monitores e dispositivos de varredura. A qualidade visual resultante depende fortemente da quantidade de pixels e dos níveis de intensidade disponíveis:

* **Faixa dinâmica:** A razão entre a intensidade máxima mensurável (limitada pela saturação) e a mínima detectável (limitada pelo ruído).
* **Contraste:** A diferença entre os níveis superior e inferior presentes na imagem.


* **Resolução espacial:** Medida do menor detalhe discernível (frequentemente em dpi). O tamanho físico deve ser sempre especificado junto com a dimensão em pixels.
* **Profundidade de bits:** Uma imagem com $2^k$ níveis é chamada de "imagem de $k$ bits" (ex: 8 bits = 256 tons de cinza). Usar poucos bits resulta em "falsos contornos" visíveis.

## Fundamentos do Domínio Espacial

As técnicas no domínio espacial manipulam os pixels diretamente no plano da imagem, sendo computacionalmente eficientes. Elas se dividem em duas categorias:

* **Transformações de intensidade:** Operam em pixels isolados (processamento ponto a ponto).


* **Filtragem espacial:** Operam considerando os arredores de um pixel (processamento por vizinhança).



A operação geral é definida pela equação $g(x,y) = T[f(x,y)]$, onde $f$ é a entrada, $g$ é a saída e $T$ é o operador definido em uma vizinhança de $(x,y)$. Quando a vizinhança é reduzida ao menor tamanho possível ($1\times1$), a equação se torna uma função de transformação de níveis de intensidade: $s = T(r)$, onde $r$ e $s$ representam as intensidades de entrada e saída, respectivamente.

## Transformações Básicas de Intensidade

Essas funções ponto a ponto são essenciais para o realce de imagens e se classificam em três famílias.

### 1. Transformações Lineares

Incluem a função identidade (saída igual à entrada) e o negativo da imagem. O negativo inverte a escala de intensidade pela fórmula $s = (L-1) - r$ (sendo $L$ a intensidade máxima). É a melhor técnica para realçar detalhes brancos ou acinzentados escondidos em fundos predominantemente escuros.

### 2. Transformações Logarítmicas

Modeladas pela equação $s = c \log(1+r)$, essa função garante que o logaritmo nunca seja zero adicionando $1$ a $r$.

* **Efeito:** Expande os níveis de cinza escuros (tornando os detalhes mais visíveis) e comprime os níveis mais claros (evitando que "estourem" e percam detalhes).


* **Aplicação:** Indispensável para visualizar o Espectro de Fourier, onde os valores podem passar de $10^6$. Sem o logaritmo, os pixels claros dominariam uma tela padrão de 8 bits e ocultariam os dados de menor amplitude.



### 3. Transformações de Potência (Correção Gama)

Definidas por $s = cr^\gamma$, essas curvas são controladas pelo valor de $\gamma$:

* **$\gamma < 1$ (fracionário):** Clareia seletivamente as áreas escuras, mapeando uma pequena faixa de entradas escuras para uma faixa ampla na saída. Útil para imagens subexpostas.


* **$\gamma > 1$:** Faz o oposto, escurecendo imagens "lavadas" e realçando detalhes em regiões brilhantes.



Vários monitores e sensores seguem uma resposta não-linear de lei de potência. Aplicar um $\gamma$ compensatório (entre 1.8 e 2.5) — processo chamado de **Correção Gama** — é o que garante que a imagem digital seja exibida na tela com as intensidades exatas da captura.

## Transformações Lineares Definidas por Partes

Esta abordagem complementar permite a criação de funções arbitrariamente complexas, com a única desvantagem de exigir configuração manual mais detalhada pelo usuário.

* **Alargamento de Contraste e Limiarização:** Se uma foto fica "cinzenta" devido a má iluminação ou erros de lente, o alargamento estica o histograma para ocupar todo o espectro disponível no dispositivo. Já a limiarização ("thresholding") aplica um corte drástico que transforma a imagem em binária (preto e branco).


* **Fatiamento de Níveis de Intensidade:** Enfatiza apenas um intervalo específico de tons. Essa técnica é aplicada em imagens de satélite para realçar massas de água, ou na medicina para isolar vasos sanguíneos e falhas em radiografias, reduzindo ou preservando o fundo da imagem original.


* **Fatiamento por Planos de Bits:** Em vez de fatiar intensidades brutas, isola a contribuição de cada bit da profundidade do pixel. Em uma imagem de 8 bits, o 8º plano (mais significativo) contém a maior parte dos dados visuais da imagem. Esse método permite a reconstrução visual descartando os planos menos significativos, servindo como uma estratégia básica de compressão de imagens.
